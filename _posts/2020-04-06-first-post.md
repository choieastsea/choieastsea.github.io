---
title:  "github.io 블로그 간단 설명"
excerpt: "jekyll을 사용한 블로그를 시작해보자!"

categories:
  - Blog
tags:
  - Blog
  - Tag
  - Test
toc: true
toc_sticky: true
toc_label: "index"
toc_icon: "file-alt"
---

## 시작하기

~.github.io 의 도메인명을 자주 보았을 것이다. 이는 깃허브에서 제공하는 정적 호스팅 서비스를 이용하여 만든 사이트로, `깃허브 페이지`혹은 블로그라고 한다. 나는 사이트의 모든 프론트 요소들을 관리할 능력이 없기 때문에 `jekyll` 테마를 사용할 것이다. 우리는 지킬을 통하여 github pages의 컨텐츠를 `markdown`과 간단한 설정들로 만들어줄 수 있다.

### 준비물(사전지식)

- ruby 설치(jekyll과 관련된 패키지를 사용하기 위해 필요함)
- markdown 문법(github page에 markdown으로 글을 작성할 것이므로)
- github 계정(github page를 만들려면 당연히 필요함)
- git bash(깃, linux와 관련된 명령어 사용하기 편함)

## 큰 흐름

깃허브 페이지에서 ~~.github.io의 도메인을 만들기 위해서는 `github`에 `{사용자이름}.github.io`라는 이름으로 저장소(repository)를 만들어준다. 이러면 해당 페이지에 접속할 수 있다. 아직은 아무것도 없으며, 처음부터 html, css, js를 통하여 만들어줄 수도 있겠지만 너무 귀찮으니 jekyll theme을 사용할 것이다.

이후, `jekyll`을 다운받아준다. 이를 사용하기 위해서는 `ruby gem`이 필요하다.

jekyll themes을 구글링하여 마음에 드는 테마를 선택한다. 나는 [minimal-mistakes](https://mmistakes.github.io/)를 사용하였다. 이를 깃허브 저장소로 가져오면 된다.(clone or download) 테마에서 어떻게 커스터마이징하는지는 해당 테마의 scss파일을 직접 수정하거나, 제공해주는 다양한 방법을 읽어보도록 하자.

_config.yml 파일을 통해 블로그의 제목이나 디폴트 속성 등에 대하여 설정을 해줄 수 있다. 테마 사이트를 참고하면 더 자세한 정보를 얻을 수 있을 것이다.



## 글(post) 작성 방법

글은 _posts라는 디렉토리에 넣어주도록 하자. (없다면 만들어주도록 한다) 글은 마크다운으로 작성되고, 파일명은 `YYYY-MM-DD-title.md`과 같은 형식으로 작성해주도록 한다.(내가 작성하고 있는 파일명은 2020-04-06-first-post.md 이다)여기의 title은 포스트의 제목과 반드시 같을 필요는 없는 것 같다. 이제 포스트의 내용을 작성해보도록 하자.



## YFM 사용방법

포스트는 YFM(YALM Front Matter)로 시작한다. typora editor에서는 `---(enter)`를 해주면 바로 만들어 준다.  포스트의 정보와 세팅을 적어주는 것이라고 볼 수 있다. (언젠가 꼭 사용하게 될 config.yml에서 선언한 설정 위에 오버라이드해줄 수도 있다.)

```markdown
---
title:  "github.io 블로그 간단 설명"
excerpt: "jekyll을 사용한 블로그를 시작해보자!"

categories:
  - Blog
tags:
  - Blog
  - Tag
  - Test
toc: true
toc_sticky: true
toc_label: "index"
toc_icon: "file-alt"
---
```

현재 포스트의 YFM은 위와 같다. 제목, 카테고리와 태그, 미리 보이는 소제목 등을 정할 수 있다. toc은 뷰포트에 따라 블로그 상단 혹은 우측에 보이는 인덱스 바라고 생각하면 좋을 것이다. 위와 같은 내용 말고도 최근 수정 일자 등 다양한 설정을 config.yml에 독립적으로 설정해줄 수 있다.

YFM에서 선언한 내용을 포스트에서도 사용할 수 있는데, 이 글의 제목은 `{{page.title}}`로 가져올 수 있으며, {{ page.title }}와 같이 사용할 수 있다. YFM이하의 내용은 마크다운 문법을 사용하여 작성할 수 있다.

마크다운 문법은 [여기를 참고한다.](../../it/how-to-md/)



## 사진 첨부 방법

이미지 파일을 첨부하는 것이 좀 까다로운 것 같다. 우리는 이미지 태그를 사용할때, 보통 이미지를 웹사이트의 url 또는 상대경로로 첨부한다. url은 알아서 가져와주니까 편하지만, 상대경로로 이미지를 첨부한 경우에는 보여지지 않을 수도 있다.

이에 대한 방법은 [여기에서 다루도록 하겠다.](../../it/how-to-img-git/)



## page와 collection을 만들어 주는 방법

404(pages not found) 혹은 네비게이션바를 눌렀을 때 나타나는 다양한 레이아웃의 페이지를 만들어 줄 수도 있다.

_pages 디렉토리에 넣어주어 포스트들을 보여주는 페이지를 만들어 줄 수도 있다. 내 블로그 상단의 About, Categories, Tags page가 그 예이다.

하나의 컬렉션을 만들어 _{collection name}디렉토리에 만들어주어서 컬렉션페이지를 보여줄 수도 있다. 블로그 상단의 Collections가 그 예이다.



## Test

github에 push하기 전에 테스트를 해보는 것이 좋다. 터미널에서 `jekyll serve`를 해주면 미리볼 수 있는 로컬 서버가 생성될 것이다.

만약, disqus 댓글을 활성화 해 놓은 상태라면, 위의 명령어로는 미리보기가 지원되지 않을 수 있다. 굳이 보고 싶다면, ``JEKYLL_ENV=production bundle exec jekyll serve`를 통하여 disqus 댓글을 활성화시킨 채로 테스트해볼 수 있다.



## TIP

```<{{ site.url }}{{ site.baseurl }}/blog/start-blog/>```
로 링크 지정해줄 수 있다. 나중에 baseurl 변경할때 설정을 다 바꿔주지 않아도 된다.
```tzinfo error```: config.yml에서 timezone을 설정했더니 생긴 [의존성 오류 해결하는 법](https://github.com/aron-bordin/neo-hpstr-jekyll-theme/issues/40)

## reference site

[이 블로그](https://devinlife.com/howto github pages/blog-config/) 튜토리얼이 매우 잘 되어있으니 참고하면 금방 만들 수 있을 것이다.

