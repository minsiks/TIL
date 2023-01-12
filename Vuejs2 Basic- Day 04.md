# 23-1-10 Vuejs 2 기초 Day 04

> 궁금한점
>
> - 수행프로젝트목록 GET으로 아무값없이 호출하면, 모든수행 프로젝트 목록이 호출되는지
> - 두개의 차이
>
> ```javascript
> getStartDate(month){
>       let now = new Date();
>       let todayYear = now.getFullYear();
>       let todayMonth = now.getMonth()+1-month;
>       let todayDate = now.getDate();
>       if(todayMonth==0){
>         todayMonth=12;
>         todayYear= todayYear-1;
>       }
>       return todayYear + '-' + (('00'+todayMonth.toString()).slice(-2)) + '-' + (('00'+todayDate.toString()).slice(-2));
>     },
>     searchProjectList: function(){
>       
>     }
> ```

> - 페이지 여부, 오프셋 값, 리미트 값

## 게시판 만들기

### 게시판 상세보기

- HomeView.vue

```vue
<template>
  <div class="list_tbl">
        <table  style="width: 100%" @row-click="rowClicked">
          <caption>프로젝트 목록</caption>
          <tbody v-for="article in articles" v-bind:key="article.id" @click="detail(article.id)">
            <tr>
              <th scope="row" prop="id" label="id">{{article.id}}</th>
              <td prop="userId" label="userId">{{article.userId}}</td>
              <td prop="title" label="title"> {{article.title}}
              </td>
            </tr>
            <tr class="space">
              <td colspan="6" />
            </tr>
          </tbody>
        </table>
      </div>
</template>

<script>
import apiBoard from '@/api/board';

export default{
  data(){
    return{
      title: "hi!",
      articles: null,
    }
  },
  mounted(){
    apiBoard.getArticles()
      .then((response)=>{
        console.log("getArticles",response);
        this.articles = response.data;
      })
      .catch((e)=>{
        console.log(e);
      })
    
  },
  methods:{
    detail(row){
      this.$router.push({
        path: `/board/detail/${row}`
      });
    }
  }
}
</script>
```

- DetailView.vue

```vue
<template>
    <div>
        <p>{{article.id}}</p>
        <p>{{article.userId}}</p>
        <p>{{article.title}}</p>
        <p>{{article.body}}</p>
    </div>
    <br>
    <button @click="goBack">Back</button>
</template>

<script>
import apiBoard from '@/api/board'

export default{
    data(){
        return{
            article : "",
        }
    },
    mounted(){
        apiBoard.getArticle(this.$route.params.id)
            .then((response) => {
                this.article = response.data;
            })
            .catch((e)=>{
                console.log(e);
            })
    },
    methods: {
        goBack(){
            this.$router.go(-1);
        }
    }
}
</script>
```

- router/index.js

```js
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import DetailView from '../views/DetailView.vue'

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/board/detail/:id',
    name: 'detail',
    component : DetailView
  }

]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

## 게시물 작성하기

- HoeView에 추가

```vue
<a href="/board/write">연필모양</a>
```

- index.js에 경로 추가

```js
import WriteView from '../views/WriteView.vue'
const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/board/detail/:id',
    name: 'detail',
    component : DetailView
  },
  {
    path: "/board/write",
    name: "write",
    component : WriteView
  }
```

- WriteView.vue

```vue
<template>
    <div>
        <input type="text" v-model="title">
        <br>
        <textarea rows="10" v-model="body"></textarea>
    </div>
    <br>
    <button @click="writeArticle">저장</button>
</template>

<script>
import apiBoard from "@/api/board";

export default{
    data(){
        return{
            title:"",
            body:"",
        }
    },
    methods:{
        writeArticle(){
            if(!this.title || !this.body){
                this.$message.error("제목과 본문을 작성해주세요.");
                return;
            }

            apiBoard
                .postArticle(0, this.title, this.body)
                .then((response)=>{
                    console.log(response);
                    this.$router.push({path:"/"});
                })
                .catch((e)=>{
                    console.log(e);
                    this.$message.error("게시물 작성 중 에러가 발생하였습니다.");
                });
        }
    }
}
</script>

```

> 궁금한점
>
> - 어바웃 히스토리
> - .zip파일 압축 파일 등록가능
> - file 보내는 것, file을 file채로 axios로 보내는지 name만 보내는지.
> - 이미지 조정 해석이 해당 조건의 미만의 이미지를 늘려서 넣는건지, 미만의 이미지를 제한을 두고 alert창 발생을 시키는지
> - 로그인 키값
> - 수행 프로젝트 상세조회에 등록자 이름, 등록일시 ?

날짜 정보를 확인해주세요

[개발일기 :: [Eclipse\] import 시 자바버전 맞지 않을때 (tistory.com)](https://dnrdl2001.tistory.com/3)

- 
