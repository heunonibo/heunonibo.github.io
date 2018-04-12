---
layout: post
title:  "React - TodoList"
date:   2018-04-09 10:47:00 +0900
categories: jekyll update
---
> ##### 이 포스팅은 `React로 구현하는 웹 애플리케이션 제작 CAMP 4기` 수업 내용 바탕으로 정리된 글 입니다.


React 로 만드는 TODOLIST
----------
<br />
<br />
<br />
## 컴포넌트 구성
<br />
<br />
#### TodoListTemplate 작성

* src/components/TodoListTemplate.js
* src/components/TodoListTemplate.css
<br />
템플릿 역할을 하는 파일을 만들어준다.<br />
React는 컴포넌트 종류별로 폴더에 나눠서 작업한다.

```
//file: src/components/TodoListTemplate.js

import React, { Component } from 'react';
import './TodoListTemplate.css'

const TodoListTemplate ({form, children}) => {              
    return (
        <main className="todo-list-template">               
            <div className="title">
                TODOLIST
            </div>
            <div className="form-wrapper">
                {form}
            </div>
            <div className="todos-wrapper">
                {children}
            </div>
        </main>
    );
};
export default TodoListTemplate;

1. CSS 를 import 할 때는 from 을 써주지 않아도 된다.
2. 함수형 컴포넌트는 prop를 파라미터로 가져온다.
3. 비구조 할당 문법 사용으로 form, children 값을 파라미터로 가져온다.
4. div 에 CSS Style 을 줄 때는 className 속성을 사용한다.
5. .todo-list-template 구조 안에 .title / .form-wrapper / .todos-wrapper 컨테이너를 만든 후
    form, children 값을 넣어준다.
```
<br />
<br />
<br />
#### CSS Style

TodoListTemplate.css 와 index.css 에 각각 Style 을 넣어준다.

```
//file: src/components/TodoListTemplate.css

.todo-list-template {
    background: white;
    width: 512px;
    box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
    margin: 0 auto;
    margin-top: 4rem;
  }

  .title {
    padding: 2rem;
    font-size: 2.5rem;
    text-align: center;
    font-weight: 100;
    background: #22b8cf;;
    color: white;
  }

  .form-wrapper {
    padding: 1rem;
    border-bottom: 1px solid #22b8cf;
  }

  .todos-wrapper {
    min-height: 5rem;
  }

----------------------------------------------------------------------------------

//file: src/index.css

body {
 margin: 0;
 padding: 0;
 font-family: sans-serif;
 background: #f9f9f9;
}

```
<br />
<br />
<br />
#### App.js

```
file: src/App.js

import React, { Component } from 'react';
import './index.css'
import TodoListTemplate from './components/TodoListTemplate'

class App extends Component {
  render() {
    return (
      <TodoListTemplate>
        템플릿
      </TodoListTemplate>
    );
  }
}

export default App;
```
Q. 근데 TodoListTemplate 안에 '템플릿' 이라고 쓰면 왜 todos-wrapper 내부에 글자가 써져?<br />
Q. 그리고 파일명 만들때 왜 대문자로 시작해?
<br />
<br />
<br />
#### form 컴포넌트 구성

* src/components/Form.js
* src/components/Form.css

```
//file: src/components/Form.js

import React, { Component } from 'react';
import './Form.css';

const Form = ({value, onChange, onCreate, onKeyPress}) => {
    return (
        <div className="form">
            <input value={value} onChange={onChange} onKeyPress={onKeyPress} />
            <div className="create-button" onClick={onCreate}>추가</div>
        </div>
    );
}

export default Form;

----------------------------------------------------------------------

//file: src/components/Form.css

.form {
    display: flex;
}

.form input {
    flex: 1;
    font-size: 1.25rem;
    outline: none;
    border: none;
    border-bottom: 1px solid #c5f6fa;
}

.create-button {
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
    padding-left: 1rem;
    padding-right: 1rem;
    margin-left: 1rem;
    background: #22b8cf;
    border-radius: 3px;
    color: white;
    font-weight: 600;
    cursor: pointer;
  }

  .create-button:hover {
    background: #3bc9db;
  }

  ----------------------------------------------------------------------

//file: src/App.js

  import React, { Component } from 'react';
  import './index.css';
  import TodoListTemplate from './components/TodoListTemplate';
  import Form from './components/Form';

  class App extends Component {
    render() {
      return (
        <TodoListTemplate form={<Form/>}>
          템플릿
        </TodoListTemplate>
      );
    }
  }

  export default App;




  1. value : 인풋의 내용
     onChange : 인풋 내용이 변경 될 때 실행 될 함수
     onCreate : 버튼이 클릭 될 때 실행 될 함수
     onKeyPress : 인풋에서 키를 입력할 때 실행되는 함수
                  ( Enter 키를 누를 때 onCreate 실행한 것처럼 하기 위해 사용 )

  2. CSS 작성
  3. App.js 에서 import 불러온 후 렌더링.
```
<br />
<br />
<br />
#### TodoItemList 컴포넌트 만들기

* 리스트가 동적인 경우에 함수형이 아닌 클래스형 컴포넌트로 작성하는게 좋다. 그 이유는, 클래스형 컴포넌트로 작성하면 나중에 컴포넌트 성능 최적화를 할 수 있기 때문이다.

```
//file: src/components/TodoItemList.js

import React, { Component } from 'react';

class TodoItemList extends Component {
    render() {
        const { todos, onToggle, onRemove } = this.props;

        return (
            <div></div>
        );
    }
}

export default TodoItemList;


1. todos : todo 객체들이 비어있는 배열
   onToggle : 체크 활성화, 비활성화 함수
   onRemove : 리스트를 삭제 시키는 함수
```
<br />
<br />
<br />
#### TodoItem 컴포넌트 만들기

* src/components/TodoItem.js
* src/components/TodoItem.css

```
//file: src/components/TodoItem.js

import React, { Component } from 'react';
import './TodoItem.css'

class TodoItem extends Component {
    render() {
        const  { text, checked, id, onToggle, onRemove } = this.props;

        return (
            <div className="todo-item" onClick={() => onToggle(id)}>
                <div className="remove" onClick={(e) => {
                    e.stopPropagation();
                    onRemove(id)  
                }}>
                    &times;
                </div>
                <div className={`todo-text ${checked && 'checked'}`}>
                    <div>{text}</div>
                </div>
                {
                    checked && (<div className="check-mark">✓</div>)
                }
            </div>
        );
    }
}

export default TodoItem;


1. text : todo 내용
       checked : 체크박스 상태
       id : todo 의 고유 아이디
       onToggle : 체크박스 활성, 비활성화 함수
       onRemove : 리스트를 삭제 시키는 함수
    2. e.stopPropagation() : 이벤트의 확산을 멈춤. (onToggle 은 실행되지 않고, onRemove만 실행)
    3. onClick={() => onToggle(id)} 로 쓰는 이유?
       onClick={onToggle(id)} <- 이와 같은 코드를 사용하면 해당 함수가 렌더링 될 때 호출이 된다.
       해당 함수가 호출되면 데이터가 변경되고, 데이터가 변경되면 리렌더링 되면서 무한반복된다.
    4. todo-text 에 유동적으로 checked 라는 문자열을 넣고 싶으면 템플릿 리터럴을 사용하면 된다.

       className={`todo-text ${checked && 'checked'}`} // checked가 true면 'checked' 문자열을 붙여라.
       className={"todo-text" + checked && 'checked'}  // 위와 같은 내용
       className={`todo-text ${ checked ? ' checked' : '' }`}  // checked가 false 일 때 결과값이 나타나니깐 이렇게도 써줄 수 있다.

----------------------------------------------------------------------------

//file: src/components/TodoItem.css

.todo-item {
  padding: 1rem;
  display: flex;
  align-items: center; /* 세로 가운데 정렬 */
  cursor: pointer;
  transition: all 0.15s;
  user-select: none;
}

.todo-item:hover {
  background: #e3fafc;
}

/* todo-item 에 마우스가 있을때만 .remove 보이기 */
.todo-item:hover .remove {
  opacity: 1;
}

/* todo-item 사이에 윗 테두리 */
.todo-item + .todo-item {
  border-top: 1px solid #f1f3f5;
}


.remove {
  margin-right: 1rem;
  color: #e64980;
  font-weight: 600;
  opacity: 0;
}

.todo-text {
  flex: 1; /* 체크, 엑스를 제외한 공간 다 채우기 */
  word-break: break-all;
}

.checked {
  text-decoration: line-through;
  color: #adb5bd;
}

.check-mark {
  font-size: 1.5rem;
  line-height: 1rem;
  margin-left: 1rem;
  color: #3bc9db;
  font-weight: 800;
}

----------------------------------------------------------------------------

//file: src/App.js

import React, { Component } from 'react';
import './index.css';
import TodoListTemplate from './components/TodoListTemplate';
import Form from './components/Form';
import TodoItemList from './components/TodoItemList';

class App extends Component {
  render() {
    return (
      <TodoListTemplate form={<Form/>}>
        <TodoItemList />
      </TodoListTemplate>
    );
  }
}

export default App;
```

여기까지 전체적인 틀을 완성 시켰습니다.
(컴포넌트 생김새 정의 끝)
<br />
<br />
<br />
----------------------------------------------------------------------------
<br />
<br />
<br />
## 상태관리 하기

#### 초기 state 값 정의하기

```
//file: src/App.js

import React, { Component } from 'react';
import './index.css';
import TodoListTemplate from './components/TodoListTemplate';
import Form from './components/Form';
import TodoItemList from './components/TodoItemList';

class App extends Component {

  id = 3      // 이미 0,1,2 가 존재하므로 3으로 설정
  state = {
    input: '',
    todos: [
      {id: 0, text: '리액트 소개', checked: false},
      {id: 1, text: '리액트 소개', checked: true},
      {id: 2, text: '리액트 소개', checked: false}
    ]
  }

  render() {
    return (
      <TodoListTemplate form={<Form/>}>
        <TodoItemList />
      </TodoListTemplate>
    );
  }
}

export default App;
```
<br />
<br />
<br />
#### Form 기능 구현하기
1. 텍스트 내용 바뀌면 State 업데이트
2. '추가' 버튼이 클릭되면 새로운 todo 생성 후 todos 업데이트
3. input 에서 'Enter' 를 누르면 '추가' 버튼을 클릭한 것과 동일한 작업 진행하기

```
//file: src/App.js

import React, { Component } from 'react';
import './index.css';
import TodoListTemplate from './components/TodoListTemplate';
import Form from './components/Form';
import TodoItemList from './components/TodoItemList';

class App extends Component {

  id = 3      // 이미 0,1,2 가 존재하므로 3으로 설정
  state = {
    input: '',
    todos: [
      {id: 0, text: '리액트 소개', checked: false},
      {id: 1, text: '리액트 소개', checked: true},
      {id: 2, text: '리액트 소개', checked: false}
    ]
  }

  handleChange = (e) => {
    this.setState({
      input: e.target.value
    });
  }

  // 추가 버튼 클릭 시
  handleCreate = () => {
    const { input, todos } = this.state;    // 생성 될 때마다 input, todos 상태 값을 변경
    this.setState({
      input: '',
      todos: todos.concat({
        id: this.id++,
        text: input,
        checked: false
      })
    });
  }

  // 눌려진 Key가 Enter 면 handleCreate 호출
  handleKeyPress = (e) => {
    if(e.key === 'Enter') {
      this.handleCreate();
    }
  }

  render() {
    const { input, todos } = this.state;     // 비구조화 할당 this.state.input 이렇게 안써도됑

    return (
      <TodoListTemplate form={(
        <Form
          value={input}
          onChange={this.handleChange}
          onCreate={this.handleCreate}
          onKeyPress={this.handleKeyPress}
        />
      )}>
        <TodoItemList todos={todos}/>
      </TodoListTemplate>
    );
  }  
}

export default App;


-------------------------------------------------------------------------------

//file: src/components/TodoItemList.js

import TodoItem from './TodoItem';

class TodoItemList extends Component {
    render() {
        const { todos, onToggle, onRemove } = this.props;

        const todoList = todos.map(
            ({id, text, checked}) => (
                <TodoItem
                    id={id}
                    text={text}
                    checked={checked}
                    onToggle={onToggle}
                    onRemove={onRemove}
                    key={id}
                />
            )
        );

        return (
            <div>
                {todoList}
            </div>
        );
    }
}

export default TodoItemList;

1. const todoList : 객체배열을 컴포넌트 배열로 변환하기 위한 코드
   원래는 const todoList = todos.map(todo => ...) 의 형태여야 하지만, 함수의 파라미터 부분에서
   비구조화 할당을 하여 객체 내부의 값을 따로 레퍼런스를 줌.
   배열을 렌더링 할 때 꼭 Key 값을 줘야한다.
   객체의 값을 모두 props 로 전달할때 이런 식으로도 할 수 있다.

   const todoList = todos.map(
      (todo) => (
        <TodoItem
          {...todo}
          onToggle={onToggle}
          onRemove={onRemove}
          key={todo.id}
        />
      )
    );

  위와 같이 {...todo} 전개연산자를 사용할 경우 depth 가 많아질 경우에 쓰기가 힘들다.
```
<br />
<br />
<br />
#### 체크 활성화/비활성화 작업

```
//file: src/App.js

(...)

handleToggle = (id) => {
  const { todos } = this.state;

  // 파라미터로 받은 id가 몇번째 아이템인지 찾음
  const index = todos.findIndex(todo => todo.id === id);  
  const selected = todos[index];      // 선택된 객체

  const nextTodos = [...todos];       // 배열을 복사

  // 기존 값을 복사하고, checked 값 넣기
  nextTodos[index] = {
    ...selected,
    checked: !selected.checked                        // Q. 이거 잘 모르겠는뎁?
  };

  this.setState({
    todos: nextTodos
  });

}

render() {
  const { input, todos } = this.state;     // 비구조화 할당 this.state.input 이렇게 안써도됑

  return (
    <TodoListTemplate form={(
      <Form
        value={input}
        onChange={this.handleChange}
        onCreate={this.handleCreate}
        onKeyPress={this.handleKeyPress}
      />
    )}>
      <TodoItemList
        todos={todos}
        onToggle={this.handleToggle}  
      />
    </TodoListTemplate>
  );
}


1. handleToggle 함수 생성
2. TodoItemList 에 메소드 입력

```
<br />
<br />
#### 리스트 제거하기

```
//file: src/App.js

(...)

handleRemove = (id) => {
  const { todos } = this.state;

  this.setState({
    todos: todos.filter(todo => todo.id !== id) // 선택된 id가 제외된 배열을 만드는 코드
  });
}

render() {
  const { input, todos } = this.state;     

  return (
    <TodoListTemplate form={(
      <Form
        value={input}
        onChange={this.handleChange}
        onCreate={this.handleCreate}
        onKeyPress={this.handleKeyPress}
      />
    )}>
      <TodoItemList
        todos={todos}
        onToggle={this.handleToggle}
        onRemove={this.handleRemove}  // 추가
      />
    </TodoListTemplate>
  );
}


1. filter 내장함수를 이용하면 특정 조건에 부합되는 원소들만 뽑아서 새 배열을 만들 수 있다.
```
