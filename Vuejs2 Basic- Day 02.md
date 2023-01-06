# 23-1-6 Vuejs 2 기초 Day 02

> [[[뷰js 2 (vuejs 2) 기초 익히기 기본 강좌\] 01 뷰 인스턴스 생성하기! - YouTube](https://www.youtube.com/watch?v=gZBKGn0wQXU&list=PLB7CpjPWqHOtYP7P_0Ls9XNed0NLvmkAh)](https://www.youtube.com/watch?v=XncTU-4i1KI&list=PLG7te9eYUi7tAQygBknaTciy8wzLCe-Ll)

## v-if, v-show

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
        .red{
            color: red;
        }
        .bold{
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- <template v-if="number === 1">
            <div>1</div>
            <div>2</div>
            <div>3</div>
        </template>
        <div v-else-if="number === 2">Hi</div>
        <div v-else>No</div> -->
        <div v-show="show">Yes</div>
        <br>
        <button @click="toggle">Increase</button> {{number}}
    </div>
    <script>
        new Vue({
            el: '#app',
            data:{
                number: 1,
                show: false
            },
            methods:{
                increaseNumber(){
                    this.number++;
                },
                toggle(){
                    this.show = !this.show;
                }
            }
        })
    </script>
</body>
</html>
```

- v-if vs v-show
  - v-if : 토글 비용이 높고, 초기 렌더링에서 조건이 거짓인 경우 아무것도 하지 않습니다.
  - v-show : 훨씬 단순, 초기 렌더링 비용이 더 높습니다.
  - 매우 자주 바꾸기를 원한다면 v-show를, 런타임 시 조건이 바뀌지 않으면 v-if를 권장

## v-for 리스트 렌더링

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
        .red{
            color: red;
        }
        .bold{
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div id="app">
        <div>
            {{people[0].name}} {{ people[0].age}}
        </div>
        <div>
            {{people[1].name}} {{ people[1].age}}
        </div>
        <div>
            {{people[2].name}} {{ people[2].age}}
        </div>
        <div>
            {{people[3].name}} {{ people[3].age}}
        </div>
        <div v-for="(person, index) in people" :key="id">
            {{person.name}} {{ person.age}} {{ index}}
        </div>

    </div>
    <script>
        new Vue({
            el: '#app',
            data:{
                people : [
                    { id:1, name: 'a', age: 20},
                    { id:2, name: 'b', age: 21},
                    { id:3, name: 'c', age: 22},
                    { id:4, name: 'd', age: 23},
                    { id:5, name: 'e', age: 24},
                    { id:6, name: 'e', age: 25},
                ]
            },
            methods:{
                
            }
        })
    </script>
</body>
</html>
```

- id는 여러가지 속성들을 이용해 생성 가능
- 객체도 for문을 사용 가능

## 여러개의 Vue 인스턴스 사용

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
        .red{
            color: red;
        }
        .bold{
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div id="app">
        {{name}}<br>
        <button @click="changeText">Click</button>
    </div>
    <div id="app-1">
        {{name}}<br>
        <button @click="changeText">Click</button>
    </div>
    <script>
        const app = new Vue({
            el: '#app',
            data:{
                name:'kossie'
            },
            methods:{
                changeText(){
                    app1.name='kossie update';
                }
            },
        })
        const app1 = new Vue({
            el: '#app-1',
            data:{
                name:'kossie1'
            },
            methods:{
                changeText(){
                    app.name='kossie1 update';
                }
            }
        })
    </script>
</body>
</html>
```

- 여러개의 인스턴스 생성 가능
- new Vue({})를 변수에 담아 그 변수명을 통해 여러가지 

## Vue 컴포넌트

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
        .red{
            color: red;
        }
        .bold{
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- {{name}}<br>
        <button @click="changeText">Click</button>

        <hr> -->

        <kossie-button></kossie-button>
    </div>
    <hr><hr>
    <div id="app-1">
         <!-- {{name}}<br>
        <button @click="changeText">Click</button>
        <hr> -->

        <kossie-button></kossie-button>
    </div>
    <script>
        // Vue.component('hello-world',{
        //     template: '<div>Hello World</div>'
        // });
        // Vue.component('kossie-button',{
        //     template: `
        //     <div>
        //         {{name}}<br>
        //         <hello-world></hello-world>
        //         <button @click="changeText">Click</button>
        //     </div>`,
        //     data(){
        //         return{
        //             name:'kossie'
        //         }
        //     },
        //     methods:{
        //         changeText(){
        //             this.name='kossie update';
        //         }
        //     },
        // });

        const HelloWorld= {
            template: '<div>Hello World</div>'
        }
        const KossieButton = {
            components:{
                'hello-world': HelloWorld
            },
            template: `
            <div>
                {{name}}<br>
                <hello-world></hello-world>
                <button @click="changeText">Click</button>
            </div>`,
            data(){
                return{
                    name:'kossie'
                }
            },
            methods:{
                changeText(){
                    this.name='kossie update';
                }
            },
        }
        const app = new Vue({
            el: '#app',
            components:{
                'kossie-button' : KossieButton
            }
            // data:{
            //     name:'kossie'
            // },
            // methods:{
            //     changeText(){
            //         this.name='kossie update';
            //     }
            // },
        })
        const app1 = new Vue({
            el: '#app-1',
            components:{
                'kossie-button': KossieButton
            }
        //     // data:{
        //     //     name:'kossie'
        //     // },
        //     // methods:{
        //     //     changeText(){
        //     //         this.name='kossie update';
        //     //     }
        //     // }
        })
    </script>
</body>
</html>
```

- 지역변수로 설정하려면 const와 같은 변수에 담아둔다

## Vue CLI로 뷰 설치하기

- Node.js 설치, `node -v`,` npm-v`로 버전 확인
- `npm install -g @vue/cli`로 vud cli 설치, `vue --version`으로 버전 확인
- `vue create test1`

### Vue CLI 설치하지 않고 뷰 설치

- `npx @vue/cli ceate test2`

## Vue Router

- 싱글페이지 어플리케이션 : 페이지가 하나
  - index.html안에서 모든게 다 해결, component만 바뀜

- router 폴더 내, index.js 등으로 경로 설정 

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import HomeView from '../views/HomeView.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home', // 페이지의 이름
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',  // 페이지의 이름
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

- .Vue에서 router를 사용하고 싶을 때
  - `  <router-link to="/">Home!</router-link>`  : a링크 대신에 경로 routre로 이동

## 싱글 파일 컴포넌트

```vue
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <KossieCoder/>
  </div>
</template>
<script lang="ts">

import KossieCoder from '@/components/KossieCoder.vue';

export default {
  components: {
    KossieCoder
  }
}
</script>
```

- .vue에서 컴포넌트를 사용하려면 import로 컴포넌트를 생성
- `exprot default{}` 안에 해당 컴포넌트 파일명 입력
- `<KossieCoder/>` 와 같은 태그로 사용할 컴포넌트 작성

## 자식 컴포넌트에 데이터 보내기 (Props)

```html
<template>
    <div>
        <h1>{{title}}</h1>
        <p>{{name}}</p>
        <button @click="updatename">Change Name</button>
    </div>
</template>

<script>
export default{
    props:{
        title:{
            type: String,
            // required: false //필수적으로 보내줘야 하는 데이터 확인
            default : 'default title'
        },
        name:{
            type: String,
            default: 'default name'
        },
    },
    data(){
        return{
            // name: 'Kossie Coder'
        }
    },
    methods: {
        updatename(){
            // this.name = 'Kossie Coder Updateed';
        }
    }
}

</script>
```

- 부모컴포넌트에서 `<KossieCoder title="home title" name="go"/>`와 같이 받아서 처리

## 부모 컴포넌트로 데이터 보내기 (Emit)

```vue
<template>
    <div>
        <label for="">Name</label>
        <input 
            type="text"
            :value="value"
            style="padding: 30px; border: 2px solid green"
            @input="$emit('input', $event.target.value);"
        >
    </div>
</template>

<script>
export default ({
    props: {
        value: {
            type: String,
            required: true
        }
    },
    // methods:{
    //     updateName(e){
    //         console.log(e.target.value);
    //         this.$emit('update-name', e.target.value);
    //     }
    // }
})
</script>
```

- 보충 필요

## 슬롯 (Slot)

```html
<template>
    <div>
        <p>header</p>
        <slot name="header" :kossie="kossie"></slot>
        <p>Body</p>
        <slot></slot>
        <p>Footer</p>
    </div>
</template>

<script>
export default{
    props:{
        title:{
            type: String,
            // required: false //필수적으로 보내줘야 하는 데이터 확인
            default : 'default title'
        },
        name:{
            type: String,
            default: 'default name'
        },
    },
    data(){
        return{
            kossie: 'coder'
            // name: 'Kossie Coder'
        }
    },
    methods: {
        updatename(){
            // this.name = 'Kossie Coder Updateed';
        }
    }
}

</script>
```

```html
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <KossieCoder>
      <template #header="{kossie}">
        <p>header11 {{kossie}}</p>
      </template>
      <template #default>
        hello 01
      </template>
    </KossieCoder>
  </div>
</template>
<script lang="ts">

import KossieCoder from '@/components/KossieCoder.vue';

export default {
  components: {
    KossieCoder
  },
  data(){
    return{
      title:5
    }
  }
}
</script>

```

- slot을 통해서 부모 vue에서 쓰인 component안에 값을 작성하면 slot을 통해 그 사이로 끼어넣거나 그 안에 정보도 넣을 수 있다.

## 라이프사이클 다이어그램

![1](C:\Users\김민식\Pictures\1.png)

- 인스턴스가 생성되서 사라질때까지의 주기

```vue
<template>
  <div>
    <h1>This is Home page</h1>
    <!-- <form action=""> -->
      <InputField v-model="name"/>
      <br><button @click="updateName">Submit</button>
    <!-- </form> -->
    <!-- <KossieCoder title="home title" name="go"/> -->
    {{name}}
  </div>
</template>

<script>
import InputField from '@/components/InputField.vue';
export default{
  components:{
    InputField
  },
  data(){
    return{
      name: 'Kossie Coder'
    }
  },

  beforeCreate(){ // 생성 전 메소드
    console.log('beforeCreate',this.name);
  },
  created(){ // 생성
    console.log('created', this.name);
  },
  beforeMount(){ // 돔띄우기 전
    alert('beforeMount');
  },
  mounted(){ // 돔 띄우고
    alert('mounted');
  },
  beforeUpdate(){ // 업데이트 전
    alert('beforeUpdate');
  },
  updated(){ // 업데이트 후
    alert('updated');
  },
  beforeDestroy(){ // 종료 전
    alert('beforeDestroy')
  },
  destroyed(){ //종료 후
    alert('destroyed');
  },
  methods:{
    updateName(){
      this.name = 'hello';
    }
  }
}
</script>

<style scoped>
h1{
  color: red;
}
</style>
```

## Todo App1

```html
<template>
  <div id="app" class="container">
    <h1 class="text-center">Todo App</h1>
    <input
      v-model="todoText" 
      type="text" 
      class="w-100 p-2" 
      placeholder="Type todo"
      @keyup.enter="addTodo"
    >
    <hr>
    <Todo 
      v-for="todo in todos" 
      :key="todo.id" 
      :todo="todo"
      
    />
    
  </div>
  
</template>

<script>
import Todo from '@/components/Todo.vue';

export default {
  components: {
    Todo
  },
  data(){
    return{
      todoText: '',
      todos:[
        {id:1, text:'buy a car', checked:false},
        {id:2, text:'play game', checked:false}, 
      ]
    }
  },
  methods: {
    addTodo(e){
      this.todos.push({
        id: Math.random(),
        text :e.target.value,
        checked: false
      })
      this.todoText = '';
    },
    
  }
}
</script>
```

## Todo App2

- App.vue

```vue
<template>
  <div id="app" class="container">
    <h1 class="text-center">Todo App</h1>
    <input
      v-model="todoText" 
      type="text" 
      class="w-100 p-2" 
      placeholder="Type todo"
      @keyup.enter="addTodo"
    >
    <hr>
    <Todo 
      v-for="todo in todos" 
      :key="todo.id" 
      :todo="todo"
      @toggle-checkbox="toggleCheckbox"
      @click-delete="deleteTodo"
    />
    
  </div>
  
</template>

<script>
import Todo from '@/components/Todo.vue';

export default {
  components: {
    Todo
  },
  data(){
    return{
      todoText: '',
      todos:[
        {id:1, text:'buy a car', checked:false},
        {id:2, text:'play game', checked:false}, 
      ]
    }
  },
  methods: {
    deleteTodo(id){
      // const index = this.todos.findIndex(todo =>{
      //   return todo.id === id;
      // });
      // this.todos.splice(index, 1);
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(e){
      this.todos.push({
        id: Math.random(),
        text :e.target.value,
        checked: false
      })
      this.todoText = '';
    },
    toggleCheckbox({id, checked}){
      const index = this.todos.findIndex(todo =>{
        return todo.id === id;
      });
      this.todos[index].checked = checked;
    }
    
  }
}
</script>
```

- Todo.vue

```vue
<template>
  <div class="mb-2 d-flex">
    <div>
        <input type="checkbox" :checked="todo.checked"
        @change="toggleCheckbox">
    </div>
        
        <span 
        class="ml-3 flex-grow-1"
        :class="todo.checked ? 'text-muted':''"
        :style="todo.checked ? 'text-decoration:line-through':''"
        >
            {{todo.text}}
        </span>
        <button class="btn btn-danger btn-sm flex-shring-1"
        @click="clickDelete">Delete</button>
    </div>
</template>

<script>
export default {
    props:{
        todo:{
            type: Object,
            required: true
        }
    },
    methods: {
        toggleCheckbox(e){
            this.$emit('toggle-checkbox',{
                id: this.todo.id,
                checked: e.target.checked
            })
        },
        clickDelete(){
            this.$emit('click-delete', this.todo.id);
        }
    }
}
</script>
<style>

</style>
```

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

