# C++
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



