---
title:  "github page(jekyll blog)에 이미지를 업로드 하는 몇가지 방법 (+ typora)"
excerpt: "로컬이미지/issue를 활용한 방법/ assets에 저장"

categories:
  - IT
tags:
  - github
  - jekyll
  - image
  - typora
toc: true
toc_sticky: true
toc_label: "index"
toc_icon: "file-alt" 
typora-root-url: ../
typora-copy-images-to: ..\assets\img\${filename}
---



자유도가 높은 깃허브 페이지를 블로그로 사용하면서, 이미지 업로드는 참 불편한 점이 많은 것 같다. 따라서 우리가 할 수 있는 몇가지의 방법을 생각해보았다.

나의 상태

- github page(~~.github.io)
- jekyll theme을 사용하여 블로그를 구축하였다.
- typora 에디터를 사용하여 markdown을 작성하고 있다.

## 1. url 이미지

url 이미지는 그냥 복사 붙여넣기만 해도, (권한의 문제만 없다면) 알아서 가져온다. 속도는 떨어지더라도 가장 간편한 방법이다.

따라서 로컬 이미지를 서버에 업로드하여 url만 입력할 방법 몇가지가 있다.

### 구글 드라이브를 이용한 업로드

아주 귀찮지만, 기본적으로 사용할 수 있는 용량이 10기가였나 15기가이고 구글 계정은 여러개 만들 수 있으므로 블로그 데이터 백업용으로 사용해도 좋은 방법인 것 같다.

### github issues를 이용한 업로드

이 방법은 편법이지만, 어쨌든 우리의 로컬 이미지를 url로 받기 편한 방법이다.



이 외에도 이미지를 url로 호스팅해주는 서비스가 있다면 그것을 사용하면 될 것 같다. 어쨋든 나는 이 방법으로 이미지를 올리진 않는다.



## 2. 로컬 이미지(jekyll을 사용하고 있다면)

아마 상당수가 github page를 jekyll로 관리하고 있을 것이다. 지킬에서는 이미지나 소스파일들을 `assets`디렉토리에 올려놓으라고 하는 것 같다. 여기에 img 디렉토리를 만들어서 올려놓아도, 에디터상의 상대경로와 웹 서비스상의 상대경로와 다르기 때문에(ex) 둘 중하나에서는 이미지가 안 나오게 된다. 

### typora를 사용하고 있다면

다른 에디터는 잘 모르지만 typora의 경우, jekyll blog에 이미지를 쉽게 가져다 쓸 수 있도록 하는 방법을 소개하고 있다. (원문은 이하의 참고사이트)

또한 간단한 설정을 통하여 포스트별 폴더를 만들어서 이미지를 관리할 수도 있다.



## 마무리

깃허브 저장소에는 최대 1기가로 용량제한이 있었던 것 같다. 따라서 로컬이미지를 너무 많이 올려도 나중에 구글 드라이브로 옮겨서 업로드해야 할 것이다.ㅎㅎ

이미지를 올리는 것은 귀찮은 것 같다...



## reference site

https://support.typora.io/Images/