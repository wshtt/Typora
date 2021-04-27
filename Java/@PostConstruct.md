

[toc]

## @PostConstruct

```java
package javax.annotation;

@Documented
@Retention (RUNTIME)
@Target(METHOD)
public @interface PostConstruct {
}
```

@PostConstruct修释的方法将在最后生成spring生成Bean的时候被调用。

在spring初始化Bean，初始化的时候会执行Bean下的@PostConstruct修释的方法，Bean只会被初始化一次，所以改方法只会执行一次。