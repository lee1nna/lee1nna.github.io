---
title:  "[Vue] store와 vuex는 무엇인가?"
excerpt: "vue의 상태관리에 필수적인 store와 vuex에 대해 알아보자!"

categories:
  - FE 지식
tags:
  - [Front-End, Vue, Store, Vuex]

toc: true
toc_sticky: true

date: 2022-12-22
last_modified_at: 2022-12-22
---
Vue에서 상태관리에 필수적인 store에 대해 알아보자.

## store와 vuex

---

**store**는 애플리케이션 전역에서 접근 가능한 **중앙 집중식 데이터 저장소**이다.
한마디로 API를 통해 받은 데이터와 컴포넌트를 제어하는 값들을 한 곳에서 관리하려고 만든 구조이다.
store는 데이터의 흐름을 **단방향으로 제어**한다.

**vuex**는 vue의 상태관리 라이브러리이다.
vuex가 각각의 컴포넌트에 사용되어야 할 상태를 관리하고 이 상태는 트리 구조의 root처럼 모든 상태의 값을 관리한다.

## store의 기본 구조

---

store는 다음과 같이 4가지의 구조를 가진다.

- state : 변수들의 집합, 변수 정의부
- mutations : 동기적으로 변수 재정의, 조작, 값 대입 등 변화를 주는 조작부
- actions : 비동기적으로 동작을 처리하는 통신부, 비동기 함수들의 집합
- getters : state 상태 값을 원하는 포맷이나 타입으로 가공해 반환

## store - state

---

state는 공통으로 참조하기 위한 **변수를 정의하는 공간**이다.
프로젝트의 모든 곳에서 이곳에 정의된 변수를 참조하고 사용할 수 있다.

```jsx
state = () => ({
	stateA: [],
	stateB: [],
})
```

## store - mutaions

---

mutations은 **state 값의 변경을 담당**하는 공간이다.
mutations을 통해서만 state의 값을 변경할 수 있으며 **동기 처리** 기준이다.
비즈니스로직을 담고 있으면 안되며, 가능한한 외부에서는 부르지 않고 actions에서만 불리는 것이 좋다.
`commit(’mutations 함수명’, ‘전달인자’)` 방식으로 호출하여 사용한다.

```jsx
mutations = {
	changeStateA(state, newStateA) {
		state.stateA = newStateA
	},
	changeStateB(state, newStateB) {
		state.stateB = newStateB
	}
```

## store - actions

---

actions는 주로 mutations를 실행시키는 트리거 역할을 한다.
보통 API 호출이나 그 결과에 대해 반환하거나 결과를 mutation으로 commit하여 상태를 변경하는 용도로 사용된다. 대다수의 경우 Promise를 리턴하게 된다.
`dispatch(’actions 함수명', ‘전달인자’)` 방식으로 호출하여 사용한다.

```jsx
actions = {
	setStateA({commit}, payloadA) {
		commit('changeA', payloadA)
	}
	setStateB({commit}, payloadB) {
		commit('changeB', palyoadB)
	}
}
```

### store - getters

---

getters는 computed와 비슷한 역할로 상태 값 state를 반환한다.

```jsx
getters = {
	getStateA(state) {
		return state.stateA
	}
	getStateB(state) {
		return state.stateB
	}
}
```

store는 store > index.js에서 모든 구조를 정의하는 단일 스토어 방식과, 역할별로 파일을 나누어 구조를 정의하는 모듈화 방식이 있는데 주로 규모가 어느정도 있는 프로젝트에서는 모듈화 방식을 사용한다.

실무에서 vuex를 사용하는 방식은 각자의 코딩스타일에 따라 조금씩 변경되는 듯하다.

이 포스팅을 작성하다가 vuex 컨벤션이 있나싶어 알아보았는데, 여태 잘못된 방식으로 vuex를 사용하고 있다는 것을 깨달았다. 알맞는 컨벤션을 사용해 위에 정의했던 변수명과 함수명을 수정해보자.

```jsx
// State -> 중첩금지, 단어 구분은 언더바 사용 (ex. category-id)
state = () => ({
	state_a: [],
	state_b: [],
})

// Getters -> is로 시작하거나 get으로 시작, 단어 구분은 camelCase 사용 (ex.getUserId)
getters = {
	getStateA(state) {
		return state.stateA
	}
	getStateB(state) {
		return state.stateB
	}
}

// Actions -> 역할에 맞게 fetch, set으로 시작, 단어 구분은 camelCase 사용 (ex. fetchProduct)
actions = {
	fetchStateA({commit}, payloadA) {
		commit('changeA', payloadA)
	}
	fetchStateB({commit}, payloadB) {
		commit('changeB', palyoadB)
	}
}

// Mutations -> 모두 대문자 사용, 단어 구분은 언더바 사용 (ex. SET_XXX, ADD_XXX, REMOVE_XXX)
mutations = {
	SET_STATE_A(state, newStateA) {
		state.stateA = newStateA
	},
	SET_STATE_B(state, newStateB) {
		state.stateB = newStateB
	}

```

각각 컨벤션 규칙이 달라서 작성할때 번거로울 수는 있겠지만 유지보수의 용이성을 위해서 앞으로는 컨벤션 규칙을 지켜서 vuex를 사용하자 !!




- 참고자료
  - [https://fe-churi.tistory.com/24](https://fe-churi.tistory.com/24)
  - [https://kdydesign.github.io/2019/05/09/vuex-tutorial/](https://kdydesign.github.io/2019/05/09/vuex-tutorial/)
  - [https://velog.io/@nuxt/Vuex-명명-규칙](https://velog.io/@nuxt/Vuex-%EB%AA%85%EB%AA%85-%EA%B7%9C%EC%B9%99)
