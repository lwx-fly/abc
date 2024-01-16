# 客户绑定流程优化

> 会员id/微信unionId 多种绑定方式支持&#x20;
>
> 后台绑定和openApi等多种渠道和节点自定义支持



### 引用概览

* 策略
* 模板方法
* 责任链

### 具体实现

* 绑定event定义。绑定、解绑、换绑。使用策略模式+模板方法
* action定义。将公共逻辑提取成action，便于复用和拓展。



```java
public abstract class AbstractGuideRelationEvent implements ApiGuideRelationEvent {
    protected Logger log = LoggerFactory.getLogger(this.getClass());
    @Autowired
    private List<GuideBindAction> actionList;
    protected List<GuideBindActionType> actionTypes;
    @Autowired
    private PlatformTransactionManager transactionManager;
    /**
     * 调用前预处理
     */
    abstract void preExecute();

    /**
     * 调用后预处理
     */
    void afterExecute() {
    }

    @Override
    public void execute() {
        GuideBindContext context = getContext();
        TransactionStatus transaction = transactionManager.getTransaction(new DefaultTransactionDefinition());
        try {
            preExecute();
            GuideActionChain chain = genActionChain();
            chain.doAction();
            afterExecute();
            transactionManager.commit(transaction);
        } catch (RuntimeException e) {
            context.setSuccess(false);
            context.setMsg(e.getMessage());
            transactionManager.rollback(transaction);
        } catch (Exception e) {
            log.error("execute error", e);
            context.setSuccess(false);
            context.setMsg("绑定流程异常");
            transactionManager.rollback(transaction);
        }
    }


    public GuideActionChain genActionChain() {
        List<GuideBindActionType> actionTypes = this.actionTypes;
        GuideActionChain chain = new GuideActionChain();
        //actionTypes 数组长度很短，为了保证执行顺序，所以使用双重循环
        for (GuideBindActionType actionType : actionTypes) {
            for (GuideBindAction action : actionList) {
                if (Objects.equals(actionType, action.support())) {
                    chain.getHandlers().addLast(action);
                }
            }
        }
        return chain;
    }

    public GuideBindContext getContext() {
        return ApiGuideRelationEvent.GUIDE_BIND_CONTEXT.get();
    }
}
```

```java
@Service
public class GuideBindEvent extends AbstractGuideRelationEvent {

    /**
     * 将绑定流程拆分为多个action，支持根据不同类型。多个action动态组合
     */
    @Override
    public void preExecute() {
        GuideBindContext context = getContext();
        GuideBindSourceType sourceType = context.getSourceType();
        this.actionTypes = getActionTypes(sourceType);
        if (Objects.equals(sourceType, GuideBindSourceType.OPENAPI)) {
            context.setPropagation(BindPropagation.NEVER);
        }
    }


    private List<GuideBindActionType> getActionTypes(GuideBindSourceType sourceType) {
        if (Objects.equals(sourceType, GuideBindSourceType.OPENAPI)) {
            return ImmutableList.of(
                    PREPARE_BIND,
                    HISTORY_BIND_RELATION,
                    BIND_PROPAGATION,
                    TRY_GET_BIND_GUIDE,
                    BIND_ASSISTANT,
                    INIT_BIND,
                    NOTIFY_KYLIN_BIND,
                    ASYNC_USER_EVENT
            );
        }

        return ImmutableList.of(
                PREPARE_BIND,
                HISTORY_BIND_RELATION,
                BIND_PROPAGATION,
                TRY_GET_BIND_GUIDE,
                BIND_ASSISTANT,
                BIND_RULE_CHECK,
                INIT_BIND,
                NOTIFY_KYLIN_BIND,
                ASYNC_USER_EVENT
        );
    }
} 
```
