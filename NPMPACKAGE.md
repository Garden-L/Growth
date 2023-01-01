# express 

<br></br>
## express

<br></br>
## Response Object
### ■ res.cookie(name, value \[, option])
https://expressjs.com/en/4x/api.html#req.signedCookies)
HTTP Header의 Set-Cookie 속성을 추가하는 기능이다. 


#### Option(Object)
* signed : true로 설정되면 cookie-parser 미들웨어의 secret 옵션의 값을 참조하여 서명된 쿠키(암호화된 값)를 만들어낸다. 서명된 쿠키는 request 객체에 signedCookies 프로퍼티로 들어간다.
  ```js
  ...
  app.use(cookieParser({ secret: "secret" });

  ...
  res.cookie('name', 'jw', { signed: true });
  // cookie-parser의 secret값을 참조하여 서명된 쿠키 값을 생성.
  // 차후 요청이 들어오면 req.signedCookies에 쿠키 값이 존재한다.
  ```
  
<br></br>
## Morgan
### ■ Morgan이란?
Nodejs를 위한 HTTP Request의 로그 기록을 남기는 로거로 미들웨어로 사용한다. 

### Morgan 생성
모간을 생성하기 위해서는 모간에 포맷 형식과 옵션을 추가해야한다. 포맷은 미리 정의된 문자열 형태로 미리 정의되어 있다. 옵션으로 로그 표시 순서를 정할 수 있다.

```js
// 기본구조
morgan(format, options)

morgan('tiny') //tiny라는 미리 정의된 포맷 형식이 존재한다.

// Option
morgan(':method :url :status :res[content-length] - :response-time ms');

// 출력예시
POST /login 200 header 32 ms 

// 함수로도 가능
morgan(function (tokens, req, res) {
  return [
    tokens.method(req, res),
    tokens.url(req, res),
    tokens.status(req, res),
    tokens.res(req, res, 'content-length'), '-',
    tokens['response-time'](req, res), 'ms'
  ].join(' ')
})
// 토큰은 모든 토큰들이 정의되어 있는 객체이다. 토큰을 통해 여러 출력 순서를 지정할 수 도 있다.
// 반환 값은 반드시 스트링 값으로 지정해야한다.

// 출력은 옵션과 동일하다
```

### 미리 정의된 포맷(Predefined format)
####  ⃞ combined
표준 아파치 형식의 포맷이다. 따로 색은 지정되어 있지 않다. 많은 정보를 로그 기록에 남기기 때문에 추후 버그를 해결할 때 유용하다.
```
:remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] ":referrer" ":user-agent"
```

#### ⃞ dev
개발 환경에서 사용할 때 쓰는 포맷이다. 응답 상태에 따라 상태(:Status)코드가 성공이면 녹색, 서버에러면 적색, 클라이언트 에러 코드면 노란색, 리다이렉션은 청녹색, 정보의 관한 코드는 검은색으로 표시된다.
```
:method :url :status :response-time ms - :res[content-length]
```
<p align="center"><img width="555" alt="image" src="https://user-images.githubusercontent.com/56042451/210140798-bc891a18-24f2-40a3-9be0-b7bb4c2a6898.png"></p>


<br></br>
## dotenv
### dotenv란?
zero-dependency module로 각 변수의 할당된 값들이 있는 .env 파일을 노드js의 process객체의 env 프로퍼티에 키(변수), 밸류(값)들로 만들어준다. 

### 사용법
#### ⃞ 1. 반드시 프로젝트 루트 위치에 ".env" 파일을 생성
<p align="center"><img width="181" alt="image" src="https://user-images.githubusercontent.com/56042451/210141257-85100575-d162-427e-b9d4-bbed4e1c7459.png"></p>

#### ⃞ 2. .env 파일에 여러 변수 값들을 할당한다.
```js
COOKIE_SECRET=cookiesecretff12331
NODE_ENV='production'

// 15.0.0 버전 이상에서는 멀티라인 value를 사용할 수 있다.
PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----
...
Kh9NV...
...
-----END RSA PRIVATE KEY-----"

// 15.0.0 버전 미만이면 \n(개행문자)를 이용해라
PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----\nKh9NV...\n-----END RSA PRIVATE KEY-----\n"
```

#### ⃞ 3. 반드시 애플리케이션에서 빨리 임포트시키고 구성한다.
```js
const morgan = require('morgan'); //임포트
dotenv.config(); //구성
```

<br></br>
## winston
### winston이란?
다중 전송을 지원하는 로깅 라이브러리 이다. 전송은 필수적으로 스토리지에 저장된다. 다중 전송은 각각 로깅 레벨을 다르게 설정할 수 있어 로깅레벨에 따라 데이터베이스 같은 영구 저장소에 기록하거나 local file, console에 출력할 수 있다.

### 




