---
title:  "[Typescript] 타입 별칭(Alias)과 인터페이스(Interface)"
excerpt: "typescript의 타입 별칭과 인터페이스에 대해 알아보자!"

categories:
  - Typescript
tags:
  - [Front-End, Typescript, alias, interface]

toc: true
toc_sticky: true

date: 2023-02-21
last_modified_at: 2023-02-21
---
이번 글에서는 타입스크립트에 유사한 타입 별칭과 인터페이스에 대해 알아보고 그 둘의 차이점에 대해 알아보자.

## 타입 별칭 (Type Alias)

---

타입 별칭은 새로운 타입을 생성한다는 개념이라기 보다는 **정의한 타입에 대해 쉽게 참고할 수 있도록 이름을 부여한 것**이다. 
타입 별칭은 `type` 키워드를 사용하여 선언할 수 있다.

```tsx
type Person = {
	name: string,
	age: number
}
```

타입 별칭을 타입으로 지정해둔 변수의 타입에 마우스를 올리면 Vscode 개발툴에서는 다음과 같이 보여준다.

![Untitled](https://user-images.githubusercontent.com/71548623/220379347-c88dac2f-8a7e-4ebc-8330-f4bc3c2871cf.png)

## 인터페이스 (Interface)

---

타입 인터페이스는 **여러가지 타입을 갖는 프로퍼티로 이루어진 새로운 타입을 정의**하는 것과 유사하다
인터페이스는 `interface` 키워드를 사용하여 선언할 수 있다.

```tsx
interface Person {
	name: string,
	age: number
}
```

인터페이스 타입으로 지정해둔 변수의 타입에 마우스를 올리면 Vscode 개발툴에서는 다음과 같이 보여준다.

![Untitled](https://user-images.githubusercontent.com/71548623/220379353-b82ab79b-370f-457a-82df-8fbc5c4124c5.png)

보통 별도의 파일에 필요한 인터페이스를 선언한 후 해당 인터페이스가 필요한 파일에서 인터페이스를 import해와서 사용한다.

## 타입 별칭과 인터페이스의 차이점

---

타입 별칭과 인터페이스의 가장 큰 차이점은 확장성의 유무이다.
**타입 별칭은 확장이 불가능**하고, **인터페이스는 `extends` 키워드를 통해 확장이 가능**하다.

좋은 소프트웨어는 확장이 용이해야하기 때문에 가급적이면 확장이 가능한 인터페이스를 사용하는 것이 더 좋다 !!