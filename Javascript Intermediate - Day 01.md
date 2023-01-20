# 23-1-20 Javascript Intermediate Day 01

## 자바스크립트 중급

> [자바스크립트 중급 강좌 #1 - 변수, 호이스팅, TDZ(Temporal Dead Zone) - YouTube](https://www.youtube.com/watch?v=ocGc-AmWSnQ&list=PLZKTXPmaJk8JZ2NAC538UzhY_UNqMdZB4)

### 1. 변수, 호이스팅, TDZ(Temporal Dead Zone)

- let, const, var
  - var : 한번 선언된 선수를 다시 선언 가능
  - var는 선언하기 전에 사용할 수 있다.
  - var는 선언하기 전에 사용 가능 (호이스팅)
    - 변수만 호이스팅 되고 할당은 선언부에 할당
  - let과 const는 TDZ에 영향을 받아 할당전에 사용 불가
-  변수의 생성과정 

 	1. 선언 단계
 	2. 초기화 단계
 	3. 할당 단계
 	 -  var
 	   1. 선언 및 초기화 단계
 	   2. 할당 단계
 	 - let
 	   1. 선언 단계
 	   2. 초기화 단계
 	   3. 할당 단계
 	 - const
 	   1. 선언 + 초기화 +할당

- 스코프
  - var : 함수 스코프(function-scoped)
  - let, const : 블록 스코프(block-scoped)

### 2. 생성자 함수

- 객체 리터럴

```javascript
let user ={
    name : 'Mike',
    age : 30.
}
```

- 생성자 함수

```javascript
function User(name, age){
    this.name = name;
    this.age = age;
    this.sayName = function(){
        console.log(this.name);
    }
}
let User1 = new User('Mike', 30);
let User2 = new User('Jone', 22)
let User3 = new User('Tom', 17);
user3.sayName(); // 'Tom'
```

### 3. 객체 메소드, 계산된 프로퍼티

- Computed property (계산된 프로퍼티)

```javascript
let a = 'age';
const user = {
    name : 'Mike',
    [a] : 30
}
const ex = {
    [1 + 4] : 5,
    ["안녕"+"하세요"] : "Hello"
}
```

- Methods

  - Object.assiign() : 객체 복제

- ```javascript
  const user = {
      name : 'Mike',
      age : 30
  }
  const cloneUser = user; // X
  const newUser = Object.assign({},user);
  ```

  - Object.keys() : 키 배열 반환
  - Object.values() : 값 배열 반환
  - Object.entries() : 키/ 값 배열 반환

- ```javascript
  const user = {name : 'Mike', age : 30, gender : 'male',}
  Object.keys(user); // ["name","age","gender"]
  Object.values(user); // ["mike",30,"male"]
  Objet.entries(user); // [ ["name","Mike"], ["age,30"], ["gender","male"] ]
  ```

  - Object.fromEntries() : 키/값 배열을 객체로

### 4. 심볼(Symbol)

- property key : 문자형

```javascript
const obj = {
    1: '1입니다.',
    false : '거짓'
}
Object.keys(obj); // ["1", "false"]
obj['1'] // "1 입니다."
obj['false'] // "거짓"
```

- Symbol : 유일한 식별자를 만들때 사용, 유일성 보장

```javascript
const a = Symbol(); // new를 붙이지 않습니다.
const b = Symbol(); 
console.log(a) // Symbol()
console.log(b) // Symbol()
a === b ; // false
a == b ; // false
const id = Symbol('id'); //id와 같이 설명을 붙여줄 수도 있음
```

- property key : 심볼형

```javascript
const id = Sysmbol('id');
const user ={
    name : 'Mike',
    age : 30,
    [id] : 'myid'
}
Object.keys(user); // ["name","age"] 키가 심볼형인건 건너 뜀
```

- Sysmbol.for() : 전역 심볼 
  - 하나의 심볼만 보장받을 수 있음
  - 없으면 만들고, 잇으면 가져오기 때문
  - Sysmbol 함수는 매번 다름 Symbol 값을 생성하지만,
  - Symbol.for 메소드는 하나를 생성한 뒤 키를 통해 같은 Symbol을 공유

```javascript 
const id1 = Symbol.for('id');
const id2 = Symbol.for('id');

id === id2; // true
Symbol.keyFor(id1); // "id"
```

- 숨겨진 Symbol key 보는 법

```javascript
const id = Symbol('id');
const user = {
    name : 'Mike',
    age : 30,
    [id] : 'myid'
}
Object.getOwnPropertySymbols(user);
Reflect.ownKeys(user);
```

### 5. 숫자, 수학 Method

- toString()
  - 10진수 -> 2진수/ 16진수

```javascript
let num = 10;
num.toString(); //"10"
num.toString(2); // "1010"

let num2 = 255;

num2.toString(16); //"ff"
```

- Math

  - Math.PI; -> 3.1415926535389793

  - Math.ceil() : 올림

  - Math.floor() : 올림

  - Math.round() : 반올림

  - toFixed() : 소숫점 자릿수 

    ```javascript
    let userRate = 30.1234;  를 소수점 둘째짜리 까지 표현 (셋째 자리에서 반올림) Math.round(userRate *100)/100 //30.12
    userRate.toFixed(2); // "30.12"
    userRate.toFixed(0); // "30"
    userRate.toFixed(6); // "30.123400"
    Number(userRate.toFixed(2)); // 30.12
    ```

  - isNaN() : NaN인지 판별

    ```javascript
    let x = Number('x'); // NaN
    x == NaN // false
    x === NaN // false
    NaN == NaN // false
    
    isNaN(x) // true
    isNaN(3) // false
    ```

    
