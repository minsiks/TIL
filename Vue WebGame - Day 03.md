# 23-1-30 Vue 웹게임 만들기 Day03

## [웹 게임을 만들며 배우는 Vue | 학습 페이지 (inflearn.com)](https://www.inflearn.com/course/web-game-vue/unit/23120?tab=curriculum)

### 가위바위보

- :class(v-bind:class)와 :style은 {}를 사용해서 객체형으로 사용 가능
  - ex `<div :class="{ state: true, hello: false }" :style="{ backgroundImage: "", fontSize: '14px'}"></div>`
- 개인적으로 구현한 가위바위보

```vue
<template>
  <div>
    <div id="computer" :style="{ background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${imgCoord}px 0` }"></div>
    <div>
      <button @click="onClickButton('바위')">바위</button>
      <button @click="onClickButton('가위')">가위</button>
      <button @click="onClickButton('보')">보</button>
    </div>
    <div>{{result}}</div>
    <div>현재 {{score}}점</div>
  </div>
</template>

<script>

let intervalId = '';
export default {
    data(){
        return{
          result:'',
          score:0,
          imgCoord: 0,
        }
    },
    computed:{
      
    },
    created() {
      intervalId = setInterval(this.changeImg,50);
    },
    methods: {
      onClickButton(choice){
        clearInterval(intervalId);
        if(choice=='바위'){
          if(this.imgCoord==0){
            this.result="비김"
          }else if(this.imgCoord==-140){
            this.result="이김"
          }else{
            this.result="짐"
          }
        }else if(choice=='가위'){
          if(this.imgCoord==0){
            this.result="짐"
          }else if(this.imgCoord==-140){
            this.result="비김"
          }else{
            this.result="이김"
          }
        }else{
          if(this.imgCoord==0){
            this.result="이김"
          }else if(this.imgCoord==-140){
            this.result="짐"
          }else{
            this.result="비김"
          }
        }
      },
      changeImg(){
        if(this.imgCoord==-280){
          this.imgCoord=0;
        }else{
          this.imgCoord -= 140;
        }
      }
    },
}
</script>

<style scoped>
 #computer{
  width: 142px;
  height: 200px;
  background-position: 0 0;
 }
</style>
```

- 강사 코드

```vue
<template>
  <div>
    <div id="computer" :style="computedStyleObject"></div>
    <div>
      <button @click="onClickButton('바위')">바위</button>
      <button @click="onClickButton('가위')">가위</button>
      <button @click="onClickButton('보')">보</button>
    </div>
    <div>{{result}}</div>
    <div>현재 {{score}}점</div>
  </div>
</template>

<script>
const rspCoords = {
  바위 : '0',
  가위 : '-142px',
  보 : '-284px'
}
const scores = {
  가위: 1,
  바위: 0,
  보: -1,
};

const computerChoice = (imgCoord) =>{
  return Object.entries(rspCoords).find(function (v){
    return v[1] === imgCoord;
  })[0];
}
let interval ='';
export default {
    data(){
        return{
          result:'',
          score:0,
          imgCoord: rspCoords.바위,
        }
    },
    computed:{
      computedStyleObject(){
        return{
          background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`,
        }
      }
    },
    methods: {
      changeHand(){
        interval = setInterval(()=>{
        if (this.imgCoord == rspCoords.바위){
          this.imgCoord = rspCoords.가위;
        }else if(this.imgCoord === rspCoords.가위){
          this.imgCoord = rspCoords.보;
        }else if(this.imgCoord === rspCoords.보){
          this.imgCoord = rspCoords.바위;
        }
      }, 100);
      },
      onClickButton(choice){
        clearInterval(interval);
        const myScore = scores[choice];
        const cpuScore = scores[computerChoice(this.imgCoord)];
        const diff = myScore - cpuScore;
        if (diff ===0){
          this.result = '비겼습니다.';
        } else if ([-1, 2].includes(diff)){
          this.result = '이겼습니다.';
          this.score += 1;
        }else{
          this.result='졌습니다.'
          this.score -= -1;
        }
        setTimeout(()=>{
          this.changeHand();
        }, 1000);
      },
    },
    created(){
      console.log('created');
    },
    mounted() {
      this.changeHand();
    },
    updated() {
      console.log('updated');
    },
    beforeDestroy() {
      console.log('beforeDestroy');
      clearInterval(interval);
    },

}
</script>
<style scoped>
 #computer{
  width: 142px;
  height: 200px;
  background-position: 0 0;
 }
</style>
```

> 다른점 (개선점)
>
> 1. 불분명한  px을 배열로 정리하여 가시성이 좋고, 사용성이 좋게 설계
> 2. 바인딩된 :style에 데이터를 제외하고도 추가적인 문자열 등이 포함 시, computed를 사용, 캐싱을 자동으로 해주기 때문
> 3. 메모리누수를 차단하기 위해 onclick함수에서만 setInterval을 멈춰주는것이 아니라, beforDestroy()에서도 화면이 없어지기전에 clearInterval을 통해 멈춰준다.
>
> * 셋타임아웃 혹은 셋인터벌 사용시 beforDestroy,혹은 destroy에서 클리어를 해주지 않으면 메모리누수의 가능성이 있다.

- 라이프사이클
  - created: 처음에 컴포넌트가 보여지긴하지만 화면에 나타나기전
  - mounted: 처음에 컴포넌트가 보여지고 화면도 나타나는차이
  - updated: 데이터가 변경될 시
  - destroyed : 화면에서 사라질 때

### 로또 추첨기

#### 개인 예습(실습)

- LottoGenerator.vue

```vue
<template>
  <div>
    <span 
      v-for="(lotto,index) in balls" 
      :key="index">
      <LottoBall :lotto="lotto" ></LottoBall>
    </span>
    <div v-if="bonus!='?'">보너스 : <LottoBall :lotto="bonus"></LottoBall></div>
  </div>
</template>

<script>
let interval = '';
import LottoBall from './LottoBall';
export default {
    components : {
      LottoBall
    },
    data(){
        return{
          balls:[],
          bonus: '?',
        }
    },
    computed:{
    },
    methods: {
      
    },
    mounted() {
      interval = setInterval(()=>{
        var num = Math.floor(Math.random()*44)+1;
        for(var j in this.balls){
          while(num == this.balls[j]){
            num = Math.floor(Math.random()* 44)+1;
          }
        }
        this.balls.push(num);
        
      },500)
    },
    updated() {
      if(this.balls.length==6&&this.bonus=="?"){
        clearInterval(interval);
        var num = Math.floor(Math.random()*44)+1;
        for(var j in this.balls){
          while(num == this.balls[j]){
            num = Math.floor(Math.random()* 44)+1;
          }
        }
        setTimeout(() => {
          this.bonus=num;
        }, 500);
      }
    },
    beforeDestroy() {
      clearInterval(interval);
    },

}
</script>

<style scoped>

</style>
```

- LottoBall.vue

```vue
<template>
    <span
      class="balls" :style="computedStyleObject">
      {{lotto}}
    </span>
</template>
<script>
export default {
   name: 'LottoBall',
   components: {
     
   },
   mixins: [],
   props: {
     lotto: [String,Number]
   },
   data() {
     return {
      color:"",
     }  
   },
   computed: {
     computedStyleObject(){
      return{
          backgroundColor:this.color
        }
     }
   },
   watch: {
     
   },
   created() {
    console.log(this.lotto);
    if(this.lotto<10){
      this.color='red';
    }else if(this.lotto<20){
      this.color='orange';
    }else if(this.lotto<30){
      this.color='green';
    }else if(this.lotto<40){
      this.color='blue';
    }else{
      this.color='yellow';
    }
   },
   methods: {
     
   }
};
</script>
<style scoped>
  .balls{
    
    display: inline-block;
    text-align: center;
    font-weight: bolder;
    line-height: 50px;
    margin: 10px;
    border-radius: 50%;
    width : 50px;
    height: 50px;
    border: solid 2px black;
  }
</style>
```

#### 강사 예

- LottoGenerator.vue

```vue
<template>
  <div>
    <div>당첨 숫자</div>
    <div id="결과창">
      <lotto-ball v-for="ball in winBalls" :key="ball" :number="ball"></lotto-ball>
    </div>
    <div >보너스</div>
    <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
    <button v-if="redo" @click="onClickRedo">한 번 더!</button>
  </div>
</template>

<script>
let interval = '';
function getWinNumbers(){
  console.log('getWinNumbers');
  const candidate = Array(45).fill().map((v,i)=>i+1);
  const shuffle = [];
  while (candidate.length>0) {
    shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length),1)[0]);
  }
  const bonusNumber = shuffle[shuffle.length-1];
  const winNumbers = shuffle.slice(0,6).sort((p,c)=> p-c);
  return [...winNumbers, bonusNumber];
}
import LottoBall from './LottoBall';

export default {
    components : {
      LottoBall
    },
    data(){
        return{
          winNumbers : getWinNumbers(),
          winBalls:[],
          redo: false,
          bonus: null,
        }
    },
    computed:{
    },
    methods: {
      onClickRedo(){
        this.winNumbers = getWinNumbers();
        this.winBalls=[];
        this.bonus = null;
        this.redo=false
      },
      showBalls(){
        for(let i = 0; i < this.winNumbers.length - 1; i++){
        setTimeout(()=>{
          this.winBalls.push(this.winNumbers[i]);
        }, (i+1)*1000)
        }
        setTimeout(()=>{
          this.bonus= this.winNumbers[6];
          this.redo=true;
        },7000)
      }
    },
    mounted() {
      this.showBalls();
    },
    beforeDestroy() {
      
    },
    watch:{
      winBalls(value,oldValue){
        if(value.length===0){
          console.log(value,oldValue);
          this.showBalls();
        }
      }
    },

}
</script>

<style scoped>

</style>
```

- LottoBall.vue

```vue
<template>
    <div class="balls" :style="background">{{number}}</div>
</template>
<script>
export default {
   name: 'LottoBall',
   components: {
     
   },
   mixins: [],
   props: {
     number: Number
   },
   data() {
    return{
      
    }
   },
   computed: {
    background(){
      let background;
      if(this.number <10){
        background = 'red';
      } else if(this.number<=20){
        background='orange';
      }else if(this.number<=30){
        background='yellow';
      }else if (this.number <=40){
        background = 'blue'
      }else{
        background = 'green'
      }
      return {
        background,
      }  
    }
   },
   watch: {
     
   },
   methods: {
     
   }
};
</script>
<style scoped>
  .balls{
    display: inline-block;
    text-align: center;
    font-weight: bolder;
    line-height: 50px;
    margin: 10px;
    border-radius: 50%;
    width : 50px;
    height: 50px;
    border: solid 2px black;
  }
</style>
```

- watch는 데이터를 감시하는 기능
  - watch는 최소한으로 사용

### 틱택토

