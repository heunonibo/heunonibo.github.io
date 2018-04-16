---
layout: post
title:  "리액트 컴포넌트 스타일링 기법"
date:   2018-04-09 10:47:00 +0900
categories: jekyll update
---
> ##### 이 포스팅은 `React로 구현하는 웹 애플리케이션 제작 CAMP 4기` 수업 내용 바탕으로 정리된 글 입니다.


# 리액트 컴포넌트 스타일링 기법

컴포넌트 스타일링이란 CSS스타일을 작성할 때 어떠한 방식으로 사용하느냐를 의미합니다.

CSS(Cascading Style Sheet)을 작성하다보면 Cascading 이라는 명칭처럼 폭포수 처럼 줄잇는 요소Depth 들이 나열되게 됩니다. 그래야 다른 요소들의 Style이 덮어지는 경우가 줄어드니깐요!

{% highlight css %}
.wrap .container .someting-box .event-box {
    width: 100px;
    height: 100px;
}
{% endhighlight %}

이와 같이 작성하다보면 가독성도 안좋고 유지보수 하기도 어렵게 됩니다.

그래서 이러한 문제를 해결하기 위해 여러가지 컴포넌트 스타일링을 사용할 수 있습니다.

* 기본적인 CSS 작성방법
* CSS Module
* SASS
* Styled-components
* Styled-JS



## 기본적인 CSS 작성방법

기본적인 CSS 작성방법은 import로 css 파일을 불러와 기존에 사용하던 방법대로 작성합니다.

{% highlight javascript %}
import './App.css';
{% endhighlight %}

## CSS Module

클래스를 작성하면, 클래스명 앞뒤에 파일 명, 해쉬 값을 넣어서 클래스 명의 고유성을 유지 시켜줍니다.

### CSS Module 셋팅

{% highlight javascript %}
yarn eject
{% endhighlight %}

* 설정을 바꿔야 되는 경우는 yarn eject 명령으로 설정을 밖으로 꺼낼 수 있다. !! 한번 꺼낸 설정은 다시 집어넣을 수 없다. 리액트 관련 도구들이 업데이트 되었을 때 이걸로 자동으로 업데이트 해줄 수 있다.

yarn eject 를 설치하면 config 폴더가 생성된다.

* config/webpack.config.dev.js : 개발 서버 전용 설정. dev 에서는 저장 할 때마다 변화가 일어나야 하기 때문에 복잡한 작업이 들어가면 안된다.
* config/webpack.config.prod.js : 프로덕션 전용 설정. 공백 같은 것을 없애줌.

{% highlight javascript %}
yarn build
{% endhighlight %}

* yarn build 를 하고 나면, build 폴더가 생성된다. CSS, JS 같은 것을 최소한으로 해준다.
경고 메세지같은 걸 코드상에서 지워냄으로 파일 크기가 줄어든다.

작업이 완료되면, config/webpack.config.dev.js 파일을 열어서 .css 를 검색한다.

{% highlight javascript %}
// config/webpack.config.dev.js

require.resolve('style-loader'),
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    // CSS modules 와 localIdentName 추가
    modules: true,
    localIdentName: '[path][name]__ [local]--[hash:base64:5]'
  },
}
{% endhighlight %}

### 사용하기

{% highlight css %}
// src/Stylish.css

.Stylish {
  width: 100px;
  height: 100px;
  background: red;
}

.test {
  width: 10px;
  height: 10px;
  background: green;
}

// src/NotStylish.css

.Stylish {
  width: 200px;
  height: 200px;
  background: pink;
}

{% endhighlight %}

{% highlight javascript %}
// src/Stylish.js

import React from 'react';
import styles from './Stylish.css';
import NotStyles from './NotStylish.css';

const Stylish = () => {
    console.log(styles);
    console.log(NotStyles);
    return (
        <div className={styles.Stylish}>
            <div className={styles.test}></div>
        </div>
    );
}

export default Stylish;

// src/App.js

import React, { Component } from 'react';
import Stylish from './Stylish.js';

class App extends Component {
  render() {
    return (
      <Stylish />
    );
  }
}

export default App;
{% endhighlight %}

CSS Module 을 사용할 땐 style 파일을 불러와서 객체의 값을 className 으로 넣어줍니다.
console.log(styles) 와 console.log(NotStyles) 로 각기 다른 class 명을 넣어주는 것을 확인 할 수 있습니다.

## SASS


## Styled-components

자바스크립
