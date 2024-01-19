# 插件

## 自定义拦截器

* 继承interceper 接口
* 使用@Intercepts 注解
* 使用@Signature

```java
@Intercepts (
@Signature (type = Executor.class, method = "query", args = (
MappedStatement. class, Object.class, RowBounds.class,
ResultHandler.class}),
@Signature (type = Executor.class, method = "close", args = (boolean.class})
} )
public class ExamplePlugin implements Interceptor {
｝
```

