# 23-1-26 Vue 웹게임 만들기

## [웹 게임을 만들며 배우는 Vue | 학습 페이지 (inflearn.com)](https://www.inflearn.com/course/web-game-vue/unit/23120?tab=curriculum)

### 구구단

- vue2 선언시 html에 script문 선언

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```

- v-if와 v-else
  - 인접해있지 않다면 실행 안됨

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>좋아요</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- <script src="https://unpkg.com/vue@3"></script> -->
</head>
<body>
    <div id="root">
        <div v-if="liked" @click="onClickButton">좋아요 눌렀음</div>
        <button v-else @click="onClickButton">Like</button>
    </div>
</body>
<script>
    const app = new Vue({
        el: '#root',
        data: {
            liked: false,
        },
        methods: {
            onClickButton(){
                this.liked = this.liked?false:true;
            },
        },
    });
</script>
</html>
```

- 구구단 화면 완성하기

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>구구단</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- <script src="https://unpkg.com/vue@3"></script> -->
</head>
<body>
    <div id="root">
        <div>{{first}} 곱하기 {{second}}는?</div>
        <input v-model="answer" type="number" ref="answer"/>
        <button @click="gugudan">입력</button>
        <div>{{result}}</div>
    </div>
    <script>
        const app =new Vue({
            el: "#root",
            data: {
                first : Math.ceil(Math.random()*9),
                second : Math.ceil(Math.random()*9),
                answer : '',
                result : '',
            },
            methods: {
                gugudan(){
                    this.result = (this.answer==this.first*this.second)?'정답':'오답';
                    if(this.result=='정답'){
                        this.first = Math.ceil(Math.random()*9);
                        this.second = Math.ceil(Math.random()*9);
                    }
                    this.answer='';
                    this.$refs.answer.focus();
                }
            },
        })
    </script>
</body>
</html>
```

- -ref
  - 태그에 직접 접근해야할때 사용

### 끝말잇기

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>끝말잇기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- <script src="https://unpkg.com/vue@3"></script> -->
</head>
<body>
    <div id="root">
        <div>{{word}}</div>
        <input ref="answer" type="text" v-model="answer"/><button @click="checkWord">입력!</button>
        <div>{{result}}</div>
    </div>
</body>
<script>
    const app = new Vue({
        el: '#root',
        data: {
            word : '초밥',
            answer : '',
            result : '',
        },
        methods: {
            checkWord(){
                this.result = this.word[this.word.length -1]==this.answer[0]?'정답':'땡';
                if(this.result =='정답'){
                    this.word=this.answer;
                }
                this.answer='';
                this.$refs.answer.focus();
            }
            
        },
    });
</script>
</html>
```

### 컴포넌트

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>끝말잇기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- <script src="https://unpkg.com/vue@3"></script> -->
</head>
<body>
    <div id="root">
        <word-relay start-word="초밥"></word-relay>
        <word-relay start-word="천재"></word-relay>
        <word-relay start-word="바보"></word-relay>
    </div>
</body>
<script>
    Vue.component('word-relay',{
        template:`
        <div>
            <div>{{word}}</div>
            <input ref="answer" type="text" v-model="answer"/>
            <button @click="checkWord">입력!</button>
            <div>{{result}}</div>
        </div>
            `,
        props: ['startWord'],
        data(){
            return{
                word : this.startWord,
                answer : '',
                result : '',
            }
        } ,
        methods : {
            checkWord(){
                this.result = this.word[this.word.length -1]==this.answer[0]?'정답':'땡';
                if(this.result =='정답'){
                    this.word=this.answer;
                }
                this.answer='';
                this.$refs.answer.focus();
            }
        }
    });
</script>
<script>
    const app = new Vue({
        el: '#root',
        data: {
        },
        methods: {
        },
    });
</script>
</html>
```

### 웹팩

- node.js 설치

- npm init
- npm i vue
- npm i webpack webpack-cli -D
- webpack.config.js 파일 생성
  - 기본 구조

```js
module.exports = {
    entry: {
        app: './main.js',
    },
    module: {
        rules: [{
            
        }],
    },
    plugins: [],
    output : {
        filename: '[name].js',
        path: './dist'
    },
}
```

- main.js 파일 생성

```js
import Vue from 'vue';

new Vue().$mount('#root');
```

- dist 폴더 생성

- NumverBaseball.vue 파일 생성

```vue
<template>
  <div>
    <h1>{{result}}</h1>
    <form v-on:submit="onSubmitForm">
        <input ref="answer" maxlength="4" v-model="value" />
        <button>입력</button>
    </form>
    <div>시도: {{}}</div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            value: '',
            result: '',
        }
    },
    methods: {
        onSubmitForm(e){
            e.preventDefault();
        }
    },
}
</script>

<style>

</style>
```

- package.json 파일 변경

```json
  "scripts": {
    "build": "webpack"
  }, // 로 변경
```

- webpack.config.js 패스 설정, vueLoaderPlugin 설정

```JS
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const path = require('path');

module.exports = {
    entry: {
        app: path.join(__dirname, 'main.js'),
    },
    module: {
        rules: [{
            test: /\.vue$/,
            loader: 'vue-loader',
        }],
    },
    plugins: [
        new VueLoaderPlugin(),
    ],
    output : {
        filename: '[name].js',
        path: path.join(__dirname,'dist'),
    },
}
```

- main.js import 추가

```js
import Vue from 'vue';
import NumberBaseball from './NumberBaseball.vue'; // 추가

new Vue().$mount('#root');
```

### 숫자야구

```vue
<template>
  <div>
    <h1>{{result}}</h1>
    <form v-on:submit="onSubmitForm">
        <input ref="answer" maxlength="4" v-model="value" />
        <button>입력</button>
    </form>
    <div>시도: {{count}}</div>
    <div>{{strike}} 스트라이크, {{ball}} 볼</div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            value: '',
            result: '',
            count : '',
            number : '',
            strike : 0,
            ball : 0,
        }
    },
    mounted() {
        const arr = [0,1,2,3,4,5,6,7,8,9];
        let str = '';
        for(let i=0; i<4; i++){
            let randomNmbr = Math.ceil(Math.random()*(arr.length-1));
            console.log(randomNmbr);
            str+=arr[randomNmbr];
            arr.splice(randomNmbr,1);
            console.log(str);
            console.log(arr.toString());
        }
        this.number = str;
    },
    methods: {
        onSubmitForm(e){
            this.strike=0;
            this.ball=0;
            console.log(`답은 ${this.number}`);
            e.preventDefault();
            this.count++;
            this.value = this.value.toString();
            for(let i=0; i<4; i++){
                for(let j=0; j<4; j++){
                    if(this.number[i] == this.value[j]){
                        if(i==j){
                            this.strike++;
                            continue;
                        }else{
                            console.log(this.number[i],this.value[j])
                            this.ball++;
                            continue;
                        }
                    }
                }
            }
            
        }
    },
}
</script>

<style>

</style>
```

