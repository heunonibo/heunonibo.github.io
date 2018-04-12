---
layout: post
title:  "Git Bash 사용법"
date:   2018-04-09 10:47:00 +0900
categories: jekyll update
---

#### 초기입력

1. Git Bash 설치
<br />
2. Git Bash 터미널에서 해당 폴더로 이동

```
$cd ..        // 폴더 뒤로가기
$cd d:        // 로컬 디스크로 이동하려면 c: 경로에서 cd d: 입력
$cd test      // test 폴더로 이동 (올릴 폴더 위치로 이동)
```

여기 부터는 터미널 입력
<br />
3. git init
+ igit 로컬 저장소로 이용하는 초기 설정 명령어. 해당 명령어를 입력하면 해당 폴더에 .git 폴더가 생성된다.
<br />
4. git status
+ 현재 상태를 확인 할 수 있다. 수정 후에도 git status 명령어를 입력하면 수정한 리스트들이 붉은 글자로 출력된다.
<br />
5. git add .  
+ git add . 혹은 git add 올릴 폴더 및 파일 을 입력한다. (.)을 입력 시 모든 파일 및 폴더가 업로드 된다.
<br />
6. git commit -m '커밋 내용'
<br />
7. git remote add origin 저장소 주소.git
<br />
8. git remote -v
<br />
9. git push origin master

<br />

#### 브랜치 생성 후 생성된 브랜치로 체크아웃 후 git 에 올리는 방법
<br />
1. git branch 브랜치생성이름
<br />
2. git branch
+ 해당 명령어를 입력하면 저장소 branch 가 출력되고 현재 잡고 있는 브랜치 명에 초록색으로 표시된다.
<br />
3. git checkout 생성한브랜치이름
+ 브랜치 변경하는 명령어
<br />
4. git status
+ 수정한 리스트 출력됨
<br />
5. git add .
<br />
6. git commit -m '커밋 내용'
<br />
7. git remote add origin 저장소 주소.git
<br />
8. git remote -v
<br />
9. git push origin 생성한브랜치이름
