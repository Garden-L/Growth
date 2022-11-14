# make Programe

<br></br>
## make
### ■ make 프로그램이란?
소스 파일을 이용하여 자동으로 실행파일 또는 라이브러리 파일을 만들어주는 빌드 관련 유틸리티이다. 빌드를 편리하게 해주는 여러가지 문법을 지원하여 빌드 시간을 단축 시킬 수 있다. 각 소스파일의 의존 관계나 빌드 순서를 정보가 필요한데 이러한 정보를 저장한 파일이 makefile이다.

### ■ 예
OS의 커널을 실행하기 위해서는 분할된 C나 어셈블리 파일을 하나의 파일로 모아야한다. 이때 하나하나 사람이 빌드 작업을 할 수가 없다. 이럴때 make프로그램을 이용하면 한번에 많은 소스코드를 빌드 시킬 수 있다.

<br></br>
## 문법
### ■ 기본 형식
기본형식은 target, Dependency, Command 세부분으로 구성되어있다. 
```make
Target: Dependency
<tab> Command
<tab> Command
<tab> Command
```
* target : 일반적으로 생성할 파일을 나타낸다.
* Dependency : Target 생성에 필요한 소스 파일이나 오브젝트 파일을 나타낸다.
* Command : Dependency에 관련된 파일이 수정되면 실행할 명령을 의미한다. cmd나 shell에서 실행할 명령 또는 프로그램을 기술한다.
* \<tab> : 탭으로 표시한 부분은 반드시 tab 문자로 띄워야한다. 공백대체는 불가능하다.
* \# : '#'문자 이후 같은 줄은 모두 주석처리된다.

#### 작성 예시
```make
# a.c, b.c 파일을 exe 파일로 생성하는 makefile

all : output.exe

a.o: a.c
  gcc -C a.c

b.o: b.c
  gcc -C b.c

output.exe: a.o b.o
  gcc -o output.exe a.o b.o
```
최종적으로 만들어낼 output.exe의 Dependency 파일들은 미리 처리해야한다. 

### ■ Target
일반적으로 생성할 파일을 이름을 나타낸다.  
#### 특별한 Target "all"
all이라고 지정된 타겟은 make 명령어를 실행했을 때 디폴트로 실행되는 타겟이다.
``` bash
$ make all

// make 명령어는 디폴트 타겟으로 all이므로 make와 make all은 같다.
$ make       
```

```make
# a.c, b.c 파일을 exe 파일로 생성하는 makefile

# all 타겟이 설정되어 있지 않으면 a.o부터 자동으로 실행된다.
all : output.exe 

a.o: a.c
  gcc -C a.c

b.o: b.c
  gcc -C b.c

output.exe: a.o b.o
  gcc -o output.exe a.o b.o
```

### ■ make 옵션
#### -C 폴더
"make -C 폴더" 명령어는 해당 디렉토리로 이동 후 make를 명령어를 실행한다. makefile안에 make -C 디렉토리를 사용할 경우 현재 make 실행을 멈추고 해당 디렉토리의 make명령을 실행시킨 후 다시 호출했던 곳에서 나머지 처리를 한다.
```make
all: output.exe

# Library 디렉토리로 이동한 후 make 수행
libtest.a:
  make -C Library

# Library 디렉토리의 makefile 실행이 다 끝난 후 나머지 처리 (계층적인 실행)
output.o: output.c
  gcc -C output.c

output.exe: libtest.a output.o
  gcc -o output.exe output.c -ltest -L./
```
  
