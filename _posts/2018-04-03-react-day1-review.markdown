---
layout: post
title:  "React Day1 - input 상태 관리, 배열(생성과 렌더링 / 제거와 수정)"
date:   2018-04-04 10:35:00 +0900
categories: jekyll update
---
> ##### 이 포스팅은 `React로 구현하는 웹 애플리케이션 제작 CAMP 4기` 수업 내용 바탕으로 정리된 글 입니다.


input 상태 관리, 배열 (생성과 렌더링 / 제거와 수정)
==============
전화번호부 프로젝트를 만들어보면서 input 상태관리 및 배열의 생성과 렌더링, 제거와 수정을 알아본다.
<br />
<br />
<br />
프로젝트 생성
-----
새 프로젝트를 만든다.
```
// 터미널
create-react-app phone-book
```
해당 디렉토리를 에디터로 열고, npm start 또는 yarn start 를 통해서 개발 서버를 시작한다.
<br />
<br />
<br />
input 다루기
-----

#### 컴포넌트 phoneForm.js 만들기
이 컴포넌트에서는 사용자에게서 이름과 전화번호를 입력 받을 것이다.<br />
input 컴포넌트의 입력을 state에 담는 방법을 알아본다.
<br />
<br />
1) src 디렉토리 내부에 components 폴더를 생성 후 PhoneForm.js 파일을 만들어서 다음 코드를 입력한다.
```
// file: src/components/PhoneForm.js

import React, { Component } from 'react';

class PhoneForm extends Component {
    state = {
        name: ''
    }
    handleChange = (e) => {
        this.setState({
            name: e.target.value
        })
    }
    render() {
        return (
            <form>
                <input
                    placeholder="이름"
                    value={this.state.name}
                    onChange={this.handleChange}
                />
                <div>{this.state.name}</div>
            </form>
        );
    }
}

export default PhoneForm;
```
1-1) input 의 텍스트 값이 바뀔 떄마다 onChange 이벤트가 발생한다.<br />
1-2) render() 부분에서는 input 을 렌더링 할 떄 (값이 바뀔 때) value 값과 onChange 이벤트를 발생시킨다.<br />
1-3) onChange -> handleChange 이벤트가 발생하면 e.target.value 값을 통하여 이벤트 객체에 담겨있는 현재의 텍스트 값을 읽어올 수 있다.<br />
1-4) 그리고 나중에 데이터를 등록하고 나면 이 name 값이 공백으로 초기화 할 수 있게 value 를 설정했다.
<br />
<br />
2) 위의 컴포넌트를 App 에서 보여주기 위한 코드를 작성한다.
```
// file: src/App.js

import React, { Component } from 'react';
import PhoneForm from './components/PhoneForm';

class App extends Component {
  render() {
    return (
      <div>
        <PhoneForm />
      </div>
    );
  }
}

export default App;
```
<br />
3) 전화번호가 입력되는 input 도 추가해준다. (input이 여러개일때 처리방식)
```
// file: src/components/PhoneForm.js

import React, { Component } from 'react';

class PhoneForm extends Component {
    state = {
        name: '',
        phone: ''
    }
    handleChange = (e) => {
        this.setState({
            [e.target.name]: e.target.value
        })
    }
    render() {
        return (
            <form>
                <input
                    name="name"
                    placeholder="이름"
                    value={this.state.name}
                    onChange={this.handleChange}
                />
                <input
                    name="phone"
                    placeholder="전화번호"
                    value={this.state.phone}
                    onChange={this.handleChange}
                />
                <div>{this.state.name}</div>
                <div>{this.state.phone}</div>          
            </form>
        );
    }
}

export default PhoneForm;
```
3-1) input 에 name 값을 부여해줌으로써 하나의 핸들러로 제어할 수 있다.<br />
3-2) name 값은 event.target.name 통해서 조회 할 수 있다.<br />
* [e.target.name] 문법은 [Computed property names][Computed-property-names] 라는 문법 :
<br />
<br />
<br />

부모 컴포넌트에 정보 전달하기
-----
부모 컴포넌트에서 메소드를 만들고 이 메소드를 자식에게 전달한 다음에 자식 내부에서 호출하는 방식을 사용한다.<br />
(state 안에 있는 값을 부모 컴포넌트(App.js)에 전달해준다.)
<br />
<br />
App.js 에서 handleCreate라는 메소드를 만들고, 이를 PhoneInfo 한테 전달해준다.<br />
PhoneInfo쪽에서 버튼을 만들어서 submit이 발생하면 props로 받은 함수를 호출하여 App에서 파라미터로 받은 값(data)를 사용할 수 있게 해준다.
```
//file: src/App.js

import React, { Component } from 'react';
import PhoneForm from './components/PhoneForm';

class App extends Component {
  handleCreate = (data) => {
    console.log(data);
  }
  render() {
    return (
      <div>
        <PhoneForm
          onCreate={this.handleCreate}
        />
      </div>
    );
  }
}

export default App;
```
PhoneForm 에서 submit 버튼과 onSubmit 이벤트를 설정.
```
import React, { Component } from 'react';

class PhoneForm extends Component {
    state = {
        name: '',
        phone: ''
    }
    handleChange = (e) => {
        this.setState({
            [e.target.name]: e.target.value
        })
    }
    handleSubmit = (e) => {
        e.preventDefault();     // 페이지 리로딩 방지
        this.props.onCreate(this.state);    // 상태값을 onCreate 통하여 부모에게 전달
        this.setState({
            name: '',                       // 값을 전달하고 나서 상태 초기화
            phone: ''
        })
    }
    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <input
                    name="name"
                    placeholder="이름"
                    value={this.state.name}
                    onChange={this.handleChange}
                />
                <input
                    name="phone"
                    placeholder="전화번호"
                    value={this.state.phone}
                    onChange={this.handleChange}
                />
                <button type="submit">등록</button>
                <div>{this.state.name}</div>
                <div>{this.state.phone}</div>
            </form>
        );
    }
}

export default PhoneForm;
```
* e.preventDefault() : 이벤트가 해야 하는 작업을 안한다는 의미. 원래 submit 되면 페이지가 리로드 되는 데 그것을 방지한다.
* props로 받은 onCreate 함수를 호출하고 상태값을 초기화 해줌

--------------------------------------------------------------------------------------

배열(생성과 렌더링)
-----
리액트에서 배열을 다룰 때는 state 내부의 값을 직접적으로 수정하면 안된다. 이를 불변성 유지라고 하는데 'push, splice, unshift, pop' 같은 내장 함수는
배열 자체를 직접적으로 수정하게 되므로 사용하지 말아야되고, 기존 배열에 기반하여 새 배열을 만들어내는 함수는 'concat, slice, map, filter'같은 함수를 사용해야한다.
<br />
<br />
리액트는 바뀌는 부분에서만 리렌더링 되어야 하기 때문에 불변성 유지가 중요하다.
<br />
<br />
#### 데이터 추가
App.js 컴포넌트의 state 에 information 이라는 배열을 만들고, 그 안에 배열의 기본값 샘플 데이터를 추가한다.<br />
```
// file: src/App.js

import React, { Component } from 'react';
import PhoneForm from './components/PhoneForm';

class App extends Component {
  id = 2
  state = {
    information : [
      {
        id: 0,
        name: '안보라',
        phone: '010-0000-0000'
      },
      {
        id: 1,
        name: '안밤',
        phone: '010-0000-0000'
      },
    ]
  }
  handleCreate = (data) => {
    const { information } = this.state;
    this.setState({
      information: information.concat({ id: this.id++, ...data })
    })
    console.log(data);
  }
  render() {
    return (
      <div>
        <PhoneForm
          onCreate={this.handleCreate}
        />
        {JSON.stringify(information)}
      </div>
    );
  }
}

export default App;

```
<br />
<br />
#### 데이터 렌더링
위 배열을 컴포넌트로 변환해서 바꿔준다.
<br />
<br />
#### 컴포넌트 만들기
* PhoneInfo.js : 각 전화번호를 보여주는 컴포넌트
* PhoneInfoList.js : 여러개의 PhoneInfo 컴포넌트를 보여줌.

1) PhoneInfo.js
defaultProps 로 컴포넌트가 크래쉬 될 떄 기본값을 보여주게 한다.
```
// file: src/components/PhoneInfo.js

import React, { Component } from 'react';

class PhoneInfo extends Component {
    static defaultProps = {
        info: {
            name: '이름',
            phone: '010-0000-000.',
            id: 0
        }
    }
    render() {
        const style = {
            border: '1px solid #000',
            padding: '10px',
            margin: '10px'
        }
        const { name, phone, id } = this.props.info;
        return (
            <div style={style}>
                <div><b>{name}</b></div>
                <div><b>{phone}</b></div>
            </div>
        );
    }
}

export default PhoneInfo;
```
<br />
2) PhoneInfoList.JS
data 배열을 가져와서 map 을 통해 JSX 로 변환해준다.<br />
여기에서 key 값은 배열을 렌더링 할 때 고정적인 고유의 값을 부여하는 것이다. 고유의 값을 부여하지 않으면 리액트의 변화를 감지할 때
성능을 좋지 못하게 한다.
```
// file: src/components/PhoneInfoList.js

import React, { Component } from 'react';
import PhoneInfo from './PhoneInfo';

class PhoneInfoList extends Component {
    static defaultProps = {
        data: []
    }
    render() {
        const { data } = this.props;
        const list = data.map(
            info => (<PhoneInfo key={info.id} info={info} />)
        )
        return (
            <div>
                {list}
            </div>
        );
    }
}

export default PhoneInfoList;
```
3) 이제 PhoneInfoList 컴포넌트를 App 에서 렌더링 해준다.
```
// file: src/App.js

import React, { Component } from 'react';
import PhoneForm from './components/PhoneForm';
import PhoneInfoList from './components/PhoneInfoList';

class App extends Component {
  id = 2
  state = {
    information : [
      {
        id: 0,
        name: '안보라',
        phone: '010-0000-0000'
      },
      {
        id: 1,
        name: '안밤',
        phone: '010-0000-0000'
      },
    ]
  }
  handleCreate = (data) => {
    const { information } = this.state;
    this.setState({
      information: information.concat({ id: this.id++, ...data })
    })
    console.log(data);
  }
  render() {
    return (
      <div>
        <PhoneForm
          onCreate={this.handleCreate}
        />
        <PhoneInfoList data={this.state.information} />
      </div>
    );
  }
}

export default App;
```

[Computed property names][Computed-property-names]
[Computed-property-names]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names
