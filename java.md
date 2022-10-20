## Write once, Run Anywhere(WORA)
### 1. 개념
c/c++은 Write Once, Compile Anywhere(WOCA)라고 불린다. 이는 c/c++소스코드를 작성하면 어디에서든 compile하면 실행한다는 의미이다. compile은 기계어로 번역하는 과정이기 때문에 특정 플랫폼에 맞는 기계어로 변역하기 때문에 플랫폼이 다른 곳에서 작동하려면 다시 재컴파일을 해야한다. 기계어(이진코드)를 실행하기에 속도가 빠르다는 장점이 있지만 컴파일했던 플랫폼과 다르다면 사용자가 다시 재컴파일을 해야한다는 불편함을 감수해야한다. 하지만 자바는 한 번 파일을 작성하고 컴파일하면 어디에서든 실행 가능하다. 자바는 c/c++과 다르게 기계어 번역하는 것이아니 바이트 코드로 번역한다. 바이트 코드는 자바 가상머신이 이해하는 코드이다. 바이트 코드로 번역된 코드는 자바인터프리터가 한줄한줄 읽어 해당 플랫폼에 맞는 기계어로 다시 번역하여 프로그램을 동작시킨다. 자바 가상 머신은 플랫폼에 종속적이기 때문에 각 플랫폼에 알맞게 코드를 번역해야하도록 만들어야하지만 사용자는 이를 신경쓸 필요가없다. 단지 자바 파일만 오류없이 작성하면 되는것이다. 이것을 플랫폼 독립적이라 말한다.

## final키워드
### 1. 개념
final 키워드는 클래스, (매개)변수, 함수 등에 쓰여 해당 인스턴스를 변경, 수정을 불가능 하도록한다. final class는 상속이 불가하고, final 변수는 초기화 이후 값을 재할당이 불가능하고 함수는 오버라이딩이 불가능하다. 

### 2. final variable
final variable 은 선언하자마자 초기화하거나 초기화되지않은 변수(blank final variable or uninitialized final variable)는 **생성자**에서 반드시 초기화 해야한다.
```java
class finalVariable{
    final int num1 = 10;
    final int num2; // uninitialized variable or blank variable
    
    finalVariable(){
        num2 = 10; // initialize;
    }
}
```

### 3. static final variable
static final variable은 선언하자마자 초기화 하거나, 초기화되지 않은 변수는 **static block**에서 반드시 초기화해야한다. 생성자는 객체가 생성될때 호출되므로 이미 객체가 생성되기전 생성되는 static 특징 때문에 생성자에서 초기화는 불가능하다.
```java
class statcfianlVariable{
    static final int num1 = 10;
    static final int num2; // uninitialized static variable or blank static variable

    static{
        num2 = 10; //  initialize
    }
}
```

### 4. final parameter
final 매개변수는 수정, 변경이 불가능하다.
```java
 void func(final pstaticMethod a){
        a = new pstaticMethod(); // 변경 불가능
    }
```
### 5. final class
final class는 상속을 불가능하게 만든다.
```java
final class pfinalClass{
}
class cfinalClass extends pfinalClass{ // final class 상속 불가능
    
}
```
### 6. final function
final function은 오버라이딩이 불가능하다
```java
class pfinalMethod{
    public final void func1(int a, int b){
        System.out.println(a + b);
    }
    public final void func1(int a){
        System.out.println(a);
    }
}

class cfinalMethod extends pfianlMethod{
    public void func1(int a){ // overriding 불가능
        System.out.println(a);
    }
}
```

## Generic

### 1. 개념
class, interface, method를 정의할 때 매개변수의 타입을 사용자가 지정하도록 하는 것이다. 제네릭을 프로그래밍은 다른 매개변수 타입에 대하여 재사용성을 증가 시키는 것에 목적을 둔다. 자바는 compile timed에 type parameter를 강하게 검사하기에 잘못된 인수를 넣는 것을 방지한다. 

### 2. generic 클래스 만들기
#### 기본 제네릭 클래스 정의
클래스 이름 다음 angle brackets(<>)에 타입 파라메터를 지정한다. 이름은 Tn이 아니여도 상관없다. 
```java
// class name<T1, T2, T3, ... , Tn> { ...}

class Box<T>{
  private T t;
  
  public void set(T t) {this.t = t}
  public T get() { return t;}
}
```

#### Type parameter 이름 규약
type parameter는 단일, 대문자를 원칙으로 한다. 이름 규약 강제적이지는 않지만 일반적으로 자바에서 통용된다.
* E - Element, 자바 컬렉션 프래임 워크를 확장할 때
* K - Key
* N - Number
* T - Type
* V - Value
* S,U,V ect - 2, 3, 4번째 type parameter로 쓰일 때

### 3. generic 클래스 사용하기
type에는 non-primitive만 가능하다(class, interface, array, even another type variable)
#### type parameter를 지정하지 않았을 때 (raw type)
객체를 사용 시 명시적으로 형변환을 사용해서 사용해야한다.

```java
List list = new ArrayList(); // type parameter를 지정하지 않았음 원시 유형(Raw type)인 List<T> 생성된다.
// type parameter를 지정하지 않았기 때문에 어떤 객체든 입력가능하다.
list.add("hello")
list.add(1) //
list.add(new Node(10, null));

//단, 사용시 명시적 타입변환을 해야한다.
String a = (String) list.get(0);
```

#### type parameter 명시
```java
List<String> list = new ArrayList<String>();
// List<String> list = new ArrayList<>(); // 가능
list.add("hello");
// list.add(1) // 타입을 명시했기 때문에 String외 다른 타입은 불가능
String a = list.get(0); // no cast

```

<br></br>
## Exception

### 1. 개념
예외는 프로그램이 실행하는 동안 해당 프로그램의 정상적인 흐름을 방해하는 하나의 이벤트이다. 예외는 프로그램을 강제로 종료시킨다. 예외처리를 핸들링하게 되면 프로그램은 종료하지 않고 예외만 처리하고 계속 실행된다.

### 2. 예외 클래스
자바에서 모든 예외는 Exception 클래스를 상속받는다.  
<img width="631" alt="image" src="https://user-images.githubusercontent.com/56042451/196189889-a46011e4-fa56-4fc8-b012-5dbe63806d91.png">

자바에서의 Exception은 컴파일예외(CompileException), 실행시간예외(RuntimeException) 두 가지 예외가 존재한다. 컴파일예외는 예외를 던지를 메소드를 호출할 때 try\~catch 구문으로 처리해주지 않으면 컴파일할 때 오류를 잡아준다. 실행시간예외는 예외를 던지는 메소르를 호출 시 try\~catch 구문으로 처리해주지 않아도 컴파일시 오류를 잡지 않는다.

* CompileException(Checked Exception) : 컴파일 시 에러를 핸들링하지 않으면 오류발생 (명시적인 처리 O)
  + Exception, SQLException ...
* RuntimeException(Unchecked Exception) : 에러를 핸들링하지 않아도 컴파일 시 오류 발생하지 않음 (명시적 처리 X)
  + NullPorinterException, NumberFromatException ...

#### RuntimeException
실행시간예외 클래스는 되도록이면 throws(예외 던지기)를 하지 않는 것이 좋다. 만약 혹시 던져야 한다면 클라이언트가 사용할 때 복구에 관한 것을 전혀 하지 못할 때는 실행시간예외 클래스를 던져도 크게 상관없다. (차후 내용수정)


### 3. 문법

#### throw(강제 예외발생)
throw는 예외를 강제로 발생한다. throw로 예외만 발생하고 catch로 처리해주지 않으면 그대로 프로그램은 종료되지만 catch로 처리해줄 경우 나머지 연산은 정상적으로 동작하게 된다.
```java

public static void generateException(int num) throws Exception { //Checked Exception 이므로 무조건 에러를 처리해줘야한다. 여기서는 클라이언트가 처리하도록 강제하므로 try~catch 구문을 사용하지 않는다.
        if (num == 0) {
            throw new Exception("0이 입력되었습니다");
        }
        System.out.println("0이 아닙니다");
}

//은에러를 던지지 않으면 무조건 내부에서 처리해야한다.
public static void generateException(int num){ 
        try{
            if (num == 0) {
                throw new Exception("0이 입력되었습니다");
            }
        }catch(Exception e){
            e.printStackTrace();
        }

        System.out.println("0이 아닙니다");
}

public static void generateException(int num){

        if (num == 0) {
            throw new NumberFormatException("0이 입력되었습니다"); 
            // Unchecked Exception은 따로 에러 핸들링은 강제화 되지 않지만 catch로 에러를 처리해주지 않으면 해당 에러를 출력하고 프로그램은 계속 실행되지 못하고 바로 종료된다.
        }

        System.out.println("0이 아닙니다");
}
```

#### throws
throws는 메소드에 해당 오류가 있음을 클라이언트에게 알리고 클라이언트가 해당 예외를 try\~catch문으로 처리할 것을 명시하는 것이다. 만약 CheckedException이라면 클라이언트는 컴파일 오류를 방지하기 위해 무조건 예외를 처리해야고, UncheckException이라면 에러 처리가 의무사항은 아니지만 처리를 하지 않을 경우 프로그램은 강제 종료된다. 예시는 throw에서 설명되었다.

#### try, catch, finally
try는 일반적은 소스코드가 들어가고, catch는 try에서 일어난 모든 예외를 처리한다. catch에서 에러처리는 하위클래스를 먼저 에러 처리로 기술하고 상위는 그 다음으로 처리한다. finally는 에러가 발생하는 발생하지 않든 무조건적으로 실행되는 코드가 들어간다.

```java
try{
    generateException(0);
}
catch(NumberFormatException e){ // NumberFormatException은 Exception 클래스의 자식클래스 이므로 Exception보다 먼저 예외 처리를 해야한다.
    e.printStackTrace();
}
catch(Exception e) {
    e.printStackTrace();
}
finally { //예외에 관계없이 finally는 무조건 실행된다.
    System.out.println("에러가 발생하든 않하든 finally는 무조건 동작합니다.");
}
```

<br></br>
## thread
### run(), start



