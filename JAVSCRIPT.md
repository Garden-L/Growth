# Javascript

<br></br>
## 연산자
### ■ Strict equality(===, !==)
엄격한 동등 연산자로 두 피연산자의 **값과 자료형(타입)** 이 모두 일치하면 여부에 따라 boolean 자료형을 반환한다. 자바스크립트에서는 단순 비교 연산자(==)가 다른 언어보다 관대한 부분이 있어서 되도록 엄격한 비교 연산자를 사용해야한다. 단, +0과 -0은 참으로 반환한다. 수학적으로 크게 구분할 의미가 없기 때문이다.

#### 배열의 index-finding method
엄격한 동등 연산자로 대표적으로 사용하는 메소드는 배열의 인덱스 파인딩 메소드들이다. 
* Array.prototype.indexOf()
* Array.prototype.lastIndexOf()
* TypedArray.prototype.index()
* TypedArray.prototype.lastIndexOf()

#### switch-case
스위치 케이스 문에서도 엄격한 동등 연산자를 사용하기 때문에 타입을 정확히 일치하도록 프로그래밍 해야한다


<br></br>
## 구조할당분해(Destructuring Assignment)
### ■ 구조할당분해란?
배열의 원소값이나 객체의 프로퍼티를 Unpack하여 새로운 변수에 할당하도록 지원하는 자바스크립트의 문법이다.

### ■ Object의 구조할당분해
객체의 프로퍼티를 새로운 변수에 할당하는 문법으로 배열과는 다르게 변수 이름과 객체 프로퍼티의 키값과 같은 변수에 할당한다. 
#### 기본적인 할당 방법
할당될 변수의 순서는 중요하지 않지만 프로퍼티의 키값과 같아야한다. 만약 변수이름과 같은 프로퍼티의 키값이 없을 경우 문법적 오류는 없고 단지 변수에 아무것도 할당하지 않는다. 아무것도 할당하지 않으므로 undefined가 된다.
```javascript
const obj = {a:10, b:20};
const {b, a} = obj;

console.log(b);
console.log(a);

//출력
20
10
```
#### 새로운 변수이름에 할당하기
기본적으로 변수이름이 같아야하지만 변수이름을 변경하여 할당할 수 있다. 세미콜론을 기준으로 왼쪽에 분해할 객체의 프로퍼티의 키값 오른쪽에는 새로 할당할 변수명을 적어주어야 한다.
```js
const o = { p: 42, q: true };
const { p: foo, q: bar } = o;

console.log(foo); // 42
console.log(bar); // trueㅊ
```

#### 디폴트 값 할당하기
만약 객체를 구조분해할당할 때 변수명의 키값이 없을 경우 기본값을 할당할 수 있다. "변수명=값"을 명시하면 디폴트 값을 할당할 수 있다.
```js
const { a = 10, b = 5 } = { a: 3 };

console.log(a); // 3
console.log(b); // 5
```
새로운 변수명에도 디폴트 값을 할당할 수 있다.
```js
const { a: aa = 10, b: bb = 5 } = { a:3 };

console.log(aa);
console.log(b);

//출력
3
5
```
a와 b 둘다 새로운 aa, bb 변수명으로 변경하고 b는 구조분해할 객체에 없으므로 5 값이 디폴트로 할당된 것을 볼 수 있다.


## Queue
자바스크립의 **런타임**환경에서는 큐를 사용하여 비동기 입출력을 지원한다. 이때 큐는 마이크로태스크 큐, 태스크 큐(매크로 태스크 큐) 총 2가지가 있다.

### Task
interval, timeout, event call back 같은 자바스크립의 표준동작원리에 의해 실행되는 자바스크립트 코드가 스케줄 되는 공간.  
이벤트 함수가 만료되고 추가적인 콜백을 요청할 경우 call back 함수가 task큐에 삽입니다. 현재 인큐된 작업만 처리되며 이후 인큐된 모든 작업은 다음 주기까지 대기해야한다.
## Javascript 비동기-논블럭킹,  동기-블록킹?
### Synchronize
자바스크립트는 무조건 동기-블록킹 방식으로 돌아간다. 하지만 일부사람들은 자바스크립트는 비동기-논블록킹이라고 착각하는 경우가 있다. 왜 착각할까? 바로 자바스크립트에서 제공하는 일부 API들 때문에 비동기-논블록킹으로 작동되기 때문에 자바스크립트를 비동기-논블록킹이라고 착각한다.  

* 원문(https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing) 
  + In its most basic form, JavaScript is a synchronous, blocking, single-threaded language, in which only one operation can be in progress at a time. But web browsers define functions and APIs that allow us to register functions that should not be executed synchronously, and should instead be invoked asynchronously when some kind of event occurs (the passage of time, the user's interaction with the mouse, or the arrival of data over the network, for example). This means that you can let your code do several things at the same time without stopping or blocking your main thread.

위에 글에서 볼수 있듯 자바스크립트는 기본적으로 동기, 블록킹, 싱글-스레드 기반 언어이다. 웹브라우저상 정의된 함수, API들을 이용하여 함수(Callback이라 불리는 함수)를 등록하고 비동기적으로 작동할 수 있다는 것이다.
## Hoisting
### Hoisting이란?
자바스크립트에서 호이스팅은 코드를 실행하기 전 인터프리터가 함수, 변수, 클래스의 **선언을 해당 스코프({})의 맨 위**로 이동한 것처럼 보이는 프로세스를 말한다.  
* 자바스크립트 Docs에서 호이스팅 추천하지는 않는다. 
* 변수는 선언만 할 뿐 초기화를 실행하지 않는다.
  + 예외) var 변수는 undefine으로 초기화

### 장점
선언 순서대로 사용하지 않아도 인터프리터는 잘 동작하게 해준다. 즉, C와 같은 언어에서는 함수 사용을 무조건 선언 후 사용해야하지만 자바스크립트는 사용 후 선언이 가능하다.

### 단점
예기치 못한 오류가 발생할 가능성이 있다.
* 특히 var 변수는 사용에 유의해야한다. EM6 문법에선든 var 변수 사용은 지양한다.


### 함수 호이스팅(function hoisting)
함수 호출이 선언 및 정의보다 미리 되었음에도 불구하고 자바스크립트는 허용한다. 인터프리터가 함수 선언부를 맨 위로 올리는 것과 같다.
```javascript
func();

function func(){
    console.log("함수 호이스팅")
}
```
위의 코드와 아래 코드는 결국 같은 Javascript에서는 같은 코드이다.

```javascript
//같은코드
function func(){
    console.log("함수 호이스팅")
}

func();
```

### 변수 호이스팅(Variable hoisting)
변수 또한 함수처럼 선언과 초기화전 사용하는 것이 가능하다. 
* 변수 호이스팅은 선언하고 초기화는 undefine(default)로 초기화한다. 작성한 초기화 코드로 초기화가 진행되지 않는다!
* 자바스크립트에서 변수 선언 후 초기화, 초기화 수 변수 선언의 코드차이는 없다 -> 어차피 호이스팅 때문에 변수 선언은 인터프리터에 의해 되기 때문이다. 

#### var hoisting
```javascript
console.log(name); 
//변수 호이스팅으로 인해 문법적으로 사용가능하다.
//단, 변수 호이스팅은 선언만 하고 undefine(default) 초기화한다.
var name = "PU";
//'PU'로 초기화 했지만 호이스팅은 이를 무시한다.
name = "JW";

console.log(name);
```

* 변수 선언이 명시적으로 되지 않았으면 호이스팅 하지 않는다.  
```javascript
console.log(name); //에러 ReferenceError 

name = "JW"; 
//인터프리터가 자동으로 할당해주지만
//명시적인 변수 선언이 아니기 때문에 선언 이전 코드사용은 불가하다.
console.log(name);
```

#### let, const hoisting
let과 const도 호이스팅이 된다. 하지만 var처럼 undefine(default)으로 초기화되지 않기 때문에 선언 코드 이전 사용은 불가능하다.

```javascript
//불가능
console.log(name); //에러 ReferenceError 
let name = "JW"; //호이스팅 되지만 default로 초기화 되지 않는다.
```

### 클래스 호이스팅(class hoisting)
클래스도 호이스팅 되지만 let과 const처럼 undefine으로 초기화되지 않기 때문에 선언 코드 이전 사용은 불가능 하다.

### 함수표현식, 클래스표현식 호이스팅(Function and class expression)
함수표현식과 클래스 표현식은 호이스팅 되지않는다!!! 표현식은 기본적으로 변수에 함수나 클래스를 할당하는 것인데 위에서 보았던 변수 호이스팅에서 var경우 undefine let과 const는 선언만하고 초기화는 실시되지 않기 때문에 함수표현식과 클래스 표현식은 호이스팅 되지 않고 변수 선언이 호이스팅 되는 것이다.

```javascript
//실제로 보이지않지만
//var func = undefine 실행
console.log(func) // var 호이스팅이므로 undefine출력

func() // 아직 함수가 할당된것이 아니고 undefine이므로 불가능

var func = function(){
    console.log("함수표현식");
}
```

## 객체 Object
자바스크립트는 Object 기반의 스크립트 언어 이며 자바스크립트를 구성하고 있는 Primitives를 제외한 모든 것이 Object이다. 자바스크립트의 Object는 키(key)와 값(value)로 구성된 속성(Property)와 메소드들의 집합니다.
```javascript
var a = {'key':'value'}
```
* Dictionary와 Object의 차이점
  + Dictionary는 c#, python 등 에서 불리는 것이며, Object는 javascript에서 불리는 단순의 의미의 차이만 있을 뿐이다.
  
### 속성 (Property)
property는 객체의 내부의 속성을 의미한다. property는 key와 value로 구성되며 key는 property의 유일한 값을 식별 할 수 있는 식별자(Identifier)이다.
```javascript
let Person = {key : value}

Object는 Person
Property는 {Key:value}
```
* Key : 빈 문자열을 포함하는 모든 문자열의 Symbol 값, symbol 값 이외 값을 지정하면 암묵적 형변환이 일어나 문자열이 된다.
  - ! Key를 중복 설정하면 나중에 선언한 Key 값이 먼저 선언된 값을 덮어버린다.
  - Key는 문자열로 간주한다.
* value : 모든 값, 함수일 경우 Property가 아닌 Method라고 부른다.

### 메소드 (Method)
Property의 Value가 함수일 경우 메소드라고 부른다. 객체 안에서 선언된 함수는 메소드를 의미한다.

### 객체 생성 방법
* **객체 리터럴**
  + 중괄호를 사용하여 생성하는 방법  
  + 연속된 구조체나 연관된 데이터를 일정한 방법으로 전송하고자 할 때 주로 사용한다.
```javascript
let empty = {} // 빈 Object

let student ={
    'name' : "JY",
    age : 10,
    getname: function(){
      console.log(this.name);
      }
  }

console.log(typeof student)
typeof student.getname()

//출력
Object
JY
```
* **Object 생성자 함수**
  + new 연산자를 이용하여 Object 생성자 함수를 호출하여 빈 객체를 생성할 수 있다.  
  + 자바스크립트 엔진은 객체 리터럴로 객체를 생성하는 코드를 만나면 내부적으로 Object 생성자 함수를 사용하여 생성한다.
```javascript
let student = new Object();

student.name = "YJ"
student['age'] = 20
student.getname = function(){
    console.log(this.name)
}

console.log(typeof student)
console.log('age' + ' : ' + student.age)
student.getname()
```

### 객체 프로퍼티 접근
* **프로퍼티 키**
  + 문자열이나 symbol값 이외에 값을 지정하면 암묵적으로 타입이 문자열로 변환된다.
  + 키는 문자열이므로 **(\'\' 또는\"\")** 를 사용한다, 사용가능한 이름인 경우 따옴표를 생략 가능하다.
  + 자바스크립트 예약어도 사용가능 하다. 하지만 에러를 발생시킬 수 있으므로 사용하지 않는 것이 원칙이다.

* **프로퍼티 값**
  + 마침표 표기법(.) : **\.** 을 이용하여 접근
     + 객체 이름이 네임스페이스(namespace) 처럼 작동한다.
  + 대괄호 표기법(\[\]) : **\[\]** 를 이용하여 접근
 ```javascript
 student.name // 마침표(점) 표기법
 student['name'] // 대괄호 표기법
 ```
* **값 갱신**
  + 객체에 이미 있는 프로퍼티에 새로운 값을 할당하면 새로운 값으로 갱신된다.
  

* **프로퍼티 동적 생성**
  + 객체에 없는 프로퍼티 키에 값을 할당하면 새로운 프로퍼티를 생성하여 객체에 추가된다.

* **프로퍼티 삭제**
  + **delete** 연산자를 사용하여 객체에 속한 프로퍼티를 제거 가능하다.

* **for-in 문**
  + for-in 문은 프로퍼티를 순회하기 위해서 사용한다. **배열을 순회시 for-of 문을 사용한다**
  + for-in 문으로 객체를 반복할 시 프로퍼티의 key값으로 접근 가능하다.
  + 객체는 순서를 보장하지 않는다. 객체의 프로퍼티에는 순서가 없다.

## this
자바스크립트에서 this문은 다른 언어랑은 다르게 동작한다. 그리고 'use strict' 사용 여부에 따라 다르게 동작한다.

자바스크립트에서 this는 함수의 호출(런타임 바인딩) 타이밍에 바인딩 된다. 실행 시간에는 할당할 수 없다. bind()메소드가 함수가 호출될 때 바인드하도록 코딩되어있다. 화살표 함수 (()=>)는 this를 사용불가능 하다. 

브라우저에서는 전역 this는 window, nodejs에서는 global로 표현된다.

내부 함수의 경우 this는 window, 또는 global 메소드로 사용되는 경우는 호출된 객체가 this가 된다.

### this 적용순서
this는 무조건 함수가 호출 될 때 바인딩 된다.
1. 내부함수인지 확인
  * 내부 함수는 무조건 글로벌 객체 참조 (window, global)
  * 메소드 내 내부함수를 정의해도 내부함수 규칙에 의거하여 this는 글로벌 객체를 참조한다.
2. 일반함수 
  * 일반 함수는 무조건 글로벌 객체 참조 (window, global)
4. 메소드인 경우 -> 해당 객체가 this

## Object prototypes

## async function
promise chain을 더욱 간결하게 표현할 수 있는 비동기식 처리 함수

### Description
비동기함수는 0개 또는 그 이상의 await를 표현할 수 있다. await는 동기적으로 작동하는 것처럼 프로미스를 반환하기 전에 실행을 중단한다. await 작업을 마친 프로미스의 값은 await 표현의 반환값으로 처리된다. async, await 구문을 사용할 시 try/catch 블록으로 비동기 코드를 감싼다. await를 async함수 내에서 사용하지 않으면 SntaxError를 발생시킨다. await 구문 포함까지 실행은 모두 동기적으로 작동한다. 하지만 그 이후 코드는 비동기적으로 작동한다.

### return
async function의 반환값은 무조건 promise이다. 만약 반환값이 promise가 아니라면 promise로 감싸서 반환한다. 반환값이 없다면 return promise.resolve(1).then(()=> undefine)으로 반환한다.

## JSON (JavaScript Object Notation)
자바스크립트의 Object 문법으로 구조화된 데이터를 표현하기 위한 문자 기반의 표준 포맷이다. 웹 어플리케이션에서 일반적으로 사용한다.  
다른 포멧보다 상당히 경량화된 포맷이다.

### 1. JSON 구조
JSON은 자바스크립트의 객체 리터럴 문법을 따른 문자열이다. 자바스크립트의 기본 데이터 타입인 문자열, 숫자, 배열, 불리언, 다른 객체를 포함할 수 있다 

```json
{
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "active": true,
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    },
    {
      "name": "Eternal Flame",
      "age": 1000000,
      "secretIdentity": "Unknown",
      "powers": [
        "Immortality",
        "Heat Immunity",
        "Inferno",
        "Teleportation",
        "Interdimensional travel"
      ]
    }
  ]
}
```

* 유의점
  + JSON는 오직 Property만 담을 수 있다. **method는 불가능**하다.
  + JSON의 Property Key 값은 오직 **큰 따옴표**만 사용해야한다. 
  + 사소한 배치 오류로 인해 JSON파일이 동작하지 않을 수 있다.
    - JSON 파일 검사 사이트 - https://jsonlint.com/
  + 큰 따옴표로만 묶인 문자열만이 프로퍼티로 사용가능하다.
  + 주석을 따로 지원하지 않는다. 
  + 데이터 타입을 강제화 할 수는 없다.

### 2. JSON 데이터 접근
JSON은 자바스크립트의 Object 기반으로 작동하기 때문에 Object에서 데이터 접근하는 모든 방식이 JSON에서도 가능하다.
* 점 표기법 
  ```javascript
  jsn.homeTown
  ```
* 대괄호 표기법
  ```javascript
  jsn["homeTown"]
  ```
  
  
<br></br>
## this
### ■ this란?
일반적으로 C++, Java에서 this는 정적으로 참조된다. 정적으로 이미 정해 질 수 있는 이유는 자신을 가리키는 키워드이기 때문이다. 아무나 this의 의미를 변경할 수 없는 것이다. 하지만 자바스크립트에서 this는 동적으로 바인딩 된다. 즉 this의 의미가 상황에 따라 달라질 수 있다는 의미이다. 자바에서는 this를 클래스 내에서 쓸 수 있지만 자바스크립트는 모든 것이 객체이기 때문에 어디서든 사용이 가능하다. 하지만 함수에서 this는 사용에 따라 의미가 달라질 수 있기 때문에 유의해야한다. 함수, 클래스, 글로벌 위치에 따라 달라진다.

### ■ 함수 문맥에서의 this
함수에서 this는 한 객체를 가리키고 있는데 일반적으로 자신을 호출한 객체를 가리킨다. C++, Java관점에서 아래 코드의 getThis의 this를 보면 global(nodejs)에서 정의된 함수이므로 this가 global을 가리키는 것이 일반적일 것이다. 하지만 JS에서는 동적으로 가리키므로 이 함수 객체가 어디에 소속되어 호출되는지에 따라 다르게 this가 참조하는 것이 다르게 된다. 첫번째 콘솔로그를 보면 global 객체를 출력하지만 두번째, 세번째를 보면 getThis가 global에 정의 되었음에도 불구하고 obj1, obj2에 소속시키고 obj1.getThis, obj2.getThis로 호출하여 결과값을 보면 obj1, obj2를 출력하는 것을 볼 수있다. 일반적으로 함수문맥에서 this는 어느 객체가 호출했는지에 따라 결정된다.
```js
function getThis() {
  return this;
}

const obj1 = { name: "obj1" };
const obj2 = { name: "obj2" };

obj1.getThis = getThis;
obj2.getThis = getThis;

consol.log(getThis()) // global 객체
console.log(obj1.getThis()); // { name: 'obj1', getThis: [Function: getThis] }
console.log(obj2.getThis()); // { name: 'obj2', getThis: [Function: getThis] }
```

#### 내부 함수에서의 this
내부함수에서 this는 무조건 전역 객체(window, global)을 가리킨다.

#### 콜백 함수에서의 this
콜백함수에서 this는 무조건 전역 객체(window, global)을 가리킨다.

## Object

### Object Static methods
Object 내부에 있는 static method로 새로운 property를 생성, 변경 후 해당 객체를 반환한다.
```js
//Syntax
Object.defineProperty(obj, prop, descriptor)
```
## Function expression
함수 표현식은 변수에 함수를 할당하는 것을 의미한다. function declaration(일반적인 함수선언)과 매우 비슷하지만 함수선언은 anonymous function을 사용하지 못하지만 funtion expression은 anonymous function을 사용할 수있다.

### hoisting 
함수 표현식은 호이스팅 되지 않는다. 함수 표현식이 있는 라인이 실행되기 전에는 변수 var에서는 undefined 할당되고, let은 사용 불가능하다.

```js 
console.log(hoisting) // undefined
var hoisting = funtion(){}
```

### 함수 표현식 property
함수 표현식의 property는 함수 선언시 생성되는 property와 동일하다. 이때 내부 프로퍼티는 변경이 불가능하다.
```js
var func = function (){}
//func.name == 'func'

func.name = 'a'
//func.name == 'func' 변경 불가능
```

### Unnamed function
변수에 함수이름이 없는(anonymous function)을 할당하면 name 프로퍼티 값은 변수명이 할당된다.

## prototype
자바스크립트의 모든 객체는 prototype 이라는 object를 가지고 있다. ECMA에 따르면 객체 내부를 뜯어보면 \[\[Prototype\]\]으로 표기된다. 프로토타입은 null을 만날때까지 연결되어 있으며 null은 유일하게 prototype을 가지고 있지 않다. 접근은 Object.getPrototypeOf()와 Object.setPrototypeOf()를 이용하여 접근한다. 자바스크립트 표준은 아니지만 \_\_proto\_\_와 동일하다
![image](https://user-images.githubusercontent.com/56042451/179647644-0425c090-e0f3-4e4d-84a2-bb4eb676bac3.png)

### prototype chaining
프로토타입 체이닝은 모든 객체가 prototype으로 연결되어 있음을 의미한다.

#### Inheriting properties
체이닝은 property의 상속을 제공한다. 객체에서 특정 property를 찾을 때 해당 객체에 없으면 그 객체의 property에서 찾고 계속적으로 prototype이 null 이 될때까지 계속 반복하여 찾고자 하는 property를 찾는다. 만약 찾을려는 property가 없다면 undefined를 반환한다. 상속된 객체에서 
는 프로토타입의 객체가 아닌 상속받은 객체를 가르킨다.





