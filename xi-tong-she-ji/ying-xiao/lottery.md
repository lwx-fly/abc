---
description: 抽奖
---

# lottery

## 表结构设计

* 抽奖规则表
  * 类型 如下单抽奖，大促抽奖，积分抽奖
* 奖品表
* 中奖记录表
* 抽奖资格表

#### ISSUSES

* 抽奖权限
  * 人的白黑名单控制
  * 控制渠道
* 中奖概率控制
* 并发控制
  * 对抽中的奖品加锁
* 抽奖后的履约方式
  * 线下履约
  * 加入购物车
  * 直接生成奖品0元订单

#### 具体实现案例 <a href="#id-21" id="id-21"></a>

* 权限判断【设计模式：责任链】

```java
//定义需要执行的chain
@Data  
public class LotteryPrivilegeChain {  
  
    private List<LotteryPrivilegeHandler> handlers = Lists.newArrayList();  
  
    public boolean process(LotteryPrivilegeContext context) {  
        for (LotteryPrivilegeHandler handler : handlers) {  
            boolean flag = handler.handle(context);  
            if (!flag) {  
                return false;  
            }  
        }  
        return true;  
    }  
  
    public void addHandlers(List<LotteryPrivilegeHandler> handlers) {  
        this.handlers = handlers;  
    }  
  
    public void addHandler(LotteryPrivilegeHandler handler) {  
        this.getHandlers().add(handler);  
    }  
  
}

//定义handler接口，有具体的权限判断需要实现这个接口
@FunctionalInterface  
public  interface LotteryPrivilegeHandler {  
    boolean handle(LotteryPrivilegeContext context);  
}

//context类，存放后续需要判断的一些字段
@Getter  
@Setter  
public class LotteryPrivilegeContext {  
    private Integer userId;
    private Integer channel;
}


//引入所有实现的handlers
@Autowired  
private List<LotteryPrivilegeHandler> lotteryPrivilegeHandlers;

//具体调用逻辑
LotteryPrivilegeChain chain = new LotteryPrivilegeChain();  
chain.addHandlers(lotteryPrivilegeHandlers);

LotteryPrivilegeContext lotteryPrivilegeContext = LotteryPrivilegeContext.create()  
        .withUserId(userId)  
        .withChannel(request.getChannel());  
boolean flag = chain.process(lotteryPrivilegeContext);

```



