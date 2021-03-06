---
title:  "[Javascript] 함수 호이스팅(hoisting)"
excerpt: "Javascript의 함수 스코프(scope)의 개념에 대해 알아보자."

categories:
  - Javascript
tags:
  - [Javascript, hoisting]

toc: true
toc_sticky: true
 
date: 2022-03-06
last_modified_at: 2022-03-07
---

호이스팅(Hoisting)은 자바스크립트의 고유 특징 중 하나이다.
이번 포스트에서는 변수 선언의 실행 시점과 관련된 특징인 호이스팅에 대해 알아보자.

## 1. 변수 선언의 실행 시점과 변수 호이스팅

```jsx
console.log(score); // undefined
var score;
```

위 코드를 실행했을때 콘솔에 에러가 나지 않고 undefined가 출력되는 것을 볼 수 있다.
변수 선언 코드가 로그 출력 코드보다 뒤에 있음에도 불구하고 왜 에러가 나지 않는 것일까?

그 이유는 자바스크립트 엔진은 **변수 선언이 소스코드 어디에 있든 다른 코드보다 우선적으로 실행**하기 때문이다. **변수 선언(선언 + 초기화)는 런타임 이전 단계에서 먼저 실행**되고 **로그 출력은 런타임 단계에서 실행**되기 때문에 에러가 나지 않는 것이다.

즉, **변수 선언문이 코드의 선두로 끌어올려진 것처럼 동작하는 자바스크립트 고유의 특징**이 바로 **호이스팅**이다. (var, let, const, function, class를 사용해 선언하는 모든 식별자는 호이스팅된다.)

## 2. 값의 할당

```jsx
var score; // 변수 선언
score = 80; // 값의 할당
```

그렇다면 값의 할당은 언제 실행될까?

**값의 할당**은 소스코드가 순차적으로 실행되는 **런타임 단계에서 실행**된다.
다음 코드의 결과값을 보면 알 수 있다.

```jsx
console.log(score) // undefined
var score // 변수 선언 -> 런타임 이전에 실행
score = 80 // 값의 할당 -> 런타임때 실행
console.log(score) // 80
```

값의 할당은 런타임 단계에서 실행되기 때문에 첫 로그 출력때 80이 아닌 undefined가 출력된 것을 알 수 있다.

그럼 선언과 할당을 동시에 했을 경우는 어떻게 될까?

```jsx
console.log(score) // undefined
var score = 80
console.log(score) // 80
```

선언과 할당을 동시에 해도 똑같은 결과가 나온다.
그 이유는 자바스크립트 엔진은 선언과 할당을 각각 2개의 문으로 나누어 실행하기 때문이다.