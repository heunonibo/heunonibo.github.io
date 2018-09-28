---
layout: post
title:  "[javascript] Cookie 확인 방법"
date:   2018-09-28 18:00:00 +0900
categories: jekyll update
---

## [javascript] Cookie 확인 방법
React 에서 작업 시 Cookie 설정이 제대로 되고 있는지 확인 방법

```
constructor(props) {
  super(props);

  const winnerPopup = this.props.cookies.get(WINNERS_POPUP_COOKIE);
  window.winnerPopup = winnerPopup !== '1';

  this.state = {
    tsWinnersPopupOpen: winnerPopup !== '1'
  }
}

브라우저 콘솔 창에서

window.winnerPopup     // True
document.cookie        // "블라블라; _get=1"

체크 박스 선택 후

window.winnerPopup     // false
document.cookie        // "블라블라; _get=1; WINNERS_POPUP=1"
```
