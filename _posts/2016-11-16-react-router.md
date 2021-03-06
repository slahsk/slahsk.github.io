---
layout: post
title:  "react router"
date:   2016-11-16
categories: react
tags: javascript react
excerpt: "react router"
---



# 라우팅


## 기본적인 라우팅

```javascript
import React, { Component } from 'react';
import { render } from 'react-dom';


import About from './About';
import Home from './Home';
import Repos from './Repos';

class App extends Component{
  constructor(){
    super(...arguments);
    this.state ={
      route : window.location.hash.substr(1)
    };
  }

  componentDidMount(){
    window.addEventListener('hashchange', () => {
      this.setState({
          route:window.location.hash.substr(1)
      });
    });
  }

  render(){
    var Child;
    switch (this.state.route) {
      case '/about':Child = About;break;
      case '/repos':Child = Repos;break;
      default:Child = Home;
    }

    return(
      <div>
          <header>App</header>
          <menu>
            <ul>
              <li><a href="#/about">About</a></li>
              <li><a href="#/repos">Repos</a></li>
            </ul>
          </menu>
          <Child/>
      </div>
    );

  }

}

render(<App />, document.getElementById('root'));

```

컴포넌트가 마운트될 때 이벤트 리스너를 추가해 URL 바뀔 때마다 route 상태를 업데이트하고 컴포넌트를 다시 렌더링하게 한다.


## 문제점
 - URL 관리가 핵심적인 작입이 됐다.
 - 직접 URL을 수신하고 조작
 - 라우팅 코드가 복잡해질 수 있다.


## React Router
 - 중첩된 URL과 중첩된 컴포넌트 계층은 리액트 라우터의 선억식 API에서 핵심적인 개념이며, 리액트 코어의 일부는 아니지만 리액트 커뮤니티에서 표준으로 인정 받고 있다.
 - 컴포넌트를 중첩 단계에 관계없이 라우트와 연결하는 방법으로 UI를 URL과 동기화 한다.
 - URL을 변경하면 자동으로 언마운트 및 마운트 된다.

 ```javascript
 import React, { Component } from 'react';
 import { render } from 'react-dom';
 import { Router, Route, Link, browserHistory , IndexRoute} from 'react-router';


 import './style.css';


 import About from './About';
 import Home from './Home';
 import Repos from './Repos';
 import RepoDetails from './RepoDetails';
 import ServerError from './ServerError';

 class App extends Component{

   render(){
     return(
       <div>
           <header>App</header>
           <menu>
             <ul>
               <li><Link to="/about" activeClassName="active">About</Link></li>
               <li><Link to="/repos" activeClassName="active">Repos</Link></li>
             </ul>
           </menu>
           {this.props.children}
       </div>
     );

   }

 }

 render((
   <Router history={browserHistory} >
     <Route path="/" component={App}>
       <IndexRoute component={Home} />
       <Route path="about" component={About} title="About Us"/>
       <Route path="repos" component={Repos}>
         {/* <Route path="details/:repo_name" component={RepoDetails}/> */}
         <Route path="/repo/:repo_name" component={RepoDetails}/>
       </Route>
       <Route path="error" component={ServerError} />
     </Route>
   </Router>
 ), document.getElementById('root'));

 ```

### 인덱스 라우트
`/` 라우트엥서 서버에 접근하면 자식이 없는 App 컴포넌트가 렌더링 된다.

`<IndexRoute component={Home} />`

### 매개변수를 이용하는 라우트
` <Route path="/repo/:repo_name" component={RepoDetails}/>`

 - `:repo_name` 매개변수를 컴포넌트의 속성에 주입한다.
 - `RepoDetails` 에서 `componentWillReceiveProps` 라는 추가 수명주기 메서드에서도 데이터를 가져와야 한다.
마운팅된 후에도 사용자가 다른 리포지토리를 클릭하면 새로운 매개변수를 받을 수 있으며며 이경우 `componentDidMount` 가 호출되지 않고 `componentWillReceiveProps`가 호출되기 때문이다.

```javascript
componentDidMount(){


  let repo_name = this.props.params.repo_name;

  this.fetchData(repo_name);
}

componentWillReceiveProps(nextProps){
  let repo_name = nextProps.params.repo_name;

  this.fetchData(repo_name);
}

```

### 속성 전달하기
` <Route path="about" component={About} title="About Us"/>` 전달하면 `{this.props.route.title}`  통해 접근할 수 있다.

동적으로 컴포넌트로 전달하기 위해서는 `{this.props.children}` 그대로 렌더링하는 대신 이를 복제하고 추가 속서을 주입할 수 있다.

```javascript
let child = this.props.children && React.cloneElement(this.props.children, {
  repositories : this.state.repositories
});

```

https://github.com/ReactTraining/react-router/blob/master/docs/API.md




## 프로그래밍 방식으로 라우트 변경
`Link` 컴포넌트는 최종 사용자가 라우트와 상호작용하는 훌륭한 방법이지만 종종 컴포넌트 안에서
프로그래밍 방식으로 라우트를 처리해야 할는 경우가 있다. 즉, 특정한 상황에서 자동으로 이전으로 돌아가거나 사용자는 다른 라우트로 리디렉션해야 할 수 있다.

이러한 목적으로 리액트 라우터는 마운팅하는 모든 컴포넌트에 자동으로 자체 `history` 객체를 주입한다

`history` 객체는 부라우저의 히스토리 스택을 관리하는 역활을 한다.


메서드 | 설명
------------ | -------------
pushState | 새로운 URL로 이동하는 기본 히스토리 탐색 메서드이며 옵션 매개변수 객체를 지정할 수 있다. <br/> history.pushState(null, '/users/123') <br/> history.pushState({showGrades:true},'/users/123')
replaceState | pushState와 동일한 구문을 사용하며 현재 URL을 새로운 URL로 대체한다<br/> 히스토리의 길이에 영향을 주지 않고 URL을 대체한다는 점에서 리디렉션과 비슷하다.
goBack | 탐색 히스토리에서 한 항목 뒤로 이동한다.
goForward | 탐색 히스토리에서 한 항목 앞으로 이동한다.
Go | n 또는 -n만큼 앞으로 또는 뒤로 이동한다.
createHref  |  라우터의 구성을 이용해 URL을 만든다.


## react-router 2.0
```
// v1.x
<Router/>

// v2.0.0
// hash history
import { hashHistory } from 'react-router'
<Router history={hashHistory} />
```

```
// v1.x
import createBrowserHistory from 'history/lib/createBrowserHistory'
<Router history={createBrowserHistory()} />

// v2.0.0
import { browserHistory } from 'react-router'
<Router history={browserHistory} />
```

```
// v1.0.x
history.pushState(state, path, query)
history.replaceState(state, path, query)

// v2.0.0
router.push(path)
router.push({ pathname, query, state }) // new "location descriptor"

router.replace(path)
router.replace({ pathname, query, state }) // new "location descriptor"
```

https://github.com/ReactTraining/react-router/blob/master/upgrade-guides/v2.0.0.md
