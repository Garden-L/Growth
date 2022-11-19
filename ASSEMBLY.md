# ARM

<br></br>
## Register
> 다양한 레지스터에 대한 설명입니다.
### Base register란?
베이스 레지스터는 데이터의 full(전체), absolute(절대), virtual(가상) 주소를 접근할 수 있는 주소이다. C에서 포인터의 역할과 같다. 베이스 레지스터의 값을 참조하려면 레지스터 이름을 대괄호에 넣어서 사용하면된다(\[X\]). arm에서 베이스 레지스터는 X레지스터이다(X1, X2,...)

### Segment register란?
메모리 분할과 관련한 레지스터를 의미한다. 보호모드에서는 세그먼트 셀렉터로 불린다. 세그먼트 디스크립터 테이블을 포인팅한다. 8086 시스템에서는 논리 주소메모리를 물리주소로 맵핑하기 위해 세그먼트 레지스터에 4비트만큼 우측으로 이동시킨 후 오프셋을 더해서 만들었다. 논리주소를 물리주소로 맵핑 하는 레지스터는 세그먼트 레지스터 뿐이다. 세그먼트레지스터에 들어있는 모든 주소는 물리주소로 맵핑하기 위한 처리가 진행된다.

#### ES(Extra Segment)
ES 레지스터는 보조 세그먼트 레지스터로 데이터를 수신하여 저장할 위치를 가리킨다. 바이오스의 인터럽트 13을 호출하면 ES레지스터를 사용하는 것을 알 수있다. https://ostad.nit.ac.ir/payaidea/ospic/file1615.pdf
#### CS(Code Segment)
실행될 기계 명령어 포함한다. 코드 세그먼트의 주소를 가리키는 레지스터. 
#### DS(Data Segment)
프로그램에서 정의된 데이터, 상수가 정의된 세그먼트의 주소를 가리키는 레지스터
#### SS(Stack Segment) 
지역변수처럼 임시로 저장할 필요가 있는 데이터를 저장하는 세그먼트를 가리키는 레지스터

### ■ 플래그 레지스터(Flag Register)
상태를 저장하는 레지스터이다. 여기서 상태란 현재 cpu에서 연산된 결과에 대한 것이다. 산술 명령어를 이용하는 경우 결과에 대한 상태가 저장된다. 

#### 플래그 레지스터 종류
* ZF(Zero Flag) : 단일 비트 플래그로써 산술 연산 결과가 0 인 경우 1로 설정된다. 산술 결과가 0이 아닌 수가 나온다면 리셋 된다. 대표적으로 and, or, xor연산의 결과 값이 0이면 1로 설정된다. 
* CF(Carry Flag) : 덧셈 또는 뺄셈의 경우 범위를 초과하는 경우
* SF(Sign Flag) : 연산 결과가 음수 일때, 최상위 비트가 1로 설정된 경우(2의 보수)
* 

## General-Purpose Register(범용 레지스터)
### 범용 레지스터란?
메모리 주소나, 값을 저장하는 용도의 레지스터이다. 특정 목적이 있지만 사용자 마음대로 사용할 수는 있다. 하위 비트를 호환하기 위해 32비트 레지스터에서는 32비트일땐 EAX, 16비트일땐 AX, 상위 바이트는 AH, 하위바이트는 AL로 나타낸다. 현대의 컴퓨터는 최대 64비트의 레지스터를 사용하는 64비트 레지스터는 접두사 R이 붙어 RAX가 된다. 사용하는 용량에 따라 이름만 다를 뿐이지 하나의 레지스터이다.
### 포맷
<img width="407" alt="image" src="https://user-images.githubusercontent.com/56042451/200746505-e3fd4432-06b3-4223-be89-d0293d8113bb.png">

### 범용레지스터의 종류
* EAX (Accumulator Register) : 누산기 레지스터이다. 데이터 전송 및 연산 후 결과를 잠시 저장하는 레지스터이다.
* EBX (Base Register) : 메모리에 저장되어 있는 데이터 주소를 저장하거나 데이터를 저장한다.
* ECX (Count Register) : 반복문이나 문자 스트링과 같은 명령어 수행할 때 반복 횟수 등을 저장하는 카운터로 사용.
* EDX (Data Register) AX 레지스터와 함게 곱셈 나눗셈 등 일부 명령어에서 필수적으로 사용한다.

<br></br>
## 인덱스 레지스터(Index Register)
### 인덱스 레지스터란?
특정 세그먼트 레지스터의 지정되어 있는 주소와 연계하여 오프셋으로 쓰이는 레지스터. 범용레지스터로도 쓰이지만 범용레지스터처럼 공간을 나누어 사용할 수 없다.

### 인덱스 레지스터의 종류
* ESI(Extended Source Index) : 현재 DS register에 의해 포인팅된 세그먼트 내에 있는 데이터의 인덱스 번호를 저장하고 있는 레지스터.
* EDI(Extended Destination Index) : 현재 ES 레지스터에 의해 포인팅된 세그먼트 내에 있는 데이터의 인덱스 번호를 저장하고 있는 레지스터.

<br></br>
## 포인터 레지스터(Pointer Register) 또는 주소레지스터
### 포인터 레지스터란?
특정 주소를 가리키고 있는 레지스터.

### 포인터 레지스터의 종류
* SP(Stack Pointer) : 기계어 스택에 들어 있는 데이터의 위치를 가리키는데 쓰인다. SS 레지스터의 오프셋으로 사용된다.
* BP(Base Pointer) : SP대신 스택 내에서 데이터를 액세스 할 때 사용한다.
* IP(Instruction Pointer) : CS 레지스터와 함께 사용되는데 CPU에서 다음으로 실행될 명령의 논리 주소를 저장한다. 이 논리주소는 CS레지스터의 오프셋으로 사용된다.

## Control register
### control register란?
다양한 운영모드를 제어하는 레지스터
#### CR0 
운영모드를 제어하는 레지스터. 80386 시스템에서는 32bit길이이다. 64비트 시스템에서는 64비트 길이를 갖는다. 과거 16비트 MSW(Machine State Word)레지스터에 발달이 CR0로 변화한 것이다.  
<p align="center"><img width="628" alt="image" src="https://user-images.githubusercontent.com/56042451/200716546-f53f67f0-080e-4831-b33b-6a20baf48950.png"></p>
* PG(Paging Enable) : PG 비트가 1로 설정되면 페이징 유닛을 사용할 수 있다.
* PE(Protection Enable) : PE비트가 1로 설정되면 보호모드로 진입할 수 있다.

<br></br>
## Intel Instruction set
### MUL A
A와 EAX 레지스터의 값과 곱하여 EAX레지스터에 저장한다.
```assembly
MUL si ; si*ax
```
### ■ JMP
#### JMP A
특정 주소로 분기시키는 명령어, EIP 레지스터에 분기할 주소를 담는다
```assembly
jmp A 

; 동작과정
mov eip, A;
```
#### JMP A:offset
특정 주소로 분기시키는 명령어
```assembly
JMP 0x1:0x5 ; CS 레지스터에 0x1을 적재하고, IP(또는 PC) 레지스터에 0x5를 적재한다. 
```

### ■ CMP dest, source
두 값을 비교하는 명령어. dest 값에서 source 값을 빼서 0이 나오면(일치) ZF(제로 플래그 레지스터)를 1로 설정한다. 

### ■ JCC
조건 점프, 특정 조건이 일치하면 점프명령어를 수행한다.

#### JE dest
Jump if Equal, cmp명령어와 함께 자주 쓰인다. ZF(제로 플래그 레지스터)가 1로 설정되어 있으면 dest로 분기한다.
```assembly
cmp 1, 0 ; ZR = 0
je dest ; ZR=0 이므로 목적지로 분기 못함

cmp 1, 1; ZR=1
je dest ; ZR=1 이므로 목적지로 분기함
```

#### JL dest
Jump if Less, cmp 명령어와 함께 쓰인다. (SF XOR OF)=1 인 경우 dest로 분기한다
```assembly
cmp 1, 10 ; SF=1, OF=0 -> SF XOR OF = 1
jl dest ; 목적지로 분기가능
```


### ■ call
함수를 호출할 때 쓰이는 명령어, call 명령어로 함수를 호출하면 현재 스택 공간의 다음에 실행해야할 주소값을 적어 두고 함수를 호출하게 된다.
```assembly
call func 

//실제 작동과정
push EIP  ; 다음에 실행될 명령어 주소를 현재 스택에 담는다
jmp func  ; 
call FUNC ; EIP = FUNC
```



<br></br>
## ARM Instruction set
### STR
store 명령으로 레지스터의 값을 메모리에 적재하는 것을 의미한다.

```assembly
; 기본 저장
STR X1, [SP]
1. [SP] = X1    ; X1의 값을 SP가 참조하는 메모리의 공간에 저장

;Unscaled된 값 
STUR WZR, [SP] 
1. [SP] = 0     ; WZR은 전체가 0으로 설정된 레지스터이다. WZR 레지스터에는 값을 적재해도 무조건 0으로 설정된다.
```

### LDR
레지스터에 값, 또는 주소를 적재하는 명령어

#### 예시
![image](https://user-images.githubusercontent.com/56042451/197333371-c8f66520-ff55-4d85-abfd-f264d5b6d2a4.png)

#### LDR W0, \[X1\]
X1이 가리키는 메모리 주소의 값을 W0 레지스터에 적재한다. 즉 위에 예에서 W0 레지스터의 값은 X1이 가리키는 공간의 값 20이 적재된다.

#### LDR W0, \[X1, #12]
X1의 주소값에서 offset 12bytes 위치에 있는 주소를 참조하여 W0 레지스터에 적재한다. X1에서 12만큼 떨어진 공간을 참조하므로 W0에 50이 적재된다

#### LDR W0, \[X1, #12]! 
느낌표(!)의 의미는 C언어의 덧셈 복합 대입연산자와 같다. X1의 값에 offset 12bytes만큼 더해 다시 X1 레지스터에 대입하는 것이다(X1 += 12). X1의 담겨진 주소값이 12bytes만큼 증가한 것이다. 즉 최종 연산결과는 X1 레지스터에 0xc가 들어가므로 W0에는 50이 적재된다.

#### LDR W0, \[X1], #12
W0에는 X1의 참조값이 대입되고 X1에 담겨진 주소값은 12만큼 증가된다. W0 = 50이고, X1 = 0x0 에서 X = 0xc로 12만큼 증가한다.

### Load Pair and Store pair
arm에서는 적재 명령어(LDR) 저장 명령어(STR) 명령어를 2번 연속으로 지원하는 명령어이다.

#### LDP W3, W7, \[X1]
LDP 명령어는 적재를 두 번한다. W3 레지스터에 X1의 참조값을 적재하고 W7에 \[X1+4]에 참조값을 적재한다.

#### STP D0, D1, \[X4]
STP 명령어는 저장을 두 번한다, D0를 X4 메모리 공간에 저장하고, D1을 \[X4+8] 메모리 공간에 적재한다.

#### STP X0, X1, \[SP, #-16]!
이 연산은 스택의 푸쉬 연산과 같다. SP는 스택을 가리키는 스택 포인터 레지스터이다.
```assembly
STP X0, X1, [SP, #-16]!

1. SP = SP -16  ; [SP, #-16]! = ADD SP, SP, #16
2. [SP] = X0    ; STR X0, [SP] , 값 하나 푸쉬
3. [SP+8] = X1  ; STR X1, [SP, #8], 연속적인 공간에 두 번째 값 푸쉬
```

#### LDR X0, X1, \[SP], #16
이 연산은 스택의 팝 연산과 같다.
```assembly
LDR X0, X1, [SP], #16

1. X0 = [SP]    ; LDR X0, [SP], 위의 STP 연산에서 스택 포인터 0에서 -16으로 변했다고 했을 때 현재 가리키고 있는 메모리 주소공간의 값을 적재
2. SP = SP + 16 ; ADD SP, SP, #16
3. X1 = [SP]    ; LDR X1, [SP]
```
