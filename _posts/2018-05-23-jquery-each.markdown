---
layout: post
title:  "[jQuery API] $.each() 와 .each() 차이 + forEach() 메서드"
date:   2018-05-08 15:06:00 +0900
categories: jekyll update
---

### jQuery.each()
배열인지 객체인지 상관없이 모든 콜렉션을 반복하는 데 사용됩니다.
배열의 경우 callback은 배열 인덱스와 해당 배열 값을 전달 받습니다.

{% highlight javascript %}
$.each(array, callback(index, value))
{% endhighlight %}

{% highlight javascript %}
var obj = {one : 1, two: 2, three: 3, four: 4};

$.each(obj, function(key, value) {
  console.log(key, value);
});
{% endhighlight %}

### .each()
선택된 셀렉터 기준으로 반복하고, 일치하는 각 요소에 대해 함수를 실행합니다.

{% highlight javascript %}
.each(function(index))
{% endhighlight %}

{% highlight javascript %}
<ul>
  <li>foo</li>
  <li>bar</li>
</ul>

$('li').each(function(index) {
  console.log(index + ':' + $(this).text());
})
{% endhighlight %}

### forEach 메서드
배열의 각 요소에 대해 지정된 작업을 수행합니다.

{% highlight javascript %}
array.forEach(callbackfn [, thisArg])
{% endhighlight %}

{% highlight javascript %}
var arr = ['arr1', 'arr2', 'arr3'];

function output(value, index) {
  console.log(value, index);
}

arr.forEach(output);
{% endhighlight %}

{% highlight javascript %}
var numbers = [10, 11, 12];
var sum = 0;

numbers.forEach(
  function addNumber(value) {
    sum += value;
  }
);

console.log(sum);
{% endhighlight %}

{% highlight javascript %}
var obj = {
  showResults : function(value, index) {
    var squared = this.calcSquare(value);
    console.log('value : ', value);
    console.log('index : ', index);
    console.log('squared : ', squared);
  },
  calcSquare: function(x) { return x * x}
}

var numbers = [5, 6];

numbers.forEach(obj.showResults, obj);

// value: 5
// index: 0
// squared: 25
// value: 6
// index: 1
// squared: 36

{% endhighlight %}

{% highlight javascript %}
var arr = [
  {"name" : "ahn"},
  {"gender" : "female"}
]

function root(value, index) {
  console.log(value, index)
}

arr.forEach(root);
{% endhighlight %}
