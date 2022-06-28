---
title:  "[Javascript] 디바운싱(Debouncing)과 쓰로틀링(Throttling)"
excerpt: "웹에서 성능을 향상시키는 2가지 방식인 디바운싱과 쓰로틀링에 대해 알아보자."

categories:
  - Javascript
tags:
  - [Javascript, Debouncing, Throttling]

toc: true
toc_sticky: true

date: 2022-06-28
last_modified_at: 2022-06-28
---
# [Javascript] 디바운싱(Debouncing)과 쓰로틀링(Throttling)

이번 포스팅에서는 웹에서 연속적으로 발생되는 이벤트를 제어하여 성능을 향상시키는 2가지 방식인 디바운싱(Debouncing)과 쓰로틀링(Throttling)에 대해 알아보자.

먼저 디바운싱은 무엇일까?

## 디바운싱(Debouncing)

---

디바운스는 **이벤트를 그룹화**하여, **연이어 호출되는 함수들 중 마지막 함수만 호출되도록 하는 기술**이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6cad0ec6-aa70-4afe-8f63-d0c73a50cce9/Untitled.png)

위 그림을 보면 알 수 있듯이 1부터 8까지 8번의 함수 호출을 디바운싱을 사용하면 단 3번으로 줄일 수 있다. 그 과정은 다음과 같다.

예를 들어 input창에 입력되는 입력이벤트를 받아 연관검색어가 뜨도록 처리할 때 디바운싱이 주로 사용되는데, 디바운싱을 사용하지 않으면 사용자가 키보드 하나를 누를때마다 입력이벤트가 발생해 오버클럭(Overclock:과도한 이벤트 발생)이 생겨 성능에 문제가 갈 수 있다.

하지만 디바운싱을 사용하면 매 키보드 입력시 함수를 호출하는 것이 아닌 delay time을 지정해 이 **delay time 이전에 실행된 이벤트들은 무시**하고 **delay time 이후에 호출된 이벤트만 실행**시키기 때문에 이벤트 발생이 훨씬 감소하게 되고 오버클럭을 막아 리소스 사용량을 줄일 수 있고 성능 문제에 긍정적인 도움을 줄 수 있다.

디바운싱 과정은 다음과 같이 설명할 수 있다.

1. setTimeout() 함수를 사용해 0.2초 마다 코드가 실행되도록 한다.
  1. 0.2초 이전에 이벤트가 발생하면 clearTimeout() 함수를 사용해 setTimeout()에서 실행될 코드를 취소시킨다.
  2. 0.2초 이후에 이벤트가 발생하면 setTimeout()에 있는 코드를 실행시킨다.

이 과정을 코드로 간단히 나타내면 다음과 같다.

```jsx
let debounce;
let searchInput = document.querySelector('#search_input')
searchInput.addEventListener('keypress', (() => {
    clearTimeout(debounce)
    debounce = setTimeout(() => {
			console.log('keypress 이벤트 실행')
    }, 500)
}))
```

0.5초동안 사용자의 keypress 이벤트가 없으면 콜백 함수를 실행하도록 짜보았다.

![드바운싱실행전](https://user-images.githubusercontent.com/71548623/176203396-69616d6b-4829-4a53-ab99-73a7063065c3.gif)

드바운싱 실행전에는 키보드 누른 그대로 6번의 콜백함수가 실행된다.

![드바운싱실행후](https://user-images.githubusercontent.com/71548623/176203421-1267f31c-6116-440a-9232-0921f5d82937.gif)

드바운싱 실행후에는 123456을 입력할동안에는 콜백함수가 실행하지 않다가 모두 입력하고 0.5초 동안 키보드 입력이 없을때 단 한번 콜백함수가 실행되는 것을 볼 수 있다.

## 쓰로틀링(Throttling)

---

쓰로틀링(Throttling)은 **정해둔 일정 시간내에 딱 한번만 함수를 호출하도록 하는 기술**로 디바운싱과 가장 큰 차이점은 **정해진 시간 간격 내에 반드시 최대 한 번 함수가 호출된다**는 것이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c7ec5e4f-24e4-46c8-8519-21b9218c57cd/Untitled.png)

쓰로틀링은 사용자가 이벤트를 일으킨 점선 박스 시간동안 기다렸다가, 가장 마지막에 호출된 이벤트를 발생시킨다.

쓰로틀링은 스크롤 이벤트를 제어할 때 많이 사용되는데, 바로 이전 포스팅이었던 **[[Javascript] Intersection Observer API로 무한스크롤 구현하기](https://lee1nna.github.io/posts/JS-IntersectionObserverAPI/)**를 쓰로틀링으로도 구현할 수 있다.

쓰로틀링 과정은 다음과 같다.

1. setTimeout()을 이용해 콜백함수를 호출할 일정 시간을 정한다.
2. 일정 시간이 지나면 setTimeout() 콜백함수 내에서 스스로 타이머를 해제시킨 후 다음 코드를 실행시킨다.

이 과정을 코드로 간단히 나타내면 다음과 같다.

```jsx
let throttle
let searchInput = document.querySelector('#search_input')

searchInput.addEventListener('keypress', (() => {
  if (!throttle) {
    throttle = setTimeout(() => {
      throttle = null
      console.log('keypress 이벤트 실행')
    }, 1000)
  }
}))
```

1초동안 기다렸다가 마지막에 호출된 이벤트를 발생하도록 코드를 짜보았다.

쓰로틀링 실행 전은 위에 드바운스 실행전과 결과가 동일하고 쓰로틀링 실행 후 결과는 다음과 같았다.
![쓰로틀링실행후](https://user-images.githubusercontent.com/71548623/176203429-61fba210-405c-4323-9fcc-c0f4e3e68537.gif)

- 참고자료
  - [[Javascript] 디바운싱과 쓰로틀링](https://velog.io/@yujuck/Javascript-%EB%94%94%EB%B0%94%EC%9A%B4%EC%8A%A4%EC%99%80-%EC%93%B0%EB%A1%9C%ED%8B%80%EB%A7%81)
  - [[Javascript] Debounce & Throttle](https://kellis.tistory.com/142)
