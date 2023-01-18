# 23-1-18 Vuejs 2 기초 Day 05

## [한시간만에 끝내는 Vue.js 입문 - YouTube](https://www.youtube.com/watch?v=sqH0u8wN4Rs)

- 컴포넌트 기반의 SPA(Single Page Application)를 구축할 수 있게 해주는 프레임 워크

### 컴포넌트(Componenet)

- 웹을 구성하는 로고, 메뉴바, 버튼, 모달창 등 웹페이지 내의 다양한 UI 요소
- 재사용 가능하도록 구조화

### SPA (Single Page Application)

- 단일 페이지 어플리케이션
- 하나의 페이지 안에서 필요한 영역 부분만 로딩 되는 형태
- 빠른 페이지 변환
- 적은 트래픽 양

## 초기 환경 설정 및 설치

### Vue CLI 설치

### Vue 프로젝트 설치

- npm run serve

- 라우팅 설치

  - npm install vue-router --save

- 부트스트랩 Vue 설치

  ```
  npm install vue bootstrap bootstrap-vue
  ```

- src/router.js

  ```js
  import Vue from "vue";
  import VueRouter from "vue-router";
  import Home from "./views/Home";
  import About from "./views/About";
  
  Vue.use(VueRouter);
  
  // eslint-disable-next-line no-unused-vars
  const router = new VueRouter({
      mode: "history",
      routes: [
          {path:"/",component: Home},
          {path:"/about",component: About},
      ]
  })
  
  export default router;
  ```

- src/main.js

  ```js
  import Vue from 'vue'
  import App from './App.vue'
  import router from './router'
  import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
  
  // Import Bootstrap and BootstrapVue CSS files (order is important)
  import 'bootstrap/dist/css/bootstrap.css'
  import 'bootstrap-vue/dist/bootstrap-vue.css'
  
  // Make BootstrapVue available throughout your project
  Vue.use(BootstrapVue)
  // Optionally install the BootstrapVue icon components plugin
  Vue.use(IconsPlugin)
  
  Vue.config.productionTip = false
  
  new Vue({
    router,
    render: h => h(App),
  }).$mount('#app')
  ```

## Vue Life Cycle

```vue
<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div>
        <h1>Welcome to {{title2}}!</h1>
        <input type="text" v-model="input1">
        <button type="button" @click="getData">Get</button>
        <button type="button" @click="setData">Set</button>
    </div>
</template>
<script>
export default{
    data(){
        return {
            title:"개발자의 품격",
            title2: "Seoul",
            input1: "abc",
        };
    },
    methods:{
        getData(){
            alert(this.input1);
        },
        setData(){
            this.input1="12345";
        }
    },
    beforeCreate(){
        console.log('beforeCreate');
    },
    created(){
        console.log('created');
    },
    beforeMount(){
        console.log('beforeMount');
    },
    mounted(){
        console.log('mounted');
    },
    beforeUpdate(){
        console.log('beforeUpdate');
    },
    updated(){
        console.log('updated');
    },
    beforeDestroy(){
        console.log('beforeDestroy');
    },
    destroyed(){
        console.log('destroyed');
    },
};
</script>
```

## watch

- 특정 데이터를 상시 관찰하고 있다가 그 데이터의 변경이 일어났을 때 감지
  - 동일한 데이터명과 같이 사용

```vue
watch: {
        input1(){
            console.log(this.input1);
        },
        title(){
            
        }
    },
```

## v-if, v-show, v-for, v-model, event(@~)

```vue
<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div>
        <h1>Welcome to {{title2}}!</h1>
        <input type="text" v-model="input1">
        <button type="button" @click="getData">Get</button>
        <button type="button" @click="setData">Set</button>

        <select class="form-control" v-model="region" @change="changeRegion">
            <option :key="i" :value="d.v" v-for="(d,i) in options">{{d.t}}</option>
        </select>

        <table class="table table-bordered" v-if="tableShow">
            <tr :key="i" v-for="(d,i) in options">
                <td>{{d.v}}</td>
                <td>{{d.t}}</td>
            </tr>
        </table>
    </div>
</template>
<script>
export default{
    data(){
        return {
            title:"개발자의 품격",
            title2: "Seoul",
            input1: "abc",
            options: [
                {v:"S",t:"Seoul"},
                {v:"J",t:"Jeju"},
                {v:"B",t:"Busan"},
            ],
            region: "B",
            tableShow:false
        };
    },
    methods:{
        getData(){
            alert(this.input1);
        },
        setData(){
            this.input1="12345";
        },
        changeRegion(){
            alert(this.region);
        }
    },
};
</script>
```

## [[Vue.js로 게시판 만들기\] 0. 프롤로그 - YouTube](https://www.youtube.com/watch?v=s1lXVr65KZg&list=PLyjjOwsFAe8ITIDUNsU_x4XNbPJeOvs-b)



### 이슈사항

- --amend
- git merge feature/projectdetail

> - 이미지 선택 사항 데이터 보내기, 현재 이미지 없이 서버에 보내면 오류
> - 수정에서 기존 파일데이터 처리, 3가지 경우의수 현재 기존에 있는 파일 유지하며 수정, 새로운 파일로 수정, 기존 파일 제거 후 수정

