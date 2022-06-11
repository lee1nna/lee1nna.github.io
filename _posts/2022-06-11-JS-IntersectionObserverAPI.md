---
title:  "[Javascript] Intersection Observer API로 무한스크롤 구현하기"
excerpt: "Intersecion Observer API를 사용해서 무한으로 나오는 홍시 사진을 구현해보자."

categories:
  - Javascript
tags:
  - [Javascript, InterSectionObserver, API]

toc: true
toc_sticky: true

date: 2022-06-11
last_modified_at: 2022-06-11
---

이번 포스팅에서는 프론트엔드를 개발하면 한번쯤은 꼭 구현해보고싶은 무한스크롤을 구현해보려고 한다.

무한스크롤을 구현할 수 있는 가장 대표적인 방식은 스크롤 이벤트를 이용하는 것으로 알고 있다. 하지만 이 방식은 스크롤이 발생할때마다 이벤트가 실행되어 웹 성능이 떨어지는 단점이 있어 스로틀링(Throttling)같은 최적화 작업이 늘 동반되어야 했다.

그래서 나는 그러한 단점을 보완할 수 있는 방식인 **Intersection Observer API**를 **사용해 무한스크롤을 구현**해보려 한다.

무한스크롤을 구현해보기전에 많은 양의 정보를 보여주는 UX 방식은 두가지가 있다. 바로 페이지네이션과 무한스크롤이다. 이 둘의 장단점은 무엇일까?

## 무한스크롤 vs 페이지네이션

---

**무한스크롤의 장점**

- 사용자 참여 및 콘텐츠 탐색이 쉽다.
- 클릭보다 더 나은 사용자경험을 제공한다.
- 모바일에 적합하다.

**무한스크롤의 단점**

- 페이지 성능이 느려진다.
- 특정 항목 검색 및 원래 위치로 되돌아오기 힘들다.
- 페이지의 가장하단인 Footer를 찾기 어렵다.

**페이지네이션의 장점**

- 사용자의 의도에 맞게 페이지를 넘길 수 있다.
- 통제감을 느낄 수 있다.
- 특정 항목의 위치를 파악하기 쉽다.

**페이지네이션의 단점**

- 추가적인 작업(다음버튼 클릭)을 필요로한다.
- 한 페이지에 제한된 내용만을 보여줄 수 있다.

⇒ 결론적으로, 어느것이 더 좋은 방식이라고 말할 순 없다. **상황에 따라 적절한 인터페이스를 사용**하면 된다 !!!

## Intersection Observer API를 사용해 무한 스크롤을 구현하기

---

자, 이제 본격적으로 Intersection Observer API를 사용해 무한 스크롤을 구현해보려 한다.

이 API는 **관찰자(observer)**, **관찰대상(entry)**, **조건(option)**, **콜백함수(callback)**이 존재한다. 이 API를 사용하는 순서는 다음과 같다.

1. 관찰자(observer)를 생성한다.
2. 관찰할 대상(entry)을 생성한다.
3. 관찰자(observer)는 관찰대상(entry)을 관찰(observe)한다.
4. 관찰대상이 조건(option)을 만족하는 상태에 놓일때 콜백함수(callback)를 실행한다.

위 순서를 코드로 구현하면 아래와 같이 나타낼 수 있다.

1. **관찰자(observer)를 생성한다.**

```jsx
// 관찰자 생성
const observer = new IntersectionObserver(callback, {threshold: 0.7})
```

생성자 함수의 리턴값은 관찰자(observer)가 된다.

위 코드에서 조건으로 **threshold**를 주었는데 저 조건은 **관찰대상이 얼마나 화면에 들어왔을 때 콜백함수를 호출할지 결정하는 옵션**이다. 기본값은 (0(0%))이고 최대(1(100%))까지 지정할 수 있다.

0.7은 화면에 70% 이상 들어왔을 때 콜백함수를 호출한다는 의미이다.

1. **관찰할 대상(entry)을 생성한다.**

```jsx
// 관측할 대상을 생성
const target = document.querySelector('#target')
```

관찰대상은 하나 이상일 수 있다.

1. **관찰자(observer)는 관찰대상(entry)을 관찰(observe)한다.**

```jsx
// 관찰 시작
observer.observe(target)
```

이제 target이 특정 조건을 만족하게 되는경후 callback 메소드 호출

1. **관찰대상이 조건(option)을 만족하는 상태에 놓일때 콜백함수(callback)를 실행한다.**

```jsx
const callback = (entries, observer) => {
    entries.forEach((entry) => {
        // 교차 관찰자의 루트와 교차하는 경우 -> entry.isIntersecting === true
        if (entry.isIntersecting) {
            // 기존 관찰 대상 관찰 중지
            observer.unobserve(entry.target)
            // 로딩 시작
            loadingStart()
            setTimeout(() => {
                // 콘텐츠 추가 + 새로운 콘텐츠의 마지막 요소를 관찰 시작
                addNewCardItem()
                loadingFinish()
                observeLastItem(observer, document.querySelectorAll('.card'))
            }, 2000)
        }
    })
}
```

이제 관찰 대상이  조건을 만족하게 되는 경우 실행될 callback 메소드까지 구현했다. callback을 제대로 사용하기 위해서는 IntersectionObserver API가 제시하는 방법대로 만들어야한다.

무한 스크롤은 다음 콘텐츠를 가져와 보여주기까지 시간이 조금 걸리기 때문에 로딩중임을 표현할 UI가 필요하다. 이걸 표현할 UI로는 처음 사용해보는 **Skeleton UI**를 선택했다.

**Skeleton UI**는 **실제 데이터가 렌더링 되기 전에 보이게 될 화면의 윤곽을 먼저 그려주는 로딩 애니메이션**이다. 기존 Spinner에 비해 훨씬 사용자 친화적이고, 사용자 이탈율도 실제로 적다고 한다.

다양한 사이트에서 이 애니메이션을 사용하고 있다.


![image](https://user-images.githubusercontent.com/71548623/173182960-64327254-142c-4f96-9bc7-bf0189ec1bd1.png)
유튜브에서 사용중인 Skeleton UI

구현하는 방법은 어렵지 않다. 기존에 들어갈 콘텐츠와 똑같은 UI를 만들어 놓고 로딩중일때 이 Skeleton UI를 보여주고 로딩이 끝나면 지우고 들어가야 할 콘텐츠를 붙여주면 된다.

## 소스 코드

---

```jsx
const makeSkeleton = () => {
    const skeleton = document.createElement('li')
    const skeletonImg = document.createElement('div')
    const skeletonText = document.createElement('p')
    skeleton.classList.add('skeleton')
    skeletonImg.classList.add('skeleton_img')
    skeletonText.classList.add('skeleton_text')
    skeletonText.textContent = ''
    skeleton.appendChild(skeletonImg)
    skeleton.appendChild(skeletonText)
    return skeleton
}

const cards = document.querySelector('.cards')
const cardItems = document.querySelectorAll('.card')
const skeletonItems = Array.from({length: cardItems.length}, () =>
    makeSkeleton()
)

const addSkeleton = () => {
    skeletonItems.forEach((item) => {
        cards.appendChild(item)
    })
}

const removeSkeleton = () => {
    skeletonItems.forEach((item) => {
        cards.removeChild(item)
    })
}

const loadingStart = () => {
    addSkeleton()
}

const loadingFinish = () => {
    removeSkeleton()
}

const addNewCardItem = () => {
    cardItems.forEach((item) => cards.appendChild(item.cloneNode(true)))
}

const callback = (entries, observer) => {
    entries.forEach((entry) => {
        // 교차 관찰자의 루트와 교차하는 경우 -> entry.isIntersecting === true
        if (entry.isIntersecting) {
            // 관찰 타겟 관찰 중지
            observer.unobserve(entry.target)
            // 로딩 시작
            loadingStart()
            setTimeout(() => {
                // 콘텐츠 추가
                addNewCardItem()
                loadingFinish()
                observeLastItem(observer, document.querySelectorAll('.card'))
            }, 2000)
        }
    })
}

// 마지막 아이템을 관찰하도록 하는 함수
const observeLastItem = (observer, items) => {
    const lastItem = items[items.length - 1]
    observer.observe(lastItem)
}

const observer = new IntersectionObserver(callback, {threshold: 0.7})
observeLastItem(observer, cardItems)
```

```css
 /*style.css*/
body {
    background: black;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

h1 {
    margin-top: 20px;
    color: aliceblue;
    text-align: center;
    font-size: 60px;
}

.cards {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    width: 100%;
    height: 100%;
    padding: 20px;
}

.card,
.skeleton {
    border-radius: 5px;
    padding: 20px;
    margin: 10px;
    background-color: aliceblue;
    list-style: none;
}

.card img {
    width: 350px;
    height: 300px;
    object-fit: cover;
}

.card_text,
.skeleton_text {
    text-align: center;
    margin-top: 15px;
    font-size: 18px;
    font-weight: bold;
}

.skeleton_img {
    width: 350px;
    height: 300px;
    animation: skeleton-gradient 1.5s infinite ease-in-out;
}

@keyframes skeleton-gradient {
    0% {
        background-color: rgba(165, 165, 165, 0.1);
    }
    50% {
        background-color: rgba(165, 165, 165, 0.3);
    }
    100% {
        background-color: rgba(165, 165, 165, 0.1);
    }
}
```

```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hongsi Infinite Photo</title>

    <link rel="stylesheet" href="style.css">
</head>
<body>
<h1>Infinite Hongsi Photo</h1>
<ul class="cards">
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010303916.png" alt="홍시">
        <p class="card_text">귀여운 홍시1</p>
    </li>
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010306045.jpg" alt="홍시">
        <p class="card_text">귀여운 홍시2</p>
    </li>
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010306844.jpg" alt="홍시">
        <p class="card_text">귀여운 홍시3</p>
    </li>
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010336784.png" alt="홍시">
        <p class="card_text">귀여운 홍시4</p>
    </li>
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010413647.jpg" alt="홍시">
        <p class="card_text">귀여운 홍시5</p>
    </li>
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010417410.jpg" alt="홍시">
        <p class="card_text">귀여운 홍시6</p>
    </li>
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010423158.jpg" alt="홍시">
        <p class="card_text">귀여운 홍시7</p>
    </li>
    <li class="card">
        <img class="card_img" src="image/KakaoTalk_20220611_010425768.jpg" alt="홍시">
        <p class="card_text">귀여운 홍시8</p>
    </li>
</ul>

<script src="app.js"></script>
</body>
</html>
```

## 완성된 Infinite Hongsi Photo

---

내가 좋아하는 우리집 강아지(홍시) 사진으로 Infinite scroll을 구현해보았다.
![image](https://lh3.googleusercontent.com/H_8PgoX9PqeJLXsy_RIhchpZsylAnIJcRepyiLDNpQVh5CyD6CK4IVjG3M6ZrWzldV5vh2vzFZXHuB9J79XeK2xozoUIQ2WyAGZuuq22HsGPrEAXaofDQH40HytRLn5nYnX9D33GFknBvPbzGOdgLvC_hQgJKhb7A4JCjJi3xrkjaVZNjyZJyp--cluvlZewMPFcxnEAOEEadzur6PpKGfqNKGBqmoKOpSC4ezdKGjudsxo21CP9vAcko9kyOvixo6FW4wVsIgElcG10LQDXX9M1l0SuUvEdYOuDDv5aC1kefVyDIhopVvIt2XbzC7wR3Ym0AKcLfir7yyycyJFYMxYu9k6gGeezG-c6X56TtOC0DUQwpyjR0wZXfx4FRFcWKtpDjx9oohwbI2WcDa5c0eswOYW90jMQlG4GlhAsT70QWqC5CpRp00GrQRAOVbXpAUP4LZArFNJscjL7Q3CPe7GZkJ8RB_fV-ygZjApnH-YP4JJYuh3yF0dn0JQXsywgPgnC7_GeBfh8wIXWNZiXyhwQ3r_K9FRrZn5QLdAOjwD-kVEfcQyBo47X2tyX2A7JffRTEOnxi4w9kpFjTydDGN3K5EMwdeOxV-MvvF5WUdOlL8HGqpmB84TjmSRPgGvg5jtuFd4DiG8c2Of71SqzyLn7q7gN0AkGhuefIsEE9lY2lok8rkrWRodLyMK_TJzdQueWMiBjl9LY9Q9BSAr8p4gsx4XX2R8kkhQJpijNz4XGD4uZsnx7UcinJetkeLi6eiWdUbqDajbM05_OYxXri_z6VX5lXyzLpfc=w328-h155-no?authuser=0)


- 참고자료
  - [[UI/UX] Skeleton UI 적용기](https://velog.io/@tech-hoon/skeleton-ui)
  - [IntersectionObserverAPI로 무한스크롤 구현](https://simian114.gitbook.io/blog/undefined/react/intersectionobserverapi)
  - [Intersection Observer API로 무한 스크롤 구현하기](https://nohack.tistory.com/124)
  - [무한스크롤 vs 페이지네이션](https://slowalk.com/2596)
