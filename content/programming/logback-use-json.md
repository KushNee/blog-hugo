---
title: "logback 自动 json 序列化和转换器原理解析"
date: 2021-02-12T22:51:58+08:00
draft: true
tags: ["java", "logback", "converter"]
categories: ["Programming"]
isCJKLanguage: true
author: "Kush Nee"
---
# 如何自动对对象进行 json 序列化
## 前情提要
java 开发中，大家一般都会用 Slf4j+logback 的组合来写日志。我也是。

logback 有个很方便的功能：当你占位符对应的参数是个对象的时候，它会自动调用该对象的 toString 方法进行序列化，不用你手动做调用。

这个已经挺方便了，但是这样子的数据展示方式还是有局限性。当你需要把一段数据拿出来给其他语言的后端、前端，甚至是业务和运营看的时候，一段 raw 的数据，对方很可能就拒绝了。

这个时候我就想到，json 是一种更通用的数据格式啊。java 里本身就有很多方便快捷的 json 框架直接使用，而且就算你拿出来的还是一行数据，现在网上有那么多 json 美化工具，查看的门槛其实很低的。

## 方案设计
确定了需要转换的格式之后，就是确定实现细节了。刚开始其实我想的是写个切面，但是很快就意识到这样并不优雅。

为了操作到所有的日志，你肯定需要把切点织入 logback 的相关方法。简单来说，就是 `ch.qos.logback.classic.Logger` 这个类的方法中。那么多方法重载，看着都心累。。。

当然，用切面肯定能实现自动 json 序列化，但是我想做的其实是「替换掉日志输出中的序列化一环」，而不是「在写日志前加一个统一操作」。两者的含义并不一样，前者是对日志框架的修改，后者是对项目本身的修改。

所幸，后来被我找到了 logback 本身的 hook-转换器，可以在内部进行 json 方法的调用。

## 具体实现
网上找到一篇自定义 MessageConverter 的文章：[自定义一个 logback 的 MessageConverter](https://www.hollischuang.com/archives/3689)。

原文用的是 fastjson。虽然公司大量用的都是这个 json 框架，但我自己的项目为了减少依赖，会直接使用 `spring-boot-starter-web` 里的 jackson。下面贴出我的修改版：

1. 自定义一个 converter：
```java
public class JsonConverter extends MessageConverter {

    private final static ObjectMapper OBJECT_MAPPER = new ObjectMapper();

    @Override
    public String convert(ILoggingEvent event) {

        try {
            List<Object> list = new ArrayList<>();
            for (Object argument : event.getArgumentArray()) {
                Object o = (argument.getClass().getClassLoader() != null || argument instanceof Collection
                    || argument instanceof Map) ? OBJECT_MAPPER.writeValueAsString(argument) : argument;
                list.add(o);
            }
            return MessageFormatter.arrayFormat(event.getMessage(), list.toArray()).getMessage();
        } catch (Exception e) {
            return super.convert(event);
        }
    }

}
```

2. 添加自定义 converter 到 logback-spring.xml 配置文件中
```xml
<configuration>
  <conversionRule conversionWord="msg" converterClass="xxx.xxx.xxx.config.JsonConverter"/>
</configuration>
```
# logback 转换器原理分析
