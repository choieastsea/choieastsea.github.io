---
title:  "나의 첫 BootStrap 사용기"
excerpt: "처음으로 bootstrap 을 사용하면서 테스트 해보고 겪어 보는 문제점들을 다뤄보았다."

categories:
  - IT
  - Front
tags:
  - BootStrap
  - 부트스트랩
toc: true
toc_sticky: true
toc_label: "index"
toc_icon: "index" 
---

# 나의 첫 BootStrap(부트스트랩) 사용기

처음으로 bootstrap 을 사용하면서 테스트 해보고 겪어 보는 문제점들을 다뤄보았다.

## 0. 프롤로그

### 나의 현재 상태

- html/css/js(vanilla, jquery)정도의 간단한 프론트엔드의 구성을 알고 어느정도 따라할 수 있다. (학교에서 웹프로그래밍 수강)
- css 속성 중 display와 box 의 가장 기본적인 속성의 이해가 간단하게 되있는 상태이다. flexbox나 grid에 대한 이해는 아직 부족하다.
- 반응형 웹사이트(responsive web)를 만들기 위해서 @media 쿼리를 사용해 본 적이 있다.
- vs code에서 모든 파일을 관리하며, 로컬의 html 파일을 크롬으로 실행해본다. 이때, 원하는 결과가 나오지 않으면, f12를 눌러서 요소들을 확인하고, 속성이 적용되었나의 여부를 판단할 수 있다.
- 이것저것 css 속성을 넣다 보면, 속성이 겹쳐서 원하지 않는 결과가 나올때가 있고, 상대~절대 크기를 혼용하여 원하는 행과 열의 배치가 나오지 않을 때가 종종 있다.
- 이에 효율적이고 간단하게 class 속성으로 관리할 수 있는 방법이 ```bootstrap```이라는 말을 듣고 찾아보게 되었다.

### 이 문서의 목표

- 부트스트랩으로 쓰여진 html을 어느정도 이해할 수 있다.
- 심플한 반응형 웹사이트를 만들 수 있다.
- 부트스트랩의 그리드 시스템을 이해하고 조작할 수 있다.



## 1. bootstrap 이란?

순수 css를 활용하여 웹 사이트를 꾸미면, PC와 동시에 모바일을 지원하는 반응형 웹사이트를 구현하기가 힘들다. 이에 ```모바일 우선```, ```반응형 웹```에 최적화된 부트스트랩이라는 프론트 엔드-라이브러리가 나왔다.

##### 장점: 코드의 재사용성

##### 단점: 일관화된 페이지, 심플하지만 재미가 없을 수도 있음



## 2. 부트스트랩 다운/링크 방법
웹 브라우저는 우리가 알다시피, ```html```, ```css```, 그리고 ```js``` 파일만 알아먹는다. 따라서 부트 스트랩을 만드시는 분들은 기존의 css와 js 파일을 활용하여 부트스트랩을 만들어 놓았다. 우리는 그 개념을 이해하고 잘 사용하기만 하면 되는 것이다!

대표적으로 이하의 방법으로 부트 스트랩을 나의 웹사이트에 적용시킬 수 있다. 다른 방법은  https://getbootstrap.com를 참고하자.

#####  1. 부트 스트랩 다운로드
web site, npm 등으로도 부트스트랩 라이브러리를 다운받을 수 있다. 다운받아놓도록 하자. 나는 ``` npm install bootstrap ```으로 간단하게 설치한 후 사용하였다.

##### 2. CDN 사용 
다운받기가 귀찮다면, 적용시키려는 HTML파일의 <HEAD>태그 안에 다음의 내용을 추가하도록 하자. 

```HTML
<!-- 합쳐지고 최소화된 최신 CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">

<!-- 부가적인 테마 -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap-theme.min.css">

<!-- 합쳐지고 최소화된 최신 자바스크립트 -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/js/bootstrap.min.js"></script>
출처: bootstrapk.com
```

아, 추가로 부트스트랩은 모바일 우선이므로

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

이 코드가 포함되어 있어야 한다. 이에 대해서는  [여기에서 살펴보자.]: /html코드앞부분파헤치기.md



##### 3. 부트스트랩 파일 구조

js/css 파일 등이 있다. 상황에 따라서 가볍게 <u>필요한 파일만 사용</u>해도 되는 듯 싶다. 일단 나는 ```bootstrap.min.js```파일과 ```bootstrap.min.css```을 <title> 태그 영역에 포함시켰다.

부트스트랩의 js 파일을 추가할 거라면 jquery를 사용하기에 <u>jquery를 우선</u> 포함시켜줘야 할 것이다.

상황에 따라서 뭘 추가할지 모르겠다면 https://getbootstrap.com/docs/4.4/getting-started/contents/

## 3. 부트 스트랩의 기본 컨셉

##### 1. 반응형/ breakpoints(구간)

반응형 웹사이트를 순수 css로 구현해본 적이 있다면 ```@media``` 에 대하여 알 것이다. 화면 스케일에 따라서 적당한 css 속성을 주는 것으로 생각할 수 있다.

모바일 우선인 부트스트랩은 media query구간(breakpoint)을

- extra small(0px~) : defualt 모바일
- small(576px~) : 가로 모드
- medium(768px~) :  태블릿
- large(992px~) : 데스크탑
- extra large(1200px~) : 큰 데스크탑

정도로 나누는 듯 하다.

좀 알아보니, 이 구간에 대한 이해가 좀 필요할 것 같다. 부트스트랩 클래스의 이름은 ```.className-scale-number```의 꼴을 많이 띄는 것을 볼 수 있는데, 각각 breakpoints또는 grid 개수(전체를 12등분했을때)가 들어가는 경향이 있다. 뒤에서 자세히 알아보자.

##### 2. 클래스 기반의 스타일 속성

부트스트랩은 스타일 속성을 ```class```로 주는 것으로 보인다. 따라서 우리는 적당한 <div>에다가 클래스를 여러 개 줌으로써 스타일을 입힐 수 있는 것 같다.

가장 기본적인 두 클래스를 알아보자.

1. ```.container``` (fixed width container)

   반응형 고정폭을 갖는 레이아웃(responsive fixed layout)을 만들 때 사용한다고 한다. 같은 media query 구간에서는 화면 너비가 늘거나 줄어도 고정폭이며, 구간이 바뀔때마다 바뀐다. 그리드 시스템을 이용한다면 container class를 사용하면 될 것같다.

2. ```.container-fluid``` (full width container)

   유동 최대폭을 갖는 레이아웃(fluid layout)을 만들 때 사용한다고 한다. 언제나 꽉차게 블럭을 차지하게 된다.

   이 두 클래스 사이에는 ```container-sm``` ```container-md ``` , ```container-lg```,  ```container-xl```의 순서대로 화면에 맞춰서 쓸 수 있다.

   | Extra small <576px | Small ≥576px | Medium ≥768px | Large ≥992px | Extra large ≥1200px |        |
   | ------------------ | ------------ | ------------- | ------------ | ------------------- | ------ |
   | `.container`       | 100%         | 540px         | 720px        | 960px               | 1140px |
   | `.container-sm`    | 100%         | 540px         | 720px        | 960px               | 1140px |
   | `.container-md`    | 100%         | 100%          | 720px        | 960px               | 1140px |
   | `.container-lg`    | 100%         | 100%          | 100%         | 960px               | 1140px |
   | `.container-xl`    | 100%         | 100%          | 100%         | 100%                | 1140px |
   | `.container-fluid` | 100%         | 100%          | 100%         | 100%                | 100%   |

##### 3. 그리드 시스템(Grid System)

> Bootstrap’s grid system uses a series of containers, rows, and columns to layout and align content. It’s built with flexbox and is fully responsive.

라고 bootstrap documentation에 나와있다. 난 ```flexbox```는 아직 잘 모르지만, 페이지를 행렬로 보면서 정렬할 수 있도록 grid를 반응형으로 구현했다라는 말 같다.

```행렬```을 모른다면 grid system을 이해하기 힘들 수 있지만, 간단하다. 가로줄이 행, 세로줄이 열이다. 한 행에는 여러 열이 들어갈 수 있다. bootstrap에서도 ```.row```안에 여러개의 ```col```이 들어갈 수 있다. 한 row의 길이는 12칸으로 보자.

```col```의 form은 다음과 같이 나올 것이다.

*: integer from 1 to 12

``` .col-*``` : 항상 수평 배치

```.col-sm-*```: small까지는 수평 배치(small보다 작아지면 stack 배치)

``` .col-md-*```: medium까지는 수평 배치

``` .col-lg-*```: large까지는 수평 배치

``` .col-xl-*```: xl까지는 수평 배치(large이하로는 stack 배치)

뒤에 붙는 숫자는 12까지의 자연수가 올 수 있다. 가로의 길이가 (가운데 인자인) scale이하가 된다면 숫자는 (stack 배치가 되므로) 의미 없이 고정폭을 갖게 되며, scale보다 큰 뷰포트라면 n/12만큼의 가로 길이를 차지하게 된다.

예를 들어보자.

```html
<div class="container-fluid">
    <div class="row">
        <div class="col-sm-5">
            col-sm-5
        </div>
        <div class="col-sm-4">
            col-sm-4
        </div>
    </div>
</div>
```

```viewport```가 small size까지는 수평으로 5/12, 4/12 만큼 차지할 것이다. 하지만 576px 밑으로 간다면(extra small) 2행 1열(stack)의 구조를 갖게 될 것이다.

화면의 가로 길이가 작을수록, stack으로 배치를 해야하므로, 상황에 따라 맞는 클래스를 사용해야 한다.

###### 만약 한 행의 숫자가 12를 넘어선다면?

```html
<div class="container-fluid">
    <div class="row">
        <div class="col-sm-9">
            col-sm-9
        </div>
        <div class="col-sm-9">
            col-sm-9
        </div>
    </div>
</div>
```

크기에 상관없이 stack구조로 될 것이다.

###### 빈 공간을 주고 싶다면?

```.col-scale-offset-number``` 클래스를 통하여 빈 공간을 줄 수 있다. 이로써 div의 구성을 좀 더 자유롭게 할 수 있을 것이다.

###### 클래스 여러개를 써준다면?





###### 중첩구조가 가능한가?

col 안에 row 또는 col을 넣어줌으로써 중첩으로 사용이 가능하다.


##### 4. z-index

다양한 컴포넌트에 적당히 z-index를 주어 충돌이 생기지 않도록 제공해준다.

```scss
$zindex-dropdown:          1000 !default;
$zindex-sticky:            1020 !default;
$zindex-fixed:             1030 !default;
$zindex-modal-backdrop:    1040 !default;
$zindex-modal:             1050 !default;
$zindex-popover:           1060 !default;
$zindex-tooltip:           1070 !default;
```

기능에 맞게 zindex 클래스를 줄 수 있을 것 같다.

## 참고

박스 사이즈를 정하는 방법 (box-sizing)

https://www.codingfactory.net/10630

## 참고 사이트

https://poiemaweb.com/bootstrap-grid-system

https://getbootstrap.com/

http://bootstrapk.com/