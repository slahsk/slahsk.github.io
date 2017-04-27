---
layout: post
title:  react propTypes
date:   2016-10-30
categories: react
tags: javascript react
excerpt: react propTypes
---


## 속성 유효성 검사

컴포넌트에 어떤 속성을 사용할 수 있고, 어떤 속성이 필수 이며, 어떤 형식의 값을 받는지 등을 명시적으로
지정하는 것이 좋다. 이를 위해서 protoType 를 선언한다.

  - 언제든지 컴포넌트를 열고 어떤 속성이 필요한지, 그리고 속성의 형식이 무엇인지 알 수 있다.
  - 문제가 발생할 경우 리엑트가 콘솔에 오류 메시지를 출력해 어떤 속성이 잘못 되거나 누락됐는지 , 그리고 문제가 발생한  render 메서드가 무엇인지 알려줄 수 있다.

### 기본으로 제공되는 PropTypes

 유효성 검사기 | 설명
 ------------ | -------------
 PropTypes.array | 속성이 배열이야 한다.
 PropTypes.bool | 속성이 부울 값(true/false) 이어야 한다.
 PropTypes.func | 속성이 함수여야 한다.
 PropTypes.number | 속성이 숫자(또는 구문 분석으로 숫자를 얻을 수 있는 값) 이어야 한다.
 PropTypes.object | 속성이 객체여야 한다.
 PropTypes.string  |  속성이 문자열이야 한다.

 유효성 검사기 | 설명
 ------------|------------------
 PropTypes.oneOfType  |  속성이 여러 형식 중 하나일 수 있다.<br> 예:<br> PropTypes.oneOfType([<br>&nbsp;PropTypes.string, <br>&nbsp;PropTypes.number <br>&nbsp;PropTypes.instanceOf(Message)<br> ])
 PropTypes.arryOf  |  속성이 특정 배열이야 한다. <br>예) `PropTypes.arrayOf(PropTypes.number)`
 PropTypes.objectOf  |  속성이 특정 형식의 속성 값을 가진 객체여야 한다.<br>예) `PropTypes.objectOf(PropTypes.number)`
 PropTypes.shape  |  속성의 특정 형태를 가진 객체여야 한다. <br> PropTypes.shape({<br>&nbsp;color:PropTypes.string, <br>&nbsp;fontSzie: PropTypes.number <br> })
 PropTypes.node  |  속성이 렌더링 가능한 어떤 값이라도 될 수있다(숫자, 문자, 요소, 배열)
 PropTypes.element  |  속성이 리엑트 요소여야 한다.
 PropTypes.instanceof  |  속성이 지정한 클래스의 인스턴스여여 한다(자바스크립트의 instanceof 연산자를 이용한다.)<br> 예: PropTypes.instanceOf(Message)
 PropTypes.oneOf  |  속성이 열거형과 같이 특정한  범위의 값으로 헌정돼야 한다. <br>예: PropTypes.oneOf(['News','Photos'])


```javascript
import React, {Component,PropTypes} from 'react';
import Card from './Card';
import style from './kanban.css';

class List extends Component {
    render() {
        var cards = this.props.cards.map((card, i) => {
            return <Card key={card.id} id={card.id} color={card.color} title={card.title} description={card.description} tasks={card.tasks}/>;
        });

        return (
            <div className={style.list}>
                <h1>{this.props.title}</h1>
                {cards}
            </div>
        );
    }
};

List.propTypes = {
  title: PropTypes.string.isRequired,
  cards : PropTypes.arrayOf(PropTypes.object)
};

export default List;
```

## propType Custom

```javascript
import React,{Component, PropTypes} from 'react';
import CheckList from './CheckList';
import marked from 'marked';
import style from './kanban.css';

let titlePropType  = (props, propName, ComponentName) => {
  if(props[propName]){
    let value = props[propName];

    if(typeof value !== 'string' || value.length > 80){
      return Error('${propName} in ${ComponentName} is longer than 80 character');
    }
  }
};

class Card extends Component{
  ...
    return (
    ...
    );
  }
};

Card.propTypes = {
  id : PropTypes.number,
  title: titlePropType,
  description : PropTypes.string,
  color : PropTypes.string,
  tasks : PropTypes.arrayOf(PropTypes.object)
};

export default Card;
```
