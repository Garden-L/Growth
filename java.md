### Exception

#### 1. 개념
예외는 프로그램이 실행하는 동안 해당 프로그램의 정상적인 흐름을 방해하는 하나의 이벤트이다. 예외는 프로그램을 강제로 종료시킨다. 예외처리를 핸들링하게 되면 프로그램은 종료하지 않고 예외만 처리하고 계속 실행된다.

#### 2. 예외 클래스
자바에서 모든 예외는 Exception 클래스를 상속받는다.  
<img width="631" alt="image" src="https://user-images.githubusercontent.com/56042451/196189889-a46011e4-fa56-4fc8-b012-5dbe63806d91.png">

자바에서의 Exception은 컴파일예외(CompileException), 실행시간예외(RuntimeException) 두 가지 예외가 존재한다. 컴파일예외는 예외를 던지를 메소드를 호출할 때 try\~catch 구문으로 처리해주지 않으면 컴파일할 때 오류를 잡아준다. 실행시간예외는 예외를 던지는 메소르를 호출 시 try\~catch 구문으로 처리해주지 않아도 컴파일시 오류를 잡지 않는다.

* CompileException(Checked Exception) : 컴파일 시 에러를 핸들링하지 않으면 오류발생 (명시적인 처리 O)
  + Exception, SQLException ...
* RuntimeException(Unchecked Exception) : 에러를 핸들링하지 않아도 컴파일 시 오류 발생하지 않음 (명시적 처리 X)
  + NullPorinterException, NumberFromatException ...

##### RuntimeException
실행시간예외 클래스는 되도록이면 throws(예외 던지기)를 하지 않는 것이 좋다. 만약 혹시 던져야 한다면 클라이언트가 사용할 때 복구에 관한 것을 전혀 하지 못할 때는 실행시간예외 클래스를 던져도 크게 상관없다. (차후 내용수정)


#### 3. 문법

##### throw(강제 예외발생)
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




