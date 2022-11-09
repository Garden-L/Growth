# ARM

<br></br>
# Register
> 다양한 레지스터에 대한 설명입니다.
## Base register란?
베이스 레지스터는 데이터의 full(전체), absolute(절대), virtual(가상) 주소를 접근할 수 있는 주소이다. C에서 포인터의 역할과 같다. 베이스 레지스터의 값을 참조하려면 레지스터 이름을 대괄호에 넣어서 사용하면된다(\[X\]). arm에서 베이스 레지스터는 X레지스터이다(X1, X2,...)

## Segment register란?
메모리 분할과 관련한 레지스터를 의미한다. 보호모드에서는 세그먼트 셀렉터로 이름을 변경하였다. 디스크립터 테이블을 가르킨다.

## General-Purpose Register(범용 레지스터)
### 범용 레지스터란?
메모리 주소나, 값을 저장하는 용도의 레지스터, 하위 비트를 호환하기 위해 32비트 레지스터에서는 32비트일땐 EAX, 16비트일땐 AX, 상위 바이트는 AH, 하위바이트는 AL로 나타낸다. 현대의 컴퓨터는 최대 64비트의 레지스터를 사용하는 64비트 레지스터는 접두사 R이 붙어 RAX가 된다. 사용하는 용량에 따라 이름만 다를 뿐이지 하나의 레지스터이다.
### 포맷
<img width="407" alt="image" src="https://user-images.githubusercontent.com/56042451/200746505-e3fd4432-06b3-4223-be89-d0293d8113bb.png">

### 범용레지스터의 종류
#### EBX
Data segment(리얼모드, 보호모드에서는 Data segment Selector)의 데이터를 포인팅하는 레지스터

#### ESI(Extended Source Index)
현재 DS register에 의해 포인팅된 세그먼트 내에 있는 데이터의 인덱스 번호를 저장하고 있는 레지스터

## Control register
### control register란?
다양한 운영모드를 제어하는 레지스터
#### CR0 
운영모드를 제어하는 레지스터. 80386 시스템에서는 32bit길이이다. 64비트 시스템에서는 64비트 길이를 갖는다. 과거 16비트 MSW(Machine State Word)레지스터에 발달이 CR0로 변화한 것이다.  
<p align="center"><img width="628" alt="image" src="https://user-images.githubusercontent.com/56042451/200716546-f53f67f0-080e-4831-b33b-6a20baf48950.png"></p>
* PG(Paging Enable) : PG 비트가 1로 설정되면 페이징 유닛을 사용할 수 있다.
* PE(Protection Enable) : PE비트가 1로 설정되면 보호모드로 진입할 수 있다.

## Instruction set
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
