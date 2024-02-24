---
layout: single
title: "VSCode Git 연동"
categories: VSCode
tag: false 
toc: true
author_profile: false
sidebar:
  nav: "docs"
toc: true
--- 

> VSCode를 Git에 연동해보자. <br>
<br>

> 먼저 Visual Studio Code와 GitHub 계정이 필요하다. 

<div style="border: 1px solid #4CAF50; padding: 10px; background-color: #f2f2f2;">
  <a href="https://code.visualstudio.com/">Visual Studio Code</a>
</div><br>

<div style="border: 1px solid #4CAF50; padding: 10px; background-color: #f2f2f2;">
  <a href="https://github.com/">Github</a>
</div><br>

> 먼저 Git을 설치해 주자 
<div style="border: 1px solid #4CAF50; padding: 10px; background-color: #f2f2f2;">
  <a href="https://git-scm.com/downloads">Git</a>
</div><br>

<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/70f88215-67d2-43ae-ad50-d0f753a9480e"><br>

> Windows Explorer integration - 'Git Bash Here'을 선택하면, Windows 탐색기 우클릭 컨텍스트 메뉴에 'Git Bash here' 옵션이 생긴다. <br>
> 노란 체크박스를 클릭 시 bash창을 따로 열지 않고 CMD 창에서도 작업이 가능하다. 

<br>
<img width="1200" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/9de8d296-d872-4aae-b268-03eb7cd8c87d"><br>

> 깃 연동에는 방법이 많지만 먼저 Git에 연동을 해놨다면 CMD 창으로 PUSH가 가능하다. 
        cd blog

        git add .

        git commit -m "change theme"

        git push --set-upstream origin main

        git push
        
        git config user.name (github 이름)
        git config user.email (github 이메일)

<br>

<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/0a1771d9-b1ff-4751-880c-8e127dab657e"><br>

> Visual Studio Code에서 좌측 Source Control 클릭. Git의 모든 기능 커밋,푸쉬 동기화 기능은 요기서 가능하다. 

<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/271dd537-a863-42e7-bac7-09acd4a0d2b3"><br>

> 먼저 업로드 할 프로젝트의 폴더를 열고 소스 제어에서 Github 게시를 선택, Github 로그인은 필수 이며 상단에서 전체(Public), 비공개(Private)를 선택 후 게시할 수 있다. 

<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/dd5720b0-3624-4960-9f06-433feebf92a5"><br>

> 새로운 파일, 추가 변경 사항이 있으면 목록에 표시가 되고 +를 눌러 목록에 올린다. 이 후 커밋 버튼을 눌러 인덱스 목록에 파일만 커밋한다. 

<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/966b748d-540c-4748-b7aa-6372fcfa3719"><br>

> 커밋 진행 시 ID와 Email을 입력해달라는 문구가 나올 시 

        git config user.name (github 이름)
        git config user.email (github 이메일)

<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/a52b6b87-341f-4885-8180-0900b3668633"><br>

> 커밋 버튼을 누르면 COMMIT_EDITMSG 창이 열린다. 커밋 할 파일 앞에 붙은 #을 지우면 파일이 커밋이 된다.



<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/e4500618-a0bb-46ab-9838-4a176b93e0c7"><br>

> 소스 제어 창의 더보기(...)을 눌러서 푸시를 선택, 깃허브에서 확인해보자


<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/cd797b7c-6349-4861-8032-ff8a6c5424c5"><br>

> repository를 가져올 때는 혹은 다른 소스를 가져오고 싶을 때 Git Clone을 통해 가져올 수 있다.<br>
> F1을 누르고 Git Clone 선택 후 로컬로 가져오고 싶은 repository를 선택


<br>
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/e7be7ab2-3f00-4e86-b667-e1be9ed75c37"><br>

> 위치 선택 후 로컬 저장소에 저장하면 git 폴더가 생성된 것을 확인할 수 있다. 


*참고자료(사진) 등*
<div style="border: 1px solid #4CAF50; padding: 10px; background-color: #f2f2f2;">
  <a href="https://velog.io/@ahnsanghyeon/VSCode%EC%97%90%EC%84%9C-Git-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0">참고자료</a>
</div><br>