---
title:  "github.io 블로그 시작하기"
excerpt: "GitHub Blog 서비스인 github.io 블로그 시작하기로 했다."

categories:
  - Blog
tags:
  - Blog
  - Tag
  - Test
---



YFM에서 정의한 제목을 이중 괄호 구문으로 본문에 추가할 수 있다.
이 글의 제목은 {{ page.title }}이고
마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.

# TIP
```<{{ site.url }}{{ site.baseurl }}/blog/start-blog/>```
로 링크 지정해줄 수 있다. 나중에 baseurl 변경할때 용이함.
```tzinfo error```: config.yml에서 timezone을 설정했더니 생긴 :[의존성 오류해결하는 법](https://github.com/aron-bordin/neo-hpstr-jekyll-theme/issues/40)



JEKYLL_ENV=production bundle exec jekyll serve로 실행하면 disqus 댓글 활성화시킨 채로 로컬에서 테스트해볼 수 있음!

## reference site

[이 블로그 튜토리얼 보고 따라옴](https://devinlife.com/howto github pages/blog-config/)

[여기 자세히 설명 적혀있으니 커스텀도 가능할 듯](https://junhobaik.github.io/jekyll-apply-theme/#minimal-mistakes)