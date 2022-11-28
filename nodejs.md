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

<br></br>
<br></br>
# sequalize

## 시퀄라이즈
### ■ 시퀄라이즈란?
Postgres, Mysql, MariaDB, SQLite, Microsoft SQL Server, Amazon Redshift, Snowflake's Data cloude 데이터베이스 프로그램들을 위한 Node.js기반 ORM 도구이다. 

### ■ 시퀄라이즈 설치
#### npm install --save sequlize
시퀄라이즈 패키지를 설치한다.

#### npm install --save mysql2
mysql과 연결하기 위한 드라이브를 설치해야한다.

### ■ 기본설정
#### npx sequelize init
시퀄라이즈와 관련된 모든 기본설정을 할 수있다.


<br></br>
## 데이터베이스 연결(Connecting to database)
### ■ 데이터베이스와 연결하기 위한 sequalize 생성하기
#### Passing parameters separately (other dialects), Mysql
Mysql과 연결하기 위한 시퀄라이즈 생성하기
```js
// index.js
const { Sequelize } = require('sequelize'};

const db = {};
module.exports = db;

// Mysql과 연결하기 위한 시퀄라이즈 객체 생성
const sequelize = new Sequelize('database', 'username', 'password', { host: 'localhost', dialect:'mysql'});

db.sequelize = sequelize;
```

## Model 
### ■ 모델 정의
시퀄라이즈에서는 모델을 정의하기 위한 두 가지 방법을 지원한다. 객체 이름은 보통 단수(User), 테이블 이름은 보통 복수(Users)로 정한다.

#### sequelize.define 메소드

#### 모델 클래스의 확장
sequelize에는 모델 클래스가 정의되어있다. 이 클래스를 상속 받아 모델을 만들 수 있다. seqelize.define 메소드는 사실 Model.init의 함수를 호출하여 모델을 구축한다. Model.init 메소드는 static으로 선언되어있어 인스턴스를 만들지 않아도 호출할 수 있다. 모델 클래스에서 중요한 점은 define 메소드는 시퀄라이즈 객체를 생성하여 자동으로 객체와 바인딩하지만 모델을 상속받은 클래스는 데이터베이스와 연결해야할 인스턴스를 지정해야한다.
```js
const { Sequelize, DataTypes, Model } = require('sequelize');
const sequelize = new Sequelize('sqlite::memory:');

class User extends Model {}

User.init({
  // 모델의 속성(열)들을 정의한다.
  firstName: {
    type: DataTypes.STRING,
    allowNull: false
  },
  lastName: {
    type: DataTypes.STRING
    // allowNull defaults to true
  }
}, {
  // 모델의 옵션으로 자세한건 모델 옵션을 참조하세요.
  sequelize, // 데이터 베이스와 연결하기 위해 데이터베이스와 연결시킨 인스턴스가 필요하다.
  modelName: 'User' // 모델의 이름을 설정한다. 
});

// the defined model is the class itself
console.log(User === sequelize.models.User); // true
```

### ■ 모델의 옵션
시퀄라이즈는 모델을 생성할 때 여러 옵션을 추가할 수 있다.

#### 테이블 이름
테이블 이름을 명시하지 않으면 모델 이름을 시퀄라이즈가 알아서 복수형으로 변경하여 테이블 이름을 설정한다. 모델이름이 person 이라면 people로 자동으로 복수형으로 변경하여 테이블 이름을 설정한다.
```js
User.init({
}, {
 sequelize,
 tableName : "Users",
});
```

#### 타임 스탬프
시퀄라이즈는 자동으로 모든 모델에 createAt, updateAt 열을 추가한다. createAt은 최초 레코드가 생성될 때 자동으로 입력되며, updateAt은 해당 레코드가 변경되면 마지막으로 변경된 날짜 및 시간을 입력한다. 만약 타임스탬프가 필요없다면 명시적으로 사용안함을 표시해야한다.
```js
User.init({
}, {
 sequelize,
 timestamps: false, //타임 스탬프 사용한함.
});
```
만약 createAt 또는 updateAt 둘 중 하나는 사용해야한다면 각각 따로 처리해야한다.
```js
User.init({
}, {
 sequelize,
 timestamp: true, // 타임스탬프 사용표시
 createAt: false, // 타임스탬프 중 creatAt은 사용안함
 updateAt: 'updateTimestamp' // updateAt은 사용하는데 열 이름을 updateTimestamp로 변경
});
```

### ■ 모델 동기화
자신이 만든 모델과 데이터베이스의 테이블과 차이가 나거나, 만약 테이블이 존재하지 않는다면? 이를 방지하기 위해 데이터베이스와 자신이 작성한 모델을 동기화 시켜야한다. 동기화를 위해 비동기 함수 sync(option)을 사용한다. 동기화를 시키는 다양한 옵션이 존재한다.

#### 개별적인 동기화 메소드 : Model.sync(option)
모델 클래스의 비동기 함수 sync를 이용하여 모델별로 동기화 가능하다.
```js
await User.sync({force: true})
//or
User.sync({force: true}).then()
```

#### 모든 모델을 한번에 동기화하는 메소드 : sequelize.sync(option)
시퀄라이즈 객체에 있는 sync는 시퀄라이즈에 연결된 모든 모델들을 한번에 데이터베이스와 동기화시킨다.
```js
await sequelize.sync({force: true})
```

#### 동기화 옵션
* sync() : 테이블이 존재하지 않으면 테이블을 생성하고 테이블이 이미 정의되어 있으면 아무것도 하지 않는다.
* sync({force: ture}) : 테이블을 드랍하고 새롭게 테이블을 생성한다
* sync({alter: ture}) : 테이블의 현재 상태를 확인하고 데이터 타입이나, 컬럼이 다르다면 모델과 일치 시키기 위해 필수적인 테이블 변경을 실행한다. 

#### 테이블 삭제
모델과 관련된 테이블을 삭제한다.
```js
await sequelize.drop();
console.log("All tables dropped!");
```

#### 동기화를 해야할까?
sync는 너무 파괴적인 연산자이다. 위에서 보았듯 테이블을 강제로 삭제하는 행위는 데이터베이스에 치명적일 것이다. 그래서 동기화 메소드를 사용하는 것보다 sequelize의 마이그레이션 사용을 추천한다.

### ■ DataTypes
컬럼을 정의하기 위해 시퀄라이즈는 데이터 타입들이 내장되어 있다. 

#### import DataTypes
내장된 데이터 타입에 접근하기 위해 DataTypes를 import시켜야한다.
```js
const { DataTypes } = require('sequelize');
```

#### 문자열
```js
DataTypes.STRING         // VARCHAR(255)
DataTypes.STRING(1234)   // VARCHAR(1234)
```

#### 숫자
```js
DataTypes.INTEGER         // INTEGER
DataTypes.BIGINT          // BIGINT

DataTypes.INTEGER.UNSIGNED    // Mysql, Maria DB
```

#### 날짜
```js
DataTypes.DATE            // sqlite, mysql의 DATETIME
DataTypes.DATEONLY        // 날짜만 설정
```
