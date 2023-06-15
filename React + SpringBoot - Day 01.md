# 23-6-12 React + SpringBoot Day 01

> https://www.youtube.com/watch?v=md7AdsH9RCA&list=PLZzruF3-_clvT8YG7aErccPAncirfPWav&index=7

## React 설정

### 생성 및 설정

```
npx create-react-app [my-app(프로젝트이름)]
cd my-app
npm start
```

- package.json 수정
  - scrpits 상단에 프록시 설정

```json
"proxy": "http://localhost:8081",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```

- 모듈 설치

  ```null
  npm install http-proxy-middleware --save
  ```

- setProxy.js 파일 생성

```js
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
  app.use(
    '/api',
    createProxyMiddleware({
      target: 'http://localhost:8080',	# 서버 URL or localhost:설정한포트번호
      changeOrigin: true,
    })
  );
};
```

### 리액트 구조

- build : 빌드 시 생성되는 디렉토리
- node_modules : 리액트 실행에 필요한 모듈 디렉토리
- public : 정적 파일 디렉토리 (ex 이미지, html)
- src : 프로젝트 source 디렉토리 *

### useState()

- 일반 변수 `let num =1;` `num = 2;`

- useState의 선언 방식

```js
let [num, setNum] = useState(1);
setNum(2);
```

- 상단에 정의
- 바로 실행 X
- 반복문 안에 정의 X
- if문 안에 정의 X

```js
import React, {useEffect, useState} from 'react';
import axios from 'axios';
import './App.css';

const RecordFrom = ({numList,setNumList}) =>{

  const [num,setNum] = useState(0);

  return (<div>
    <div>현재 숫자 : {num}</div>
    <button onClick={() => setNum(num+1)}>숫자 증가</button>
    <button onClick={() => setNum(num-1)}>숫자 감소</button>
    <button onClick={() => setNum(0)}>숫자 초기화</button>
    <hr/>
    <button onClick={() => setNumList([...numList, num])}>숫자기록</button>
    <button onClick={() => setNumList([])}>기록 초기화</button>
  </div>)
}
const RecordList = ({numList}) =>{
  return (
    <div>
      <h2>기록된 숫자</h2>
      {numList.length > 0 ? <div>기록 있음</div> : <div>기록 없음</div>}
    </div>
  )
}
const App=() => {
  const [numList, setNumList] = useState([]);
  return(
    <div style={{margin : "40px auto", width : "800px", textAlign : "center"}}>
      <RecordFrom numList={numList} setNumList={setNumList}/>
      <RecordList numList={numList}/>
    </div>
  )
   
}

export default App;
```

