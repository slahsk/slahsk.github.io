---
layout: post
title:  "JPA EntiyManager!"
date:   2017-03-18
categories: jpa
tags: java jpa
excerpt: JPA EntiyManager
---

## EntiyManager
 데이터베이스를 하나만 사용하는 애플리케이션은 일반적으로 `EntiyManagerFactory` 를 하나만 생성한다.

`EntiyManagerFactory`를 만드는 비용은 상당히 크다. 따라서 한 개만 만들어서 애플리케이션 전체에서 공유 하도록 설계되어 있다.

반면에 `EntiyManagerFactory` 에서 `EntiyManager`생성하는 비용은 거이 들지 않는다.

`EniyManagerFactory`는 여러 스레드가 동시에 접급해도 안전하므로 서로 다른 스레드 간에 공유해도 되지만, `EntiyManager`는 여러 스레드가 동시에 접그하며 동시성 문제가 발생하므로 스레드 간에 절대 공유하면 안된다

