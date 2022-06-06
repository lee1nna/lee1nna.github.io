---
title:  "[Javascript] 이벤트 위임"
excerpt: "Javascript의 이벤트 위임에 대해 알아보자."

categories:
  - Javascript
  tags:
  - [Javascript, event-delegation]

toc: true
toc_sticky: true

date: 2022-04-28
last_modified_at: 2022-04-28
---

이번 포스팅은 [유데미](https://www.udemy.com/)라는 사이트에서 강의를 진행하시는 Maker Jun 강사님의 Vanilla JS Lv1. 문벅스 카페 메뉴 앱 만들기 스텝1 파트를 수강하고, 배웠던 부분을 까먹지 않기위해 회고하는 포스터를 작성하려고 한다.

우선 이 강의를 수강하게 된 이유는 요즘 Vue 프레임워크로 회사에서 업무를 보다보니 Vanila JS에 대한 부족함을 계속 느껴 강의를 수강하고 싶었던 와중 유데미에서 여러 강의들을 할인하고 있었다. 이 강의에 대한 평도 좋았고, 또 강의 커리큘럼을 보니 기초강의지만 리팩토링 하는방법도 강의에 포함되어 있어 들어보기로 했다.

강의의 스텝1까지 들어본 결과 강의 난이도는 쉬운편이지만 몰랐던 부분들과 어렴풋이 알고 있던 부분들도 꽤 있었고 더 공부하고 싶은 부분들도 있었다.

강의에서 배웠던 부분 중 오늘은 이벤트 위임에 대해서 더 알아보고자 한다.

## 1. 이벤트 위임

---

이벤트 위임은 강의에서 메뉴의 수정과 삭제를 구현하기 위해 사용했었다.

이벤트 위임을 알기 위해서는 이벤트 버블링과 캡쳐링에 대한 이해가 필요하다.

### 이벤트 버블링

---

**이벤트 버블링**이란 **특정 엘리먼트에 이벤트가 발생하면 해당 이벤트가 그 엘리먼트의 조상들에게 까지 전달되는 현상**이다.

```html
<div>
	조상 element
		<p> 자식 element </p>
</div>
```

```jsx
const parent = document.querySelector('div')
const child = document.querySelector('p')

function Alert(message) {
	return function() {
        alert(message)
    }
}

parent.addEventListener('click', Alert('div tag event'))
child.addEventListener('click', Alert('p tag event'))
```

위 예시를 보면 div태그와 p태그 클릭시 alert을 띄우도록 코드를 구현했다.

![이벤트 버블링](https://user-images.githubusercontent.com/71548623/164967315-3cbec0e4-fd28-4a2b-8881-bd04bdfd6284.gif)
parent를 눌렀을 때는 div alert창만 뜨지만, child를 눌렀을 때는 p alert, div alert창 모두 뜨는 것을 확인할 수 있다. 이 현상이 바로 이벤트 버블링이다.

### 이벤트 캡쳐링

---

이벤트 캡쳐링은 특정 엘리먼트에 이벤트가 발생했을 경우 이벤트가 최상단의 부모 엘리먼트로부터 전달되어져 내려오는 현상이다.

캡쳐링을 수행하기 위해서는 이벤트 핸들러에 `{capture: true}` 혹은 `true` 로 캡쳐링 옵션을 활성화시켜주어야 한다. 디폴트는 false이기 때문에 활성화해주지 않으면 캡쳐링은 일어나지 않는다.

```jsx
const elements = document.querySelectorAll('*')

for(let el of elements) {
    el.addEventListener('click', e => alert(`캡쳐링 이벤트 발생: ${el.tagName}`), true)
}
```

![이벤트 캡쳐링](https://user-images.githubusercontent.com/71548623/164967322-ab34d6f5-c532-4deb-a8c5-303619a409f2.gif)
위 예시를 보면 알 수 있듯이 이벤트 캡처링을 활성화하면 엘리먼트 클릭시 가장 최상단 부모 엘리먼트 부터 클릭된 엘리먼트까지 전달되어 내려오는 것을 알 수 있다. 이 현상이 바로 이벤트 캡쳐링이다.

### 버블링과 캡쳐링 멈추기

---

버블링은 대체로 `<html>` 엘리먼트까지 올라가는데, 이러한 이벤트를 멈추기 위해서는 최초로 이벤트가 발생되는 엘리먼트의 이벤트 핸들러에 다음과 같은 코드를 작성해주면 된다.

```jsx
event.stopPropagation() // 이벤트 버블링 멈추기
```

위 코드는 캡쳐링을 멈출때에도 동일하게 사용된다.

버블링에서는 타겟 엘리먼트만 이벤트가 발생하도록 해주고, 캡처링에서는 타겟 엘리먼트 기준으로 최상단 엘리먼트에만 이벤트가 발생하도록 해준다.

만약 하나의 이벤트에 여러 핸들러가 붙어있어 모든 버블링을 멈추고 싶은 경우에는 다음과 같이 코드를 작성해주면 된다.

```jsx
event.stopImmediatePropagation() // 모든 이벤트 버블링 멈추기
```

### 이벤트 위임

---

이벤트 위임은 캡쳐링과 버블링을 이용한 것으로, **여러 엘리먼트마다 각각 이벤트 핸들러를 할당하지 않고, 공통되는 부모에 이벤트 핸들러를 할당하여 이벤트를 관리하는 방식**이다.

**여러개의 자식 엘리먼트의 이벤트를 관리할때 유용하게 사용**된다. 각각의 정해진 액션에 따라 다른 동작을 하는 여러 버튼에 대한 이벤트를 발생시키고자 할 때 모든 버튼에 대해 이벤트 리스너를 등록하는 것은 매우 비효율적이다.

따라서 이벤트 위임을 이용해 **공통 부모에 이벤트를 등록하고 정해진 데이터의 액션에 따라 다른 함수를 실행하는 것**이다.

```jsx
<div id="crud">
    <button id="create">생성</button>
    <button id="read">읽기</button>
    <button id="update">수정</button>
    <button id="delete">삭제</button>
</div>
```

```jsx
const crud = document.getElementById('crud')

crud.addEventListener('click', (e) => {
    if(e.target.getAttribute('id') === 'create') {
        alert('생성')
    } else if(e.target.getAttribute('id') === 'read') {
        alert('읽기')
    } else if(e.target.getAttribute('id') === 'update') {
        alert('수정')
    } else if(e.target.getAttribute('id') === 'delete') {
        alert('삭제')
    } else {
        alert('빈 영역입니다.')
    }
})
```

![이벤트 위임](https://user-images.githubusercontent.com/71548623/164967321-76caf470-fdf2-4e27-9af5-5f4dc16e404d.gif)
위 코드를 통해 이벤트 위임을 간단히 나타낼 수 있다.

생성, 읽기, 수정, 삭제 각각의 버튼에 이벤트를 넣지 않고 모든 버튼을 감싸고 있는 부모인 div 박스에 이벤트를 할당해 각각의 버튼 엘리먼트에 이벤트를 주고 있다. 이것이 바로 이벤트 위임이다.

그렇다면 왜 이벤트 위임을 사용해야하는 것일까?

### 이벤트 위임을 사용하는 이유

---

모든 엘리먼트에 이벤트 핸들러를 할당하게 되면 엘리먼트가 삭제되는 경우가 있을 때 엘리먼트는 삭제되지만 **해당 엘리먼트에 적용되어 있던 이벤트는 삭제되지 않아 메모리 누적이 계속되어 후에는 메모리 누수가 발생할 가능성이 커지게 된다.**

따라서 **메모리 누수의 가능성을 줄이기 위해 이벤트 위임을 사용해주는 것**이 좋다.


- 참고자료
  - [[JavaScript] 이벤트 위임](https://velog.io/@moonheekim0118/JavaScript-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81)
  - [[JS] 이벤트 전파와 이벤트 위임](https://ingg.dev/event-delegation/)
  - [왜 이벤트 위임(delegation)을 해야 하는가?](https://ui.toast.com/weekly-pick/ko_20160826)
