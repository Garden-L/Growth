## module

### module.exports와 exports
nodejs에서 다른 모듈 파일을 import 하기 위해서 import될 파일을 module.exports 변수에 함수, 객체 등을 연결하여 내보내야한다. module.exports는 기본적으로 객체({})를 포인팅 하고있다. 객체를 기본적으로 포인팅 하고있으므로 내부에 프로퍼티를 내장하거나 아니면 새로운 객체를 가르키도록 변경가능하다. module.exports를 사용하지않고 exports만 사용할 수도있는데 문법에 약간 차이점이 있다.

#### module.exports와 exports의 구조
![image](https://user-images.githubusercontent.com/56042451/179345807-81394fd1-ee54-4522-9659-66f69533b01d.png)

exports는 module.exports를 포인팅하고있다. exports에 직접 값을 할당할 경우 exports가 가르키고있는 module.exports의 값에 덮어쓰는 것이기 때문에 export가 되지 않는다. 그러므로 아래와 같이 사용했을 경우 module.exports의 기본값인 {}가 출력된다.
```js
// car.js
exports = { car : "benz"}


// main.js
const car = require('./car.js')

console.log(car) // output {}
```
![image](https://user-images.githubusercontent.com/56042451/179345911-18afeb7b-9027-454d-a746-1dce77f1c7ac.png)


exports의 사용은 module.exports에 property 값을 추가할 때 사용한다. 
```js
// car.js
exports.car = "benz"

// main.js
const car = require('./car.js')

console.log(car) // output { car : "benz"}
```

