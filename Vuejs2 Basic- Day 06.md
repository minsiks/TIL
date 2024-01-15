# 23-1-19 Vuejs 2 기초 Day 06

## [[Vue.js로 게시판 만들기\] 0. 프롤로그 - YouTube](https://www.youtube.com/watch?v=s1lXVr65KZg&list=PLyjjOwsFAe8ITIDUNsU_x4XNbPJeOvs-b)

## [[node.js로 서버만들기\] 2. koa 사용법 - YouTube](https://www.youtube.com/watch?v=Gvx0CANaEdk&list=PLyjjOwsFAe8KCoyjks3aCQAXG3gaWd_s2&index=2)

- node 다운후 index.js를 만들면 npm i koa로 koa 설치후, 서버에 사용가능 

```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);  
```

를 작성하면 `http:127.0.0.1:3000` 포트에 hello world 생성

- nodeMon 설치
  - npm i -g nodemon
  - nodemon --watch src/ src
  - 바로바로 변경사항을 적용

> 마리아 DB 비밀번호 바꾸기 
>
> https://jemmaa.tistory.com/26

### 이슈사항

- --amend
- git merge feature/projectdetail

> - 이미지 선택 사항 데이터 보내기, 현재 이미지 없이 서버에 보내면 오류
> - 수정에서 기존 파일데이터 처리, 3가지 경우의수 현재 기존에 있는 파일 유지하며 수정, 새로운 파일로 수정, 기존 파일 제거 후 수정
> - 로그인 정보 찾아오기

