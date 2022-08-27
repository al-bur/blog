---
title: '[우테코 프리코스]1주차 미션을 MVC 패턴으로 다시 구현해보기'
date: 2021-12-09
category: 우아한테크코스
thumbnail: { thumbnailSrc }
draft: false
---

이번 포스팅의 목적은 `MVC 패턴을 1주 차 미션에 적용해본 것을 공유하기위한 목적`입니다.

2주 차 미션때 MVC 패턴을 활용해서 미션을 수행했습니다.

3주차 미션을 수행하기 전에 MVC 패턴으로 프로그램을 구현하는 것에 좀 더 익숙해지기 위해서 프리코스 지난 1주 차에서 구현했었던 `숫자 야구 게임을 MVC 패턴으로 다시 구현해보기`로 하였습니다.

<br>

아래는, MVC 패턴으로 구현한 Repository 주소입니다.

바쁘신 분들은 해당 레포로 가셔서 코드만 보셔도 될 것 같습니다.

[MVC 패턴으로 다시 구현해본 숫자 야구게임 Repository](https://github.com/sam-ssong/javascript-baseball-mvc-practice/tree/main/javascript-baseball-precourse)

<br>

### 설계 및 구현

---

설계는 MVC 패턴을 바탕으로 진행하였습니다.

<br>

**gameModel.js**

숫자 야구 게임에서 사용되는 데이터는 두 가지가 있습니다.

컴퓨터 입력값과 사용자 입력값이 있기 때문에 두 데이터를 gameModel 클래스의 프로퍼티로 정의를 해주었습니다.

또한, 해당 데이터들에 대한 유효성 검사에 관한 메서드를 만들어주었습니다.

유효성 검사에 대한 함수들이 많이 존재하여 validator.js로 따로 모듈화 해주었습니다.

<br>

**gameView.js**

모든 DOM 조작에 관한 프로퍼티와 메서드를 정의해주었습니다.

<br>

**gameController.js**

VIew와 Model을 컨트롤 하는 메서드와 프로퍼티를 정의해주었습니다.

View와 Model의 메서드들 대부분은 Controller를 통해서 호출되도록 구현하려고 노력했습니다.

<br>

**기타**

MVC 패턴 이외에 아래와 같은 점들을 신경써서 구현했습니다.

1주 차, 2주 차 공통 피드백을 모두 적용해보려고 노력했지만 빠르게 구현을 하려고 하다보니 모든 공통 피드백을 적용했다고 보긴 어려울 것 같습니다.

- eslint & prettier 설정

- 단일 책임의 함수

- 직관적인 변수명 짓기

- 함수명과 함수가 반환하는 것을 일치시키기

- commit message를 보면 무슨 commit인지 파악할 수 있도록 message 작성하기

- README에 구현해야할 기능 목록을 동적으로 작성하기

- 상수들 상수화 시켜주기

- 에러 핸들링

- etc.

<br>

### 글을 마무리하며

---

MVC 패턴에 익숙해지기 위해 1주 차 미션을 다시 한 번 구현해보았는데 코드를 재구성하여 작성해보면서 MVC 패턴 익히기의 목적은 조금 달성한 것 같습니다.

[MVC 패턴을 적용하지 않은 Repository](https://github.com/sam-ssong/javascript-baseball-precourse/tree/sam-ssong)

위 repository는 MVC 패턴을 적용하지 않고 프로그램을 구현한 것입니다.

이번에 작성한 코드와 비교해보니 MVC 패턴을 활용한 방식이 더 가독성있는 코드처럼 보이긴 하지만 여러분들의 생각은 어떠신가요? 피드백 부탁드립니다😊

\*추가로, 이번 프리코스가 끝나고 나서 디자인 패턴에 대해서 깊게 공부해봐야겠다는 생각이 드는 하루였습니다.
