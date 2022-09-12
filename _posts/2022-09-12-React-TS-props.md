---
title:  "[React + Typescript] interface를 사용한 props 전달하기"
excerpt: "interface를 사용한 props 전달 방법에 대해 알아보자!"

categories:
  - Typescript
tags:
  - [React, Typescript, props]

toc: true
toc_sticky: true

date: 2022-09-12
last_modified_at: 2022-09-12
---
react + typescript를 사용해서 props를 전달하면 다음과 같은 에러메시지가 뜬다.

![Untitled](https://user-images.githubusercontent.com/71548623/189608521-48a484c0-2e3d-4384-8f66-749b6c7213a3.png)

아래에 나와있듯이 `props: any` 를 사용하면 문제가 해결된다.
그치만 타입스크립트에서 any는 가급적 안쓰는 것이 좋기 때문에 interface를 사용해 props의 타입을 지정해주어 해당 타입 에러를 없애보자!

props를 전달 받을 자식 컴포넌트에 최상단에 interface를 사용해서 타입을 선언해준다.

```jsx
interface tooltipProps {
    tooltipBgColor: string
}
```

이전에 에러가 발생했던 props 파라미터 부분에 타입을 정의해준다.

```jsx
function Tooltip(props: tooltipProps)
```

props를 사용할 때에는 다음과 같이 사용한다.

```jsx
<div className='tooltip_wrap' style={{backgroundColor:**props.tooltipBgColor**}}>
```

props를 생략하기 위해 구조분해할당을 사용할 경우에는 다음과 같이 타입을 정의해준다.

```jsx
function Tooltip({tooltipBgColor}:tooltipProps)
```

그럼 다음과 같이 props를 생략하여 사용이 가능하다.

```jsx
<div className='tooltip_wrap' style={{backgroundColor:**tooltipBgColor**}}>
```
