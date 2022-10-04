# C++

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

