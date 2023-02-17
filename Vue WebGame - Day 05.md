# 23-2-17 Vue 웹게임 만들기 Day05

## [웹 게임을 만들며 배우는 Vue | 학습 페이지 (inflearn.com)](https://www.inflearn.com/course/web-game-vue/unit/23120?tab=curriculum)

### Vue Router

- 라우터 설정
  - npm  -i vue-router
  - routes.js

```js
import Vue from 'vue';
import VueRouter from 'vue-router';
import NumberBaseball from '../3.숫자야구/NumberBaseball';
import NumberBaseball from '../4.반응속도체크/ResponseCheck';
import NumberBaseball from '../5.가위바위보/RockScissorsPaper';
import NumberBaseball from '../6.로또/LottoGenerator';

Vue.use(VueRouter);

export default new VueRouter({
    routes:[
        {path:'/number-baseball',component: NumberBaseball },
        {path:'/reponse-check', component : ResponseCheck},
        {path:'/rock-scissors-paper', component : RockScissorsPaper},
        {path:'/lotto-generator', component : LottoGenerator},
    ]
});
```

- Router.vue
  - router-view : 경로에 맞는 화면 출력 부분
  - router-link : a링크로 경로를 설정

```vue
<template>
  <div>
      <div>
        <router-link to="/number-baseball">숫자야구</router-link>
        <router-link to="/response-check">반응속도</router-link>
        <router-link to="/rock-scissors-paper">가위바위보</router-link>
        <router-link to="/lotto-generator">로또생성기</router-link>
      </div>
      <router-view></router-view>
  </div>
</template>

<script>
import router from './routes';

export default {
    router,
}
</script>

<style>
  table{
    border-collapse: collapse;
  }
  td,th{
    border: 1px solid black;
    width: 40px;
    height: 40px;
    text-align: center;
  }
</style>
```

> 해시라우터 : 새로고침할때 좋지만 , 검색엔진엔 마이너스가 있기에 쓰지않음

### 동적라우팅 매칭

- this.$router : vue라우터 전체에대한 정보
- this.$route : 현재 라우트에 대한 정보

- routes.js 변경

```js
import Vue from 'vue';
import VueRouter from 'vue-router';
import NumberBaseball from '../3.숫자야구/NumberBaseball';
import ResponseCheck from '../4.반응속도체크/ResponseCheck';
import RockScissorsPaper from '../5.가위바위보/RockScissorsPaper';
import LottoGenerator from '../6.로또/LottoGenerator';
import GameMatcher from './GameMatcher';

Vue.use(VueRouter);

export default new VueRouter({
    mode:'history',
    routes:[
        {path:'/number-baseball',component: NumberBaseball },
        {path:'/reponse-check', component : ResponseCheck},
        {path:'/rock-scissors-paper', component : RockScissorsPaper},
        {path:'/lotto-generator', component : LottoGenerator},
        {path:'/game/:name', component : GameMatcher}, // 동적 라우팅/game/number-baseball
    ]
});
```

- GameMatcher.vue

```vue
<template>
    <div v-if="currentGame === 'number-baseball'">
        <number-baseball></number-baseball>
    </div>
    <div v-else-if="currentGame === 'response-check'">
        <response-check></response-check>
    </div>
    <div v-else-if="currentGame === 'lotto-generator'">
        <lotto-generator>
        </lotto-generator>
    </div>
    <div v-else>일치하는 게임이 없습니다.</div>
</template>
<script>
import NumberBaseball from '../3.숫자야구/NumberBaseball.vue';
import ResponseCheck from '../4.반응속도체크/ResponseCheck.vue';
import LottoGenerator from '../6.로또/LottoGenerator.vue';
    export default{
  components: { NumberBaseball, LottoGenerator,ResponseCheck },
        mounted(){

            console.log(this.$router);
            console.log(this.$route);
        },
        computed :{
            currentGame(){
                return this.$route.params.name;
            }
        }
    }
</script>
```

### 주소 쿼리스트링

- https://www.www.www/ddd?id=111
