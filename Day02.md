# 4/8 Git(깃) & Git hub (깃 허브) 특강 Day2

## 4/7 (목) 복습

- Why?
  - Git 버전관리
    - 백업 - `git add`, `git commit`, `git push`
    - 복구 - `git rest`
    - 협업 - `git clone`, `git pull, git branch`, `git merge`
  
- 사전지식
  - CLI
  - Vscode
  - Markdown ->역할
  
- 명령어
  - `git romote -v` : 현재연결된 링크 보여줌
  
  - `git config --global --list` : user. name, user. ID 확인
  
  - `git remote add origin`[원격 저장소 URL] : 원격 저장소 (깃허브)와 연결
  
    > ~~'origin'은 변경 가능 Only 'git remote add origin' 단계서~~

------------------------------------

> *[github README 참고 링크](https://github.com/matiassingers/awesome-readme)*

### 4/8 본문

### GIT과 GITHUB

#### 1. 꼭만들어야 하는 파일 (시작하자마자 만드는게 용이)

- .gitignore : 특정 파일 버전관리 무시 (ex.올리면 안되는 파일, 쓸데없는 파일)
- README.md : 소개 파일

> [gitignore.io - .gitignore을 위한 사이트](https://www.toptal.com/developers/gitignore) \*쓰는 프로그램을 참고하여 찾을 수 있음

#### 2. GIT ClONE 과 PULL 

>Remote -> Local로 가져오기 위한 명령어
>
>협업 할때 필요
>
>\* 협업자를 초대할 때 새로운 Repository를 생성하고 git init된 폴더를 push한 후  setting에서 collaborators 설정

- clone : Vestion (commits) 다운로드 (git bash here을 통해 `git clone URL`을 입력)
  - 폴더도 가져 온다.
  - git init 불필요
  - commits 받아온다.
  - git remote add 까지 미리 된다.
- pull : Vestion (commits) 업데이트
  - 명령어 : `git pull origin master`
  - commit의 업데이트 상황만 가져온다.
  - git pull은 git fetch, git merge 가 같이 온다.

#### 3. GIT PULL의 비정상적인 흐름

- 새로 만든 로컬 저장소의 버전과 원격 저장소의 버전이 다를 때 로컬저장소에서 'Push'할 시 **충돌 (conflict)** 발생

- 오류가 생길 시 다시 Pull하여 commit을 합쳐 새로운 commit을 생성 후 재 push

  ​		\*그냥 git commit만 작성할 때 vim이라는 프로그램 나온다

  ​			i는 insert 그 후 :wq

#### 4. ★★★ BRANCH, MERGE ★★★  

> *Verson1 : root- commit이라 한다.*
>
> VS code -> Git Graph install

- branch를 직역하면 나뭇가지

- Git의 꽃과 같다. 평행우주와 비유

- master도 branch (상용)

- 명령어

  ``` 명령어 
  $ git branch <브랜치 이름> 
  : 브랜치 만들기
  $ git branch
  : 브랜치 목록조회
  $ git log --oneline --graph --all
  : 로그를 그래프로 보여주기
  $ git switch <브랜치 이름>
  : 브랜치 바꾸기
  $ git branch -D <브랜치 이름>
  : 브랜치 삭제
  $ git merge master
  : 브랜치 병합
  $ git reset --hard <해시 코드> 
  : 커밋 돌아감

- Merge 의 3 방법
  - Fast -forward
    - 앞서나간 갈래를 따라 잡음
  - Auto-merging
    - 서로 다른 버전으로 나눠진 것들이 자연스럽게 합쳐진다.
  - Conflict
    - 서로 다른 버전이 부자연스럽게 충돌하며 합쳐진다.
#### 5. PULL REQUEST

> *원격 저장소 소유권이 없는 경우 (Fork & pull model) ex.회사*

- ①Fork -> ②Clone -> ④push -> **⑤pull request** 

  ex. (github 회사 repositories)->(내 github repositories)->(내 Local 저장소)->(다시 내 github repositories)- ->(github 회사 repositories)

  (1) 개념

  - 오픈 소스 프로젝트와 같이, 자신의 소유가 아닌 원격 저장소의 경우 사용합니다.
  - 원본 원격 저장소를 그대로 내 원격 저장소에 복제합니다. (이 행위를 `fork` 라고 합니다.)
  - 기능 완성 후 `push는 복제한 내 원격 저장소에 진행` 합니다.
  - 이후 `Pull Request`를 통해 원본 원격 저장소에 반영 될 수 있도록 요청합니다.

###### 