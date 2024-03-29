## Base64
### Base64란?
아스키 코드의 a-z, A-Z, 0-9 총 62개의 문자 + 2개의 '+','/'를 이용한 64진법(2^6)의 인코딩 방식이다. 보통 앞 62개의 문자는 공통으로 사용되며 63,64번째 문자는 라이브러리마다 다를 수는 있다. base64는 암호화하는 것이 아닌 단순한 바이너리 데이터를 base64의 문자 테이블을 이용하여 base64 문자열로 변환시킨 것을 의미한다.

#### Base64 문자 테이블
<p align="center"><img width="634" alt="image"  src="https://user-images.githubusercontent.com/56042451/209285554-a88588dd-023d-49cb-955f-8b3441f63218.png"></p>

### Base64 인코딩 과정
#### Base64의 버퍼 
Base64는 버퍼 하나 당 24비트(3바이트)를 기본으로 설정한다. 버퍼 하나 당 4개의 문자가 출력되게 된다. 만약 아스키코드로 표현된 문자가 3개라면 Base64 버퍼 하나에 정확히 알맞을 것이다. 아스키 문자 1개나 2개만 들어간다면 어떻게 될까? 1개만 들어간다면 나머지 16비트는 모두 0으로 채워지고 앞 두 문자 출력 후 '='문자 2개를 출력한다. 아스키 문자가 두 개만 들어간다면 나머지 8비트는 0으로 채워지고 앞 세문자 출력 후 '='문자 1개를 출력한다.

#### 문자열 "ABCD"를 Base64로 인코딩하는 과정. 
1. "ABCD" 아스키코드 -> 65 66 67 68
2. 이진수로 변환 -> 01000001 01000010 01000011 01000100
3. 1번째 버퍼가 24비트를 읽어서 변환한다. 010000 010100 001001 000011 -> QUJD
4. 2번째 버퍼가 24비트를 읽어서 변환한다. 010001 00 
  4-1. 24비트가 안되고 아스키 문자 하나만 들어갔으므로 나머지는 0으로 채우고 마지막에 '='문자 두개를 추가한다. -> RA==
5. 최종 합치면 -> QUJDRA== 으로 변환된다.

### Base64의 장점
Base64로 인코딩하게되면 데이터의 크기가 증가하게 된다. 위에서 "ABCD"는 총 4바이트이지만 Base64로 변환했더니 8바이트로 증가되었다. 무려 2배로 증가되었다. 보통 문자하나당 4/3크기로 변환된다. 크기만 문제가 되는 것이 아니라 인코딩 디코딩 과정의 비용까지 생각한다면 장점이 없는 것 같지만 Base64를 사용하는 이유는 무엇일까?
#### OS의 charset
OS마다 ascii 일부 문자 집합의 정의가 다르다. 하지만 a-z, A-Z, 0-9의 이진 코드는 모든 OS가 같다. 즉 a의 아스키 코드는 97번으로 같다는 것이다. 그렇기 때문에 Base64는 모든 OS가 같은 아스키로 인식하는 문자들로 인코딩하는 것 이다. 정확히 다음 글에서 이유를 알 수 있을 것이다.

#### 애플리케이션 수준의 인코딩
A와 B는 채팅 중이라고 가정하자. A는 B에게 "##"이란 메세지를 보냈다. 그런데 A의 OS는 맥이고 B의 OS는 윈도우다. 맥에서는 '#'문자가 아스키로 0011 0000이고 윈도우에서는 이진수 0011 0000이 '$' 라고 가정하자. 그럼 A의 OS는 채팅프로그램의 메세지를 "00110000 00110000"으로 변환해서 B에게 보낼 것이다. B의 OS는 이진 데이터를 $라고 인식하고 B의 채팅 프로그램에게 변환하여 보낼 것 이다. 이렇게 OS마다 정의된 문자집합이 달라서 올바른 문자를 표시할 수 없다. 그럼 여기서 단순히 B의 채팅 프로그램에서 $라고 인식되는 이진수를 #으로 표시되게 애플리케이션 수준에서 새로 문자집합을 정의해주면 되지 않나?라고 생각할 수 있다. 예시가 OS 2개로만 되서 그렇지 만약 리눅스, 유닉스 ... 등 모든 OS마다 채팅 프로그램이 알맞게 돌아가도록 정의해야한다. OS마다 알맞게 작동하기 위해 애플리케이션 수준의 문자집합을 재정의하는 것은 힘든 일이 될 것이다. 이런 어려움 때문에 모든 OS가 공통으로 정의된 a-z, A-Z, 0-9를 이용한다. 그럼 이제 애플리케이션 레벨에서 #은 00001111이라고 정의하자. A의 채팅프로그램은 A가 보낼 메세지인 "##"을 00001111 00001111로 변화하여 Base64로 인코딩 후 OS에 메세지를 B에 보내달라고 요청한다. Base64로 변환된 코드는 ABC라고하자. 그럼 B에서 OS는 ABC를 받을 것이다. 그럼 B의 채팅프로그램은 Base64로 디코딩을 진행하면 00001111 00001111로 디코딩 결과값을 도출할 것이다. B의 채팅 프로그램은 00001111은 #이라고 정의되어 있으니 ##을 출력할 것이다. OS별로 문자집합을 따로 정의 하지 않고 애플리케이션 레벨로 단 한번의 정의로 모든 OS에서 지원 가능한 애플리케이션이 되었다. 


