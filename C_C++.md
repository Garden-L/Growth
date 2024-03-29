# C++


<br></br>
## Values
### ■ C++의 난해한 Value들...
C++11부터 추가된 Rvalue reference 때문에 기존의 C, 이전 버전의 C++에서 R,Lvalue 개념들이 난해해졌다. 일단 기존의 L-value, R-value 개념부터 살펴봐야한다.

### ■ 기존의 L-value, R-value
####  ⃞ L-value
기존 L-value 정의는 좌측에 올 수 있는 값이 였다. 좌측에 올 수 있는 값이란 메모리 위치를 참조하는 표현이였다. 이 표현을 식별자(Identifier)라고 부른다. 흔히 우리가 알고있는 변수가 L-value이다. 좀 더 깊이 들어가 L-value는 수정가능한 L-value, 수정불가능한 L-value로 나뉘게 되는데 수정가능한 L-value는 int, int\* 등 값이 수정가능한 변수들을 말하고, 수정 불가능한 L-value는 배열 타입(배열 원소를 나타내는 a\[1]이 아니라 단순 배열 int a\[10]= {0}으로 선언된 배열 타입을 a=40과 같이 변경하는 것을 불가능한 것을 의미한다), 불명확한 타입, const로 지정된 타입들이 대표적이다. 

* modifiable l-value
  + 정수형, 실수형, 포인터, 구조체, 유니온 식별자, 배열요소(원소) (ex. a\[1]), 포인터 표기법
* nonmodifiable l-value
  + 배열타입, const 객체, 문자열 리터럴("aba")
    + 문자열 리터럴이 l-value인 이유는 문자열은 자동으로 메모리 공간을 컴파일러가 할당한다. 문자열 앞에 주소표기(&)를 하면 주소값이 나오는 것을 알 수 있다.

####  ⃞ R-value
기존 R-value의 정의는 우측에 올 수 있는 값이였다. 모든 l-value는 r-value가 될 수 있다. 우리에게는 이것이 자연스러운 일이다. a변수에 b변수 값을 담는 것은 매우 자연스럽다. 이때 b값이 이동되지 않고 복사된다. 이동은 주소값이 옮겨가는 것을 의미하고 복사는 새로운 주소 공간에 값을 할당하는 것을 의미한다. 이 개념은 이후 c++11에 나오는 value 개념에서 매우 중요하다. 
* R-value
  + 모든 L-value
  + 문자열 리터럴을 제외한 모든 리터럴(정수형 리터럴, bool형 리터럴, pointer 리터럴, 문자(char) 리터럴 등등)

### ■ C++11이후 Values
####  ⃞ value expression
<p align="center"><image src="https://user-images.githubusercontent.com/56042451/209667694-d0a6cfce-71fe-4e3e-9821-651782129896.png"/>
</p>

####  ⃞ Lvalue 
C++ 에서 L-value란 **이동 불가능하고 식별자가 있는 값**을 의미한다. 식별자란 메모리 위치를 참초하는 것을 일컫는다고 했다. 그럼 이동이 불가능하다라는 말은 무엇인가? 바로 값을 복사한다는 것이다. 이 개념이 복사생성자로 발달된 것 같다(추측). 복사한다는 것은 새로운 공간에 값을 할당하는 것이다. 변수 b에 10을 할당하고 a=b로 다시 a의 변수의 값을 할당한다고 하면 a는 b자체를 가르키는가 b가 가지고 있는 공간을 가리키는가? a라는 새로운 공간에 b라는 값을 할당한 것이다. 이것이 바로 복사이다. 그럼 복사생성자도 바로 이해가 될 것이다. L-value로 인식된 값은 모두 복사 생성자를 통해 새로운 값에 기존의 값들을 할당하는 것이다. 

####  ⃞ prvalue (pure rvalue)
prvalue는 (pure rvalue)이다. 식별자를 가지고 있지 않으면서 이동될 수 있는 것을 의미한다. rvalue를 prvalue라는 것을 따로 만들 이유를 추측해보면 임시객체도 rvalue 범주에 속하기 때문에 임시객체의 rvalue와 순수한 rvalue를 구분할 필요성이 있었을 것이라 추측된다. prvalue는 c++11 이전의 rvalue를 의미한다. 즉 문자열 리터럴을 제외한 모든 리터럴을 의미한다. 

####  ⃞ xvalue (eXpiring value)
lvalue, prvalue는 레거시 표현들과 같다. 제일 문제가 되는 것이 xvalue이다. xvalue는 식별자도 가지고 있고 이동도 가능한 것을 의미한다. eXpiring의 X를 따와서 xvalue이다. 곧 만료되는 값을 의미한다. 곧 xvalue가 정확한 임시객체를 의미한다. 임시객체의 특징은 연산이나 반환 등에서 잠깐 값을 저장하기 위해 생성되는 객체이다. 이 객체는 참조하지 않으면 직접 접근은 어렵지만 주소 공간에 할당되어 식별자를 가지고 있고 값들을 다른 변수에 대입할 때 복사되지 않고 이동되는 특징있다. 직접 접근이 어렵기 때문에 주소연산을 사용할 수는 없지만 임시객체에 이름을 부여하고 사용하면 매우 효율적인 공간, 시간을 사용할 수 있기 때문에 rvalue reference라는 것이 도입된 것 같다(추측). xvalue 즉 임시객체는 rvalue reference 함으로써 lvalue처럼 사용할 수 있고 참조하지 않으면 바로 만료(expire)되어 삭제되어 버린다.

####  ⃞ gvalue (generalized value)
gvalue는 단순히 식별자들이 있는 카테고리들을 묶은 것이다. generalied(일반적인)라는 명칭이 붙은 것으로 보아 식별자가 있는 것은 매우 일반적인 표현이라 그런 것일지 않을까라는 추측을 해본다.

####  ⃞ rvalue
이동되는 값들을 카테고리로 묶은 것이다. c++도 lvalue가 rvalu될 수 있다. 단 lvalue가 rvalue로 사용되면 값이 복사되니 복사가 필요하지 않는 경우는 이동시켜야한다. 이를 std::move함수가 지원한다.

<br></br>
## Reference
### ■ 들어가기전...
C++만든 아저씨는 똑똑한건지... 아니면 사람을 괴롭게 하려고 만든건지... 공부하면 공부할수록 암담해지는 자신을 보며 레퍼런스를 공부하자! C++의 레퍼런스는 어렵다. But 어느정도 개념만 알면 규칙성이 있으니깐 개념만 잘 파악해도 대부분 깨우칠 수 있다. 특히 용어에 대한 이해가 있다면 레퍼런스 어렵지않다!!!

### ■ 레퍼런스란?
포인터와 비슷한 개념이다. 하지만 다른 점은 포인터는 다른 변수를 가리키기 위해 4바이트(시스템 마다 다름)정도 변수를 할당할 공간을 확보한다. 하지만 레퍼런스는 공간을 할당하지 않는다. 단지 새로운 변수에 별칭만 붙이는 것이다. 변수인데 공간을 할당하지 않는 것이 가능한가? 가능하다. 컴파일러 수준에서 a와 a의 별명은 b라고 인식시키면 된다. 컴파일러는 당연히 a, b를 문자 그대로의 a,b로 인식하지 않고 같은 주소값이라고 표기할 것이다(추측, 차후확인해봄)

### ■ lvalue reference(lvalue 참조)

#### lvalue
lvalue참조란 무엇인가? 쉽다 그냥 Lvalue를 참조하는 것이다. Lvalue는 무엇인가? 자세한건 위에서 lvalue를 확인하면 알 수 있지만 보통 변수, 문자열 리터럴 등, 정확히는 식별자(주소 값)가 있고 이동할 수 없는 것들은 모두 lvalue이므로 참조하기 위해서는 lvalue 참조를 사용해야한다. 그런데 c++에서는 흠이 한가지 있다. lvalue 참조에 const가 붙는 경우이다. 자세한건 다음 주제에서 확인하자.

#### const lvalue reference
const lvalue reference는 조금 특이하다. lvalue만을 가리키진 않는다. rvalue도 가리킬 수 있다. lvalue참조인데 rvalue를 가리킨다고? 사실 const lvalue reference는 무조건 참조대상이 lvalue일 필요는 없다. 자세한건 아래 c++을 만든 아저씨의 말을 보면 안다.
```
The initializer for a const T& need not be an lvalue or even of type T. In such cases:

[1] First, implicit type conversion to T is applied if necessary.

[2] Then, the resulting value is placed in a temporary variable of type T.

[3] Finally, this temporary variable is used as the value of the initializer.
```
const는 우리가 아는 일반적인 사실은 해당 변수를 변경하지 못하도록 한정하는 것이다. 대부분 매개변수로 값을 변화시키지 못하도록 const를 하는 경우가 많다. 그럼 당연하게 lvalue를 변경 못하게하는 것이 const lvalue reference 아닌가? 맞다. 그러나 이것도 맞고 C++에서 예외적으로 Rvalue도 참조 할 수 있도록 한다. C++ 아저씨 말에 따르면 const lvalue reference는 rvalue 값을 참조하게 되면 암시적으로 lvalue에 대한 임시 변수가 생성되고 rvalue 값으로 초기화 시킨다. 참조한 값은 그래도 const 이기 때문에 변경은 못한다. 아무래도 임시로 생성된 객체다보니 함부로 값을 바꾸는 행위가 시스템에 문제를 초래할 수도 있기 때문에 const lvalue reference가 rvalue도 참조할 수 있도록 만들지 않았을까 추측해본다.
```c++
// int형 임시변수가 생성되고 10으로 초기화한다. a는 임시변수를 참조한다.
const int& a = 10;

// 클래스에서는 Aclass 임시 객체가 생성되고 10으로 초기화한다. a는 임시객체를 참조한다.
const Aclass a = 10;
```

### rvalue reference(rvalue 참조)
모든 rvalue(xvalue, prvalue)를 참조하는 것이다. lvalue참조처럼 딱히 예외 사항은 없다. rvalue 사용에서 중요한 점은 rvalue로 참조한 변수는 lvalue라는 것이다.???? 자세한건 아래코드를 보자.
```c++
void func(AClass&& ref){
  AClass b = ref; // 핵심
}
...

func(AClass(10));
```
위에 코드에서 AClass(10)은 임시객체이므로 ref가 rvalue 참조하는 것이 자명하다. 이것도 모르면 C++때려...ㅊ ㅓㅓㅓㅓㅓㅓ.... 그럼 AClass b = ref는 복사생성이 일어날까 이동생성이 일어날까? rvalue 참조한 값이니깐 당연 이동 생성이지! 라고 말하면 큰일난다. b=ref는 복사생성이 일어난다! 엥??????? 이유는 ref는 lvalue이기 때문이다. 이건 무슨소리지 싶은데 AClass(10)은 임시 객체이므로 rvalue이고 ref는 임시객체를 참조하는 rvalue 참조자이기 때문에 rvalue아닌가? 원래 임시객체는 사실 없어질 객체이다. 하지만 성능을 극대화 하기 위해 임시객체를 사용하는 방법론을 만든것이 rvalue 참조이다. 즉 임시객체에 너는 이제 내가 사용하겠다고 변수로 못 박는 것이다. "변수로 못 박는다!" 즉 ref는 변수, 그럼 변수는 lvalue인가 rvalue인가? lvalue이다. 즉 ref는 lvalue이다. lvalue가 rvalue로 사용되면 복사 생성을 하기 때문에 복사생성자가 호출된다. 이정도면 충분한 이해가 됐으리라 생각된다. 여기서 코드의 문제점을 찾아야한다. 임시객체를 참조하는 것은 공간, 시간의 효율성이다. 이동 생성자, 이동 대입연사자를 최대한 활용해야 임시객체의 효율성을 맛볼 수 있다. 하지만 복사생성이 되어버리면 의미가 없어지지 않나? 맞다. 의미가 없어진다. 그래서 우리는 rvalue 참조를 한 lvalue의 변수를 이동 생성자로 활성화 시켜야한다. 그게 바로 std::move이다. rvalue 레퍼런스가 lvalue로 인식되어 복사생성을 하니 굳이 공간을 복사하지 않는 이동의미론에 알맞게 xvalue로 인식시켜 값들을 복사하는 것이아닌 이동시켜야한다. 

### univalsal reference
유니버셜 레퍼런스의 표기는 &&이다. 그럼 rvalue 참조아닌가요? 맞기도 아니기도하다. 유니버셜 레퍼런스는 lvalue or rvalue 레퍼런스가 된다. 즉 lvalue 참조가 될지 rvalue 참조가 될지 결정되지 않는 참조자다. 아니 &&이면 rvalue라면서 rvalue참조가 될지 lvalue참조가 될지 결정되지 않았다니? c++에서 형식이 결정되지 않는 키워드가 무엇인가? 바로 매개변수의 auto, 템플릿의 typename(calss)가 형식이 결정되지 않는다. 이것들은 대입되는 변수값에 의해 형식이 결정된다. 즉 auto &&, typename&& 같은 것들이 univalsal reference이다.

#### 레퍼런스 충돌(reference collapsing)
레퍼런스 충돌은 레퍼런스 표시자가 세개 이상 있을 경우를 말한다. 이는 컴파일러가 다시 재해석한다. 홀수개는 lvalue reference, 짝수개는 rvalue reference이다. 이는 매우 중요하다.
* T& -> T&
* T&& -> T&&
* T&&& -> T&
* T&&&& -> T&&

#### 타입 추론방식
int a= 10, a의 타입은 무엇인가? lvalue 이므로 int &이다. 10의 타입은 무엇인가? rvalue이므로 int&&이다. 개쉽다....

#### template에서 univalsal reference
그럼 유니버셜 레퍼런스가 어느정도 감이 오지 않나? 타입이 명확하지 않게 인식되는 rvalue reference로 선언된 모든 것들은 lvalue가 들어오느냐, rvalue가 들어오느냐에 따라 개발자가 직접 어떻게 인식시킬지 결정할 수 있다. 자세한 것은 아래에서 확인하자.

```c++
#include <iostream>
#include <string>
using namespace std;

template<typename T> struct S;

template<typename T> struct S<T&> {
   static void print(T& t)
   {
      cout << "print<T&>: " << t << endl;
   }
};

template<typename T> struct S<const T&> {
   static void print(const T& t)
   {
      cout << "print<const T&>: " << t << endl;
   }
};

template<typename T> struct S<T&&> {
   static void print(T&& t)
   {
      cout << "print<T&&>: " << t << endl;
   }
};

template<typename T> struct S<const T&&> {
   static void print(const T&& t)
   {
      cout << "print<const T&&>: " << t << endl;
   }
};

template <typename T> void print_type_and_value(T&& t)
{
   S<T&&>::print(std::forward<T>(t));
}

const string fourth() { return string("fourth"); }
int main()
{
   // print_type_and_value<string&>(string& && t)
   // print_type_and_value<string&>(string& t)
   string s1("first");
   print_type_and_value(s1);

   // print_type_and_value<const string&>(const string& && t)
   // print_type_and_value<const string&>(const string& t)
   const string s2("second");
   print_type_and_value(s2);

   // print_type_and_value<string&&>(string&& t)
   print_type_and_value(string("third"));

   // print_type_and_value<const string&&>(const string&& t)
   print_type_and_value(fourth());
}
```


<br></br>
## Virtual Table
### 1. 개념
가상테이블은 프로그램언어에서 dynamic dispatch(late method binding, dynamic binding) 지원하는 방법이다.  
dynamic binding이란 compile time에 명확하게 호출될 요소를 정하지 못하여 런타임에 명확한 요소를 정하는 것하는 것이다. 동적 바인딩이 새로운 방법으로 요소를 찾는 것이 아닌 컴파일타임에 런타임에서 요소를 찾을 수 있도록 도와준다. c++에서는 vtable의 주소를 컴파일타임에 설정한다.

#### virtual table 생성과정
컴파일러는 virtual 함수가 있는 클래스에 가상 테이블을 가리키는 **포인터(vpointer)** 를 하나 생성한다. 클래스에 아무것도 없이 virtual 함수를 하나 만들면 32비트 시스템에서는 4바이트, 64비트 시스템에서는 8바이트 만큼 크기가 할당된 것을 볼 수 있다.
``` c++
#include <stdio.h>

class A{
    virtual void bar();
    virtual void qux();
};

class B{
    void func();
};

class C : public A{
    void bar() override;
};

int main(void)
{
    printf("%d\n", sizeof(A)); //32bits system : 4bytes, 64bits system: 8bytes 출력,
    
    printf("%d\n", sizeof(B)); //1byte, 빈클래스는 컴파일러가 1byte 무조건 할당 -> 추후 자세히 알아서 변경
}
```


A클래스와 C클래스의 모습을 도식화 하면 아래와 같이된다. 인스턴스 생성시 메소드가 각각 인스턴스별로 생성되는 것이 아니라 객체의 메소드를 참조한다는 것이다. 멤버변수는 각각 인스턴스마다 생성된다. 그래서 멤버 변수는 인스턴스변수라고도 한다.   
![image](https://user-images.githubusercontent.com/56042451/197188995-838380fc-5a17-4235-adfb-07f078e1b790.png)

### 3. 동적 바인딩을 함으로써 이득
동적 바인딩은 결국 업캐스팅(부모가 자식을 가르키는 것)되었을 때 의미가 있다. 일반 함수나 업캐스팅을 하지 않았을 경우 사용용도가 크게 달라지는 것이 없기 때문에 의미없다.
```c++
#include <stdio.h>

class A{
public:
    virtual void bar(){
        printf("A bar\n");
    }
    virtual void qux(){
        printf("A qux\n");
    }
    void normal(){
        printf("A normal");
    }
};

class C : public A{
public:
    void bar() override{
        printf("C bar\n");
    }
    void normal(){
        printf("C normal");
    }
};
int main(void)
{
    C d;
    A& casting = d;
    A* a = new A();
    A* c = new C();

    casting.bar(); // virtual은 동적바인딩 되기때문에 참조하는 객체의 함수를 호출
    casting.normal(); // 일반함수는 정적바인딩되기 때문에 의미없음
    a->bar(); // A내의 호출
    c->bar(); // c내의 호출
}
```

## using

### 1. 개념
기존에는 typedef 키워드를 이용하여 별칭을 지정하였지만 C++11에서는 using을 이용하여 별칭을 지정하는 것이 가능해졌다. 기존 typedef는 타입을 짧게 하는 것에 가까운 반면, using은 alias declaration에 가깝다.  

#### 사용방법
```c++
c   : typedef int INT;
c++ : using INT = int;
```
위와 같이 사용하면 된다.  

```c++
// typedef는 불가능한 문법
template<typename T>
typedef vector<T> type;

// typedef로 템플릿을 사용하기위해서는
// 무조건 구조체나 클래스를 이용해서 사용해야한다.
template<typename T>
struct xvector
{
  typedef vector<T> type;
}

xvector::type vec;

// using은 구조체나 클래스를 이용하지 않고도 가능하다.
template<typename T>
using type = vector<T>

type vec; //훨씬 간단하다.
```

## typename

## dependent type(의존 타입 또는 의존 네임)
템플릿 매개변수에 종속된 변수를 의존 타입이라고한다.
```c++
class R{
  typedef int INT;
};
template<typename T>
void func()
{
    T::INT a; // x
    //typename T::INT a // o
}

func<R>(); // 컴파일에러
```
INT 변수는 템플릿 매개변수 T에 종속된 변수(dependent type)이다. 이런경우 템플릿을 받는 함수에서 T::INT 앞에 typename을 써줘야한다. typename을 써주는 이유는 INT가 R의 변수 타입일 수 도있지만 일반 변수일 수도있다. typedef int INT가 아닌 int INT = 10;이라면 T:INT a라는 구문은 변수 INT a라는 구문이되고 이는 문법 오류이다. 이렇기 때문에 의존 타입(템플릿 매개변에 종속된 변수)은 무조건 typename을 지정하여 자료형임을 명시해야한다. INT는 클래스에 정의된 변수이기 때문에 이는 nested dependent name이 정확한 표현이다. 

## typename이 불필요한경우
부모 클래스에 있는 변수는 종속 변수가 아니기 때문에 typename을 붙일 필요가 없다.

## template

### 템플릿 매개변수에서 레퍼런스(Reference Collapsing)
```c++
template<class T>
void func(T& A)

int a = 1;

//test
func<int>(a);
func<int&>(a);
func<int&&>(a);
```
위와 같은 선언이 있다고 할 때 test 아래에 경우는 모두 가능하다. 가능한 이유는 일단 모두 Lvalue라는 것이다. 그리고 실제 T에 자료형이 대입댈 때 int & A, int& &A, int&& & A, 총 세가지 경우인데 c++에서는 변수의 레퍼런스, 레퍼런스의 레퍼런스, 레퍼런스의 레퍼레퍼런스 들은(Reference collapsing) 컴파일러가 모두 lvalue 레퍼런스로 변화시켜준다.  

```c++
template<class T>
void func(T&& A)


//test
func<int>(10);
func<int&>(10); //불가능 int& &&A-> int& A
func<int&&>(10);
```
그럼 위와 같은 Rvalue reference같은 경우에서 레퍼런스 충돌을 보면 int &&A, int& &&A, int&& && A, 경우인데 위에서 설명한것과 같이 하나의 lvalue reference가 있다면 무조건 lvalue reference으로 바꾸기 때문에 2번 test만 불가능하게된다.


## std::vector

### ■ 원소 지우기
벡터는 콘테이너에 있는 원소를 삭제할 수 있다. 원소를 삭제하는 특정 원소를 지정해서 삭제하거나 특정 범위를 지정하여 범위 전체를 삭제할 수 있다. vector 내의 원소를 삭제하기 위해서 erase 메소드를 사용해서 삭제 가능하다. 원소를 삭제하면서 중요해야할 점은 반복문을 쓰면서 원소를 제거하는 경우이다. 벡터는 원소를 삭제할 시 뒤에 원소를 앞으로 당겨오는 것으로 내부를 처리한다. 인덱스를 사용하면서 원소를 제거하면 제대로 제거가 안되거나 배열의 사이즈 크기를 넘어서 잘못된 공간을 불러올 수 있다. 내부적으로 에러로 처리하게 된다.

#### 인덱스로 반복문을 돌리는 경우 - 제거하려는 원소가 제대로 제거가 안되는 경우
```c++
#include <iostream>
#include <vector>

using namespace std;

int main(void)
{
    vector<int> list;

    for(int i = 0; i < 50; i++)
        list.push_back(1); // 1만 삽입

    
    for(int i = 0; i < list.size(); i++)
        if(list[i] == 1) // 모든 1을 제거하기
        {
            list.erase(list.begin() + i);
        }

    for(int i = 0; i < list.size(); i++) 
        cout << list[i] << ' '; // 모든 1이 제거가 안된다.

}

//출력 : 모든 1이 삭제가 안된다.
//1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 
```
위의 코드는 작성자가 모든 1을 제거할 목적으로 작성한 코드지만 출력결과를 보니 모든 1은 제거가 되지 않았다. 원소를 삭제 할때마다 벡터는 뒤 원소를 앞으로 당겨오지만 반복문의 인덱스 번호는 증가되므로 제대로 삭제를 할 수가 없다.

#### 인덱스로 반복문을 돌리는 경우 - 잘못된 공간 참조로 인한 오류가 발생하는 경우
```c++
#include <iostream>
#include <vector>

using namespace std;

int main(void)
{
    vector<int> list;

    for(int i = 0; i < 50; i++)
        list.push_back(1); // 1만 삽입

    size_t size = list.size();
    for(int i = 0; i < size; i++)
        if(list[i] == 1) // 모든 1을 제거하기
        {
            list.erase(list.begin() + i);
        }

    for(int i = 0; i < list.size(); i++) 
        cout << list[i] << ' '; // 모든 1이 제거가 안된다.
}
//출력 : 잘못된 공간 참조
//segmentation fault
```
앞에 코드와 유사하지만 이번엔 반복문을 돌면서 벡터의 사이즈를 계속 확인하는 것이아닌 초기 벡터의 사이즈를 변수에 저장해 놓고 사용하는 경우이다. 이러면 원소가 삭제 될때마다 벡터의 크기가 줄어들어 벡터내부 size 변수도 줄어들게 된다. 하지만 인덱스번호는 계속 증가하여 벡터의 사이즈보다 큰 공간을 참조하려하려 하기 때문 segmentation fault 오류를 발생한다. 

#### erase를 잘 사용하여 올바르게 원소를 제거하자!
erase 메소드는 반환값으로 재조정된 다음 원소의 반복자를 반환하게 된다. 그렇기 때문에 반복문을 사용하여 원소를 삭제하기 위해서는 반복자를 이용하여 삭제해야한다. 아래에 코드가 올바르게 삭제하는 방법의 코드이다.
```c++
#include <iostream>
#include <vector>

using namespace std;

int main(void)
{
    vector<int> list;

    for(int i = 0; i < 50; i++)
        list.push_back(1); // 1만 삽입

    std::vector<int>::iterator iter = list.begin();
    while(iter != list.end())
    {
        if(*iter == 1)
            iter = list.erase(iter); // 삭제할 원소라면 삭제 이후 재조정된 다음 원소의 반복자
        else
            iter++; // 삭제할 원소가 아니면 다음 원소의 반복자
    }
    for(int i = 0; i < list.size(); i++) 
        cout << list[i] << ' '; // 모든 1이 제거가 안된다.
}
```

#### std::vector<T,Allocate>::erase 메소드
earse 메소드는 두 가지 형태를 지원한다. 범위를 지정해서 삭제하거나 원소의 반복자 위치로 삭제할 수 있다. 삭제 후에는 새로 업데이트된 다음 원소의 반복자를 반환한다.


## Algorithm
### bool std::next_permutation
해당 배열 또는 컨테이너의 원소를 다음 순열로 정렬한다. 처음 실행하기 위해서는 무조건 오름차순으로 정렬 되어있어야하며 마지막 순열에서서는 첫번재 원소로 변경 후에 false를 반환한다.


