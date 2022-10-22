# JAVA
자바 정리~
<br></br>

## JVM
### 1. 개념
JVM(Java Virtual Machine)은 자바 컴파일러에 의해 컴파일된 자바 파일(.class)파일을 읽어 실행시키고 자바의 메모리 관리, 안정적인 실행 환경을 위한 자바의 기본 어플리케이션이다. JVM은 JRE에 포함된다.  
<img width="481" alt="image" src="https://user-images.githubusercontent.com/56042451/197320619-696b4d0a-c572-4bf3-8b1e-e36ebd4d77f1.png">

### 2. JVM에서 메모리관리(Garbage Collection)
JVM은 자바 프로그램을 위한 Garbage collection이란 자동 메모리 관리 요소를 내장하고 있다. 가비지 콜렉션은 계속적으로 사용되지 않는 메모리 공간을 식별하여 제거한다. 초기에는 메모리 관리에 대한 부정적인 비평이 많았지만 지속적인 개발과 최적화로 인해 가비지 콜렉션은 엄청난 진화를 이뤘다.

### 3. Class loader
자바에서 클래스는 자바의 모든 것이다. 모든 자바 어플리케이션들은 클래스로 구성되어있다. 그래서 하나의 자바 프로그램을 만들면 엄청난 클래스의 수가 많들어진다. 그래서 자바프로그램을 구동할때 적절하게 사용될 클래스가 메모리로 적재되어야 한다. JVM은 클래스를 메모리에 로드하는 것을 클래스 로더에 의존하다. 클래스 로더는 lazy-loading, cahing 방법을 이용하여 효율적으로 클래스가 로딩될 수 있도록 한다. 모든 JVM은 클래스 로더를 포함하고 있다. 

### 4. Execution engine
클래스 로더가 클래스를 로드 시키면 JVM이 적제된 코드를 실행시키기 시작하는데 이 시작하는 요소가 Execution engine이다. 만약 new 키워드로 새로운 공간을 할당해야한다면 new와 그외 모든 관련있는 것은 실행엔진에서 처리하게 된다.

## JRE
### 1. 개념
JRE(Java Runtime Enviroment)는 JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 및 기타 파일들을 제공한다. JVM은 JRE에 속해있다. 컴파일된 자바 파일을 실행하려면 JRE만 구성되어있으면 프로그램을 동작하는데 문제없다.

### 2. 자바 라이브러리
자바 라이브러리에는 다양한 기능들이 내장되어있다. JWT, swing 등등 .jar파일로 제공한다.

## JDK
### 1. 개념
JDK(Java Development Kit)은 자바 프로그램을 개발하기 위한 도구이다. JRE는 JDK에 포함되는 요소이다.  
<img width="618" alt="image" src="https://user-images.githubusercontent.com/56042451/197320811-cd5c75bc-9b59-4744-8eb6-2fcd9c41a156.png">  

<img width="652" alt="image" src="https://user-images.githubusercontent.com/56042451/197320772-582025f7-de7b-446a-a069-2b4e135eaabb.png">

### 2. javac(자바 컴파일러)
JDK에는 자바 컴파일러가 내장되어있다. 자바 컴파일러는 자바(.java)파일을 JVM이 해석할 수 있는 바이트코드(.class)파일을 변환한다. C/C++에서 컴파일러가 소스파일을 어셈블리어로 변환시키는 과정과 같다. 
```java
자바파일을 컴파일
javac hello.java
컴파일후
hello.class 목적파일 생성
```

### 3. jar

### 4. debugging tools

### 5. javap

<br></br>
## Write once, Run Anywhere(WORA)
### 1. 개념
c/c++은 Write Once, Compile Anywhere(WOCA)라고 불린다. 이는 c/c++소스코드를 작성하면 어디에서든 compile하면 실행한다는 의미이다. compile은 기계어로 번역하는 과정이기 때문에 특정 플랫폼에 맞는 기계어로 변역하기 때문에 플랫폼이 다른 곳에서 작동하려면 다시 재컴파일을 해야한다. 기계어(이진코드)를 실행하기에 속도가 빠르다는 장점이 있지만 컴파일했던 플랫폼과 다르다면 사용자가 다시 재컴파일을 해야한다는 불편함을 감수해야한다. 하지만 자바는 한 번 파일을 작성하고 컴파일하면 어디에서든 실행 가능하다. 자바는 c/c++과 다르게 기계어 번역하는 것이아니 바이트 코드로 번역한다. 바이트 코드는 자바 가상머신이 이해하는 코드이다. 바이트 코드로 번역된 코드는 자바인터프리터가 한줄한줄 읽어 해당 플랫폼에 맞는 기계어로 다시 번역하여 프로그램을 동작시킨다. 자바 가상 머신은 플랫폼에 종속적이기 때문에 각 플랫폼에 알맞게 코드를 번역해야하도록 만들어야하지만 사용자는 이를 신경쓸 필요가없다. 단지 자바 파일만 오류없이 작성하면 되는것이다. 이것을 플랫폼 독립적이라 말한다.
<br></br>
## Binding(바인딩)
### 1. 개념
바인딩은 식별자(variable name, function name, ...)를 각각의 주소로 연결시키는 메카니즘을 말한다. 바인딩은 함수 또는 변수에 대해서 실행 된다. 예를 들어 함수 바인딩의 경우, 오버라이딩된 함수의 경우 오버라이딩 전의 함수를 호출 시킬지 오버라이딩 된 함수를 호출시킬지 컴파일 또는 실행타임에서 제대로 연결해야한다. 

### 2. Compile time binding(Early Binding)
컴파일시간 바인딩은 식별자들을 모두 컴파일 시간에 주소 변환하는 것을 의미한다. 컴파일 과정에서 링킹을 끝내며 주소를 결정한다. 메모리에 로딩되기전 주소로 바인딩되면 컴파일 타임 바인딩, 운영체제의 메모리 관리자와 소통, 가상주소, 절대주소(논리적주소), 정적바인딩,오버로딩된 함수의 호출을 결정하는 것은 컴파일러가 컴파일 타임에 결정한다. 그외 정의가 변하지 않는것들, static은 정적이므로 재정의해도 무시된다. final은 재정의가 불가능하다, 그리고 private도 하위클래스에서 접근자체가 불가능할 뿐만아니라 재정의가 불가능 하므로 컴파일 타임에 처리된다. 컴파일러는 이런 키워드가 재정의가 불가능 하다는 것을 알고 컴파일타임에 처리하여 성능을 향상시킨다. 결국 이런 호출은 특정 클래스객체에서만 사용가능 항상 컴파일타임에 바인딩된다.

### 3. Load Time Address Binding
프로그램이 메모리에 적제 되기전 운영체제의 메모리 관리자에 의해 주소가 바인딩된다. 물리주소, 절대주소가 상대주소로 변경, 정적바인딩이지만 운영체제에 의해 동적바인딩을 지원하기도 한다.

### 4. Dynamic binding(late binding)
동적바인딩이란 실제 서브루틴이 호출되는 메소드의 이름을 연결하기 위해 실행시간 전까지 기다리는 컴퓨터 메커니즘이다. 대표적으로 오버라이딩된 메소드를 호출하는 경우이다.
자바 바이트코드를 보면 static메서드는 invokestatic으로 설정하는 것을 볼 수 있고 동적으로 참조하는 것은 invokevirtual로 설정한것을 볼 수 있다. 이를 예측해보면 c++의 viurtual 테이블을 설정하여 동적참조하는 것 처럼 자바 또한 참조한다고 예측할 수 있다. (나의 가정) 아직 확실하게 정확한 것은 아님.

<br></br>
## abstract class(추상클래스, 추상메소드)
### 1. 개념
추상클래스란 abstract로 선언된 클래스를 의미한다. 추상클래스는 0개이상 추상메소드를 가진 클래스이다. 추상클래스를 상속받는 subclass는 추상메소드를 **무조건** 구현해야한다(추상메소드 정의 강제). 추상클래스는 객체를 생성할 수 없다.

### 2. 사용법
```java
abstract class parent{
    abstract void func(); // 선언만하고 정의는 하지 않음
}

class child extends parent{
    void func(){
        System.out.println("child");
    }
}
```

### interface와 다른점
1. 추상클래스는 클래스이기 때문에 자바에서 상속을 단 하나만 받을 수 있도록 강제화 하지만 인터페이스는 여러개 상속 할 수 있다.
2. 인터페이스의 filed(변수)들은 자동으로 public static final로 선언되고 메소드들은 모두 public으로 공개된다. 하지만 추상클래스는 abstrac 메소드를 갖는 것 말고는 일반 클래스와 동일하다

## interface

<br></br>
## final키워드
### 1. 개념
final 키워드는 마지막이라는 단어의 뜻과 같이 클래스, (매개)변수, 함수 등 선언에 같이 쓰여 추후 변경 수정이 불가능하게 만든다. final class는 상속불가, final variable은 초기화 이후 재할당 불가, final function은 오버라이딩이 불가능하다.

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
static final variable은 선언하자마자 초기화 하거나, 초기화되지 않은 변수는 **static block**에서 반드시 초기화해야한다. 생성자는 객체가 생성될때 호출되므로 이미 객체가 생성되기전 생성되는 static 특징 때문에 생성자에서 초기화는 불가능하다. 단, 인터페이스에서는 불가능하다.
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

## Functional interface
### 1. 개념
함수형 인터페이스는 단 하나의 추상 메소드를 가지고 있는 인터페이스이다. 자바SE 8 부터 함수형 인터페이스는 람다 표현식으로 사용될 수 있다. 함수형 인터페이스는 다수의 default 메소드 및 static 메소드를 가질 수 있다. 대표적인 함수형 인터페이스는 Runnable, ActionListener, Comparable 등이 있다. 함수형 인터페이스는 다른 말로는 Single Abstract Method Interface(SAM interface)라고도 한다. 자바 java.util.function에 다양한 functional interface가 내장되어있다.

### 2. 사용
자바SE 8 이전에서는 익명 내부 클래스로 사용했다.
```java
public class test {
    public static void main(String args[]){
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Override run function");
            }
        });
    }
}
```
자바SE 8이후로 functional interface를 람다 함수로 사용할 수 있다.
```java
public class test {
    public static void main(String args[]){
        new Thread(()->{System.out.println("Override run method,");});
    }
}
```

#### @FunctionalInterface Annotation
@FunctionalInterface 어노테이션을 사용하면 함수형 인터페이스가 추상 메소드 2개 이상 갖지 안도록 보장한다. 어노테이션 사용은 강제화는 아니지만 명시적으로 지정해주는 것이 좋을듯하다. 
```java
@FunctionalInterface // 함수형 인터페이스 보장
interface Anno{
    abstract void func1();

    public static void func2(){
        System.out.println("스태틱 사용가능");
    }

    default void func3(){
        System.out.println("JavaSE 8부터 default 메소드도 가질 수 있다.");
    }
}

class child implements Anno{
    @Override
    public void func1() {
        System.out.println("child func1");
    }

    @Override
    public void func3(){
        Anno.super.func3();
        System.out.println("child func3");
    }
}

public class test {
    public static void main(String args[]){
        Anno t = ()->{System.out.println("람다식으로 표현");};

        t.func1();
        t.func3();

        Anno t2 = new child();
        t2.func1();
        t2.func3();

    }
}
```

## lambda function
### 1. 개념
람다 표현식은 기본적으로 functional interface의 인스턴스이기 때문에 람다식으로 작성할 객체가 functional interface인지 확인해야한다. 

### 2. Annonymouse vs Lambda
익명 클래스와 비교하여 람다식은 길이 이외 다를 것이 없다. 익명 클래스의 길이를 함축한 것이 람다식이다 동작은 익명클래스와 똑같이 동작한다. 단지 익명클래스는 1개 이상의 추상 메소드를 재정의하여 구현할 수 있지만 람다는 무조건 functional interface인지 확인해야한다.
```java
// 추상 메소드가 2개이상이므로 functional interface가 되지 못하여 람다로 작성 불가능하다.
interface Anno{
    abstract void func1();
    abstract void func2(); 
}

public class test {
    public static void main(String args[]){
        Anno t = new Anno(){
            @Override
            public void func1() {
                System.out.println("func1 재정의");
            }

            @Override
            public void func2() {
                System.out.println("func2 재정의");
            }
        };
        t.func1();
        t.func2();
    }
}
```

### 3. 사용
#### parameter가 없는경우
```java
() -> { ... }
```

#### parameter가 있는경우
```java
(int a, int b) -> {...}
```

#### 반환 값이 있는경우
```java
(int a, int b) -> a*b; // 바로 반환값을 작성할 수 있는 경우 중괄호 없이 반환가능
(int a, int b) -> { c = a*b; return c} // 바로 반환 값을 작성 못하면 중괄호 필수
```

#### 객체 대입
```java
Runnable의 람다표현식 익명 클래스와 같으므로 Runnable로 참조 가능하다.
Runnable run = ()-> {System.out.println("객체에 대입")};
```
<br></br>
## thread
### run(), start



