# 23-1-9 Vuejs 2 기초 Day 03

> [[[뷰js 2 (vuejs 2) 기초 익히기 기본 강좌\] 01 뷰 인스턴스 생성하기! - YouTube](https://www.youtube.com/watch?v=gZBKGn0wQXU&list=PLB7CpjPWqHOtYP7P_0Ls9XNed0NLvmkAh)](https://www.youtube.com/watch?v=XncTU-4i1KI&list=PLG7te9eYUi7tAQygBknaTciy8wzLCe-Ll)

## Vuex 준비 및 설치

` npm i vuex `  vuex 설치

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.user(Vuex);

export default new Vuex.Store({
    state:{

    },
    mutations:{

    },
    actions:{

    },
    getters:{

    }
});
```

- state : 데이터 들어가는 곳
- mutation : 데이터를 실질적으로 바꾸는 곳
- action : 함수가 들어가는 곳, 비동기적 함수를 실행함

- getters : computed와 비슷

## Vuex State

```js
import Vue from 'vue';
import Vuex from 'vuex';


Vue.use(Vuex);

export default new Vuex.Store({
    state:{
        todos:[
            {id:1, text:'buy a car', checked:false},
            {id:2, text:'play game', checked:false}, 
          ]
    },
    mutations:{

    },
    actions:{

    },
    getters:{

    }
});
```

## Vuex Mutations

```js
import Vue from 'vue';
import Vuex from 'vuex';


Vue.use(Vuex);

export default new Vuex.Store({
    state:{
        todos:[
            {id:1, text:'buy a car', checked:false},
            {id:2, text:'play game', checked:false}, 
          ]
    },
    mutations:{
        ADD_TODO(state, value){
            state.todos.push({
                id: Math.random(),
                text :value,
                checked: false
              })
        },
        TOGGLE_TODO(state, {id, checked}){
            const index = state.todos.findIndex(todo =>{
                return todo.id === id;
              });
              state.todos[index].checked = checked;
        },
        DELETE_TODO(state, todoId){
            const index = state.todos.findIndex(todo =>{
                return todo.id === todoId;
              });
              state.todos.splice(index, 1);
        }
    },
    actions:{

    },
    getters:{

    }
});
```

## Vuex actions

```js
import Vue from 'vue';
import Vuex from 'vuex';
import axios from 'axios';

Vue.use(Vuex);

export default new Vuex.Store({
    state:{
        todos:[
            {id:1, text:'buy a car', checked:false},
            {id:2, text:'play game', checked:false}, 
          ],
        users:[]
    },
    mutations:{
        SET_USERS(state, users){
            state.users = users;
        },
        ADD_TODO(state, value){
            state.todos.push({
                id: Math.random(),
                text :value,
                checked: false
              })
        },
        TOGGLE_TODO(state, {id, checked}){
            const index = state.todos.findIndex(todo =>{
                return todo.id === id;
              });
              state.todos[index].checked = checked;
        },
        DELETE_TODO(state, todoId){
            const index = state.todos.findIndex(todo =>{
                return todo.id === todoId;
              });
              state.todos.splice(index, 1);
        }
    },
    actions:{
        getUsers({commit} ){
            axios.get('https://jsonplaceholder.typicode.com/users').then(res =>{
                commit('SET_USERS',res.data);
            });
        },
        addTodo({ commit}, value){
            setTimeout(function(){
                commit('ADD_TODO',value);
            }, 2000);
        },
        toggleTodo({commit}, payload){
            setTimeout(function(){
                commit('TOGGLE_TODO',payload);
            },2000);
        },
        deleteTodo({commit}, todoId){
            setTimeout(function(){
                commit('DELETE_TODO',todoId);
            }, 2000 );
        }
    },
    getters:{

    }
});
```

## Vuex Getters & Map 헬퍼

```vue
<template>
    <div>
        <div v-for="user in people" :key="user.id">
            {{user.name}}
        </div>
    </div>
</template>
<script>

import { mapState, mapActions } from 'vuex';
export default {
    data(){
        return{
        }
    },
    created(){
        this.getUsers();
    },
    computed: {
        ...mapState({people : 'users'}),
        // users(){
        //     return this.$store.state.users;
        // },
        // todos(){
        //     return this.$store.state.todos;
        // }
    },
    methods: {
        ...mapActions(['getUsers'])
        // getUsers(){
        //     this.$store.dispatch('getUsers');
        // }
    }
}
</script>
```

## Vuex Modules

```js
import Vue from 'vue';
import Vuex from 'vuex';
import todo from './modules/todo';
import user from './modules/user';


Vue.use(Vuex);

export default new Vuex.Store({
    state:{

    },
    modules:{
        todo,
        user
    }
});
```

## Vue 게시판 만들기

- HomeView.vue

```vue
<template>
  <el-table : data="articles" style="width: 100%">
      <el-table-column prop="id" label="id" width="120"></el-table-column>
      <el-table-column prop="userId" label="userId" width="120"></el-table-column>
      <el-table-column prop="title" label="title"></el-table-column>
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
      })
      .catch((e)=>{
        console.log(e);
      })
    
  }
}
</script>
```

- main.js

```vue
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import ElementPlus from "element-plus"
import ko from "element-plus/es/locale/lang/ko"

createApp(App)
    .use(store)
    .use(router)
    .use(ElementPlus, {locale: ko})
    .mount('#app')
```

- App.vue

```vue
<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view/>
</template>
<script>
import "element-plus/dist/index.css";

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
```

- board.js

```js
import axios from "axios";

const BASE_URL = "https://jsonplaceholder.typicode.com/";

export default {
    getArticle: function(id){
        return axios.get(BASE_URL + `posts/${id}`);
    },
    getArticles: function(page){
        console.log(page);
        return axios.get(BASE_URL + "posts");
    },
    postArticle: function(userId, title, body){
        return axios.post(
            BASE_URL + "posts",
            {
                userId: userId,
                title: title,
                body:body,
            }
        )
    }
}
```

