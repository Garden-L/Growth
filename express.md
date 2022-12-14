# express 코드 뜯어보기

<br></br>
## Application

### ■ express().set(name, value)
name에 value를 할당한다. 원하는 모든 값들을 원하는 name에 할당할 수 있다. 하지만 서버의 행동을 구성하기 위한 이미 사용되는 name이 있으므로 유의해야한다. settings table에 명시되어 있다. 

#### 값 불러오기
할당한 값을 불러오려면 app.get(name)으로 불러올 수 있다. 
```js
app.set('title', 'My Site')
app.get('title') // "My Site"
```

#### bool 값의 올바른 세팅
만약 name에 true, false인 bool 값을 할당하고 차후 bool값을 true에서 false, false에서 true로 변경하려면 set을 다시 사용하지말고 app.enable, app.disable 메소드를 사용한다. 강제적이진 않지만 명확하다. 
```js
app.set('foo', true);
app.disable('foo') foo = false로 값 변경
app.enable('foo') foo = true로 값 변경
```

#### Settings table
서버의 행동을 위한 지정된 name들이 있다. 이 네임들은 app.set의 name으로 지정해서도 안되고 디폴트 값이 없는 값은 상속을 해서도 안된다. 디폴트 값이 없는 name들은 상속해도 된다.

* 'views' : 어플리케이션의 뷰 파일들이 있는 디렉토리의 Array 또는 디렉토리(문자열)를 설정한다. array 값이면 배열의 값 순서대로 조회한다. 기본 디폴트 값으로 process.cwd() + '/views' 값이 지정되어 있어 따로 설정을 안하면 프로젝트 내 views 폴더에 html파일들을 넣어야한다. 
* 'view engine' : String 값으로 기본 뷰 엔진을 확장할 때 사용한다. 서브 어플리케이션들은 이 값을 상속한다. 디폴트 값은 undefined 이므로 특정 뷰 엔진을 사용하기 위해서는 꼭 지정해야한다.
