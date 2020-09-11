---
title:  "django가 뭔지도 모르지만 일단 설치해보자!"
excerpt: "mdn django 문서를 보고 따라함"

categories:
  - python
tags:
  - python
  - 파이썬
  - django
  - 장고
toc: true
toc_sticky: true
toc_label: "index"
toc_icon: "file-alt"
typora-root-url: ../
typora-copy-images-to: ..\assets\img\${filename}
---



내가 까먹을까봐 적는 django set up 순서와 정보

이미 설치 된 것: win10 pro, python 3.7.4

1. 가상환경에서 돌려야 버전충돌 발생시 유연하게 대처가능하다 하여 virtualenvwrapper-win 설치

   - pip : python package index tool,,, npm과 비슷한 건가?

   - python 설치 후, ```pip3 install virtualenvwrapper-win```입력

     

2. 가상환경 ```my_django_environment```생성
   - ```mkvirtualenv my_django_environment```

3. `django` 설치

   - `pip3 install django`

4. `django`설치 확인

   - `py -m django --version` (python 3.7부터는  py -3 이 아닌 py 키워드 사용)
   - 리눅스의 `python3` 명령어 대신 window는 `py -3`를 사용함

5. `django`프로젝트 만들기

   ```bash
   mkdir django_test
   cd django_test
   ```

6. `django` test

   ```bash
   mkdir django_test
   cd django_test
   ```

   ``` bash
   django-admin startproject mytestsite
   cd mytestsite	
   py manage.py runserver
   ```

   127.0.0.1:8000으로 들어가라고는 떴는데, 오류가 떴다.

   `ConnectionAbortedError: [WinError 10053] 현재 연결은 사용자의 호스트 시스템의 소프트웨어의 의해 중단되었습니다`

   혹시 몰라 (현재 핫스팟 usb연결하여 사용하는중) 와이파이를 사용하니까 해결됨.

   웹 브라우저에 `127.0.0.1:8000`으로 접속한 결과

   ![image-20200831202552156](/assets/img/2020-08-31-django-setup/image-20200831202552156.png)

set up 완료!

[장고 튜토리얼](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django) 을 따라가보면서 웹 어플리케이션에 대한 이해를 해보려고 한다...ㅎ