---
title:  "[Typescript] 타입스크립트 필수 문법"
excerpt: "Typescript의 필수 문법에 대해 알아보자!"

categories:
  - Typescript
tags:
  - [Typescript, narrowing, assertion, type alias]

toc: true
toc_sticky: true

date: 2022-08-03
last_modified_at: 2022-08-03
---
## 타입스크립트 시작하기 - static 웹페이지

---

프레임워크를 사용하지 않고 그냥 static 웹페이지 개발시에 타입스크립트 사용하는 방법에 대해 알아보자.

1. node.js 설치
2. `npm install -g typescript` 로 타입스크립트 설치
3. 프로젝트 생성
4. 프로젝트에 `tsconfig.json` 파일 추가한 후 아래 내용 추가

```jsx
{
  "compilerOptions": {
    "target": "ES5",  // js ES5 문법으로 컴파일
    "module": "commonjs" // module은 commonjs 방식으로 컴파일
  }
}
```

타입스크립트는 브라우저에서 읽을 수 없기때문에 자바스크립트로 컴파일해주는 과정이 필요하다.

따라서 ts → js 컴파일 시 옵션을 `tsconfig.json` 파일에 넣어주면 된다.

## 타입을 지정하는 다양한 방법

---

타입스크립트는 변수를 만들 때 타입 지정이 가능하다.

`변수명 :타입명` 과 같이 작성한다.

타입으로 쓸 수 있는 것은 원시타입 (string, number, boolean, bigint, null, undefined)과 객체타입([], {})등이 있다.

```tsx
let name :string = '홍시'
name = 123
```

타입을 미리 지정해놓으면 타입이 의도치 않게 변경될 경우 에러메시지를 띄워주기 때문에 사전에 타입 관련 버그들을 없앨 수 있다.

array나 object 자료 타입은 다음과 같이 지정이 가능하다.

```tsx
let name :string[] = ['홍시','홍홍']
let person :{name:string, sex:string} = {name:'홍시', sex:'여자'}
```

만약 변수에 여러가지 타입의 데이터가 들어올 수 있다면
| 기호를 이용해 or 연산자를 표현할 수 있다.
아래 코드는 변수에 숫자 또는 문자를 집어넣을 수 있다.

```tsx
let zipCode :string | number = '11333'
```

타입스크립트에서는 `type` 키워드를 이용해 타입을 변수처럼 담아서 사용이 가능하다. 이렇게 타입을 지정하는 것을 **type alias**라고 한다.

위에서 | 기호를 사용해서 여러가지 타입의 데이터를 들어오도록 했던 것을 type 키워드로 나타내면 다음과 같다.
type 키워드를 사용해 타입을 변수처럼 담을 때에는 보통 대문자로 변수명을 시작한다.

```tsx
type ZipType = string | number
let zipCode :ZipType = 11133
```

string, number 와 같은 타입뿐만 아니라 나만의 타입을 만들어서 사용하는 것도 가능하다. 만약 동물의 종을 말티즈와 치와와만 받고싶은 타입을 만들고자한다면 아래와 같이 나타낼 수 있다.
이런 타입을 **literal type** 이라고 부른다.

```tsx
type AnimalSpecies = '말티즈' | '치와와'
let animal :AnimalSpecies = '말티즈'
```

함수는 파라미터와 return 값이 어떤 타입일지 지정 가능하다.
만약 지정되어 있는 타입과 다른 파라미터와 return 값이 있을 경우 에러를 내준다.
return 할 값이 없는 경우 void 타입으로 설정 가능하다.

```tsx
function typeFunc(x: number) :number {
	return x * 2
}
```

타입스크립트는 지금 변수의 타입이 확실하지 않으면 마음대로 연산할 수 없다.
따라서, **narrowing**이나 **assertion**을 사용해 타입을 확실히 해주어야 한다.
narrowing은 if문을 통해 타입을 정해주는 것이고,
assertion은 as를 이용해 타입을 덮어쓰는 방법이다.

```tsx
// narrowing을 이용한 타입 지정
function narrowingType(x) {
    if (typeof x === 'string') {
        return x + '1';
    }
    else if (typeof x === 'number') {
        return x + 1;
    }
    else {
        return 0;
    }
}
```

narrowing에서는 `typeof` 를 이용해 변수가 어떤 타입인지 체크한다.
타입이 array인지 체크할 때에는 `Array.isArray()` 를 사용할 수 있다.

```tsx
// assertion을 이용한 타입 지정
function assertionType(x) {
    var arr = [];
    arr[0] = x as number; // 'x의 타입을 number로 생각해주세요'
}
```

assertion은 아무때나 사용해선 안되고, 무슨 타입이 들어올 지 확실할 때에만 사용해야 한다. 주로 에러를 디버깅 할 때 잠깐 사용한다.

array 자료 안에 순서를 포함해 어떤 타입들이 들어올지 정확히 지정하고 싶으면 **튜플(Tuple)** 타입을 사용할 수 있다. 튜플타입은 다음과 같이 사용한다.

```tsx
type Member = [number, boolean];
let john:Member = [100, false]
```

object타입도 정의가 너무 길면 type키워드로 변수에 담아서 사용이 가능하다.
type 키워드 대신 비교적 최근에 나온 interface 키워드를 사용해도 무방하다.
object에 특정 속성이 옵션이면 ?통해 나타낼 수 있다.

```tsx
type Member = {
  name? : string,
  age : number
}

let choi :member = {
  name : 'kim',
  age : 50
}
```

object내에 어떤 속성이 들어갈지 아직 모른다면 object 전체에 타입 지정도 가능하다. 이것을 **index signature**라고 하며 다음과 같이 사용 가능하다.

```tsx
type Member = {
	[key: string]: number
}

let choi :member = {
	weight: 100,
  age : 50
}
```

key 값은 string 타입, value 값은 number 타입만 들어올 수 있다.

class도 타입 설정이 가능하다.
다만, 중괄호 내에 미리 변수를 만들어놔야 constructor 내에서 `this.변수명` 과 같이 사용이 가능하다.

```tsx
class Person {
  name;
  constructor(name :string){
    this.name = name;
  }
}
```

- 참고강의
  - [타입스크립트 문법 10분 정리와 설치 셋팅](https://codingapple.com/unit/how-to-install-typescript-in-local-vue-react/?id=11721)
