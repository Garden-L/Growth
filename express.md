# express 코드 뜯어보기
express 코드를 뜯어보면서 js 공부하기!

## lib/application.js

### app.set
```js
app.set = function set(setting, val) {
  if (arguments.length === 1) { //
    // app.get(setting)
    var settings = this.settings

    // prototype chain을 이용하여 검사 
    // hasOwnProperty.call 함수는 해당 객체에 property 값이 있는지 검사하는 함수
    // 검사 시 property의 key로 검사한다.
    while (settings && settings !== Object.prototype) {
      if (hasOwnProperty.call(settings, setting)) {
        return settings[setting]
      }
      //object의 프로토타입을 반환
      settings = Object.getPrototypeOf(settings)
    }

    return undefined
  }
```
만약 app.set의 매개변수를 하나만 주어진 경우(setting값만) app.set 함수는 app 객체에서 setting값이 있는지 검사하여 반환한다. 없으면 undefined를 반환한다.  
hasOwnProperty함수는 프로토타입 체인을 확인하지 않기 때문에 해당 object의 소속된 property 값만 확인한다. 그래서 while문으로 직접 프로토타입 체인을 사용하여 property가 존재하는지 확인하는 것이다.
```js
this.settings[setting] = val;
```
setting값이 없으면 app 객체에 setting 값을 넣는다.

