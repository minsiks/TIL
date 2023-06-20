# 23-6-15 React + SpringBoot Day 02

> https://www.youtube.com/watch?v=md7AdsH9RCA&list=PLZzruF3-_clvT8YG7aErccPAncirfPWav&index=7

## React 설정

## SpringBoot

### CORS에러 발생

- @CrossOrigin(origins = "http://localhost:3000") 
  - 컨트롤러에 해당 어노테이션 넣어서 해결

### 로그인 횟수 제한



### React.js 가 아닌 Next.js

- Next.js : 서버 사이트 렌더링, 정적 웹 페이지 생성 등 리액트 기반 웹 애플리케이션 기능들을 가능케 하는 Node.js 위에서 빌드된 오픈 소스 웹 개발 프레임 워크
- Routing 설정하려다 발견
- _app.js
  - props로 모든 다른 페이지들을 Component로 가져올 수 있음
  - 로직, 전역 스타일 등 컴포넌트에 공통 데이터를 다룬다
- _document는 공통적으로 적용할 HTML 마크업을 중심으로 다룬다.

- React Next 공통으로 사용하는 기능
  - useCallback
    - 특정 함수를 새로 만들지 않고 재사용하고 싶을 때 사용

### Next.js

- Router
  - router.push와 router.replace차이
  - 
