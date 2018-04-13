---
layout: post
title:  "React - TodoList"
date:   2018-04-09 10:47:00 +0900
categories: jekyll update
---
> ##### 이 포스팅은 `React로 구현하는 웹 애플리케이션 제작 CAMP 4기` 수업 내용 바탕으로 정리된 글 입니다.


## 리액트 컴포넌트 스타일링 기법

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



### 기본적인 CSS 작성방법

기본적인 CSS 작성방법은 import로 css 파일을 불러와 기존에 사용하던 방법대로 작성합니다.

{% highlight javascript %}
import './App.css';
{% endhighlight %}

### CSS Module

클래스를 작성하면, 클래스명 앞뒤에 파일 이름, 해쉬 값을 넣어서 클래스명이 언제나 유일하게 하는 기술입니다.


### SASS


### Styled-components

자바스크립
