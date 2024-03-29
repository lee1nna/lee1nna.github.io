---
title:  "[FE 지식] 브라우저의 렌더링 방식 (CSR, SSR, SSG, ISR)"
excerpt: "브라우저의 렌더링 방식에 대해 알아보자!"

categories:
  - FE 지식
tags:
  - [Front-End, Rendering, CSR, SSR, SSG, ISR]

toc: true
toc_sticky: true

date: 2023-05-19
last_modified_at: 2023-05-19
---
브라우저 렌더링 방식은 4가지가 있다. 이번 포스트에서는 각 렌더링 방식의 특징에 대해서 알아볼 것이다.
먼저 렌더링이 무엇일까?

## 렌더링(Rendering)

---

렌더링이란 페이지 접근 시에 서버에 요청해서 받은 내용을 브라우저 화면에 뿌려주는 과정이다.

이때 **화면에 보일 페이지의 내용을 그리는 주체가 누구냐**에 따라 크게 클라이언트  사이드 렌더링과 서버 사이드 렌더링 방식으로 나뉜다.

이름에서 나타나듯이 클라이언트 사이드 렌더링 방식에서 페이지의 내용을 그리는 주체는 클라이언트이고.
서버 사이드 렌더링 방식에서 페이지의 내용을 그리는 주체는 서버이다.

### CSR (Client Side Rendering)

---

클라이언트 사이드 렌더링은 페이지의 내용을 브라우저에서 그린다.
클라이언트 사이드 렌더링의 경우 서버에 요청 시 서버에서 웹 페이지를 렌더링하는 대신 웹 페이지의 골격이 될 단일 페이지를 클라이언트에 보내면서 Javascript 파일도 함께 보낸다.

서버에서 받아온Javascript 파일은 클라이언트쪽에서 브라우저에 하나하나 연결해서 DOM을 생성한다. 
이후 페이지 렌더링이 완료된 이후에는 사용자의 인터렉션이 발생된 화면만 부분적으로 재렌더링을 진행한다.

<aside>
💡 Client Side Rendering

**장점**

- 한번 로딩이 되고 나서는 부분적으로 재렌더링 되기 때문에 빠른 UX를 제공한다.
- 클라이언트에서 렌더링을 진행하므로 서버 부하가 적다.

**단점**

- 클라이언트에서 렌더링을 진행하기 때문에 처음 페이지 로딩 시간이 길다.
- index.html 하나만 받아와서 작동하기 때문에 각 페이지에 대한 정보를 담기 힘들기 때문에 SEO 최적화가 힘들다.
- 보안에 취약하다.
- CDN에 캐시가 안된다.
</aside>

### SSR (Server Side Rendering)

---

SSR은 브라우저에 이미 렌더링이 준비된 HTML 파일을 보내준다.
서버에서 이미 완성된 웹페이지를 보내주기 때문에 페이지 첫 진입시 로딩속도가 빠른편이다.
SSR은 사용자의 인터렉션이 발생되면 전체 페이지를 다시 서버로 부터 받아와야 한다.

<aside>
💡 Server Side Rendering

**장점**

- CSR보다 초기 진입 시의 로딩이 빠르다.
- html에 모든 정보가 담겨져 있어 SEO가 가능하다.

**단점**

- 사용자의 인터렉션이 발생함에 따라 새로운 html을 받아와 요청시마다 새로고침 되어 화면이 깜빡이는 현상이 생긴다.
- 중복되는 레이아웃 부분도 다시 렌더링해서 받아와야 하기 때문에 페이지간 이동은 CSR보다 느리다.
</aside>

### SSG (Static Site Generation)

---

SSG는 클라이언트에서 필요한 페이지들을 빌드시에 미리 생성해두었다가 요청을 받으면 이미 완성된 파일을 단순히 반환하여 브라우저에서 보여준다.

SSR과의 차이점은 서버에서 요청할 때 즉시 만드느냐와 미리 만들어 놓느냐에 차이가 있다.

SSR은 요청시 서버에서 즉시 HTML을 만들어 응답하기 때문에 데이터가 달라져서 미리 만들어두기 어려운 페이지에 적합하고 SSG는 미리 다 만들어두고 요청시에 해당 페이지를 응답하기 때문에 바뀔일이 거의 없어 캐싱해두면 좋은 페이지에 사용된다.

### ISR (Incremental Static Regeneration)

---

ISR은 빌드 시점에 페이지를 렌더링 한 후 설정한 일정 주기에 따라 페이지를 새로 렌더링 한다. 즉 SSG에 포함되는 개념이라고 할 수 있다. SSG는 빌드시에 페이지를 생성하기 때문에 fetching 하는 데이터가 변경되면 재빌드해야 하지만 ISR는 일정 시간마다 알아서 페이지를 업데이트 해준다.

### 결론

---

각각의 렌더링 방식은 장단점이 명확하기 때문에 개발할 서비스가 어떤 특징을 가지며 어떤 기능이 포함되어야 하느냐에 따라서 적절한 렌더링 방식을 사용하는 것이 중요하다.

- 참고자료
    - [https://velog.io/@ka0son/렌더링-삼형제-CSR-SSR-SSG-이해하기](https://velog.io/@ka0son/%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%82%BC%ED%98%95%EC%A0%9C-CSR-SSR-SSG-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

    - [https://velog.io/@wkfwktka/서버-사이드-렌더링SSR과-클라이언트-사이드-렌더링CSR](https://velog.io/@wkfwktka/%EC%84%9C%EB%B2%84-%EC%82%AC%EC%9D%B4%EB%93%9C-%EB%A0%8C%EB%8D%94%EB%A7%81SSR%EA%B3%BC-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%82%AC%EC%9D%B4%EB%93%9C-%EB%A0%8C%EB%8D%94%EB%A7%81CSR)

    - [https://jess2.xyz/web/csr-ssr/](https://jess2.xyz/web/csr-ssr/)