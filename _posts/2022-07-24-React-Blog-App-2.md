---
title:  "[React] 간단한 Blog App 만들면서 React 기본 다지기 - 2"
excerpt: "React 라이브러리를 사용해 간단한 Blog를 만들며 기본기를 다져보자!"

categories:
  - React
tags:
  - [Javascript, React, useState, props]

toc: true
toc_sticky: true

date: 2022-07-24
last_modified_at: 2022-07-24
---
# 1. Blog App 만들기

## 1-1. 중요한 변수를 담을때는 useState()

---

블로그에는 포스팅되는 글들은 제목, 내용과 같이 중요한 데이터를 담고있다. 중요한 데이터들은 React Hook중 하나인 **useState()**를 사용해 데이터를 저장하는 것이 좋다.

그렇다면 **중요한 변수의 기준**이 뭘까?

1. **값이 변경되자마자 html에 반영되어야 하는 데이터**
2. **자주 변경되는 데이터**

위와 같이 중요한 데이터를 정의 내릴 수 있다.

state변수는 값이 변경되면 html을 자동으로 재렌더링 해주기 때문에 위 두 기준에 부합할 경우 useState()를 사용해 변수를 생성해주는 것이 좋다.

위에 해당되는 것이 아니면 일반 변수 선언 방법인 let, const를 사용해도 무관하다.

useState()의 사용 방법은 다음과 같다.

```jsx
// 1. useState import
import {useState} from 'react'
// 2. 변수 선언
let [변수명, 변수를 변경 할때 호출할 함수명] = useState(value)
```

useState()로 선언한 변수는 변수를 변경할때 함수를 호출해서 변경하게 되는데 그때 `변수를 변경할 때 호출 할 함수명(변경할 변수의 값)` 변경 함수를 호출해 변수의 값을 변경할 수 있다.
만약, state변경 함수로 값을 변경하지 않고 일반 대입식을 사용해 값을 변경할 경우 html 재렌더링이 안되게 되어 변경된 값을 화면에서 확인할 수 없다.

```jsx
// useState() 예시
let [name, setName] = useState('홍시') // useState로 변수 선언
setName('이홍시') // 변수 값 변경
```

useState()로 state 변수를 만들 때 사용되는 문법은 [구조분해할당(Destructuring)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 문법이다.

## 1-2. Array, Object State 변수의 값 변경하기

---

배열이나 객체의 state 값을 변경하는 경우에 대해 알아보자.

state 변경 버튼을 클릭했을 경우 title의 0번째 인덱스의 state값을 ‘여자코트 추천’으로 변경하고 싶으면 다음과 같이 코드를 짜면 된다고 생각할 것이다.

```jsx
function App(){
  let [title, setTitle] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );

  return (
      <button onClick={ ()=>{setTitle(['여자코트 추천', '강남 우동맛집', '파이썬 독학'])} }> state 변경 </button>
  )
}
```

물론 위와 같이 코드를 작성해도 문제없이 동작한다. 하지만 변경이 불필요한 부분까지 다 입력해주어야 하는 번거로움이 있고 정적으로 데이터가 들어가는 방식이라 확장성에 문제가 있다.

만약 **setTitle(’여자코트 추천’)** 이렇게 넣으면 어떻게 될까?

답은 **정상적으로 작동하지 않는다**이다.

state 변경함수는 아예 기존에 있던 값을 갈아 치워주기 때문에 정상적으로 동작하지 않는다.

자바스크립트의 배열이나 객체는 원시타입이 아닌 reference 타입이기 때문에 배열이나 객체에 담은 변수는 그 변수가 담긴 공간을 가르키는 화살표만 존재하는데, 변수 값을 변경해줘도 화살표는 변경되지 않기 때문에 정상적으로 작동하지 않는 것이다.

따라서 spread operator 문법을 통해 화살표의 방향까지 바꿔주어야 state 변경이 정상적으로 가능하다. spread operator 문법을 통해 state값을 변경하는 방법은 아래와 같다.

```jsx
function App(){
  let [title, setTitle] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );

  return (
      <button onClick={ ()=>{
			let copy = [...title] // spread operator 문법 사용
			copy[0] = '여자코트 추천'
			setTitle(copy)
			}}> state 변경 </button>
  )
}
```

spread operator 문법은 독립적인 배열의 복사본을 생성한다. 이 복사본을 생성하는 것을 shallow copy 또는 deep copy 라고 한다.

## 1-3. 컴포넌트

---

react에서는 컴포넌트라는 독립적인 단위를 통해 UI를 재사용 할 수 있다. 그렇다면 어떤 HTML들을 컴포넌트로 만들어 주는 것이 좋을까?

1. **사이트에 반복해서 나타나는 HTML 덩어리**
2. **내용이 자주 변경될 것 같은 HTML 부분들**
3. **페이지 단위**

위 기준에 부합하면 컴포넌트로 만들어주는 것이 UI 재사용에 용이하다.

react에서 컴포넌트 만드는 방법은 함수를 만드는 방법과 동일하다. App.js 내에서 모달 컴포넌트를 생성해 메인페이지에 모달 컴포넌트를 넣어보자.

```jsx
function Modal(){
  return ( <div>모달입니다.</div> )
}

let Modal = () => {
  return ( <div>모달입니다.</div>)
}
```

이렇게 모달 컴포넌트를 생성할 수 있다. 두 가지 방법 중 어느 방법을 사용해도 무관하다.
react에서 컴포넌트 생성할 때 규칙이 있는데 규칙은 다음과 같다.

1. 컴포넌트 작명시 첫 문자는 영어대문자로 작명한다.
2. return() 내에는 html 태그들이 평행하게 여러개 들어갈 수 없다.
3. 다른 컴포넌트 내부에 또 다른 컴포넌트를 생성할 수 없다.

App 내에서 모달 컴포넌트를 사용하고 싶으면 <Modal/> 혹은 <Modal></Modal> 이렇게 작성해주면 사용할 수 있다.

```jsx
function App (
	return (
		<Modal/>  // 방법1
		<Modal><Modal/> // 방법2
	)
}
```

같은 파일내에 컴포넌트를 생성했으면 위와 같이 바로 사용가능하지만 만약 컴포넌트 파일을 따로 분리해둔 경우에는 **생성한 컴포넌트를 export**하고 **사용하고 싶은 곳에서 import** 해서 사용해야 한다.

## 1-4. 동적 컴포넌트 만들기

---

react에서 동적 컴포넌트 만드는 방법은 다음과 같다.

1. html/css로 UI 디자인
2. UI 현재 상태를 state에 저장
3. state값의 변화에 따라서 UI가 어떻게 보일지 조건문으로 작성

이 방법을 이용해서 간단한 모달창을 만들어 보자!

### 1. html/css로 UI 디자인

```jsx
function Modal(props) {
    return (
        <div className="modal_bg">
            <div className="modal">
                <h4>Title:</h4>
                <p>날짜</p>
                <p>상세내용</p>
            </div>
        </div>
    )
}

export default Modal
```

매우 간단하게 모달창 UI를 구현했다.

### 2. UI 현재 상태를 state에 저장한다.

```jsx
let [modal, setModal] = useState(false)
```

모달의 상태를 저장하는 변수를 생성해주었다.
모달창은 보통 닫혀있거나 열려있거나의 상태를 가지고 처음에는 닫혀있는 상태로 있기 때문에 false로 초기화해주었다.

### 3. state값의 변화에 따라 UI 변경하기

```jsx
function app() {
    return (
				modal값이 true면 <Modal></Modal>
				modal값이 false면 아무것도 보여주지마세요.
    )
}

export default Modal
```

우리가 원하고자 하는 것은 위의 코드 처럼 나타낼 수 있는데 보통 저 경우에는 if문을 사용한다.
react에서는 JSX 문법을 사용하고 JSX문법은 if-else 문법을 바로 사용할 수 없다.
그 대신 삼항연산자를 사용해 위 코드를 바꿔볼 수 있다.

```jsx
function app() {
    return (
				{
					modal? <Modal></Modal> : null
				}
    )
}

export default Modal
```

## 1-5. 동일한 div박스들을 반복문으로 줄이기

---

<img width="352" alt="image" src="https://user-images.githubusercontent.com/71548623/180650254-392e3122-ca13-4886-82fc-c262f292b94f.png">

만든 블로그 웹페이지를 보면 포스팅 글들이 모두 동일한 구조를 갖고 있는 것을 볼 수 있다.
이런 부분들은 react에서 map 함수를 사용해 코드를 축약할 수 있다.

```jsx
function App (){
  return (
    <div>
      {
        title.map((t, i) => {
          return (
          <div className="list">
            <h4>{ t[i] }</h4>
            <p>2월 18일 발행</p>
          </div> )
        })
      }
    </div>
  )
}
```

## 1-6. 부모 컴포넌트의 state 가져다 쓰기 - props

---

컴포넌트간 데이터 통신을 할때는 `props` 를 사용한다.
이전에 만들었던 모달창의 background 를 클릭했을 때 모달창이 닫히게끔 기능을 구현하고 싶으면
자식 컴포넌트(Modal)에서 부모 컴포넌트(App)의 modal 값을 가져와 모달의 background를 클릭 했을 때 modal값을 false로 변경해주면 된다.

```jsx
function App (){
  let [modal, setModal] = useState(false);
  return (
    <div>
      <Modal modal={modal} setModal={setModal}></Modal>
    </div>
  )
}

function Modal(props) {
    return (
        <div className="modal_bg" onClick={() => {props.setModal(false)}}>
            <div className="modal">
                <h4>Title:</h4>
                <p>날짜</p>
                <p>상세내용</p>
            </div>
        </div>
    )
}
```

위 코드에서 3가지 부분만 보면 props 사용법을 알 수 있다.

1. `<Modal modal={modal}></Modal>`

   ⇒ 자식 컴포넌트를 선언하는 부분 내부에서 `자식컴포넌트에서 쓰일 변수명 = { 부모컴포넌트에서 보낼 변수명 }` 를 포함해준다.

2. `function Modal(props)` / `function Modal({modal, setModal})`

   ⇒ 자식 컴포넌트에 파라미터에 props를 추가한다. 변수 사용 시 props를 따로 적고싶지 않으면 오른쪽과 같이 파라미터를 추가해주면 된다.

3. `<div className="modal_bg" onClick={() => {props.setModal(false)}}>` / `<div className="modal_bg" onClick={() => {setModal(false)}}>`

   ⇒ 부모 컴포넌트에서 받은 변수를 props.변수명으로 사용한다. 2번에서 우측 방법으로 파라미터 전달시에는 props를 따로 적어주지 않아도 된다.


코드를 보면 알 수 있겠지만 **자식 컴포넌트에서는 부모 컴포넌트에서 받은 변수를 사용할 수는 있지만 대입식으로 값을 변경 할 수 는 없다.** 따라서 **값 변경 함수도 props로 전달해주고 그 함수를 통해 변수 값을 변경**해주어야 한다.

이렇게 코딩애플님의 강의를 들으면서 React의 기초를 다지면서 간단한 블로그 앱을 만들어보았다.

![react-blog](https://user-images.githubusercontent.com/71548623/180648177-c450217c-d524-45a0-b171-1c3d8fe64b33.gif)


- 참고강의
  - [[React] React 리액트 기초부터 쇼핑몰 프로젝트까지!](https://codingapple.com/course/react-basic/)
