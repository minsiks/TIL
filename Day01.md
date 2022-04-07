# 4/7 Git(깃) & Git hub (깃 허브) 특강 Day1

> [깃 특강 링크](https://hphk.notion.site/hphk/Git-22-04-07-22-04-08-AI-14-83024d717d9b41a7b76636858f95a21b)

## 1. Git 이란

- 분산 버전 관리 프로그램

- Why : 백업, 복구 협업

- 리누스 토발즈(Linus Tovalds) 가 개발

- commits : 버전을 의미

- 경로 : Working Directory -> Staging Area (준비) - Commits

  ​                                        (git add)                          (git commit)
> *[깃의 Straging Area는 어떤 점이 유용한가 사이트](https://blog.npcode.com/2012/10/23/git%EC%9D%98-staging-area%EB%8A%94-%EC%96%B4%EB%96%A4-%EC%A0%90%EC%9D%B4-%EC%9C%A0%EC%9A%A9%ED%95%9C%EA%B0%80/)*


- 명령어

  - git config :  환경설정
    - git config --global user.name --~~
    - git config --global user.email ---@~~.com
    - git config --global --list : 확인
  - git init : 깃이 관리하는 폴더로 만들기
  - git status : 상태보기
  - git add @ : staging area로 올리기 (@: 파일명)
  - git commit -m "@" : commit으로 올리기 (m : message 약자, @ : 이유)
  - git log : git 상태보기
  - git remote add origin

## 2. Github 란

- Social Coding
- Why : 포트폴리오 , sns와 비슷

## 3. CLI

- Command Line Interface

#### 경로

- 파일이나 폴더의 고유한 위치를 나타내는 문자열 (주소)
- 운영체제에 따라서 다르게 표현된다.

#### 루트디렉토리

- 모든 파일/폴더를 담고 있는 폴더
- windows의 경우 보통 C  드라이브를 의미함

#### command

- mkdir : 폴더를 만든다
- cd : 폴더를 이동한다
- rm : 파일을 지운다
- rm -r : 폴더 및 등등을 지운다
- touch : 파일을 생성한다
- ls : 폴더 안 리스트를 보여준다
- ls -a : 폴더내 숨겨진 리스트까지 보여준다
- . : 현재폴더
- .. : 상위폴더
- ~ : Home 폴더



## 3. Mark down

> *Mark up (마크 업) : 마크를 통해 각 문구에 역할을 부여*

- 마크업의 한 일종 (경량화된 마크업)

#### 마크다운의 장점

1. 문법이 쉽다
2. 관리가 쉽다
3. 지원 가능한 플랫폼과 프로그램이 다양하다

#### 마크다운의 단점

1. 표준이 없어 사용자마다 문법이 상이할 수 있다.
2. 모든 HTML 마크업을 대신하지 못한다

#### 명령어

- \# 제목1
- \## 제목2
- \### 제목3
- \#### 제목4
- \##### 제목5
- \###### 제목6
- \- 목록1
- tab : 들여쓰기
- Shift tab : 들여쓰기 취소
- 1. 순서가 있는목록
- \*@* : 이탤릭체 (기울임)
- \**@** : 볼드체 (굵게)
- \~~@~~ : 취소선
- \`@@@@` : 코드로 변환 (인라인 코드, 한줄)
- \``` java (or python)  : 블록 코드, 여러줄
  @@@@@@@@@@@
  @@@@@@@@@@
- \[텍스트](링크 or URL or 이미지 경로 or 이미지 등등) : [@] <-(abcde....)
- ctrl+/ : Row, 본문 왔다갔다
- \> : 인용문자
- \ : 마크다운 무시
- \-------------- : 선
