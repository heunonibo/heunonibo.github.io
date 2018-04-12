---
layout: post
title:  "React Day1 - LifeCycle API"
date:   2018-04-03 17:33:00 +0900
categories: jekyll update
---
> ##### 이 포스팅은 `React로 구현하는 웹 애플리케이션 제작 CAMP 4기` 수업 내용 바탕으로 정리된 글 입니다.

LifeCycle API
==============
* LifeCycle API는 브라우저에서 1) 나타날 때 2) 사라질 때 3) 업데이트 될 때 호출되는 API 입니다.


컴포넌트 순서
-------------
* Lifecycle simnulators : https://reactarmory.com/guides/lifecycle-simulators

```
컴포넌트 초기 설정 ) constructor -> componentWillMount(사라질 것) -> render() -> componentDidMount
컴포넌트 업데이트  ) componentWillReceiveProps(사라질 것) -> shouldComponentUpdate ->  componentWillUpdate (사라질 것) -> render() -> componentDidUpdate
컴포넌트 제거 ) componentWillUnmount
컴포넌트 에러 발생 ) componentDidCatch

componentWillReceiveProps(사라질 것) 을 대체할 만한 신규 API : getDerivedStateFromProps()
componentWillUpdate(사라질 것) 을 대체할 만한 신규 API : getSapshotBeforeUpdate() DOM 업데이트 되기 바로 직전의 상태
```

컴포넌트 초기 설정
---------
* 컴포넌트가 브라우저에 나타나기 전, 후에 호출되는 API
* 컴포넌트가 초기 설정될 때 : constructor 호출되고
                          내부에서는 componentWillMount -> componentDidMount 순서로 호출된다.

컴포넌트가 화면에 나타나기 전
1) constructor : 컴포넌트가 새로 만들어질 때마다 이 함수가 호출됨
2) componentWillMount : (추가적으로 삭제됨 v16.3) 컴포넌트가 화면에 보여지기 전에 호출되는 함수
                        기존에 이 API에서 하던 것들은 componentDidMount 에서 처리 할 수 있다.
컴포넌트가 화면에 나타난 후
3) componentDidMount : 컴포넌트가 화면에 나타나게 됐을 때 호출됨.
                       * 외부 라이브러리 연동 : D3, masonry
                       * 컴포넌트에서 필요한 데이터 요청 : Ajax, GraphQL
                       * DOM에 관련된 작업 : 스크롤 설정, 크기 읽어오기 등


컴포넌트 업데이트
----------
* 컴포넌트 업데이트는 props 의 변화, state의 변화에 따라 결정된다.
* 컴포넌트가 부모에게 props를 받아올 때도 업데이트되고, state가 바뀔 때도 업데이트가 됨   

1) componentWillReceiveProps: (추가적으로 삭제됨) 컴포넌트가 props 를 받을 떄 호출한다. props는 nextProps로 조회 할 수 있다. 이때 this.props 를 조회하면 업데이트 되기 전에 API가 보여진다.

<pre><code>
componentWillReceiveProps(nextProps){
  // this.Props 는 아직 바뀌지 않은 상태
}
</code></pre>

2) shouldComponentUpdate : 리액트에서는 변화가 발생하면 렌더링이 된다. 그때 부모 컴포넌트가 렌더링 되면, 자식 컴포넌트도 렌더링 되게 되는데 이 때 변화가 발생한 부분만 감지하는 부분이 해당 API이다.
이 API는 모든 상황에서 true 를 반환하게 된다. 특정 조건에 따라 false 를 반환시키게 하면 변화 되는 부분만 리렌더링이 되게 할 수 있다. 이덯게 바뀌는 부분이 어떤 것인지 알 수 있을때 Virtual DOM 의 성능을 아낄 수 있다.
* 렌더링 된다는 것은 render() 함수가 호출 된다는 것.

<pre><code>
shouldComponentUpdate(nextProps, nextState) {
  // return false 하면 업데이트를 안함
  // return this.props.checked !== nextProps.checked
  return true;
}
</code></pre>

3) componentWillUpdate : (추가적으로 삭제됨) 이 전 shouldComponentUpdate API 에서 true 를 반환하게 되었을 때 호출된다. 여기선 주로 애니메이션 효과를 초기화하거나 이벤트 리스너를 없애는 작업을 한다.
이 함수가 호출되고 난 후에 render() 가 호출된다.

<pre><code>
componentWillUpdate(nextProps, nextState) {
  // 추가적으로 삭제될 API
}
</code></pre>

4) componentDidUpdate  : componentWillUpdate -> render()를 호출하고 난 다음에 발생한다.
컴포넌트가 업데이트 되고 (this.props 와 this.state 가 바뀌어져있음) DOM Node 에 대해 접근을 할 수 있다.
그리고 파라미터 값을 통해 이전의 Props 와 State 값을 가져올 수 있다.

<pre><code>
componentDidUpdate(prevProps, prevState) {
}
</code></pre>

컴포넌트 제거
----------
1) componentWillUnmount : setTimeOut 또는 다른 라이브러리와 연동해서 사용 후 더이상 필요로 하지 않을 때 그런 작업들을 여기서 처리한다. (이벤트 제거, 써드파티 라이브러리 인스턴스 삭제 등)


컴포넌트 에러 발생
----------
1) componentDidCatch : render() 함수에서 에러가 발생되면 리액트 앱이 크래쉬 된다(빈 화면). 그럴때 componentDidCatch API를 사용한다.
* 에러가 발생하면 componentDidCatch 가 실행하게 되고, state.error 를 true 로 설정하면 render 함수에서 이에 따라 에러가 띄워주게 된다.
* 주의할 점은 컴포넌트 자신은 render 함수에서 에러가 발생하는 것은 잡아낼 수 없지만, 내부 자식 컴포넌트에서 발생하는 에러는 잡을 수 있다.
역시 내 문제는 어렵지만 타인의 문제는 쉬운 것 같은 맥락인가?

<pre><code>
componentDidCatch(error, info) {
  this.setState({
    error: true
  });
}

// 에러가 발생하는 이유
1-1) 존재하지 않는 함수를 호출하려고 할 때
     this.props.onClick();
1-2) 배열이나 객체가 올 줄 알았는데, 해당 객체나 배열이 존재하지 않을때
     this.props.object.value;
     this.props.object.length;

이럴 때 render 함수에서 다음과 같은 형식으로 막아 줄 수 있다.

render() {
  if (!this.props.object || !this.props.array || this.props.array.length ===0) return null;
  // object 나 array 를 사용하는 코드
}

혹은 defauleProps를 통해서 설정하면 된다.

class Sample extends Component {
  static defaultProps = {
    onIncrement: () => console.warn('onIncrement is not defined'),
    object: {},
    array: []
  }
}
</code></pre>


LifeCycle API 다룬 전체 코드
-------
```
import React, { Component } from 'react';

const Problematic = () => {
  throw (new Error('버그 나타남'));
  return (
    <div></div>
  )
}

class Counter extends Component {
  state = {
    number: 0,
    error: false
  }

  componentDidCatch(error, info) {
    this.setState({
      error: true
    })
  }

  constructor(props) {
    super(props);
    console.log('constructor');
  }

  componentWillMount() {
    console.log('componentWillMount (deprecated)');
  }

  componentDidMount() {
    console.log('componentDidMount');
  }

  shouldComponentUpdate(nextProps, nextState) {
    // 5 의 배수라면 리렌더링 하지 않음
    console.log('shouldComponentUpdate');
    if (nextState.number % 5 === 0) return false;
    return true;
  }

  componentWillUpdate(nextProps, nextState) {
    console.log('componentWillUpdate');
  }

  componentDidUpdate(prevProps, prevState) {
    console.log('componentDidUpdate');
  }


  handleIncrease = () => {
    const { number } = this.state;
    this.setState({
      number: number + 1
    });
  }

  handleDecrease = () => {
    this.setState(
      ({ number }) => ({
        number: number - 1
      })
    );
  }

  render() {
    console.log('render');
    if (this.state.error) return (<h1>에러발생!</h1>);
    return (
          <div>
            <h1>카운터</h1>
            <div>값: {this.state.number}</div>
            { this.state.number === 4 && <Problematic /> }
            <button onClick={this.handleIncrease}>+</button>
            <button onClick={this.handleDecrease}>-</button>
          </div>
    );
  }
}

export default Counter;
```
