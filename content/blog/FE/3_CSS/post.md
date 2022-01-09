---
title: 'CSS'
date: 2022-01-09 19:44:13
category: 'FE'
draft: false
---

<!-- <p align="center"><img src="1.png" height="300px" width="600px"></p> -->

이번 포스팅은 CSS이다. CSS는 태그들과 기능들이 워낙 많기 때문에 관련 태그들은 필요할 때마다 각 사이트에서 검색해보며 사용하고, 오늘은 CSS의 정의와 문법들을 사용할 때 필요한 개념들에 대해 정리해볼 것이다.

### CSS 공부 관련 검색 사이트

- [TCP School](http://tcpschool.com/css/intro)
- [MDN](https://developer.mozilla.org/ko/docs/Web/CSS)
- [W3School](https://www.w3schools.com/css/default.asp)

# CSS

- CSS는 Cascading Style Sheet의 약자로, 문서의 콘텐츠와 레이아웃, 글꼴 및 시각적 요소들로 표현되는 문서의 외관(디자인)을 분리하기 위한 목적으로 만들어졌다. 즉 HTML 요소들이 각종 미디어에서 어떻게 보이는가를 정의하는데 사용되는 스타일 시트 언어이다. 오늘날 대부분의 웹 브라우저들은 모두 CSS를 지원하고 있다.

- HTML만으로 웹 페이지를 제작 할 경우 HTML요소의 세부 스타일을 일일이 따로 지정해주어야하는 불편함을 해소하기위해 W3C(World Wide Web Consortium)에서 만든 스타일 시트 언어이다.

- 확장자로 .css를 사용한다.

# CSS 문법

<p align="center"><img src="1.png" height="200px" width="600px"></p>
CSS의 문법은 선택자(Selector)와 선언부(declaratives)로 구성된다.

CSS에서 각 선택자에 따라 속성들을 정의할 때 해당 방법을 따르므로 위 사진에 나와있는 대로만 속성을 정의하면 원하는대로 CSS문법에 따라 웹페이지의 스타일을 변경시킬 수 있다.

## 선택자

선택자는 CSS를 적용하고자 하는 HTML 요소(element)를 가리킨다.

- HTML 요소 선택자

  ```css
  <style>
    div {color: yellow;}
  </style>
  ```

  이런식으로 각 요소를 직접 선택하면 된다.

- 아이디(id) 선택자

  ```css
  <style>
    #test {color: yellow;}
  </style>

  <div id="test">이 부분에 스타일을 적용</div>
  ```

  - 이렇게 각 태그에 id가 있을 경우 # 연산자를 사용하여 특정 아이디를 선택할 수 있다.
  - 한 웹페이지에 속하는 요소에는 하나의 id만 사용하여 작업할 수 있다. (그러지 않을 시 JS에서 오류발생)

- 클래스(class) 선택자

  ```css
  <style>
    .test {color: yellow;}
  </style>

  <div class="test">이 부분에 스타일을 적용</div>
  ```

  - 이렇게 각 태그에 class가 있을 경우 . 연산자를 사용하여 특정 아이디를 선택할 수 있다.
  - 클래스는 특정 집단으로 같은 클래스이름을 가지는 요소들을 모두 선택해준다.
    들이 있다.

- 그룹(group) 선택자

  ```css
  <style>
    div, h1 {color: yellow;}
  </style>

  <div>div 태그, 이 부분에 스타일을 적용</div>
  <h1>h1태그, 이 부분에 스타일을 적용</h1>
  ```

  - 이렇게 여러 태그에 , 연산자를 사용하여 각 선택자를 구분하여 사용할 수 있다.
  - 이렇게 , 로 구분된 모든 태그들에 같은 스타일을 적용시킨다.
    들이 있다.

# Reference

CSS : http://tcpschool.com/css/intro
