# 23-2-6 Vue 웹게임 만들기 Day04

## [웹 게임을 만들며 배우는 Vue | 학습 페이지 (inflearn.com)](https://www.inflearn.com/course/web-game-vue/unit/23120?tab=curriculum)

### 틱택토

- 개인적인 풀이
- 핵심 컴포넌트인 tdComponent.vue 코드

```vue
<template>
    <td class="squal" @click="change">{{mark}}</td>
</template>

<script>
let cnt = 0;
export default {
    props:{
        tdData: Array
    },
    data(){
        return{
            mark:'',
        }
    },
    computed:{
      
    },
    methods: {
        change(){
            if(this.$root.ox==true&&this.$root.tableData[this.tdData[0]][this.tdData[1]]==''){
                this.mark="X";
                this.$root.tableData[this.tdData[0]][this.tdData[1]]="X"
                this.$root.ox=false;
                cnt++;
            }else if(this.$root.ox==false&&this.$root.tableData[this.tdData[0]][this.tdData[1]]==''){
                this.mark="O";
                this.$root.tableData[this.tdData[0]][this.tdData[1]]="O"
                this.$root.ox=true;
                cnt++;
            }
            console.log(this.$root.tableData);
            for(let i=0; i<3; i++){
                if(this.$root.tableData[0][i]==this.$root.tableData[1][i]&&this.$root.tableData[1][i]==this.$root.tableData[2][i]){
                    this.$root.result=this.$root.tableData[0][i]+"가 이겼습니다";
                }else if(this.$root.tableData[i][0]==this.$root.tableData[i][1]&&this.$root.tableData[i][1]==this.$root.tableData[i][2]){
                    this.$root.result=this.$root.tableData[i][0]+"가 이겼습니다";
                }   
            }
            if(this.$root.tableData[0][0]==this.$root.tableData[1][1]&&this.$root.tableData[1][1]==this.$root.tableData[2][2]){
                this.$root.result=this.$root.tableData[0][0]+"가 이겼습니다";
            }else if(this.$root.tableData[0][2]==this.$root.tableData[1][1]&&this.$root.tableData[1][1]==this.$root.tableData[2][0]){
                this.$root.result=this.$root.tableData[0][2]+"가 이겼습니다";
            }
            if(cnt==9){
                this.$root.result="끝났다";
                console.log(this.$root.tableData);
                let win = true;
                const firstRow = [];
                const secondRow = [];
                const thirdRow = [];
                const firstColumn = [];
                const secondColumn = [];
                const thirdColumn = [];
                
                for(let i =0; i<3; i++){
                    firstRow.push(this.$root.tableData[i][0]);
                    secondRow.push(this.$root.tableData[i][1]);
                    thirdRow.push(this.$root.tableData[i][2]);
                    firstColumn.push(this.$root.tableData[0][i]);
                    secondColumn.push(this.$root.tableData[1][i]);
                    thirdColumn.push(this.$root.tableData[2][i]);
                }
                
            }
        },
    },
}
</script>

<style scoped>
  .squal{
    font-size: 100px;
    text-align: center;
    border: 3px solid black;
    width: 200px;
    height: 200px;
  }
</style>
```

- 가장 상위 부모 tictactoe.vue

```vue
<template>
  <div>
    <table-component :table-data="tableData"/>
    <h2>{{result}}</h2>
  </div>
</template>

<script>
import TableComponent from './TableComponent'
export default {
    components: {
      TableComponent
    },
    data(){
        return{
            tableData: [
              ['','',''],
              ['','',''],
              ['','',''],
            ],
            ox : true,
            result : '',
        }
    },
    computed:{
      
    },
    methods: {
        
    },
    
}
</script>

<style scoped>
  
</style>
```

 #### 강사 풀이

- this.$root.$data : 루트에있는 컴포넌트 데이터를 가져옴

> 주의점 : 배열의 내부를 index를 사용해서 데이터를 바꾸면 화면에 반영 되지 않음
>
> - Vue.set을 사용 or this.$set(this.tableData[1],0,'X')

- eventbus: 이벤트를 한곳에 모아놓고 관리
  - EventBus.js 생성

```js
import Vue from 'vue';

export default new Vue();
```

- 가장 상위 컴포넌트에 `import EventBus from './EventBus';` 이벤트버스 import 후 created로 이벤트 등록
- 해당 이벤트 작성 ( 상위 컴포넌트에서 이벤트를 수행하기에 $root와 같은 코드 사용 하지않고 this로 사용가능)
  - ex: ` EventBus.$on('clickTd',this.onClickTd);`

```vue
<template>
  <div>
    <table-component :table-data="tableData"/>
    <h2>{{turn}}님의 턴입니다.</h2>
    <h2 v-if="winner">{{winner}}님의 승리</h2>
  </div>
</template>

<script>
import Vue from 'vue';
import TableComponent from './TableComponent'
import EventBus from './EventBus';

export default {
    components: {
      TableComponent
    },
    data(){
        return{
            tableData: [
              ['','',''],
              ['','',''],
              ['','',''],
            ],
            turn : 'O',
            winner : ''
        }
    },
    created(){
      EventBus.$on('clickTd',this.onClickTd);
    },
    computed:{
      
    },
    methods: {
        onClickTd(){
            this.$set(this.tableData[this.rowIndex],this.cellIndex, this.turn);
            let win = false;
            if(this.tableData[this.rowIndex][0] === this.turn && this.tableData[this.rowIndex][1]===this.turn && this.tableData[this.rowIndex][2]===this.turn){
                win = true;
            }
            if(this.tableData[0][this.cellData] === this.turn && this.tableData[1][this.cellData]===this.turn && this.tableData[2][this.cellData]===this.turn){

            }
            if(this.tableData[0][0] === this.turn && this.tableData[1][1] === this.turn &&this.tableData[2][2]=== this.turn){
                win = true;
            }
            if(this.tableData[0][2] === this.turn && this.tableData[1][1] === this.turn &&this.tableData[2][0]=== this.turn){
                win = true;
            }
            if(win){
                this.winner = this.turn;
                this.turn ='O';
                this.tableData =[['','','',],['','','',],['','','',] ]
            }else{
                let all =true;
                this.tableData.forEach((row) =>{
                    row.forEach((cell) => {
                        if(!cell){
                            all =false;
                        }
                    });
                });
                if(all){
                    this.winner = '';
                    this.turn ='O';
                    this.tableData =[['','','',],['','','',],['','','',] ]
                } else{
                    this.turn = this.turn === 'O' ? 'X' : 'O';
                }
                
            }
        }
    },
    
}
</script>

<style scoped>
  
</style>
```

- 사용되는 해당 하위 컴포넌트에는 $emit으로 사용(파라미터도 전달 가능)

```vue
<template>
    <td class="squal" @click="onClickTd">{{cellData}}</td>
</template>

<script>
import EventBus from './EventBus';
export default {
    props:{
        cellData: String,
        rowIndex : Number,
        cellIndex: Number,
    },
    data(){
        return{
            mark:'',
        }
    },
    computed:{
      
    },
    methods: {
        onClickTd(){
            if(this.cellData) return;
            
            EventBus.$emit('clickTd', this.rowIndex, this.cellIndex);
        }
    },
}
</script>

<style scoped>
  .squal{
    font-size: 100px;
    text-align: center;
    border: 3px solid black;
    width: 200px;
    height: 200px;
  }
</style>
```

- Vuex

  - store.js 생성
  - npm i vuex

  - store.js

```js
import Vuex from 'vuex';

const SET_WINNER = 'SET_WINNER';
const CLICK_CELL = 'CLICK_CELL';
const CHANGE_TURN = 'CHANGE_TURN';

export default new Vuex.Store({
    state:{
        tableData: [
            ['','',''],
            ['','',''],
            ['','',''],
          ],
          turn : 'O',
          winner : ''
    }, // vue의 data와 비슷
    getters:{

    }, // vue의 computed와 비슷
    mutations:{
        [SET_WINNER](state, winner){
            state.winner = winner;
        },
        CLICK_CELL(state, {row,cell}){
            state.tableData[row][cell] = state.turn;
        },
        CHANGE_TURN(state){
            state.turn = state.turn === 'O' ? 'X' : 'O';
        },
        RESET_GAME(state){
            state.turn = 'O';
            state.tableData=[
                ['','',''],
                ['','',''],
                ['','',''],
            ];
        },
        NO_WINNER(state){
            state.winner ='';
        }
    }, //state를 수정할때 사용. 동기적
    actions:{

    },// 비동기를 사용시, or 여러 뮤테이션을 연달아 실행할 때
});
```

- Object Dynamic Properties
  - 변수로 빼줘서 모듈로 사용

- ...mapState
  - state에 데이터를 매핑
  - `...mapState(['winner','turn']),`
  - 사용할때

```vue
 ...mapState({
            tableData: state => state.tableData,
            turn: state => state.turn,
            cellData(state){
                return state.tableData[this.rowIndex][this.cellIndex],
            }
        })
```

- slot

  - 부모컴포넌트에서 데이터를 활용하여 자식 컴포넌트의 내용을 지정해둠.

  - 자식컴포넌트의 <slot> 태그로 위치

### 지뢰찾기

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export const START_GAME = 'START_GAME';
export const OPEN_CELL = 'OPEN_CELL';
export const CLICK_MINE = 'CLICK_MINE';
export const FLAG_CELL = 'FLAG_CELL';
export const QUESTION_CELL = 'QUESTION_CELL';
export const NOMALIZE_CELL = 'NOMALIZE_CELL';
export const INCREMENT_TIMER = 'INCREAMENT_TIMER';

export const CODE = {
    MINE: -7,
    NORMAL: -1,
    QUESTION : -2,
    FLAG : -3,
    QUESTION_MINE : -4,
    FLAG_MINE : -5,
    CLICK_MINE : -6,
    OPENED : 0,
}

const plantMine = (row,cell,mine)=>{
    console.log(row,cell,mine);
    const candidate = Array(row*cell).fill().map((arr,i)=>{
        return i;
    });
    const shuffle = [];
    while (candidate.length > row * cell - mine){
        const chosen = candidate.splice(Math.floor(Math.random()*candidate.length),1)[0];
        shuffle.push(chosen);
    }
    const data = [];
    for(let i = 0; i<row; i++){
        const rowData =[];
        data.push(rowData);
        for(let j =0; j<cell; j++){
            rowData.push(CODE.NORMAL);
        }
    }
    
    for(let k = 0; k < shuffle.length;k++){
        const ver =  Math.floor(shuffle[k]/cell);
        const hor = shuffle[k] % cell;
        data[ver][hor] = CODE.MINE;
    }
    console.log(data);
    return data;
}

export default new Vuex.Store({
    state:{
        tableData: [],
        data:{
            row:0,
            cell:0,
            mine:0
        },
        timer:0,
        halted : true, //중단된
        result:'',
    }, // vue의 data와 비슷
    getters:{

    }, // vue의 computed와 비슷
    mutations:{
        [START_GAME](state,{row,cell,mine}){
            state.data ={
                row,
                cell,
                mine,
            };
            state.tableData = plantMine(row,cell,mine);
            state.timer=0;
            state.halted = false;
            state.openedCount = 0;
            state.result='';
        },
        [OPEN_CELL](state,{row,cell}){
            let openedCount = 0;
            const checked = [];
            function checkAround(row, cell){   // 주변 8칸 지뢰인지 검색
                const checkUndefined = row<0||row>=state.tableData.length || cell<0 || cell >= state.tableData[0].length;
                if(checkUndefined){
                    return;
                }
                if ([CODE.OPENED, CODE.FLAG, CODE.FLAG_MINE, CODE.QUESTION_MINE, CODE.QUESTION].includes(state.tableData[row][cell])){
                    return;
                }
                if(checked.includes(row+'/'+cell)){
                    return;
                }else{
                    checked.push(row+'/'+cell);
                }
                let around = [];
                if(state.tableData[row-1]){
                    around = around.concat([
                        state.tableData[row-1][cell -1], state.tableData[row-1][cell], state.tableData[row - 1][cell + 1]
                    ]);
                }
                around = around.concat([
                    state.tableData[row][cell -1], state.tableData[row][cell+1]
                ]);
                if(state.tableData[row+1]){
                    around = around.concat([
                        state.tableData[row+1][cell -1], state.tableData[row+1][cell], state.tableData[row + 1][cell + 1]
                    ]);
                }
                const counted = around.filter(function(v){
                    return [CODE.MINE, CODE.FLAG_MINE, CODE.QUESTION_MINE].includes(v);
                });
                if(counted.length === 0 && row > -1){ // 주변칸에 지뢰가 하나도 없으면
                    const near = [];
                    if(row - 1> -1){
                        near.push([row-1,cell-1]);
                        near.push([row-1,cell]);
                        near.push([row-1,cell+1]);
                    }
                    near.push([row,cell-1]);
                    near.push([row,cell+1]);
                    if(row + 1< state.tableData.length){
                        near.push([row+1,cell-1]);
                        near.push([row+1,cell]);
                        near.push([row+1,cell+1]);
                    }
                    near.forEach((n)=>{
                        if(state.tableData[n[0]][n[1]] !== CODE.OPENED){
                            checkAround(n[0],n[1]);
                        }
                    });
                }
                if(state.tableData[row][cell] === CODE.NORMAL){
                    openedCount += 1;
                }
                Vue.set(state.tableData[row],cell,counted.length);
            }
            checkAround(row,cell);
            let halted = false;
            let result = '';
            if (state.data.row * state.data.cell - state.data.mine === state.openedCount +openedCount){
                halted = true;
                result = `${state.timer}초만에 승리하셨습니다.`;
            }
            state.openedCount += openedCount;
            state.halted = halted;
            state.result = result;
        },
        [CLICK_MINE](state,{row,cell}){
            state.halted = true;
            Vue.set(state.tableData[row],cell, CODE.CLICK_MINE);
        },
        [FLAG_CELL](state,{row,cell}){
            if(state.tableData[row][cell] == CODE.MINE){
                Vue.set(state.tableData[row],cell,CODE.FLAG_MINE);
            }else{
                Vue.set(state.tableData[row],cell,CODE.FLAG);
            }
        },
        [QUESTION_CELL](state,{row,cell}){
            if(state.tableData[row][cell] == CODE.FLAG_MINE){
                Vue.set(state.tableData[row],cell,CODE.QUESTION_MINE);
            }else{
                Vue.set(state.tableData[row],cell,CODE.QUESTION);
            }
        },
        [NOMALIZE_CELL](state,{row,cell}){
            if(state.tableData[row][cell] == CODE.QUESTION_MINE){
                Vue.set(state.tableData[row],cell,CODE.MINE);
            }else{
                Vue.set(state.tableData[row],cell,CODE.NORMAL);
            }
        },
        [INCREMENT_TIMER](state){
            state.timer +=1;
        },

    }, //state를 수정할때 사용. 동기적
    actions:{

    },// 비동기를 사용시, or 여러 뮤테이션을 연달아 실행할 때
});
```

