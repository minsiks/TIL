# 23-8-29 React 웹게임 만들기

## [웹 게임을 만들며 배우는 React | 학습 페이지 (inflearn.com)](https://www.inflearn.com/course/lecture?courseSlug=web-game-react&unitId=21557&tab=curriculum)

### 리액트 문법

```js
<html>
<head>
        <meta charset="utf-8">
        <title>웹게임</title>
</head>
<body>
<div id="root"></div>
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<!-- <script src="https://unpkg.com/react@16/umd/react.production.min.js" crossorigin></script> -->
<!-- <script src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js" crossorigin></script> -->
<script>
    'use strict';

    class LikeButton extends React.Component{
        constructor(props){
            super(props);
            this.state = {liked: false};
        };
        render(){
            if(this.state.liked){
                return 'You liked this.';
            }
            return React.createElement('button',{onClick: ()=> this.setState({liked : true})},'Like');
        }
    } // ErrorBoundary만 사용
</script>
<script>
    ReactDOM.render(React.createElement(LikeButton), document.querySelector('#root'))
</script>
</body>
</html>
```

### 가독성을 위한 JSX(XML임)

```jsx
<html>
<head>
        <meta charset="utf-8">
        <title>웹게임</title>
</head>
<body>
<div id="root"></div>
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<!-- <script src="https://unpkg.com/react@16/umd/react.production.min.js" crossorigin></script> -->
<!-- <script src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js" crossorigin></script> -->
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script> 
<script type="text/babel">
    'use strict';

    class LikeButton extends React.Component{
        constructor(props){
            super(props);
            this.state = {liked: false};
        };
        render(){
            if(this.state.liked){
                return 'You liked this.';
            }
            // return React.createElement('button',{onClick: ()=> this.setState({liked : true})},'Like');
            return (
                <button onClick={()=> this.setState({liked: true})}>
                    Like
                </button>
            );
        }
    } // ErrorBoundary만 사용
</script>
<script type="text/babel">
    // ReactDOM.render(<LikeButton/>, document.querySelector('#root')); Reat 17버전 코드
    ReactDOM.createRoot(document.querySelector('#root')).render(<LikeButton />); // React 18버전 코드
</script>
</body>
</html>
```

### 함수 컴포넌트

```js
<html>
<head>
        <meta charset="utf-8">
        <title>웹게임</title>
</head>
<body>
<div id="root"></div>
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<!-- <script src="https://unpkg.com/react@16/umd/react.production.min.js" crossorigin></script> -->
<!-- <script src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js" crossorigin></script> -->
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script> 
<script type="text/babel">
    'use strict';

    const LikeButton = () => { // 함수형 컴포넌트 X 함수 컴포넌트 O
        const [liked, setLiked] = React.useState(false); // 구조분해
        // 결국 return 한게 화면
        if(liked) {
            return 'You liked this.';
        }
        return(
            <button onClick={()=>{setLiked(true);}}>Like</button>
        );
    }
    
</script>
<script type="text/babel">
    // ReactDOM.render(<LikeButton/>, document.querySelector('#root')); //Reat 17버전 코드
    ReactDOM.createRoot(document.querySelector('#root')).render(<LikeButton />); // React 18버전 코드
</script>
</body>
</html>
```

