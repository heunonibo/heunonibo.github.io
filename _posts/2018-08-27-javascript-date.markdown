---
layout: post
title:  "[Javascript] 원하는 시간에 팝업창 출력"
date:   2018-07-04 18:00:00 +0900
categories: jekyll update
---

## Javascript 원하는 시간에 팝업창 출력

```
$('#button').on('click', function() {

  var date = new Date('2018/08/20 00:00:00');
  var today = new Date();

  if(today.getTime() >= date.getTime()) {
    alert('이벤트가 종료되었습니다');
  } else {
    $('#popup').show();
  };

});
```
