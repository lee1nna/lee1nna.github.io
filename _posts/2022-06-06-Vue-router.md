---
title:  "[Vue] Vue 라우터(Router) 설정"
excerpt: "Vue에서 라우터 설정하는 방법에 대해 알아보자."

categories:
  - Vue
tags:
  - [Vue, router]

toc: true
toc_sticky: true

date: 2022-06-06
last_modified_at: 2022-06-06
---

Vue에서 라우터를 설정하는 방법을 알아보자.

먼저 Vue에서 Router와 Routes의 차이는 무엇일까?

## $Router

---

Router는 인스턴스를 나타낸다.
**Router 인스턴스**는 **웹 애플리케이션 전체에서 딱 하나만 존재하는 것**으로 **전반적인 라우터 기능을 관리**한다.

### Router 객체의 property와 method

**프로퍼티**

- app : 라우터를 사용하는 루트 Vue 인스턴스
- mode : 라우터 모드 (hash모드와 history모드가 있음)
- options : 라우터의 설정 값(base, mode, routes)를 반환
- currentRoute : 현재 라우트에 대한 Route 객체

**메소드**

- push(location, onComplete?, onAbort?) : 페이지 이동 실행 메소드, **히스토리에 새 엔트리를 추가**하고 브라우저에서 뒤로가기 버튼을 누르면 이전의 URL로 돌아감
- replace(location, onComplete?, onAbort?) : 페이지 이동 실행 메소드, **히스토리에 새 엔트리를 추가하지 않음.**
- go(n) : 히스토리 단계에서 n 단계를 이동함, window.history.go(n)와 비슷함.
- back() : 히스토리에서 한 단계 전으로 돌아감, window.history.back()과 같음.
- foward() : 히스토리에서 한 단계 앞으로 나아감.
- addRoute(routes) : 라우터에서 동적으로 라우트를 추가함.

## $Route

---

현재 활성화된 Route의 상태를 저장한 객체이다.
페이지 이동 등으로 라우팅이 발생할 때마다 생성된다.
현재의 경로 및 URL 파라미터 등의 정보를 이 객체에서 받을 수 있다.

### Route 객체의 property와 method

**프로퍼티**

- path : 현재 라우트의 경로를 나타내는 문자열
- params : 정의된 URL 패턴과 일치하는 파라미터의 키-값 쌍을 담고있는 객체, 파라미터가 없다면 빈 값임.
- query : 쿼리 문자열의 키-쌍 값을 담고 있는 객체, 쿼리가 없다면 빈 값임.
- hash : 현재 URL에 URL 해시가 있을 경우 라우트의 해시값을 가짐, 해시가 없다면 빈 값임.
- fullPath : 쿼리 및 해시를 포함하는 전체 URL
- name : 이름을 가진 라우트일 경우 라우트의 이름

정리하자면, Router를 구성하고 있는 것이 Route이며 Router는 전체 라우팅 기능을 관리하는 인스턴스고 Route는 각 페이지마다의 상태를 저장한 객체인 것이다.

그럼 Vue2와 Vue3에서는 라우터를 어떻게 처리하고 관리할까?

Vue에서는 Vue-router라는 Vue.js 공식 라이브러리를 사용해 라우터를 처리하고 관리한다.

먼저, Vue-router를 설치하자.

```html
[npm install/yarn add] vue-router--save
```

그 다음, 라우터를 생성해야하는데 라우터를 생성하는 방식은
vue-router 3.x 버전에서 사용하는 **인스턴스 방식**과 vue-router4 버전 이후에 도입된 **create 방식** 두 가지가 존재한다.

> **인스턴스 방식으로 router 생성**
>

```jsx
// 📂routes/router.js 또는 index.js
import Vue from 'vue'
import Router from 'vue-router'
import Home from '../pages/Home'

Vue.use(VueRouter)

const router = new VueRouter({
	mode: 'history',
	routes: [{
		path:'/',
		redirect: '/home'
	},
	{
		path:'/home',
		component:Home
	}]
})
```

```jsx
// 📂main.js
import Vue from 'vue'
import App from './App.vue'
import {router} from '../routes/index.js'

new Vue({
	router,
	render: h => h(App),
}).$mount('#app')
```

> **Create 방식으로 생성**
>

```jsx
// 📂routes/router.js 또는 index.js
import {createRouter, createWebHashHistory}from 'vue-router'
import Home from '../pages/Home'

export default createRouter({
	history: createWebHashHistory(),
	routes: [
		{
			page: "/home".
			name: Home,
			component: Home
		}
	]
})
```

```jsx
// 📂main.js
import {createApp} from 'vue'
import App from './App.vue'
import {router} from '../routes/index.js'

createApp(App).use(router).mount('#app')
```

라우터 생성을 완료했으면 라우터를 생성한 컴포넌트에 `router-view` 태그를 선언하면 URL에 맞게 컴포넌트들이 맵핑된다.

```jsx
// 📂App.vue
<template>
  <RouterView/>
</template>
```

- 참고자료
  - [Vue 라우터 개념 및 사용방법](https://jinyisland.kr/post/vue-router/)
  - [Vue.js-route-router-차이](https://velog.io/@qkrqudcks7/Vue.js-route-router-%EC%B0%A8%EC%9D%B4)
