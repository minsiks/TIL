# 2023.04.13 넥사크로N 교육

## 넥사크로N

### NRE

- Nexacro Runtime Enviroment : 설치 필요

### Web

- 설치하지 않고 사용

> 제너레이트 .xmdl -> .js 로 변환

### 화면에 따른 분류

- SDI - 하나의 화면
- MDI - 여러개의 화면으로 분류 (ex. 헤더, 네브바)

### 넥사크로 기능 사용법

- environment_Variables : 로컬 스토리지로 변수가 저장
- Application Variables : 통상적인 변수를 저장
- tool-base Library : Nexacrolib가 위치, 모듈을 모아놓은 폴더
- TypeDefinition
  - Objects : 개발에 필요한 모듈을 모아놓는 곳
  - Services
    - IncludeSub-definition : 서비스에서 서브 폴더를 설정

- 퀵뷰어
  - 만들어놓은 화면 볼 수 있는 기능
  - Ctrl + f6

- Generate Path 설정 : Options - Generate - General
  - 제너레이트된 js파일 위치 폴더
  - 만들어놓은 소스를 Generate한 소스를 담은 path
- Base Path : 기본 소스를 담은 path
- stepcount : 화면을 페이징해서 분할해서 나눔
- maskedit : 지정한 형식이 있는 text Input
- edit : 패스워드 등
- tab을 구성할 때는 URL 방식으로 사용해야만 함
- Grid Expr
  - getRowcount() : Grid에서 행수
  - currow : index값
