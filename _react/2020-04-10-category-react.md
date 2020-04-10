---
title:  "처음 시작해보는 react"
excerpt: "복잡한 세팅없이 시작해보는 react.js"
tags:
  - React
  - Javascript
  - Web
  - Front

---



(우선 node, npm, npx가 깔려있어야 한다.)



``` bash
$ npx create-react-app my_app_2020
```

를 통하여 git에 올라와 있는 create-react-app(CRA)를 받아올 수 있다.(name_app은 프로젝트 명으로 하면 될 듯)

![image-20200406174034656](C:/Users/choieastsea/Documents/GitHub/choieastsea.github.io/_posts/2020-04-06-react-start_img/image-20200406174034656.png)

`package.json`파일에서 scripts를 다음과 같이 수정한다.

``` json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  }
```

또한, npm을 이용할 것이므로 `yarn.lock`은 삭제한다.

그러고

```bash
$ npm start
```

해주면 실시간으로 서버를 통하여 리액트 결과를 볼 수 있다.

`component는 HTML DOM을 반환하는 함수라고 볼 수 있다.`

ex) <App /> --> 이러한 표기를 JSX 형식이라고 함

JSX는 HTML처럼 보이지만 js의 일부이기에, class for(label에서 쓰임)과 같은 html 속성은

className, HTMLfor로 바꿔서 사용해야 한다. 왜냐하면 class는 js에서 객체이고 for는 loop이기 때문에 구분이 안되기 때문이지 ㅎ하하



---

map 함수를 활용하여 배열입력 배열 출력

key property로 딱히 할게 없을때, map 함수의 두번째 index parameter를 활용하여 key 값을 주도록 하자.





## class component와 function component의 차이

class: return 존재 x, render 함수 통하여 컴포넌트를 보여줌 / react component를 상속받음

function: return 존재

-> react는 자동으로 class component의 render()를 실행한다.



복잡한 class component 사용하는 이유? state object 사용하기 위해서 (dynamic data를 처리)

render()에서 state 의 field를 사용하는 방법 : ``{this.state.fieldName}` 으로 가져오면 되겠지?

state의 field를 모두 미리 선언해놓지는 않아도 된다. 나중에 쓸 field를 그때 추가해도 되고, 미리 선언해도 된다. 미리 선언해놓으면 편하긴 할듯..?



state를 어떻게 바꿔주는가?

ex) 버튼을 만들어서 클릭 이벤트 발생할때마다 바꿔준다고 가정해보자.

주의) onclick = this.functionName 이 되야함

this.functionName()이 되면 바로 실행된다.

하지만, onClick 시 function으로 state의 field를 직접적으로 건들면 아무것도 바뀌는 것이 없을 것이다.(예시화면) 왜냐하면 render method는 refresh 되지 않기 때문이다. 따라서 우리는 동적인 컴포넌트가 바뀔때마다(state를 바꿔줄때마다) render 함수를 refresh해줘야 한다. 따라서 setState()함수를 사용하여 동적으로 바뀔때마다  render를 refresh해주도록 한다.(react는 똑똑해서 setState를 통하여 바뀌는 컴포넌트에 대하여는 알아서 바뀌는 부분만 찾아서 render를 refresh해준다.)

setState 함수는 컴포넌트의 내용을 바꿔주는 역할을 하는 함수 안에다가 넣어주면 된다.(여기서는 onclick마다 실행되는 함수 안에다가 this.setState({field : value})의 꼴로 필드를 변경해줄 수 있다. 

추가로 주의할 점: react에서는 setState를 해줄때 , 외부의 값에 의존하는 방법은 좋지 않다.(why??) 따라서 current함수를 활용하여 현재의  state 값을 가져오도록 한다.

---

react에는 render 함수 말고도 life-cycle과 관련한 함수를 가지고 있다. 다양한 함수들이 render 이후에도 실행되지만, 일단 몇가지 함수들만 알아두도록 하자.

1. mounting: 생성될때

   constructor()가 실행됨 (생성자--> js에서 class 생성시 호출되는 함수) 이후 render가 실행된다. render가 완료되면 componentDidMount()함수가 실행된다.

2. updating: 컴포넌트의 refresh가 일어날 때(setState 사용)

   이것저것 실행되고 render가 된다. 이후에, compenentDidUpdate()가 실행된다.

3. unmounting: component가 페이지를 떠날때

   componentWillUnmount()가 실행된다.

---

react에서 fetch를 어떻게 할까?

우선 fetch가 뭐고 언제 필요한 것일까

fetch()함수를 사용할 수 있지만, axios(fetch 안에 있는 작은 layer라고 생각하면 편하다)를 활용해본다.

```bash
$ npm i axios
```

axios를 import해주고 사용할 수 있다.

axios.get(url)을 통하여 api에서 제공하는 JSON등을 fetch해올 수 있다. 하지만, axios의 fetch 속도는 느리다. 따라서 axios.get을 사용하는 함수는 async(비동기)이며 await와 같은 메소드로 get해올때까지 기다려야 한다.

> 데이터 fetch를 기다리기 위해서는 async와 await를 해줘야 한다.

+ES6 script 문법을 활용하여

const movies로 JSON을 받아와서 movies.data.data.movies로 가져오는 것 보다는

JSON을 const {data:{data:{moivies}}}로 movies 객체들을 가져올 수도 있다는 것을 참고하면 코드가 이뻐진다.



어쨋든, data를 받아왔으면 setState를 이용하여 state 안에다가 넣어주도록 한다.

---



snippet: 코드 자동 완성기 비슷하다고 보면됨



### react에 대한 궁금증

*export* *default* App; 는 어떤 역할을 하는 것인가?

index.js와 app.js 등의 역할? 이것들은 항상 있어야 CRA가 알아먹고 리액트 프로그램을 시작할 수 있는 것인가?

import는 react 문법인가?







