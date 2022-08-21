---
title:  "[React] 간단한 Blog App 만들면서 React 기본 다지기 - 1"
excerpt: "React 라이브러리를 사용해 간단한 Blog를 만들며 기본기를 다져보자!"

categories:
  - React
tags:
  - [Javascript, React, library]

toc: true
toc_sticky: true

date: 2022-07-12
last_modified_at: 2022-07-24
---

이번 포스팅은 웹 프론트엔드 라이브러리인 **React**를 사용해 간단한 블로그를 만들어 볼 것이다.

회사에서 Vue 프레임워크를 사용하다보니 1년전에 공부했던 React에 대해 거의 다 까먹은 상태이다.

왜 갑자기 React를 공부하느냐하면 ,, Vue에 대해서 조금 익숙해 지다보니 1년 전에 공부했을 때 너무 어렵게 느꼈던 React도 Vue가 익숙해진 상태에서 다시 공부하면 더 빠르게 이해할 수 있을 거 같았고, 사이드 프로젝트를 진행하고 싶은데 Vue보다 React 개발자를 구하는 곳이 훨씬 많았다..

마침 1년전쯤 수강해놨던 강의 만료 기간이 일주일이 남았길래 일주일 안에 빠르게 React 기본을 다시 공부 할 것이다 !!!!

강의에서 간단한 블로그를 만들며 배웠던 부분을 다시 한 번 정리해보자.

# 1. React 시작 하기

## 1-1. React는 왜 사용할까?

---

웹사이트의 전체 페이지를 하나의 페이지에 담아 동적으로 화면을 바꿔가며 표현하는 방식이 **SPA(Single Page Application)**이다. 이 방식에서는 html 파일은 딱 하나만 존재한다.

하나의 페이지에 담아 동적으로 표현하기 때문에 화면 이동이 부드럽게 동작한다.

바닐라 JS를 사용해서 SPA를 만들 수 있지만, React를 사용하면 훨씬 더 빠르고 간단하게 SPA를 쉽게 만들 수 있다.

React에서는 function, array, object에 html을 보관하고 재사용 할 수 있어 프로젝트 규모가 커질수로 html의 관리가 용이하다.

같은 리액트 문법으로 모바일 앱개발도 가능하다. (React Native)

즉, React를 사용하는 이유는 다음과 같다.

- 빠르고 간단하게 SPA 구현
- html 관리 용이
- 같은 리액트 문법으로 앱개발 가능 (html, css문법만 살짝 다름)

## 1-2. 개발 환경 세팅 & React 설치

---

react도 js 라이브러리이기 떄문에 Node.js가 설치되어 있어야 한다.

다음과 같은 순서로 React 프로젝트를 생성한다.

```jsx
1. Node.js 설치 (LTS버전 권장)
2. 프로젝트 파일 생성
3. 프로젝트 파일에서 `npx create-react-app 프로젝트명`
```

## 1-3. 프로젝트 디렉토리 구조

---
![react-directory](https://user-images.githubusercontent.com/71548623/178297488-b0b158c0-6487-4c95-867c-005c557841ca.png)
- node_modules: 프로젝트에 필요한 라이브러리 소스가 포함되어 있는 폴더
- public: static 파일들이 포함되어 있는 폴더
- src: 메인 폴더로 이 곳에 컴포넌트 폴더를 추가해 컴포넌트 파일을 넣으면 된다.
- package.json: 프로젝트 정보 + 라이브러리 버전 정보

src 폴더 내에 있는 App.js 가 메인 페이지이므로 여기에 코드를 짜면 된다.

생성된 프로젝트를 웹사이트로 띄우고 싶다면 `npm start` 명령어를 입력해 실행한다.

## 1-4. 주요 JSX 문법

---

React에서는 Javascript를 확장한 문법인 `JSX` 를 사용한다.

`JSX`는 보이는 것으로는 html과 굉장히 비슷해서 이해하는데 어려움은 없지만 몇 가지 주요 문법에 대해서는 알아두는 게 좋다.

1. class 태그는 `class`가 아닌 `className`을 사용한다.
2. 변수명을 포함하고 싶을 때는 중괄호 {} 를 이용한다.
3. style 태그를 사용할 때는 `style = {{ 스타일명: '스타일 값' }} 으로 넣고 두 단어 이상의 스타일명 일 경우에는 카멜 케이스로 작성한다.
   ex) fontSize (x)
   font-size (o)

```jsx
// jsx 문법 예시
function App() {
    return (
        <div>
            <p style={{ font-size = '15px' }}>안녕하세요.</p>
        </div>
    )
}
```

오늘은 React로 간단한 블로그를 만들기 이전에 React로 프로젝트를 생성하고 React에서 주로 사용되는 문법에 대해 알아보았다.

다음 포스팅에서 부터는 본격적으로 React로 블로그를 만들면서 배웠던 부분에 대해 기록할 것이다.

- 참고강의
  - [[React] React 리액트 기초부터 쇼핑몰 프로젝트까지!](https://codingapple.com/course/react-basic/)
