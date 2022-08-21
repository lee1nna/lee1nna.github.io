---
title:  "[FE 지식] Semantic Tag와 Semantic Web"
excerpt: "Semantic Tag와 Semantic Web에 대해 알아보자!"

categories:
  - FE 지식
tags:
  - [Front-End, Semantic-tag, Semantic-web, SEO]

toc: true
toc_sticky: true

date: 2022-08-21
last_modified_at: 2022-08-21
---
## [FE 지식] Semantic Tag와 Semantic Web

---
## Semantic Tag?

html에는 `Semantic Tag`라는 것이 존재한다.

[w3schools.com](https://www.w3schools.com/html/html5_semantic_elements.asp)에서`Semantic Tag` 는 다음과 같이 정의한다.

```
💡 What are Semantic Elements?
시멘틱 요소는 자신의 의미를 브라우저와 개발자 모두에게 명확하게 설명한다.
non-semantic 요소들의 예: <div> 와 <span> : 자신의 컨텐츠에 대해 아무것도 설명해주지 않는다.
semantic 요소들의 예: <form>, <table>, <article> : 자신의 컨텐츠를 명확하게 정의한다.
```

## Semantic Web?

그렇다면 `Semantic Web` 은 무엇일까?
`Semantic Web` 이란 **컨텐츠의 의미를 부여한 요소인 `Semantic Tag` 를 사용해서 구축한 웹 사이트**를 말한다. 웹 크롤러가 **웹 사이트의 의미와 컨텐츠의 내용을 이해하도록 하여 더 고품질의 정보를 수집하고 처리할 수 있도록 하는 것에 도움**을 준다.

또한, 시멘틱 웹은 시각 장애가 있는 유저가 사용하는 Screen reader에게 정보를 제공하는 것에도 사용될 수 있다.

> 📍 Screen reader란?
컴퓨터 화면 내 문자를 음성으로 읽어주는 SW
>

## 왜 Semantic Web을 사용해야 할까?

시멘틱 웹은 **SEO에 유리**하다.

> 📍 SEO란?
Search Engine Optimization
검색 유저에게 양질의 컨텐츠와 경험을 제공하는 것으로 포털 사이트 검색 엔진에 웹사이트를 상위에 노출시키기 위해 기술적으로 웹 사이트를 최적화하는 대책을 말한다.
>

유저가 검색한 키워드와 웹 사이트의 컨텐츠가 연관성이 있다는 것을 웹 크롤러에게 인지 시켜주어야 웹 사이트가 상위에 노출될 수 있고 웹 크롤러로부터 높은 평가를 받은 웹사이트는 검색 순위의 상위권에 노출될 확률이 높아지게 된다.

이 때 `**Semantic Tag` 를 사용해 구축한 사이트는 컨텐츠를 인식할 수 있기 때문에 검색 키워드와 Semantic Web의 컨텐츠가 서로 부합하는지 검토할 수 있다.**

## Semantic Tag는 어떻게 사용하는 것이 좋을까?

![image](https://user-images.githubusercontent.com/71548623/185796640-0792a077-2c9e-4e98-8ee4-7a57be3d709f.png)

출처: [https://velog.io/@gud_wns/Semantic-Web과-Semantic-Tag](https://velog.io/@gud_wns/Semantic-Web%EA%B3%BC-Semantic-Tag)

### `header`

웹 페이지 도입부, 주로 페이지 상단 웹사이트 로고와 네비게이션 링크를 감싸는 것에 사용한다.

일반적으로 <body> 상단에 표기되어 웹 사이트의 Title이나 네비게이션이 존재하는 영역에 사용된다. 하지만 <article>. <section> 내에서도 사용될 수 있다.

- <body> 내에서 사용될 경우 : 웹 페이지 전체 헤더를 정의한다.
- <article>, <section> 내에서 사용될 경우 : 해당 영역에 대한 특정 헤더를 정의하는 경우에 사용한다.

### `<nav>`

홈페이지의 메인 영역으로 연결하는 태그다. 대부분의 메뉴 버튼이나 링크, 탭으로 표현될 수 있다.
<body> 하위의 <header>에 위치하기도 하며 링크 그룹을 모아둔 곳이다.

- <header> 하위에 말고 동등한 위치에 두는 것이 SEO 측면으로 더 좋다.
- <nav> 는 하나의 문서에서 여러 개의 <nav>를 가질 수 있다. 단, 모든 링크가 <nav>안에 있어야 하는 것은 아니다.
- 한 페이지 당 1개를 사용해 네비게이션 메뉴에만 제한해서 쓰기도 한다.
- 내용을 쉽게 이해할 수 있도록 nav 요소 내부에는 비순차 목록(<ul>)을 사용하는 것이 좋다.
- 사이트 하단에 위치한 링크는 <footer>로 충분하다.

### `<main>`

메인 컨텐츠 내용이 들어갈 태그다.

- <body>에 포함되지만 섹션 요소가 아니며, 페이지 당 여러개가 존재할 수는 있지만 단 1개만 시각적으로 보여야 한다. (나머지 main은 hidden 속성 처리 해주어야 한다.)
- 섹션 요소인 section, article, aside, nav 요소는 main 요소를 자식으로 포함할 수 없다.
- 타이틀은 <header>안 <h1>~<h6>로 하며 본문 내용은 주로 <p>를 사용한다.

### `<section>` 과 `<article>`

- article 내부에 section 태그를 포함할 수 있고, section 내부에 article 을 포함할 수 있다.
- 콘텐츠가 사이트에 포함된 독립적인 섹션의 성향이 크다면 section 요소 대신 article 요소를 사용하는 것이 좋다.
- <article> 은 웹 접근성을 위해 <header>로 감싼, 제목(h1~h6 요소)을 포함시켜 요소를 식별하게 하자. 마찬가지로 본문 내용은 <p> 를 사용하자. 만약 섹션 제목을 감춰야 하는 상황이라면 hidden 속성을 사용하면 된다.

### `<aside>`

- 문서의 주요 내용과 간접적으로 연관된 부분을 나타낸다.


- 참고자료
  - [시멘틱 태그 Semantic Tag](https://kutar37.tistory.com/entry/%EC%8B%9C%EB%A9%98%ED%8B%B1-%ED%83%9C%EA%B7%B8-Semantic-Tag)
  - [Semantic Web에 대해 더 깊이 알아보기](https://pozafly.github.io/html/semantic-web/)
