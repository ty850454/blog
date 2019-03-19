---
title: '[转]Spring Boot 2.x 启动全过程源码分析（全）'
date: 2019-03-18 23:39:38
tags:
- 转载
categories:
- 技术文档
- java语言
- spring组件
- spring boot
---
```java
@SpringBootApplication
public class SpringBootBestPracticeApplication {
  public static void main(String[] args) {
    SpringApplication.run(SpringBootBestPracticeApplication.class, args);
  }
}
```
分析SpringApplication 对象的 run 方法的源码和运行流程

> **原文作者：** 微信公众号 Java技术栈
**原文链接：** [https://mp.weixin.qq.com/...](https://mp.weixin.qq.com/s/iMPXjuKRKT5lMZ4oVSp4Ww)

<!-- more -->
