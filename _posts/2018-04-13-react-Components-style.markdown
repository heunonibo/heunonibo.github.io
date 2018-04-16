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
클래스 이름이 겹치는 것을 방지하고 싶을 때 해당 모듈을 사용합니다.

### CSS Module 셋팅

{% highlight javascript %}
yarn eject
{% endhighlight %}

* 설정을 바꿔야 되는 경우는 yarn eject 명령으로 설정을 밖으로 꺼낼 수 있습니다. !! 한번 꺼낸 설정은 다시 집어넣을 수 없고요?. 리액트 관련 도구들이 업데이트 되었을 때 이걸로 자동으로 업데이트 해줄 수 있습니다.

yarn eject 를 설치하면 config 폴더가 생성됩니다.

* config/webpack.config.dev.js : 개발 서버 전용 설정. dev 에서는 저장 할 때마다 변화가 일어나야 하기 때문에 복잡한 작업이 들어가면 안됩니다.
* config/webpack.config.prod.js : 프로덕션 전용 설정. 공백 같은 것을 없애줍니다.

{% highlight javascript %}
yarn build
{% endhighlight %}

* yarn build 를 하고 나면, build 폴더가 생성된다. CSS, JS 같은 것을 최소한으로 해줍니다.
()경고 메세지같은 걸 코드상에서 지워냄으로 파일 크기가 줄어듬.)

작업이 완료되면, config/webpack.config.dev.js 파일을 열어서 .css 를 검색합니다.

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
<!-- src/Stylish.css -->

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

<!-- src/NotStylish.css -->

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

CSS Module 을 사용할 땐 style 파일을 불러와서 객체의 값을 className 으로 넣어줍니다.<br />
console.log(styles) 와 console.log(NotStyles) 로 각기 다른 class 명을 넣어주는 것을 확인 할 수 있습니다.


### 고유하지 않은 클래스명을 사용하고 싶을 때
클래스명 앞에 :global 을 붙여주면 클래스명 앞뒤에 파일 명, 해쉬 값이 들어가지 않은 클래스명을 사용할 수 있습니다.

{% highlight css %}
<!-- src/Stylish.css -->
:global .Stylish {
  width: 100px;
  height: 100px;
  background: red;
}
{% endhighlight %}

{% highlight html %}
<div className="Stylish"></div>
{% endhighlight html %}


### classnames 모듈 사용
CSS Module 사용 시에 claanames 라는 라이브러리를 같이 사용하면 유용합니다.

{% highlight javascript %}
yarn add classnames
{% endhighlight %}

라이브러리를 설치 후 다음과 같이 사용하면 됩니다.

{% highlight javascript %}
// src/Stylish.js

import React, { Component } from 'react';
import classname from 'classnames/bind';
import styles from './Stylish.css';

const cx = classnames.bind(styles)

const Stylish = ({bordered}) => {
    console.log(styles);
    return (
        <div className={cx('Stylish')}>
            <div className={cx('box', {bordered})}></div>
        </div>
    );
}

export default Stylish;
{% endhighlight %}

classname 은 조건부 클래스 설정을 할 때 유용하게 사용됩니다.

classnames 라이브러리를 사용하지 않은 채 조건부 클래스를 사용하려면 아래와 같이 작성해야 됩니다.

{% highlight javascript %}
// src/Stylish.js

import React, { Component } from 'react';
import classname from 'classnames/bind';
import styles from './Stylish.css';

const cx = classnames.bind(styles)

const Stylish = ({bordered}) => {
    console.log(styles);
    return (
        <div className={cx('Stylish')}>
            <div className={`${styles.box} ${styles.bordered}`} /></div>
        </div>
    );
}

export default Stylish;
{% endhighlight %}



## SASS




## Styled-components

자바스크립


## classnames 모듈의 활용
