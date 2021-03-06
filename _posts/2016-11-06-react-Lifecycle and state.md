---
layout: post
title:  "react Lifecycle and state"
date:   2016-11-06
categories: react
tags: javascript react
excerpt: react Lifecycle and state
---



## Lifecycle
 리엑트 컴포넌트는 각각의 `Lifecycle` 을 가지고 있으며 이것을 `override` 하여 특정한 코드를 삽이 할 수 있다.

 ### Mounting
  - constructor()
  - componentWillMount()
  - render()
  - componentDidmount()


 ### Updating
  - componentWillReceiveProps()
  - shouldComponentUpdate()
  - componentWillUpdate()
  - render()
  - componentDidUpdate()


 ### Unmouting
  - componentWillUmmount()

## state
 리엑트는 컴포넌트의 내부 상태를 변경하기 위한 `setState()` 메서드를 제공 한다. 컴포넌트 UI 상태를 업데이트할 떄는
 `this.state`를 직접 조작하는 것이 아니랑 항상 `setState` 메서드를 이용해야 한다.

 여기에는 여러 이유가 있다. 우선 `this.state`를 직접 조작하는 것은 리엑트의 상태 관리를 우회 하는 것이다.
 따라서 리엑트의 패러다임을 위반하게 되며, 나중에 `setState()`를 호출하면 이전에 수행한 변경이 무효화될 위험성까지
 내포 하고 있다. 또한 `this.state`를 직접 조작하면 나중에 애플리케이션에서 성능을 개선할 수 있는 기회가 줄어든다.
