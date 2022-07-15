---
title: 没有ui界面的Sentinel Quickstart
date: 2022-07-15 15:11:00
tags: Java,sentinel,Spring,Linux,annotation
---

# Not UI For Sentinel Quickstart 

> 在没有使用独立界面的 Sentinel 使用

## 1、 引入相关依赖

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-core</artifactId>
    <version>1.8.0</version>
</dependency>
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-annotation-aspectj</artifactId>
    <version>1.8.0</version>
</dependency>
```

## 2. 增加 Sentinel 配置

```java
package com.demo.config;

import com.alibaba.csp.sentinel.annotation.aspectj.SentinelResourceAspect;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @description:
 * @author: lucas
 * @date: 2022/6/24
 * @since: this is write current version
 */
@Configuration
public class SentinelConfig {
    @Bean
    public SentinelResourceAspect sentinelResourceAspect() {
        return new SentinelResourceAspect();
    }
}
```

## 3. 创建规则

```java
package com.demo.core;

import com.alibaba.csp.sentinel.slots.block.degrade.DegradeRule;
import com.alibaba.csp.sentinel.slots.block.degrade.DegradeRuleManager;
import com.alibaba.csp.sentinel.slots.block.degrade.circuitbreaker.CircuitBreaker;
import com.alibaba.csp.sentinel.slots.block.degrade.circuitbreaker.CircuitBreakerStrategy;
import com.alibaba.csp.sentinel.slots.block.degrade.circuitbreaker.EventObserverRegistry;
import com.alibaba.csp.sentinel.util.TimeUtil;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.util.ArrayList;
import java.util.List;

/**
 * @description:
 * @author: lucas
 * @date: 2022/6/24
 * @since: this is write current version
 */
@Slf4j
@Component
public class Rule {

    public static final String key = "lucas";

    @PostConstruct
    public void init() {
        log.info("rule init...");
        this.rule();
    }

    public void rule() {
        List<DegradeRule> degradeRules = new ArrayList<>();
        DegradeRule degradeRule = new DegradeRule(key)
                .setCount(5)
                .setTimeWindow(30)
                .setStatIntervalMs(60000)
                .setGrade(CircuitBreakerStrategy.ERROR_COUNT.getType());
        degradeRules.add(degradeRule);
        DegradeRuleManager.loadRules(degradeRules);
        registerStateChangeObserver();
    }

    private void registerStateChangeObserver() {
        EventObserverRegistry.getInstance().addStateChangeObserver("logging", (prevState, newState, rule, snapshotValue) ->{
            log.info(rule.toString());
            if (CircuitBreaker.State.OPEN == newState) {
                log.info("{} -> OPEN at {} snapshotValue = {}", prevState, TimeUtil.currentTimeMillis(), snapshotValue);
            } else {
                log.info("{} -> {} at {} snapshotValue = {}", prevState, newState, TimeUtil.currentTimeMillis(), snapshotValue);
            }
        });
    }
}

```

## 4. 逻辑处理

> 在对应方法上增加注解`@SentinelResource`  
> 在方法内通过 `SphU` `Tracer.traceEntry(exception, entry);`进行打点

```java
package com.demo.service;

import com.alibaba.csp.sentinel.Entry;
import com.alibaba.csp.sentinel.SphU;
import com.alibaba.csp.sentinel.Tracer;
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.demo.core.Rule;
import org.springframework.stereotype.Service;

import java.util.Objects;

/**
 * @description:
 * @author: lucas
 * @date: 2022/6/24
 * @since: this is write current version
 */
@Service("sentinelService")
public class SentinelService {


    @SentinelResource(value = Rule.key, blockHandler = "defaultFallback")
    public String helloAnother(String name) {
        Entry entry = null;
        try {
            entry = SphU.entry(Rule.key);
            int i = 1/0;
        } catch (Exception exception) {
            if (!BlockException.isBlockException(exception)) {
                Tracer.traceEntry(exception, entry);
            }
        } finally {
            if (Objects.nonNull(entry)) entry.exit();
        }
        return "Hello, " + name;
    }

    public String defaultFallback(String name,
                                  BlockException blockException) {
        System.out.println("Go to default fallback");
        return "default_fallback";
    }
}

```

## 6、请求打印，请求5次

```text
2022-06-24 18:14:03.658  INFO 39773 --- [nio-9090-exec-1] com.demo.controller.SentinelController   : Hello, bad
2022-06-24 18:14:06.027  INFO 39773 --- [nio-9090-exec-2] com.demo.controller.SentinelController   : Hello, bad
2022-06-24 18:14:06.172  INFO 39773 --- [nio-9090-exec-3] com.demo.controller.SentinelController   : Hello, bad
2022-06-24 18:14:06.335  INFO 39773 --- [nio-9090-exec-4] com.demo.controller.SentinelController   : Hello, bad
2022-06-24 18:14:06.498  INFO 39773 --- [nio-9090-exec-5] com.demo.controller.SentinelController   : Hello, bad
2022-06-24 18:14:06.671  INFO 39773 --- [nio-9090-exec-6] com.demo.core.Rule                       : DegradeRule{resource=lucas, grade=2, count=5.0, limitApp=default, timeWindow=30, minRequestAmount=5, slowRatioThreshold=1.0, statIntervalMs=60000}
2022-06-24 18:14:06.671  INFO 39773 --- [nio-9090-exec-6] com.demo.core.Rule                       : CLOSED -> OPEN at 1656065646671 snapshotValue = 6.0
2022-06-24 18:14:06.673  INFO 39773 --- [nio-9090-exec-6] com.demo.controller.SentinelController   : Hello, bad
Go to default fallback
2022-06-24 18:14:06.979  INFO 39773 --- [nio-9090-exec-7] com.demo.controller.SentinelController   : default_fallback
```
