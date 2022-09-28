---
title:  "[Nuxt + Typescript] 카카오톡 공유하기 sdk 사용하기"
excerpt: "Nuxt + Typescript 환경에서 카카오톡 공유하기 기능을 사용해보자!"

categories:
  - Typescript
tags:
  - [Nuxt, Typescript, Kakao, Share]

toc: true
toc_sticky: true

date: 2022-09-28
last_modified_at: 2022-09-28
---
카카오톡에서는 다양한 기능의 sdk를 제공한다.
이번 포스팅에서는 그 중에서 카카오톡 공유하기 sdk를 nuxt 환경에서 어떻게 사용할 수 있는 지 순서대로 알아보자!
참고로 나는 nuxt2 + composition API  + typescript 환경에서 진행했다.

## 1. 애플리케이션 등록

먼저 카카오톡 sdk 를 사용하기 위해서는 [카카오톡 개발자 홈페이지](https://developers.kakao.com/)에서 애플리케이션을 등록해야 한다.
로그인 후 애플리케이션 추가하기를 통해 애플리케이션 등록을 할 수 있다.

![Untitled (1)](https://user-images.githubusercontent.com/71548623/192806442-cd565a6d-3887-40f6-8a56-aa6b96d5da89.png)

애플리케이션 등록 후에는 위와 같이 앱 정보를 확인할 수 있다.
나는 웹 플랫폼에서 sdk를 사용할거기 때문에 **Javascript 키**가 꼭 필요하다!

## 2. 웹 플랫폼 등록

이제 웹 플랫폼을 등록해보자.

웹 플랫폼은 아래 사진에서 보이는 경로로 등록이 가능하다.
웹 등록을 할때 사이트 url을 적게 되는데 보통 카카오톡 공유하기 기능을 사용할 서비스 url을 적는다.
하지만 나는 테스트가 목적이기 때문에 http://localhost:3000로 등록을 진행했다.

![Untitled](https://user-images.githubusercontent.com/71548623/192806465-2fd23da4-4f52-4916-a959-5f47d463c163.png)

## 3. Nuxt에서 카카오톡 sdk 사용하기

nuxt에서 쉽게 카카오톡 sdk를 사용하기 위해서 [vue-kakao-sdk](https://github.com/eggplantiny/vue-kakao-sdk) 설치한다.

`npm install vue-kakao-sdk`

## 4. 플러그인 설정

설치한 vue-kakao-sdk를 사용하기 위해서 플러그인을 추가해준다.

```jsx
 📂 /plugins/kakao.js

import Vue from 'vue'
import VueKakaoSdk from 'vue-kakao-sdk'

const apiKey = 'Your Kakao API Javascript Key'

// You have to pass your "Kakao SDK Javascript apiKey"
Vue.use(VueKakaoSdk, { apiKey })
```

apiKey 변수에 이전에 애플리케이션 등록 후 받은 Javascript키를 넣어준다.
플러그인 생성 후 nuxt.config.ts파일에서 plugins 경로를 연결해준다.

```jsx
📂 /nuxt.config.ts

plugins: [
    {src: '~plugins/kakao.js'}
  ],
```

플러그인 경로 연결 후 만약 `window is not defined` 에러가 발생했다면, 아래 ssr 속성을 추가해준다.

```jsx
📂 /nuxt.config.ts

plugins: [
    {src: '~plugins/kakao.js', ssr:false}
  ],
```

## 5. 카카오톡 공유하기 기능 사용하기

자 이제 모든 세팅을 마쳤으니 공유하기 기능을 사용해보자.
카카오톡 공유하기는 템플릿이 있는데 이 템플릿을 지정하는 방식은 두가지가 있다.

1. 카카오에서 제공하는 템플릿 빌더 사용
2. 내가 직접 코드로 템플릿 제작

두가지 방법중 어느 방법을 사용해도 무관하다. 먼저 1번 방법에 대해 알아보자.

### 5-1. 카카오에서 제공하는 템플릿 빌더 사용

![Untitled (2)](https://user-images.githubusercontent.com/71548623/192806450-3beb7ec2-408e-4bbd-84bb-b58121313e91.png)
![Untitled (3)](https://user-images.githubusercontent.com/71548623/192806454-6c4070ec-790b-431d-90d0-180678646fd5.png)

메시지 템플릿 빌더 바로가기 링크로 이동하면 템플릿을 만들 수 있다.
어느 영역에 어떤 정보가 들어가는지 엄청 직관적으로 잘나와 있어서 문제 없이 템플릿을 제작할 수 있다.

`${argument}` 을 통해 동적으로 변수값을 받아서 처리해 줄 수도 있다.

템플릿 빌더를 만들었으면 이제 카카오 공유하기 기능을 구현해보자.

```tsx
<template>
  <button @click="shareKakao">
    카카오톡 공유하기
  </button>
</template>

setup(){
    const shareKakao = () => {
      window.Kakao.Share.sendScrap({
        requestUrl: 'http://localhost:3000/',
        templateId: 83474 //template ID
      });
    }

    return {
      shareKakao
    }
 }
```

위 코드로 간단하게 카카오톡 공유하기 기능을 구현할 수 있다.
sendScrap에 들어가는 속성들은 [카카오톡 공유하기](https://developers.kakao.com/docs/latest/ko/message/js-link)에서 모두 확인할 수 있다.
typescript를 사용할 경우에는 window.kakao에 아래와 같은 에러가 날 수 있다.

![Untitled (4)](https://user-images.githubusercontent.com/71548623/192806458-8df324c4-15b8-46fb-a61b-e8fc52656860.png)

window객체에 kakao프로퍼티가 추가가 되어있지 않아서 나는 에러로 index.d.ts 파일을 생성해 다음과 같이 프로퍼티를 추가해준다.

```tsx
export {};

declare global {
  interface Window {
    Kakao: any;
  }
}
```

카카오톡 템플릿 빌더를 사용해 공유하기 기능을 구현한 결과이다 !!!

![Untitled (5)](https://user-images.githubusercontent.com/71548623/192806460-de20266b-6cef-4cee-ba94-cad584f5f57b.png)
### 5-2. 내가 직접 코드로 템플릿 제작

내가 직접 코드로 템플릿을 제작하는 방법도 어렵지 않다.
`sendScrap`을 `sendDefault`로 변경하고 템플릿에 추가하고 싶은 속성들을 추가해주면 된다.

```tsx
const shareKakao = () => {
      window.Kakao.Share.sendDefault({
        objectType: 'feed',
        content: {
          title: '직접 만든 템플릿',
          description: '설명부분 입니다',
          imageUrl: 'https://mblogthumb-phinf.pstatic.net/MjAxOTA4MjVfMjgg/MDAxNTY2NjYzNTA3MjEy.vWFFLv1u7SzDBes1sRdbFhn5uB8WnnbG95hAiWHkzV0g.r9fWqRf8vSfbDP_NlOGE_YA38GAcT50ZJLkTymx3a7Ag.JPEG.cdhh108/889F9465-4BD3-4523-8D8B-409CD0D70453.jpg?type=w800',
          imageWidth: 340,
          imageHeight: 600,
          link: {
            mobileWebUrl: 'http://localhost:3000/',
            androidExecutionParams: 'test',
          },
        },
        buttons: [
          {
            title: '카카오톡 공유하기 기능 만들러가기',
            link: {
              mobileWebUrl: 'http://localhost:3000/',
              webUrl: 'http://localhost:3000/',
            },
          },
        ],
    })
}
```

직접 템플릿을 사용해 구현한 결과이다.

![Untitled (6)](https://user-images.githubusercontent.com/71548623/192806463-28d562df-838a-4a80-a26d-447f97204007.png)
