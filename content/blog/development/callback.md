---
title: callback
date: 2022-10-05 16:09:04
category: development
thumbnail: { thumbnailSrc }
draft: false
---

여기서, callback이란 콜백 함수를 나타낸다.

## callback function(콜백 함수)?

자바스크립트 엔진은 코드를 위에서 아래로 실행한다. 실행하는 과정은 평가와 실행 단계로 세분화할 수 있다.

```tsx
function makeSomething(callbackFunction) {
  // doSomething

  callbackFunction()

  return something
} // 1-1. 자바스크립트 평가 과정에서 처리

function doExtraJob() {
  console.log('extra job done')
} // 1-2 자바스크립트 평가 과정에서 처리

makeSomething(doExtraJob) // 2. 자바스크립트 실행 과정에서 처리
```

1-1. makeSomething이라는 함수가 실행 컨텍스트에 함수 객체로 저장된다.(실행 컨텍스트에 대해서는 추후에 포스팅하려고한다.)

1-2. doExtraJob이라는 함수가 실행 컨텍스트에 함수 객체로 저장된다.(실행 컨텍스트에 대해서는 추후에 포스팅하려고한다.)

1. makeSomething이라는 함수에 doExtraJob을 인자로 전달하여 호출한다.

여기서 `doExtraJob` 함수가 바로 `콜백 함수`이다. makeSomething 함수에 인자로 전달되자마자 호출되는 것이 아니라 makeSomething이 원하는 시점에 호출(실행) 되는 것이다.

## MDN상에서의 정의

MDN 공식문서에서는 어떻게 콜백 함수를 정의하고 있는 지 살펴보자

> A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.
> [https://developer.mozilla.org/en-US/docs/Glossary/Callback_function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)

즉, 콜백 함수는 다른 함수에 인자로 전달되어지는 함수라고 보면 될 것 같다.

## 콜백 함수를 사용하면 어떤 장점이 있을까?

함수를 사용하는 것의 장점은 반복되는 로직을 하나의 함수로 추출하여 해당 함수를 재사용해줌으로써 중복을 줄여줄 수 있고, 잘 정의된 함수명을 통해 내부 코드들의 역할을 잘 표현해줌으로써 유지보수도 편하게 만들어 줄 수 있다.

콜백 함수도 함수이기 때문에, 함수와 같은 장점을 가진다고 보면된다. 하지만, 조금 다른 것은 다른 함수 내부에서 그 역할을 해준다는 것 뿐

## 비동기 콜백 함수

콜백 함수는 사용 용도에 따라 두 가지 종류로 나뉠 수 있다. 동기적인 상황에서 사용되는 `동기 콜백 함수` 와 비동기적인 상황에서 사용되는 `비동기 콜백 함수`이다. 사실 이름만 다르게 부르는 것이지 동작은 동일하다. 이것은 마치, 학교에서는 `반장`으로 불리고 집에서는 `막내`라고 불리는 것과 비슷하다고 보면 될 것 같다. 글로만 설명하기에는 헷갈리니, 코드로 살펴보자

```tsx
function callbackFunction() {
  console.log('callbacked')
}
```

```tsx
function syncFunction(callbackFn) {
  console.log('start')
  callbackFn() // 동기 콜백 함수
  console.log('end')
}

syncFunction() // start end callbacked
```

syncFunction의 내부 코드는 모두 동기적으로 실행된다. `start end callbacked`가 console에 출력이 된다. 여기서는 `callbackFn` 콜백 함수가 동기적으로 동작하는 함수의 인자로 전달되었기 때문에 `동기 콜백 함수`로 불릴 수 있다.

```tsx
function asyncFunction(callbackFn) {
  console.log('start')
  setTimeout(callbackFn, 1000) // 비동기 콜백 함수
  console.log('end')
}

asyncFunction() // start end callbacked
```

asyncFunction의 내부에는 비동기적으로 실행되는 setTimeout 함수가 있다. (비동기는 순서대로 동작하지 않는다라는 의미이다. 자바스크립트는 싱글 스레드 언어이기 때문에 비동기 작업을 활용하여 blocking을 최소화 해준다.) setTimeout의 인자로 `callbackFn`이 전달되었다. 비동기적으로 실행되는 함수에 인자로 전달되는 콜백 함수이니 여기서는 `비동기 콜백 함수`로 불릴 수 있다.

## 비동기 콜백 지옥(hell)

비동기 콜백 함수는 클라이언트가 서버와 통신할 때 자주 사용된다.

아래의 코드는 하나의 음식을 만들고 서빙하는 과정을 묘사한 코드이다. 음식을 만들기 위해서는, 재료를 준비하고 요리를 하고 음식에 데코레이션까지 순서대로 진행되야할 것이다. makeFood 함수와 내부 콜백 함수들이 서버와 통신을 해야하는 비동기적으로 실행되야하는 코드들이라고 한다면, 이렇게 비동기 콜백 함수를 사용해서 함수가 호출되는 순서들을 보장해줄 수 있다. 지금은 괜찮아보여도 만약 작업들이 더 있다면 코드는 더 장황해질 것이고 가독성이 많이 떨어지게 될 것이다.

```tsx
const makeFood = next => {
  getIngredient(function (ingredient) {
    cookFood(ingredient, function (food) {
      decorateFood(function (decoratedFood) {
          next(decoratedFood)
      })
    })
  })
}

makeFood(function (decoratedFood) => {
  serve(decoratedFood)
})
```

이러한 현상을 보고, `콜백 지옥(callback hell)` 이라고 한다.
