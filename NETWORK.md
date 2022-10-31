# Computer Network
## base
### Network Edge : Host, Applications
end systems(hosts)
client/server model
sever : 항시 on 되어있어서 클라이언트의 요청을 기다림
peer-peer model
### Connection-oriented(연결지향) service
통신을 제공하는 방법
TCP 서비스 : 사용자에게 reliable(신뢰적 전송), in-order byte-stream data transfer(순서대로 전송), flow control
플로우 콘트롤 : receive가 받는 속도에 맞춰서 샌더의 데이터를 전송한다.
congestion control : 네트워크 상황에 맞춰서 데이터를 전송한다.
computer resource, network resource 비용이 많이 든다.  web, FTP
### connectionless(비연결성) service
UDP 서비스 : connectionless, unreliable data tranfer, no folow control, no congestion control
udp를 사용하는 경우 : 받는 사람의 상관없이 무조건적으로 데이터를 전송한다. 도착한다는 보장이 없음. voice-ip, 실시간 음성 데이터 전송, 
Network Core : Router, Network of network (네트워크의 네트워크)
access Network : physical media, communications links

### protocol?
의례, 규약 이라는 단어로 Network Protocol은 네트워크의 통신 규약을 의미한다. 서로 약속된 규칙으로만 통신을 해야 개발할 시 혼선이 없다. 같은 프로토콜끼리만 통신이 가능하다.

### Network core
네트워크 가운데 라우터라는 데이터 전달 기기가 존재.  
* 서킷 스위칭 : 출발지에서 목적지까지 가는 길을 예약하고 특정 사용자만 사용하도록 한다. 유선 전화망
  + band width가 1Mbps, 유저의 전송속도 100Kbps 라면 최대 10명까지 연결을 지원한다.
  + 인터넷 사용자들은 계속 데이터를 전송하지 않는다. 대역폭의 
* 패킷 스위칭 : 사용자 보내는 패킷을 그때 그때 마다 목적지로 forwording 해준다.
  + band width에 상관없이 N명의 유저를 지원한다.(사실 이런 개념이 없음)
  + 패킷 검사(processing delay) -> queueing(buffer) delay(유저의 네트워크 사용량) -> transmission delay -> propagation delay(링크길이를 빛의속도로 나눈것)
### 통신서비스 제공자

### Socket
OS 수준에서 제공하는 API, Application program, OS 에 transport layer가 구현되어있음(TCP, UDP만), tcp socket과 udp소켓은 아예다른 소켓이다.  
tcp => data stream, udp -. data gram

## Application Layer

### HTTP (Hypertext Transper Protocol)

#### URL Structure
![image](https://user-images.githubusercontent.com/56042451/178882344-40ced9b7-03be-4c68-a98e-4cb27d55459b.png)
1. Protocol : 통신 규격, 웹상에서는 http 프로토콜을 이용한다.
2. host address: 도메인 네임 혹은 ip주소가 들어간다.  
3. port : 통신할 프로세스의 주소이다. 웹서버는 기본으로 80포트를 사용하기 때문에 생각해도 상관없다.
4. path : 사용자가 찾고자하는 파일의 서버 내 경로이다
5. query : key=value 형태로 path 뒤 ?를 기점으로 사용자가 요청하고자하는 데이터를 요청한다.

## Transport layer
상위레이어 application layer에 서비스를 제공.
### UDP (User datargram protocol)
에러검출, multiplexing, demultiplexing

### TCP
#### reliable data transtfer
message error, message loss
<hr/>  


### RDT(reliable data transport)
송수신 하는 데이터가 오류없이 전송되는 것을 의미한다. Transport layer에서 제공하기 때문에 하위레이어에서 따로 구현할 필요가 없다. 하위레이어는 데이터가 목적지에 빠른 도착만을 추구하기 때문에 신뢰성 있는 데이터 전송은 Transport layer에서 구현했다.  

#### RDT의 중요요소
* packet error
* packet loss
신뢰적인 데이터 전송을 하기 위해서는 패킷의 에러, 패킷 유실 두 가지를 해결해야한다. 패킷 에러는 패킷 전송 중에 일어날 수 있는 데이터 변이를 뜻하고, 패킷 유실은 패킷 전송 중 패킷이 목적지까지 제대로 도착을 못하는 것을 의미한다. 패킷 유실의 대표적으로 라우터의 큐가 가득차 라우터에서 패킷을 버리는 것이다.

#### RDT 1.0 (reliable transfer over a perfect channel)
RDT 1.0은 채널이 완전하게 안정적이라고 생각한다. 즉 신뢰적 전송의 중요 요소 에러와 유실이 발생하지 않는다고 보는 것이다. 에러와 유실이 발생하지 않으면 무조건적으로 데이터가 송신측에서 수신측으로 올바르게 전송된다는 것을 의미하므로 패킷 송신자와 수신자는 패킷의 에러와 유실을 걱정할 필요가 없다. **현실적으로 이런 상황은 매우 비현실적이다.**  
![image](https://user-images.githubusercontent.com/56042451/178205493-529ce115-8c8f-4db1-b75c-b668d3e15bd5.png)


#### RDT 2.0 (channel with packet error)
RDT 2.0은 패킷에 유실은 일어나지 않지만 **에러**가 발생 할 수 있다는 가정을 한다. 에러를 발생 유무를 확인하기 위해서는 일단 패킷에 **Error detection**을 체크하는 항목과 에러를 확인하고 다시 재전송 해줄것을 요청하는 feedback(Acknowledgements(acks), negative acknowledgements(naks)) 마지막으로 재전송하는 메카니즘이 필요하다.  

* Error detection 방법
  + 패킷 내부에 에러를 탐지할 수 있는 요소를 삽입하고 Transfer Layer에서 확인한다. 대표적으로 Checksum bit
* feedback 방법
  + ACKs : 수신측이 송신측에 패킷을 제대로 받았다고 응답한다.
    - 송신측에서 ACKs가 담긴 패킷을 받으면 연결을 종료한다.
  + NAKs : 수신측이 송신측에 패킷에 오류가 있다고 응답한다.
    - 송신측에서 NAKs가 담긴 패킷을 받으면 해당 패킷을 재전송한다.  
![image](https://user-images.githubusercontent.com/56042451/178205889-cdc702e4-3bea-4be4-8bd8-5e400523685b.png)


<font color="green"> **RDT 2.0의 결함**</font>  
ACKs와 NAKs도 패킷에 담겨저 보내지기 때문에 패킷 오류가 발생할 수 있다(패킷 유실은 RDT 2.0에서 고려하지 않음). 패킷 오류가 일어날 시 sender에서는 NAK 신호로 인식하고 패킷을 재전송하게 된다. 이런 현상의 첫번째 문제는 응답 패킷이 계속적으로 오류가 난다면 sender에서는 무한히 동일 패킷을 보낼 것이고 이는 네트워크에 큰 부하를 가하게된다. 두번째 문제는 sender에서는 오류로 인해 동일 패킷을 전송했지만 receiver에서는 이게 오류로 인한 패킷 재전송인지 새로운 패킷인지 구분하지 못한다.(duplicate packets problem)

#### RDT 2.1(handles garbled ack/naks)
RDT 2.0의 duplicate packets 결함을 제거하기 위해 고안된 방법. 패킷마다 sequence 번호를 붙여서 송수신을 한다. 이때 시퀀스번호는 transport layer의 header에 담기게 되는데 header의 크기가 커지면 이 또한 전송에 오버헤드이므로 최소화 해야한다. 시퀀스 번호는 두 개(0,1)로만 변경되는 것을 표현하면 되므로 1 bit면 충분하다.

* 동작
  + 1. sender가 처음 보내는 패킷에 sequence #0으로 receiver에게 전송
  + 2. receiver는 패킷에 오류가 없다면 sequence #0번 패킷이 잘 전송되었다는 ACK 시그널이 담긴 패킷을 sender에게 전송
    - 2-1. receiver는 패킷에 오류가 있다면 sequence #0번 패킷이 오류가 있다는 NAK 시그널이 담긴 패킷을 sender에게 전송
  + 3. sender는 두 번째 패킷에 sequence #1으로 receiver에게 전송
    - 3-1. NAK 시그널이 담긴 패킷을 받으면 다시 해당 sequence 번호로 재전송
    - 3-2. 오류가 있는 시그널 패킷도 NAk로 인식하고 receiver에게 해당 번호로 재전송
  + 4. receiver는 sequence 번호만 변경된채로 2, 2-1 반복

![image](https://user-images.githubusercontent.com/56042451/178208497-268d3d79-1432-453f-8234-f6059091cf07.png)


#### RDT 2.2
RDT 2.2는 NAK를 사용하지 않고 ACK만 사용한다. 사용가능한 이유는 sequence 번호가 있기 때문에 sequence + ack조합으로 사용한다면 nak는 사용하지 않을 수 있다.


### RDT 3.0
RDT 1.0 부터 RDT 2.2까지는 packet error에 관해서만 다뤘다. RDT 3.0에서는 에러 뿐만 아니라 packer loss를 추가하여 완벽한 신뢰적인 데이터 전송을 완성한다.  
RDT 3.0에서 패킷 유실을 해결하기위해 Timer를 도입했다. 합리적인(resonable)시간 안에 ACK 메세지가 오지않는다면 유실이라 간주하고 패킷을 재전송하는 것이다.

### Selective Repeat (선택적 반복, SR)
GBN의 주요 문제점은 패킷이 하나만 유실돼도 윈도우안에 있는 응답받지않은 모든 패킷을 불필요하게 전송하는 것이다. 이는 네트워크 지연을 증가시키는 요인이다. 이를 해결시키기 위해 SR에서는 개별적인 확인응답하는 방법을 추가하였다. 모든 송신자의 윈도우내 패킷은 GBN과 달리 개별적으로 타이머가 존재한다. 송신자가 보낸 패킷이 수신자에게 올바르게 전송되어 송신자가 수신자에게 ACK를 받으면 해당 패킷의 타이머는 종료된다. 개별적인 타이머를 가지므로 유실판단도 개별적으로 판단할 수 이ㅆ다.  



## Application Layer

## Session
> 개인정보와 같은 private한 데이터를 쿠키로 단순하게 보낼 시 보안에 취약하다. 세션은 쿠키의 단점을 보완하기 위해서 쿠키에 암호화된 SID(Session ID)를 담아서 클라이언트에 보내고 본래의 데이터는 서버에 저장한다. 세션의 데이터가 많으면 서버의 오버헤드가 커지기 때문에 세션으로 해야할 데이터인지 쿠키로 해도되는 데이터인지 잘 구분하여 사용하여야한다.


## TCP 연결
### 1. 개념
TCP서버와 클라이언트와 같이 두 호스트 사이에 실질 데이터 통신전 네트워크를 연결하는 메카니즘이다. 두 호스트 사이에서는 반드시 synchronization과 acknowledgement 패킷을 교환해야한다.   
<img width="611" alt="image" src="https://user-images.githubusercontent.com/56042451/198359720-7dd1fb53-28f9-4d77-93ad-1d646c2fce67.png">  
TCP 헤더를 보면 SYN(synchronization), ACK(ackowledgement, acknowledgement number와 다른 필드) 필드가 있다. 그리고 TCP 헤더는 최소 20바이트에서 옵션 필드의 최대 40바이트 사용까지 합쳐 60바이트까지 가능하다.

### 2. 연결을 위한 3-way handshake 과정
#### 1. Client -> Server
클라이언트는 TCP연결을 위해 서버에게 SYN message를 보낸다. SYN message는 SYN flag를 1로 설정한 메세지를 뜻하고 그 외 서버와 동기화를 위해 시퀀스넘버, ACK, window size, maximum segment size 등 여러가지 설정을 진행한다.

#### 2. Server -> Client
서버는 클라이언트가 보낸 SYN message를 확인하고 SYN와 ACK 필드를 1로 세팅하고 클라이언트에 메세지를 보낸다. 이때 acknowledgement number는 받았던 패킷의 sequence number에서 1증가한 만큼(메시지 크기가 없기 때문)으로 세팅한다. 그리고 서버의 window size, maximum segment size를 클라이언트에게 알린다. 이로서 클라이언트와 서버 사이간 연결이 생성되었다.

#### 3. Client -> Server
클라이언트는 서버가 보낸 ACK, SYM message를 확인하고 acknowledgement number를 1만큼 증가하고 ACK 플래그만 1로 세팅해서 다시 서버로 보낸다. 이로써 서버와 클라이언트 사이간 연결이 완성되었다. 이 단계 연결에서 클라이언트는 ACK메세지와 동시에 데이터를 담아 서버에 보낼 수 있다.

#### 4. 예시
<img width="852" alt="image" src="https://user-images.githubusercontent.com/56042451/198365787-4b8dae00-ce8d-4f05-b07d-b18d0e27c856.png">


## TCP 종료
### 1. 개념
TCP 연결에서 더 이상 보낼 데이터가 없으면 연결을 해제해야한다. 연결 종료는 두 종단간 사이 어느 누가 종료 시그널을 보내도 상관 없다. 서버-클라이언트 모델에서는 주로 클라이언트가 종료 요청을 하게 되지만 서버가 먼저 할 수 도있다. TCP 연결 종료에서는 종료를 먼저 요청하는 쪽을 Active close, 종료 요청을 받는 쪽을 Passive close라고 한다. 클라이언트가 보낼 데이터가 없어 서버에 종료를 요청하면 클라이언트는 Active close, 서버는 Passive close이다. TCP 종료를 어플리케이션 수준에서 제대로 처리하지 못하면 TCP 자원이 제대로 회수되지 못하여 자원의 낭비가 발생하기 때문에 제대로 처리해야한다. 이런 경우가 생기는 이유는 연결 종료는 연결시작과는 다르게 누가 먼저 해야할지 정해져 있지 않기 때문이다.

### 2. TCP state
1. FIN-WAIT-1 : 연결된 상대 TCP의 연결 종료 요청을 기다리는 상태이거나 이전에 보냈던 연결종료 요청에 대한 ACK 메세지를 기다리는 상태이다. 일반적으로 이 상태는 보통 짧은 시간을 갖는다.
2. FIN-WAIT-2 : 상대 TCP의 종료 요청을 기다리는 상태
3. CLOSE-WAIT : 상대 TCP의 종료 요청을 받고, 로컬 애플리케이션의 종료 요청을 기다리는 상태
4. LAST-ACK : 이전에 보냈던 종료 요청에 대한 ACK 메세지를 기다리는 상태.
5. TIME-WAIT : 상대 TCP가 ACK 메세지를 제대로 받고 CLOSE를 제대로 처리하기 위해 기다리는 상태
6. CLOSE : 오든 종료(상대 TCP, 로컬 애플리케이션)요청에 대한 메시지를 받고 ACK 메세지를 기다리는 상태
7. CLOSED : TCP의 연결이 종료된 상태

### 3. 과정
#### 예제 
<img width="488" alt="image" src="https://user-images.githubusercontent.com/56042451/198864366-bfd49839-24e4-4ead-a526-6846a8a55876.png">  
#### 1. Active close 종료 요청 
예제에서 client가 종료 요청(close())을 하므로 클라이언트는 Acitve close가 되고 TCP 메세지에 FIN flag를 1로 설정한 후 서버(Passive close)에 종료 요청을 보내고 자신은 FIN-WAIT-1 상태에 빠지게 된다.

#### 2. Passive close 종료 승인
서버 측에서는 FIN flag에 1이 설정된 것을 확인한 후 클라이언트가 종료하고자 함을 판단하고, 자신도 종료를 하겠다고 ACK 메세지를 보내게 된다. 이때 서버가 보내는 acknowlegement number는 sequence number + 1이다. 그리고 서버 자신은 CLOSE-WAIT 상태에 들어간다.

#### 3. Active close 서버 종료진행 확인
클라이언트에서는 서버에서 보낸 ACK 메세지를 확인하고 서버가 종료 진행과정을 시작했다는 것을 판단하고 자신은 FIN-WAIT-2 상태로 들어간다. FIN-WAIT-2 상태에서 서버에서 FIN flag가 1로 설정된 메세지가 안오면 일정 시간이 지난 후 자동으로 TIME-WAIT 상태로 진입하게 된다.

#### 4. Passive close 서버 애플리케이션 종료
CLOSE-WAIT에 들어간 TCP는 서버 애플리케이션에 종료 요청을 하고 서버 애플리케이션이 종료하면 TCP는 FIN flag를 1로 설정 후 클라이언트에 메세지를 보내게된다. 그리고 LAST-ACK 상태에 진입하게 된다.

#### 5. Active close TIME-WAIT 진행
FIN flag가 1로 설정된 서버측의 메세지를 받으면 FIN-WAIT-2 상태는 TIME-WAIT으로 진행하고, 서버에 ACK 메세지를 보낸다. 이미 이 상태는 애플리케이션과 TCP 소켓의 연결은 끊어진 상태고, 단지 TCP 자원만 점유하고 있는 상태이다.  
* TIME-WAIT가 존재하는 이유
  + active closer가 보낸 ack패킷 유실 : 만약 마지막으로 보낸 active closer의 ack패킷이 유실된다면 passive closer는 LAST-ACK 상태에 머무르게 되고 자신의 패킷이 유실되서 ACK가 안오는지 단지 ACK가 유실되서 안오는 건지 알 수 없기 때문에 FIN패킷을 보내게된다. 하지만 TIME-WAIT가 없는 active closer는 이미 closed상태이기 때문에 TCP 자원을 반납하였고 passive closer는 계속 FIN 패킷만 보내고 자원을 회수 못하게된다. 
  + 연결이 해제된 사용자가 다시 동일 포트로 재연결을 시도할 때 : 만약 연결이 해제된 사용자가 기존에 보냈던 데이터가 담긴 패킷이 active closer에게 도착하지 못한채 이미 연결이 해제된 상태에서 사용자가 동일 포트로 연결을 시도하고 연결이 된 후 새로운 데이터를 보내는 과정에서 새로운 데이터가 담긴 패킷과 기존에 도착하지 못했던 패킷이 동일한 시퀀스 넘버로 도착하게 된다면 active closer에서는 기존에 도착하지 못했던 패킷을 새로운 패킷으로 인식하여 데이터 무결성이 깨지게 된다. 이 때문에 TIME-WAIT 시간을 설정하여 설정한 시간(2MSL, centos는 60초)동안은 TCP 자원을 점유하고 있으면서 동일한 TCP자원으로 연결이 되지 못하도록 한다. 
  
#### 6. Passive close CLOSED
서버는 CLOSED 상태에 들어가고 모든 TCP 자원은 회수가 된다.

#### 7. Active close CLOSED
클라이언트에 연결은 모두 종료된 상태.


## Window Scale Option(RFC 1323)
### 1. 개념
윈도우 스케일은 TCP 포멧을 확인하면 16비트로 최대 64Kbytes(2^16)까지 데이터를 보낼 수 있다. 하지만 현실적으로 64Kbytes는 초당 1기가까지 보내는 회선을 다 활용하기에는 너무 적은 수치이다. 그래서 윈도우 스케일 옵션을 추가하여 16비트에서 30비트(1GBytes)까지 보낼 수 있도록 확장했다. 이 옵션은 무조건 3 hand shake과정에서 SYN 플래그가 1로 설정 될 때만 보낸다. 핸드쉐이크 과정에서 SYN 플래그가 설정되는 경우는 총 2번이다. 이 2번에서 1번은 클라이언트에 윈도우 스케일 설정, 나머지 한 번은 서버의 윈도우 스케일을 설정하는 것이다. 그리고 설정된 윈도우 스케일은 연결을 다시 하지 않는 이상 재설정은 불가능하다. 재설정을 못하게 막는 이유는 TCP 패킷에 들어가는 데이터 오베헤드를 감소하기 위해서다 

### 2. 포맷
TCP Window Scale Option(WSopt)은 총 3바이트 옵션 크기를 차지한다. 윈도우의 최대크기는 현재 2^31 보다 작아야한다. 쉬프트는 최대 14만큼까지 이동할 수 있기 때문이다(최대 1GBytes). 만약 14를 초과하면 14로 사용해버린다.
* kind = 3 // Wsopt를 나타낸다.
* Length = 3 // 3bytes크기를 나타낸다
* shift.cnt = 5 // 5비트 만큼 좌측으로 이동한다. 2^5승크기의 곱


