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
