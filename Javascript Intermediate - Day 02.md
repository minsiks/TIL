# 23-1-25 Javascript Intermediate Day 02

## 자바스크립트 중급

> [자바스크립트 중급 강좌 #1 - 변수, 호이스팅, TDZ(Temporal Dead Zone) - YouTube](https://www.youtube.com/watch?v=ocGc-AmWSnQ&list=PLZKTXPmaJk8JZ2NAC538UzhY_UNqMdZB4)

### 5. 숫자, 수학 method

- parseInt() : 앞에 숫자가 있으면 숫자까지 읽음, 2진수 , 16진수 변경 가능
- parseFloat : 실수까지 반환

```javascript
let margin = '10px';

parseInt(margin); // 10
Numver(margin); // NaN

let redColor = 'f3';
parseInt(redColor); // NaN
parseInt(redColor, 16); // 243
parseInt('11',2)//3

let padding = '18.5%';
parseInt(padding); // 18
prseFloat(padding); // 18.5
```

- Math.random() : 0~1사이 무작위 숫자 생성
- Math.max() :  최댓값
- Math.min() : 최소값
- Math.abs() : 절대값
- Math.pow(n,m) : 제곱
- Math.sqrt() : 제곱근

```javascript
* 1~100 사이 임의의 숫자를 뽁고 싶다면
Math.floor(Math.raandom()*100)+1
```

### 6. 문자열 메소드

- toUpperCase() / toLowerCase():

- str.indexOf(text)
- str.slice(n, m) : n은 시작점 , m은 없으면 문자열 끝까지 양수면 그 숫자까지 (포함하지 않음) , 음수면 끝에서부터 셈
- str.substring(n,m) : n과 m 사이 문자열 반환, n과 m 을 바꿔도 동작, 음수는 0으로 인식
- str.substr(n,m) : n부터 시작 m 개를 가져옴
- str.trim() : 앞 뒤 공백 제거
- str.repeat(n) : n번 반복
- 문자열 비교 
  - "a" < "c" // true
  - "a".codePointAt(0)  // 97
  - String.fromCodePoint(97) // "a"

```javascript
let desc = "Hi guys. Nice to meet you.";
desc.indexOf('to'); // 14
desc.indexOf('man'); // -1
if(desc.indexOf('Hi') > -1){
    console.log('Hi가 포함된 문장입니다.');
}

let desc = "abcdefg";

desc.slice(2) // "cdefg"
desc.slice(0,5) // "abcde"
desc.slice(2,-2) // "cde"

desc.substring(2,5); // "cde"
desc.substring(5,2); // "cde"

```

### 7. 배열 메소드1

- push() : 뒤에 삽입
- pop() : 뒤에 삭제
- unshift() : 앞에 삽입
- shift() : 앞에 삭제
- arr.splice(n,m) : 특정 요소 지움 / 삭제된 요소 반환
- arr.splice(n,m,x) : 특성 요소 지우고 추가
- arr.slice(n,m) : n부터 n까지 반환
- arr.concat(arr2, arr3 ...) : 합쳐서 새 배열 반환 
- arr.forEach(fn) : 배열 반복
- arr.indexOf(); 
- arr.includes() : 포함하는지 확인
- arr.find(fn): 
- arr.filter(fn) : 만족하는 모든 요소를 배열로 반환
- arr.reverse() : 역순으로 재정렬  

```javascript
let arr = [1,2,3,4,5];
arr.splice(1,2);
console.log(arr); // [1,4,5]
arr.splice(1,3,100,200);
console.log(arr); [1,100, 200, 5]

let array = [1,2];
array.concat([3,4]); // [1,2,3,4]
array.concat([3,4].[5,6]) // [1,2,3,4,5,6]

let arr = [1,2,3,4,5,1,2,3]
arr.indexOf(3); // 2
arr.indexOf(3,3) // 7
arr.lastIndexOf(3); // 7

```

### 8. 배열 메소드 2

- arr.sort() : 배열 재정렬, 배열 자체가 변경

- arr.reduce() 

### 9. 구조 분해 할당

- Destructuring assignment : 구조 분해 할당 구문은 배열이나 객체의 속성을 분해해서 그 값을 변수에 담을 수 있게 하는 표현식

```javascript
let [x,y] = [1,2];
console.log(x); // 1
console.log(y); // 2

배열 구조 분해
let users = ['Mike', 'Tom', 'Jane'];
let [user1, user2, user3] = users;
console,log(user1); // 'Mike'
console.log(user2); // 'Tom'

let [a,b,c] = [1,2];
console.log(c) // undefined
let[a=3, b=4, c=5] = [1,2];

객체 구조 분해
let user = {name: 'Mike', age: 30}
let {name, age} = user;

console.log(name) // 'Mike'
console.log(age); // 30
```

### 10. 나머지 매개변수 ,전개 구문 (Rest parameters, Spread syntax)

- arguments

  - 함수로 넘어 온 모든 인수에 접근
  - 함수내에서 이용 가능한 지역 변수
  - length / index
  - Array 형태의 객체
  - 배열의 내장 메스더 없음 (forEach, map)

  ```javascript
  function showName(name){
      console.log(arguments.length); // 2
      console.log(arguments[0]); // 'Mike'
      console.log(arguments[1]); // 'Tom'
  }
  showName('Mike','Tom');
  ```

- 나머지 매개변수 (Rest parameters)

  ```javascript
  function showName(...names){
      console.log(names);
  }
  showName(); // []
  showName('Mike'); // ['Mike']
  showName('Mike', 'Tom'); // ['Mike', 'Tom']
  
  function User(name, age, ...skills){
      this.name = name;
      this.age = age;
      this.skills = skills;
  }
  
  const user1 = new User('Mike', 30, 'html', 'css');
  const user1 = new User('Mike', 30, 'js', 'react');
  const user1 = new User('Mike', 30, 'English');
  ```

- 전개 구문(Spread syntax) 

  ```javascript
  let arr1 = [1,2,3];
  let arr2 = [4,5,6];
  
  let result = [...arr1, ...arr2];
  let result2 = [0, ...arr1, ...arr2, 7, 8, 9];
  console.log(result); // [1,2,3,4,5,6]
  
  let user = {name:'Mike'}
  let mike = {...user, age: 30}
  
  console.log(mike) // {name:"Mike",age : 30}
  ```

### 11. 클로저

- 어휘적 환경(Lexical Environment)
  - Lexical 환경 
    - one : 초기화 X 
    - addOne : function
- Closure : 함수와 렉시컬 환경의 조합, 함수가 생성될 당시의 외부 변수를 기억 생성 이후에도 접근 가능 

```javascript
let one;
one = 1;
function addOne(num){
    console.log(one + num);
}

addOne(5);
```

### 12. setTimeout / setInterval

- setTimeout : 일정 시간이 지난 후 함수를 실행
- clearTimeout : 예정된 작업을 없앰
- setInterval : 일정 시간 간격으로 함수를 반복

```javascript
function fn(){
    console.log(3)
}
setTimeout(fn,3000);
function showName(name){
    console.log(name);
}
setTiemout('함수명',시간, '인수명');

const tId = setInterval(showName,3000,'Mike') // 3초마다 Mike가 찍힘
```

### 13. call, apply, bind

- 함수 호출 방식과 관계없이 this 를 지정할 수 있음
- call

```javascript
const mike = {
    name: "Mike",
};
const mike = {
    name: "Mike",
};
function showThisName(){
    console.log(this.name);
}
showThisName.call(mike);

function update(birthyear, occupation){
    this.birthYear = birthYear;
    this.occupation = occupation;
}

update.call(mike, 1999, "singer")
```

- apply
  - 함수 매개변수를 처맇나느 방법을 제외하면 call과 완전히 같음
  - call은 일반적인 함수와 마찬가지로 매개변수를 직접 받지만, apply는 매개변수를 배열로 받음

```javascript
const mike = {
    name: "Mike",
};
const mike = {
    name: "Mike",
};
function showThisName(){
    console.log(this.name);
}

function update(birthyear, occupation){
    this.birthYear = birthYear;
    this.occupation = occupation;
}

update.apply(mike, [1999, "singer"]);
```

- bind
  - 함수의 this 값을 영구히 바꿀 수 있습니다.

```javascript
const mike = {
    name: "Mike",
};
const mike = {
    name: "Mike",
};

function update(birthyear, occupation){
    this.birthYear = birthYear;
    this.occupation = occupation;
}

const updateMike = update.bind(mike);

updateMike(1980,"police")
console.log(mike)
```

### 14. 상속, 프로토타입

```javascript
const user = {
    name : 'Mike'
}
user.name // 'Mike'
user.hasOwnProperty('name') // true
user.hasOwnProperty('age') // false
```

- ProtoType Chain

```javascript
const car = {
    wheels : 4,
    drive() {
        console.log("drive..")
    },
};

const bmw = {
    color : "red",
    navigation : 1,
};

bmw.__proto__ = car;

const x5 ={
    color : "white",
    name : "x5",
};
x5.__proto__ = bmw;
```

```javascript
const Bmw = function(color){
    this.color = color;
}

Bmw.prototype.wheels = 4;
Bmw.prototype.drive = function(){
    console.log("drive..");
};
Bmw.prototype.navigation = 1;
Bmw.prototype.stop = function(){
    console.log("STOP!");
};

const x5 = new Bmw("red");
const z4 = new Bmw("blue")
```

### 15. 클래스

```javascript
const User = function (name, age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        console.log(this.name);
    };
};
const mike = new User("Mike",30);
class User2 {
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    showName(){
        console.log(this.name);
    }
}
const tome = new User2("Tom",19);
```

- 클래스 상속

```javascript
class Car {
    constructor(color){
        this.color = color;
        this.wheels = 4;
    }
    drice(){
        console.log("drive..");
    }
    stop(){
        console.log("STOP!");
    }
}
class Bmw extends Car {
    part(){
        console.log()
    }
}
```

