---
title:  "Parcel을 활용한 react setup"
excerpt: "GitHub Blog 서비스인 github.io 블로그 시작하기로 했다."

categories:
  - Blog
tags:
  - Blog
  - Tag
  - Test
last_modified_at: 2020-04-06
---



react project를 설정해줄때--> web pack 사용한다.(Create React App...)

web pack은 모든 것을 설정해줄 수 있지만, 그럴 필요가 없는 작은 프로젝트의 경우에는 간단하게 초기설정을 할 필요가 있다.

## PARCEL

'zero configuration web applilcation bundler'

## feater

- react
- ES6 with Babel
- SCSS(Sass)
- CSS Modules
- Typography.js
- Build for production



Barbel의 역할: .barbelrc 파일을 통하여 최신 JS문법을 사용할 수 있음

PostCSS 역할: .postcssrc 파일을 통하여 최신 CSS문법(SCSS),  ```auto prefixer```, ```CSS modules```를 사용할 수 있음



Typography.js를 활용하여 글꼴 설정을 js에서 해줄 수 있음.