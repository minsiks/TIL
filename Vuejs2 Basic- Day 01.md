# 23-1-5 Vuejs 2 기초 Day 01

> [[[뷰js 2 (vuejs 2) 기초 익히기 기본 강좌\] 01 뷰 인스턴스 생성하기! - YouTube](https://www.youtube.com/watch?v=gZBKGn0wQXU&list=PLB7CpjPWqHOtYP7P_0Ls9XNed0NLvmkAh)](https://www.youtube.com/watch?v=XncTU-4i1KI&list=PLG7te9eYUi7tAQygBknaTciy8wzLCe-Ll)

## 뷰 인스턴스 생성, Vue Data와 Method, Data Binding 

### Vue.js란

- 프로그레시브 JavaScript 프레임워크

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{nextYear('안녕하세요')}}
        <input :type="type" v-bind:value="inputData">
        <a :href="getKossieCoderLink('kossiecoder')">심봉사</a>
        <button></button>
    <script>
        new Vue({
            el: '#app',
            data:{
                person:{
                    name: '홍실동',
                    age: 34
                },
                inputData: 'hello',
                type:'text',
                link: 'https://www.youtube.com/'
            },
            methods:{
                getKossieCoderLink(channel){
                    return this.link + channel;
                },
                nextYear(greeting){
                    return greeting + '! ' + this.person.name + ' 는 내년에 ' + (this.person.age + 1) + '살 입니다.';
                },
                otherMethod: function(){
                    this.nextYear();
                }
            }
        })
    </script>
</body>
</html>
```

## Vue Events, 양뱡향 바인딩

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <form v-on:submit.prevent="submit">
            <!-- <input type="text" :value="text" @keyup="updateText"><br> -->
            <input type="text" v-model="text"><br> <!--양방향 바인딩-->
            {{text}}<br>
            <button type="submit">Submit</button>
        </form>
        
    </div>
    <script>
        new Vue({
            el: '#app',
            data:{
                year: 2018,
                text: 'text'
            },
            methods:{
                plus(){
                    this.year++;
                },
                minus(){
                    this.year--;
                },
                submit(){
                    alert('submiited');
                    console.log('hello');
                },
                // updateText(event){
                //     this.text = event.target.value;
                // }
            }
        })
    </script>
</body>
</html>
```

## Computed 속성

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <button @click="changeMessage">Click</button>
        {{reversedMessage}}
    </div>
    <script>
        new Vue({
            el: '#app',
            data:{
                message: '안녕하세요'
            },
            methods:{
                changeMessage(){
                    this.message='홍길동';
                }

            },
            computed:{
                reversedMessage(){
                    return this.message.split('').reverse().join('')
                }
            }
        })
    </script>
</body>
</html>
```

## Watch 속성

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>뷰 기초 익히기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}<br>
        <button @click="changeMessage">Click</button><br>
        {{ update }}
        

    </div>
    <script>
        new Vue({
            el: '#app',
            data:{
                message: '안녕하세요',
                update:'아니요'
            },
            methods:{
                changeMessage(){
                    this.message='홍길동';
                }

            },
            computed:{
                reversedMessage(){
                    return this.message.split('').reverse().join('')
                }
            },
            watch:{
                message(newVal, oldVal){
                    console.log(newVal, oldVal);
                    this.update='네';
                }
            }
        })
    </script>
</body>
</html>
```

## 클래스 & 스타일 바인딩

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
        <div :style="{ color: red, fontSize: size}">Hello</div>
        <button @click="update">Click</button>
    </div>
    <script>
        new Vue({
            el: '#app',
            data:{
                red: 'red',
                size: '30px'
            },
            methods:{
                update(){
                    this.isRed = !this.isRed;
                    this.isBold = !this.isBold;
                }
            }
        })
    </script>
</body>
</html>
```

