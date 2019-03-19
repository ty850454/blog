---
title: '[转]spring boot注解'
date: 2019-03-18 15:43:12
tags:
- 注解
- 转载
categories:
- 技术文档
- java语言
- spring组件
- spring boot
---

Spring Boot 最核心的注解

> **原文作者：** 知乎用户 Java技术栈
**原文链接：** [https://zhuanlan.zhihu.com/...](https://zhuanlan.zhihu.com/p/57689422?utm_source=wechat_session&utm_medium=social&utm_oi=73532679651328)

<!-- more -->



### @SpringBootApplication

这是 Spring Boot 最最最核心的注解，用在 Spring Boot 主类上，标识这是一个 Spring Boot 应用，用来开启 Spring Boot 的各项能力。

其实这个注解就是 `@SpringBootConfiguration`、`@EnableAutoConfiguration`、`@ComponentScan` 这三个注解的组合，也可以用这三个注解来代替 `@SpringBootApplication` 注解。源码如下：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
        @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
        @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

}
```

### @EnableAutoConfiguration

> org.springframework.boot.autoconfigure.EnableAutoConfiguration

允许 Spring Boot 自动配置注解，开启这个注解之后，Spring Boot 就能根据当前类路径下的包或者类来配置 Spring Bean。

自动配置更多详细使用及实战可以看这篇文章[《Spring Boot自动配置原理、实战》](https://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247484365&idx=1&sn=a4ab1d977d6b03bf122b4d596d7ee1ab&scene=21#wechat_redirect)。

如：当前类路径下有 Mybatis 这个 JAR 包，`@MybatisAutoConfiguration` 注解就能根据相关参数来配置 Mybatis 的各个 Spring Bean。

### @Configuration

> org.springframework.context.annotation.Configuration

这是 Spring 3.0 添加的一个注解，用来代替 `applicationContext.xml` 配置文件，所有这个配置文件里面能做到的事情都可以通过这个注解所在类来进行注册。

下面几个相关注解也是非常重要的！

#### @Bean

用来代替 XML 配置文件里面的 `<bean ...>` 配置。

#### @ImportResource

如果有些通过类的注册方式配置不了的，可以通过这个注解引入额外的 XML 配置文件，有些老的配置文件无法通过 `@Configuration` 方式配置的非常管用。

#### @Import

用来引入额外的一个或者多个 @Configuration 修饰的配置文件类。

#### @SpringBootConfiguration

这个注解就是 `@Configuration` 注解的变体，只是用来修饰是 Spring Boot 配置而已，或者可利于 Spring Boot 后续的扩展，源码如下。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {

}
```

### @ComponentScan

> org.springframework.context.annotation.ComponentScan

这是 Spring 3.1 添加的一个注解，用来代替配置文件中的 `component-scan` 配置，开启组件扫描，即自动扫描包路径下的 `@Component` 注解进行注册 `bean` 实例到 `context` 中。

另外，`@ComponentScans` 是可重复注解，即可以配置多个，用来配置注册不同的子包。

关于重复注解，大家可以看这篇文章[《JAVA元注解@interface详解》](https://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247484195&idx=1&sn=e28f9c3a2f7628061fe15bef668f2ea7&scene=21#wechat_redirect)。

> 前面 5 个注解可以在这篇文章[《Spring Boot 最核心的 3 个注解详解》](https://mp.weixin.qq.com/s/kNvy_0jb4oJtYdaxryq5xg)中了解更多细节的。

### @Conditional

这是 Spring 4.0 添加的新注解，用来标识一个 `Spring Bean` 或者 `Configuration` 配置文件，当满足指定的条件才开启配置。

#### @ConditionalOnBean

组合 `@Conditional` 注解，当容器中有指定的 Bean 才开启配置。

#### @ConditionalOnMissingBean

组合 `@Conditional` 注解，和 `@ConditionalOnBean` 注解相反，当容器中没有指定的 Bean 才开启配置。

#### @ConditionalOnClass

组合 `@Conditional` 注解，当容器中有指定的 `Class` 才开启配置。

#### @ConditionalOnMissingClass

组合 `@Conditional` 注解，和 `@ConditionalOnMissingClass` 注解相反，当容器中没有指定的 `Class` 才开启配置。

#### @ConditionalOnWebApplication

组合 `@Conditional` 注解，当前项目类型是 `WEB` 项目才开启配置。

当前项目有以下 3 种类型。
```java
enum Type {
    /**
     * Any web application will match.
     */
    ANY,
    /**
     * Only servlet-based web application will match.
     */
    SERVLET,
    /**
     * Only reactive-based web application will match.
     */
    REACTIVE
}
```

#### @ConditionalOnNotWebApplication

组合 `@Conditional` 注解，和 `@ConditionalOnWebApplication` 注解相反，当前项目类型不是 WEB 项目才开启配置。

#### @ConditionalOnProperty

组合 `@Conditional` 注解，当指定的属性有指定的值时才开启配置。

#### @ConditionalOnExpression

组合 `@Conditional` 注解，当 SpEL 表达式为 true 时才开启配置。

#### @ConditionalOnJava

组合 `@Conditional` 注解，当运行的 Java JVM 在指定的版本范围时才开启配置。

#### @ConditionalOnResource

组合 `@Conditional` 注解，当类路径下有指定的资源才开启配置。

#### @ConditionalOnJndi

组合 `@Conditional` 注解，当指定的 JNDI 存在时才开启配置。

#### @ConditionalOnCloudPlatform

组合 `@Conditional` 注解，当指定的云平台激活时才开启配置。

#### @ConditionalOnSingleCandidate

组合 `@Conditional` 注解，当指定的 class 在容器中只有一个 Bean，或者同时有多个但为首选时才开启配置。

### @ConfigurationProperties

用来加载额外的配置（如 .properties 文件），可用在 `@Configuration` 注解类，或者 `@Bean` 注解方法上面。

关于这个注解的用法可以参考[《Spring Boot读取配置的几种方式》](https://mp.weixin.qq.com/s/aen2PIh0ut-BSHad-Bw7hg)这篇文章。

### @EnableConfigurationProperties

一般要配合 `@ConfigurationProperties` 注解使用，用来开启对 `@ConfigurationProperties` 注解配置 Bean 的支持。

### @AutoConfigureAfter

用在自动配置类上面，表示该自动配置类需要在另外指定的自动配置类配置完之后。

如 Mybatis 的自动配置类，需要在数据源自动配置类之后。
```java
@AutoConfigureAfter(DataSourceAutoConfiguration.class)
public class MybatisAutoConfiguration {
```

### @AutoConfigureBefore

这个和 `@AutoConfigureAfter` 注解使用相反，表示该自动配置类需要在另外指定的自动配置类配置之前。


> 本文原创首发于微信公众号：Java技术栈（id:javastack），关注公众号在后台回复 "boot" 可获取更多，转载请原样保留本信息。