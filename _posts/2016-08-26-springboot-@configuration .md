---
layout: post
title:  "springboot @Configuration"
date:   2016-08-26
categories: spring
tags: spring java
excerpt: springboot
---

## ComponentScan

spring 파일에서 `@Configuration` 은 설정파일이라는것을 알려준다.

`Spring Boot` 에서 `bean` 등록할때 사용 된다.
 
또한 `@Configuration` 설정한 자바파일도 `bean` 등록 된다.


```java
@Configuration
@EnableAutoConfiguration
@ComponentScan
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

`@ComponentScan` 을 등록하면 스프링에서 자동으로 `@Configuration` 찾아서 `bean` 을 등록 시켜준다.

하지만 `@ComponentScan` 사용 안한다면 `@Import` 이용하여 각각 등록을 해줘야 한다.

`java` 가 아닌 `xml` 설정 파일을 등록 하고 싶을때는 `@ImportResource` 이용하여 등록을 해줘야 한다.

## Auto-configuration
 
`@EnableAutoConfiguration` 를 등록함으로서 `Spring` 에서 기본적으로 사용하는 `jar` 들을 추가 해준다

만약 자동으로 등록되는 설정파일중에 제외 시키고 싶다면
```
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
```

이러한 식으로 따로 등록을 해줘야 한다.

##  @SpringBootApplication

`@SpringBootApplication` 를이용한다면 위에서 이용했던

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan

```

3개를 기본적으로 포함하고 있다.


```java
@SpringBootApplication // same as @Configuration @EnableAutoConfiguration @ComponentScan
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```
