---
title:  "BootStrap으로 웹사이트 만들어보기"
excerpt: "nav bar~ jumbotron"

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

점보트론

내비게이션 바

외부 스타일 시트

푸터

모달

미디어

웹 사이트 확장

테이블

인용구

리스트 그룹

동영상 불러오기

소스코드 작성하기

폼



# Bootstrap으로 responsive website 만들어보기

체육 시설물 메인 페이지를 만들어보면서 ```부트스트랩```을 익히도록 하자. 처음 배우면서 적는 것이라 오류가 있을 수 있다. md 파일도 아직 낯설어 가독성이 떨어질 수 있다.

## 개발 환경

### 사용한 라이브러리

```bootstrap```

```jQuery```

### 테스트 환경

```Chrome browser```

```Visual Studio Code```



## 1. navigation bar(네비게이션 바)

### 1-1. 구현 목표

PC: 메인 로고, 간단한 메뉴, 메뉴 속 드롭다운

Mobile: 줄 세개 버튼 누르면 드롭다운으로 나오는 메뉴

### 1-2. class 속성 간단 요약

- .navbar-brand : main logo(favicon) 옆의 brand name
- .navbar-nav : 네비게이션 바
- .dropdwon : 드롭다운 메뉴 바
- .navbar-toggler : 토글러(누르면 나오는)와 관련
- .colloapse .navbar-collapse: 모바일에서 메뉴바를 토글러로 숨기는 기능
- .form-inline : 폼 형식
- .navbar-text : 수직 정렬된 문자열

### 1-3. 만들어 보기

#### 1-3-1. Brand&logo

##### brand

```html
<nav class="navbar navbar-light bg-light">
    <a class="navbar-brand h1" href="#">ArenaTron</a>
</nav>
```

우선 nav tag는 .navbar 클래스는 기본적으로 들어간다. 

네비게이션 바의 색에 따라 컨텐츠의 색을 navbar-light (navbar가 light한 색이므로 글씨는 진한 계열이 될 것임), 배경 색을 bg-light로 줄 수 있다. 이름 찾기가 귀찮다면, style 속성으로 줘도 무방함.

.navbar-brand를 주어서 brand를 왼쪽 적당한 위치에 가도록 하고, h1으로 bold하게 글씨를 주었다.



##### 폰트어썸에서 icon 삽입(https://fontawesome.com/icons?d=gallery 참고)

``` <i class="fas fa-volleyball-ball">```icon을 brand 옆에 넣어주었다.



#### 1-3-2. menu 구성& toggler

모바일 화면의 경우, 담을 수 있는 데이터가 적기에 모든 메뉴를 보여줄 필요 없이 드롭다운 버튼을 눌러야 볼 수 있도록 구성할 수 있다.

이를 위해 화면이 작아지면 메뉴는 드롭다운 메뉴 안으로 사라지고 드롭다운을 할 수 있도록 하는 버튼은  나타나야한다.

```html
<nav class="navbar navbar-expand-md navbar-dark" id="navBar">
    <!--Brand-->
    <a class="navbar-brand" href="#">
        <!--Logo-->
        <i class="fas fa-volleyball-ball"></i>
        배구왕
    </a>
    <!--Toggler Button-->
    <button id="navbarButton" class="navbar-toggler" type="button" data-toggle="collapse"
        data-target="#navbarToggle" aria-controls="navbarToggle" aria-expanded="false"
        aria-label="Toggle navigation"><span class="navbar-toggler-icon"></span>
    </button>

    <!--Navbar links-->
    <div class="collapse navbar-collapse" id="navbarToggle">
        <!--오른쪽 정렬 참고:https://stackoverflow.com/questions/19733447/bootstrap-navbar-with-left-center-or-right-aligned-items-->
        <div class="navbar-collapse">
            <ul class="navbar-nav ml-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="#">Home</a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link" href="#">시설물 예약</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">시설물 소개</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">예약 조회</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">예약 방법</a>
                </li>
             </ul>
        </div>
    </div>
</nav>
```

```nav``` 태그에는 ```.navbar-expand-md```를 달아주도록 하여 nav 태그이하의 ```.collapse```된 클래스들을 숨겨주도록 한다. md은 분기가 일어나는 뷰포트 크기로, bootstrap의 ```breakpoints```를 참고하도록 하자. 여기서는 적당한 사이즈 이하로 내려가면 드롭다운 메뉴가 생기도록 하였다.

```button```은 위의 속성을 따라가도록 하고, data-target에 해당되는 영역(id: navbarToggle)을 보여주고 숨기도록 할 수 있도록 한 것이다. ```button``` 이하의 ```span```은 ```.navbar-toggler-icon```을 주면 우리가 아는 세줄의 아이콘이 나오게 된다.

```collapse```되는 영역에는 메뉴 구성을 해준다. ```.nav-item .nav-link```를 통하여 포커스시 변화 효과도 줄 수 있는 듯 하다. ```active```를 통하여 현재 focus된 메뉴를 알 수 있도록 한다.

```.sr-only```클래스는 자식 컨텐츠를 숨기지만, 내용은 존재하게 하는 것으로 구글 검색 결과에는 나오지만 보이지는 않는 그런 어그로성 컨텐츠를 만들 때 사용하는 클래스인 것으로 보인다.(출처: https://rgy0409.tistory.com/3079) 중요하진 않으니 패스



##### nav bar 오른쪽 정렬

```.ml-auto``` 클래스를 이용하여 navbar의 link들을 오른쪽으로 정렬시켜준다.

참고: https://stackoverflow.com/questions/19733447/bootstrap-navbar-with-left-center-or-right-aligned-items

##### nav bar 결과-1(PC/모바일)

![nav1](C:\Users\choieastsea\Documents\typora_md\web\front\nav1.PNG)

![nav2](C:\Users\choieastsea\Documents\typora_md\web\front\nav2.PNG)

#### 1-3-3. toggle dropdown 형식의 sub-menu 달아주기

하지만 시설물 예약이나 소개는 <u>서브 메뉴가 있으면 좋을 것 같다.</u> 

```html
<li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" data-toggle="dropdown"
        aria-haspopup="true" aria-expanded="false">시설물 예약</a>
        <!--Dropdown-->
    <div class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
        <a class="dropdown-item" href="#">김축구장</a>
        <a class="dropdown-item" href="#">이축구장</a>
        <a class="dropdown-item" href="#">박축구장</a>
        <a class="dropdown-item" href="#">최축구장</a>
    </a>
    </div>
</li>
```

원하는 메뉴에 ```.dropdown```을 주고 이하에 ```.dropdown-menu```의 div 태그를 넣어준다. 거기에 서브 메뉴를 넣어주었다.

```toggle``` 형식(클릭 이벤트로 서브 메뉴 보여주기를 지속할 수 있게 함)으로 서브 메뉴를 달아주도록 하자.

```.dropdown``` :  dropdown-menu를 밑에다가 달아준다.

```.dropdown-menu``` : dropdown class 밑에 dropdown-item들을 달아준다.

```dropdown-toggle```: 토글 표시를 달아주어 서브 메뉴로 유도한다.



#### 1-3-4. sticky navigation bar

```<nav>```에```.fixed-top```클래스를 적용시켜줄 수 있다. 그러면 navigation bar가 항상 브라우저 상단에 붙어있게 된다. 

스크롤의 위치에 따라서 navigation bar의 모양을 바꿔주면, 좀 더 동적인 페이지로 만들어 줄 수 있을 것 같다. 이는 js파일에서 ```scroll event```를 처리해주었다.

```javascript
$(document).on("scroll", function(){
    if
  ($(document).scrollTop() > 50){
      $("#navBar").addClass("shrink-nav");
    }
    else
    {
        $("#navBar").removeClass("shrink-nav");
    }
});
```

```css
/*scroll*/
.shrink-nav {
    padding-top: 0;
    padding-bottom: 0;
    background-color: #212529;
}
```

이벤트 발생시 해당 element에 ```.shink-nav```라는 미리 정해 놓은 클래스를 붙여주고, 떼주었다.



##### nav bar 결과-2(PC/모바일)

평소-pc

![nav3](C:\Users\choieastsea\Documents\typora_md\web\front\nav3.PNG)

스크롤시-pc

![nav4](C:\Users\choieastsea\Documents\typora_md\web\front\nav4.PNG)

모바일 화면

![nav5](C:\Users\choieastsea\Documents\typora_md\web\front\nav5.PNG)

## 2. 점보트론



## 참고

https://getbootstrap.com/docs/4.0/components