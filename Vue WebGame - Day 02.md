# 23-1-26 Vue 웹게임 만들기

## [웹 게임을 만들며 배우는 Vue | 학습 페이지 (inflearn.com)](https://www.inflearn.com/course/web-game-vue/unit/23120?tab=curriculum)

### 숫자야구

- v-for문

```vue
<template>
  <div>
    <form v-on:submit="onSubmitForm">
        <input ref="answer" maxlength="4" v-model="value" />
        <button>입력</button>
    </form>
    <div>시도: {{count}}</div>
    <div>{{strike}} 스트라이크, {{ball}} 볼</div>
        <ul :key="index" v-for="(res,index) in result">
            <li>{{res.value}}</li>
            <div>{{res.strike}} 스트라이크, {{res.ball}} 볼입니다.</div>
        </ul>
  </div>
</template>

<script>
export default {
    data(){
        return{
            result: [
            ],
            count : '',
            number : '',
            strike : 0,
            ball : 0,
            value : '',
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
            const write = {
                strike : this.strike,
                ball : this.ball,
                value : this.value,
            };
            this.result.push(write);
            this.value = "";
            this.$refs.answer.focus();
        }
    },
}
</script>

<style>

</style>
```

## 반응속도체크

- watch, npm i -D webpack-dev-server설치

  - 새로고침까지도 안해도 됨

  - 계속 재빌드를 직접 안해주고 자동으로 빌드 되도록 만드는 기능
  
  - package.json에 추가

```json
{
  "name": "number-baseball",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack --watch" // -watch를 추가,
     "dev": "webpack-dev-server --hot" // dev-server를 추가
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^6.7.3",
    "vue": "^2.7.14",
    "vue-loader": "^15.10.1",
    "vue-style-loader": "^4.1.3",
    "vue-template-compiler": "^2.7.14",
    "webpack": "^5.75.0",
    "webpack-cli": "^5.0.1"
  }
}
```

- webpack.config.js

```js

const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');


module.exports = {
    mode : 'development',
    devtool : 'eval',
    resolve: {
        extensions: ['.js','.vue']
    },
    entry: {
        app: path.join(__dirname, 'main.js'),
    },
    module: {
        rules: [{
            test: /\.vue$/,
            loader: 'vue-loader',
        },{test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader',
        ]}
        ],
    },
    devServer: {  // devServer 추가
        devMiddleware: { publicPath: '/dist' },
        static: { directory: path.resolve(__dirname) },
        hot: true
      },
    plugins: [
        new VueLoaderPlugin(),
    ],
    output : {
        filename: '[name].js',
        path: path.join(__dirname,'dist'),
        publicPath: '/dist' //퍼블릭 패스 추가
    },
}
```

- 반응속도 체크 완성

  - v-vind와 vue style

  - <style scoped> - scoped는 해당 css만 해당 다른컴포넌트에서는 해당 안됨

- computed

  - 일반 데이터를 가공해서 사용시 사용

```vue
<template>
  <div>
    <div id="screen" :class="state" @click="onClickCreen">{{message}}</div>
    <template v-if="result.length">
      <div>평균 시간 : {{average}} ms</div>
      <button @click="onReset">리셋</button>
    </template>
  </div>
</template>

<script>
let startTime = 0;
let endTime = 0;
let timeout = null;
export default {
    data(){
        return{
            state : "waiting",
            message : "클릭해서 시작하세요.",
            result : [],
        }
    },
    computed:{
      average(){
        return this.result.reduce((a,c)=>a+c,0)/ this.result.length || 0;
      }
    },
    methods: {
        onClickCreen(){
          if(this.state ==='waiting'){
            this.state = 'ready';
            this.message = '초록색이 되면 클릭하세요.'
            timeout = setTimeout(()=>{
              this.state = 'now';
              this.message = '지금 클릭!'
              startTime = new Date();
            }, Math.floor(Math.random()*1000)+2000);
          }else if(this.state ==='ready'){
            clearTimeout(timeout);
            this.state = 'waiting';
            this.message = '너무 성급하시군요 ! 초록색이 된 후 클릭하세요.'
          }else if(this.state === 'now'){
            endTime = new Date();
            this.state = 'waiting'
            this.message = '클릭해서 시작하세요.'
            this.result.push(endTime-startTime);
          }
        },
        onReset(){
          this.result = [];
          this.state = "waiting"
          this.message ="클릭해서 시작하세요."
        }
    },
}
</script>

<style scoped>
  #screen {
    width: 300px;
    height: 200px;
    text-align: center;
    user-select: none;
  }
  #screen.waiting{
    background-color : aqua;
  }
  #screen.ready {
    background-color: red;
    color: white;
  }
  #screen.now{
    background-color: greenyellow;
  }
</style>
```

