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
GBN의 주요 문제점은 패킷이 하나만 유실돼도 윈도우안에 있는 응답받지않은 모든 패킷을 불필요하게 전송하는 것이다. 이는 네트워크 지연을 증가시키는 요인이다. 이를 해결시키기 위해 SR에서는 개별적인 확인응답하는 방법을 추가하였다. 모든 송신자의 윈도우내 패킷은 GBN과 달리 개별적으로 타이머가 존재한다. 송신자가 보낸 패킷이 수신자에게 올바르게 전송되어 송신자가 수신자에게 ACK를 받으면 해당 패킷의 타이머는 종료된다. 개별적인 타이머를 가지므로 유실판단도 개별적으로 판단할 수 있다.



## Application Layer

## Session
> 개인정보와 같은 private한 데이터를 쿠키로 단순하게 보낼 시 보안에 취약하다. 세션은 쿠키의 단점을 보완하기 위해서 쿠키에 암호화된 SID(Session ID)를 담아서 클라이언트에 보내고 본래의 데이터는 서버에 저장한다. 세션의 데이터가 많으면 서버의 오버헤드가 커지기 때문에 세션으로 해야할 데이터인지 쿠키로 해도되는 데이터인지 잘 구분하여 사용하여야한다.

## TCP Flow Control
### 1. 개념
flow control은 receiver가 자신의 TCP 버퍼가 저장할 수 있는 최대 양을 sender에게 알려주어 보낼 수 있는 데이터의 양을 제한하는 행위를 말한다. 이런 처리를 하는 이유는 sender와 receiver의 하드웨어, 소프트웨어 적인 성능에 차이가 있을 수 있기 때문이다. 만약 receiver의 cpu처리 속도가 sender보다 느리다면 sender에서 빠르게 보낸 데이터는 수신자에 TCP 버퍼에 쌓이게 되고 허용량보다 초과해서 데이터가 오면 해당 데이터는 손실처리 된다. 수신자가 송신자가 보내는 속도보다 빠르게 데이터를 처리한다면 이런 문제는 발생하지 않겠지만 현실에서는 알 수 없는 문제이다. 그렇기 때문에 수신자는 자신의 TCP 버퍼의 수용 가능량을 (sliding window에서는 window size) 송신자에게 알린다. 만약 송신자의 window사이즈가 크더라도 수신자의 window 사이즈가 작다면 수신자의 윈도우 사이즈만큼 밖에 보낼 수 없다. 

### 2. 해결 방안 : stop and wait protocol
해결 방안 중 하나는 stop and wait 메카니즘을 사용하는 것이다. sender는 패킷을 하나는 전송하고 해당 패킷의 ack패킷이 온다면 그 다음 패킷을 전송하는 것이다. stop and wait는 정확하게 작동은 한다. 하지만 패킷 하나씩 처리하면 네트워크에 부하는 적겠지만 네트워크에 사용률이 현저히 떨어지기 때문에 효율성면에서는 좋지 못하다.

### 3. 해결 방안 : Sliding window

### 4. TCP Zero Window
수신자의 애플리케이션이 버퍼의 데이터를 빠르게 처리하지 못해 버퍼가 가득찬 경우 송신자에게 버퍼 수용량이 0이라고 전송한다. 이 패킷의 메세지 이름을 TCP Zero Window라고 한다.
#### deadlock
TCP Zero Window를 받은 송신자는 수신자의 버퍼가 언제 수용 가능할지 모른다. 왜냐하면 수신자의 버퍼 수용량은 송신자가 보낸 패킷의 응답패킷(ACK)과 함께 오기 때문에 송신자가 패킷을 보내지 않는 이상은 응답패킷을 수신자에서 보내지 않기 때문이다. 송신자는 수신자 버퍼의 수용가능한 상태를 받고 싶어하지만 수신자는 송신자에서 패킷이 오지않아 자신의 버퍼 수용량을 보내지 못하는 상황이기 때문에 이 상황은 *데드락* 상태인 것이다. 이를 해결하기 위해 송신자는 수신자가 버퍼 수용량을 0이라고 해서 패킷을 보내면 일정 시간이 경과할때마다 지속적으로 수신자의 버퍼 수용량을 받기위한 1바이트 데이터가 담긴 패킷을 전송한다. 와이어샤크에서는 TCP ZeroWindowProb라는 이름으로 분석을 제공한다.

<img width="1624" alt="image" src="https://user-images.githubusercontent.com/56042451/198960302-2ddd1347-56ca-4143-bcca-3ef9d166059e.png">

## TCP Congestion control
### 혼잡 시나리오
#### 1. 무한 버퍼를 같은 라우터
라우터가 무한한 버퍼를 갖더라도 네트워크의 혼잡 가능 성이 존재한다. 라우터는 무한한 버퍼를 가지고 출력 회선의 대역폭이 R bytes/sec이고, 두 명의 송신자 A,B의 애플리케이션이 λ bytes/sec로 데이터를 전송한다고 가정한다. 이때 각 계층의 헤더 정보 삽입에 대한 오버헤드는 존재하지 않고 애플리케이션이 데이터를 직접 라우터에 연결된 입력 링크로 데이터를 전송한다. 두 명이 송신자가 데이터를 전송하므로 각 송신자에게 부여된 최대 전송률은 R/2 bytes/sec이다. 각 송신자들은 최대 전송률 R/2를 초과하는 속도로 데이터를 보내더라도 최대 R/2밖에 처리량을 얻지 못한다. 송신자들이 R/2내에서 데이터를 전송한다면 완벽하게 처리 효율을 잘 사용할 것이지만 R/2이상으로 보낸다면 라우터에는 패킷들이 버퍼에 계속 누적되며 이는 큐잉 지연으로 이어질 것이다. 무한 버퍼를 갖는 라우터에서도 결국 대역폭에 문제 때문에 혼잡가능성이 존재한다.

#### 2. 유한 버퍼를 같는 라우터
유한 버퍼이기 때문에 최대한 좋은 조건에서 보기위해 TCP 재전송 기능이 추가된다고 가저한다. 만약 TCP에서 라우터로 데이터 전송 시 라우터의 버퍼가 비어 있을 때만 전송한다면 패킷 손실은 발생하지 않을 것이다. 라우터의 버퍼가 비워 있을 때만 데이터를 전송하므로 2명의 송신자는 R/2 처리량 내에서만 사용할 것이다. 만약 패킷이 확실하게 손실 처리 되었다고 확신할 수 있을 때 재전송을 한다고 하면 각 사용자가 최대로 활용할 수 있는 전송률 R/2보다 더 작은 전송률을 사용하여 새로운 데이터를 전송하는데 사용할 것이다. 결국 혼잡으로 인해 패킷이 손실되고 새로운 데이터의 전송률은 낮아 진다고 알 수 있다. 만약 패킷 손실이 나지 않았음에도 불구하고 타임아웃 시간이 짧아 손실로 인식하여 재전송 패킷이 늘어난다면 새로운 데이터를 전송하기 위한 최대 전송률은 더욱 감소할 것이고 불필요한 재전송으로 인해 네트워크는 더욱 혼잡해질 것이다.

### 1. 개념
네트워크에 너무 많은 패킷이 전송되면 여러 처리 과정 중에서 패킷이 누락처리가 될 수 있다. 예로 들면 라우터에 있는 버퍼에 패킷이 가득 차 다음에 들어오는 패킷을 drop 처리하는 것이다. 이런 과정이 지속되면 유실되는 패킷은 지속적으로 늘어날 것이다. 네트워크에 과부하를 줄여주기위해 TCP 송신자는 패킷양을 적게 보내 네트워크의 과부하를 줄여야한다. 이런 과정이 4계층 TCP에서 실행되는 이유는 3계층에서는 패킷을 올바르게 포워딩하기 위해 많은 연산을 처리하기 때문에 과부하 처리까지 담당하면 많은 부담이 간다. 그렇기 때문에 3계층에서 처리하지 않고 종단 끝의 3계층에서 처리하게 된다. 이것을 종단간의 혼잡제어라고 불린다.
그럼 네트워크 예를 들어 라우터와 같은 장치에서 혼잡제어를 하지 않는가? 이 물음에 대해서는 아니다. 네트워크 장치에서도 충분히 혼잡제어가 가능하다. 방법은 전형적으로 두 가지 정도 존재한다. 송신자에게 직접 현재 네트워크가 혼잡하다고 알리는 것과 송신자에서 수신자에게 흐르는 패킷에 특정 필드에 혼잡하다는 표시하고 수신자 해당 필드를 확인하고 응답 패킷으로 네트워크가 혼잡하다는 것을 알린다. 

### 2. flow control과의 차이점
흐름제어의 주된 목적은 송신자와 수신자의 플랫폼(하드웨어, 소프트웨어)의 차이 때문에 발생하는 데이터 처리 속도로 인해 송신자가 수신자의 데이터 처리 속도보다 빠르게 패킷을 보내게 되면 수신자 버퍼가 빠르게 가득차게되어 손실처리되는 패킷에 증가로 인한 네트워크의 과부하 뿐만 아니라 쓸모없는 데이터 재전송까지 하게 때문에 이것을 해결하기 위한 것이다. 반면 혼잡제어는 종단간에서 발생하는 것이 아닌 현재 네트워크 상에서 패킷이 다량 존재하여 과부하를 일으키는 것으로 인해 네트워크의 과부하를 줄여줄기 위한 것이다.  
그러면 Sliding window protocol을 사용하는 TCP에서 한번에 보낼 수 있는 패킷에 양은 무엇일까? rwnd는 흐름제어로 인해 보낼 수있는 최대 패킷양, cwnd는 네트워크의 과부하로 인한 전송할 수 있는 최대 패킷양 이므로 한번에 최대로 보낼 수 있는 패킷양은 min(rwnd, cwnd)이다.

### 3. 혼잡제어 메카니즘 시나리오(self-clocking)
TCP에서 네트워크가 과부하 상태에 있거나 과부하 상태로 진행 중이라는 사실을 어떻게 알 수 있을까? 바로 재전송 메커니즘으로 알 수 있다. 재전송은 TCP에서 타임아웃 이벤트가 발생하거나 송신한 패킷의 이전 패킷의 중복된 ACK메세지로 정확하지는 않지만 네트워크에 무슨 문제가 있다는 것을 예측할 수 있다. 네트워크 장비에 이상이 없다면 분명 대게 많은 데이터로 인한 과부하라는 것을 전제로 한다. 만약 지속적으로 중복 ACK메세지를 받거나 ACK메세지가 오지 않아 타임아웃 이벤트가 발생한다면 TCP는 네트워크가 혼잡하다는 것으로 인식하고 cwnd의 크기를 줄인다. 만약 빠르게 ACK메세지가 온다면 cwnd의 속도를 빠르게 증가시켜 네트워크에 최대 대역폭을 효율적으로 사용하도록 한다. TCP는 cwnd를 스스로 ack메세지로 조절하기 때문에 자체 클로킹(self-clocking)이라고 한다. 또 다른 문제로 그럼 혼잡했을 때는 윈도우크기는 얼마로 줄여야하는지 혼잡하지 않을 때는 얼마나 크기를 늘려야 할지의 문제를 처리해야한다.

### 4. Slow start
cwnd는 초기에 1MSS로 설정된다. 이후 혼잡이 발생하기 전까지 ACK메세지 수만큼 cwnd를 MSS만큼 늘리게 된다.(두배씩 증가)
```
초기 cwnd = 1MSS -> 1개 패킷 전송
1ACK -> cwnd = cwnd(1MSS) + 1MSS -> 2개 메세지 전송
2ACK -> cwnd = cwnd(2MSS) + 2MSS -> 4개 메세지 전송
4ACK -> cwnd = cwnd(4MSS) + 4MSS -> 8개 메세지 전송
...

cwnd의 일반식 : ACK메세지 하나 당 cwnd = cwnd + MSS
```
이후 네트워크가 혼잡하다고 인식하면(중복 ACK패킷 3개를 받거나, 타임아웃 이벤트가 발생) cwnd = 1MSS로 설정하고 ssthresh(slow start threshold, 슬로우 스타터 임계치)를 cwnd의 절반으로 설정한다. 
```
혼잡 발생
ssthresh = cwnd/2
cwnd = 1
```
이후 다시 슬로우 스타트를 반복하고, cwnd가 ssthresh를 초과하게 되면 네트워크가 혼잡해질 것이라 예상하고 혼잡회피 단계로 진입한다. 

### 5. Congestion avoidence
혼잡 회피 단계에서는 네트워크가 혼잡하지는 않을 수도 있지만 혼잡해질 것이라 예상하는 것이다. 그러므로 슬로우 스타트처럼 두 배씩 증가하는 것은 네트워크에 과부하로 이어질 수 있다고 보고 천천히 cwnd를 증가 시켜야 할 것이다. 혼잡 회피에서 선택한 방법은 현재 보낸 패킷의 모든 ack메세지를 받아야 1MSS증가한다. 즉 ACK메시지 하나당 MSS*(MSS/cwnd)이다. (선형증가)
```
가정 : ssthresh = 10, MSS = 1, cwnd = 16이면 혼잡 회피단계로 진입, 송신자는 16개 메세지를 전송한 상태
16개 메세지중 1개의 메세지가 오면
cwnd = cwnd + MSS*(MSS/cwnd) // cwnd = 16 + 1*(1/16)
...
16개 메세지 중 16개 ACK메세지가 오면
cwnd = cwnd(16+15/16) + 1*(1/16) // cwnd = 17MSS

cwnd의 일반식 : cwnd = cwnd + MSS*(MSS/cwnd)
```
혼잡회피 단계에서도 네트워크가 혼잡하다고 인식되면(중복 ACK패킷 3개를 받거나, 타임아웃 이벤트 발생) cwnd = 1MSS로 설정하고, ssthresh를 cwnd를 절반으로 설정한다. cwnd는 1로 설정되었으므로 ssthresh보다 작을 것이고 슬로우 스타터 상태로 전이 된다.


### 5. Fast recovery
빠른 회복은 혼잡제어에서 권고 사항이지만 필수는 아니다. 빠른 회복은 혼자 발생여부인 중복패킷, 타임 아웃 이벤트 중에서 중복패킷을 따로 처리하는 로직이다. 초기 TCP Tahoe버전은 빠른 회복을 채택하지 않았다(슬로우스타터, 혼잡 회피만 존재). 만약 슬로우 스타터나 혼잡회피 상태에서 중복 패킷을 3개 받으면 혼잡하다고 예상하고 ssthresh=cwnd/2으로 설정하고 cwnd=ssthresh+3MSS로 설정한다. ssthresh+3MSS를 하는 이유는 원래 3개를 패킷만큼 cwnd를 이동해야 하지만 다른 ACK를 받지 못한 패킷때문에 cwnd를 옆으로 이동할 수 없기 때문에 윈도우 크기를 3MSS만큼 늘려 새로운 패킷을 전송할 수 있는 공간을 확보한다. 중복된 패킷을 계속 받는다면 계속 윈도우 사이즈를 1MSS만큼 늘리고 타임아웃이 되면 슬로우 스타트로 새로운 ACK이 오면 혼잡 회피 단계로 진입한다. 빠른 회복을 채택한 버전은 TCP Reno이다. 대부분 현대의 TCP구현은 Reno알고리즘으로 이용한다. 

```
중복 패킷 3개 발생 -> ssthresh = cwnd/2, cwnd = ssthresh + 3MSS 빠른회복 진입
빠른 회복 진입 후 계속 중복 패킷이 발생하면 -> cwnd = cwnd + 1MSS, 윈도우를 이동할 수 없으니 새로운 패킷을 전송하기 위해 사이즈를 늘려준다
타임 아웃 발생 -> ssthresh = cwnd / 2, cwnd = 1 -> 슬로우 스타터로 진행
새로운 ACK메세지 수신 -> cwnd = ssthresh -> 혼잡 회피 진행 cwnd >= ssthresh 이면 혼잡회피 진행 가능하기 때문
```
#### TCP 혼잡제어 상태 머신
<img width="571" alt="image" src="https://user-images.githubusercontent.com/56042451/200111058-2b7af221-d88c-4fe0-9a3c-b88a91e55340.png">

### 6. 가법적 증가, 승법적 감소(additive-increase, multiplicative decrease, AIMD)
만약 혼잡제어 조건을 3개의 중복 패킷의 전송만 있다고 가정고 슬로우 스타트는 하지 않는다고 하면, 즉 빠른회복과 혼잡회피만 존재하게 된다면 혼잡회피 상태에서 선형적으로 증가하다 3개의 중복 패킷이 오면 빠른 회복단계로 들어가서 윈도우는 절반+3MSS로 줄어들게 된다.  
<img width="479" alt="image" src="https://user-images.githubusercontent.com/56042451/200157032-e93d4fa3-f2f2-4270-bafc-fcca6101fb4e.png">  
위 그래프를 참고하면 혼잡윈도우는 선형적으로 증가하다 3개의 중복 패킷으로 인해 윈도우가 절반 정도의 크기로 줄어든 것을 볼 수 있다.



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
6. CLOSED : TCP의 연결이 종료된 상태

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

## Network layer
### 1. 개념
Network laye는 Open Systems Interconnection(OSI)의 3계층에 존재한다. 네트워크 계층에 주요기능은 Transport Layer의 데이터를 받아 Datagram으로 만든 후 다른 네트워크 상으로 이동시키는 것이다. 데이터를 이동기위해 목적지에 대한 정보를 추가하고(캡슐화), 효율적인 경로를 선택하여 전송한다(라우팅). 대표적인 3계층 장비로는 라우터가 있다.

### 2. 용어
1. routing : 라우팅이란 포워딩 테이블을 만들기 위한 프로세스이다. 목적지에 대한 최단 경로를 설정하여 포워딩 테이블을 유지하는 것이 주된 목적이다.
2. forwarding : 포워딩을 데이터 패킷을 네트워크로 보내기 위한 프로세스이다. 라우터는 포워딩 테이블을 만들고 라우터에 연결되어있는 수 많은 네트워크 중에서 어디로 패킷을 보내야 할지 포워딩 테이블을 참고하여 패킷을 전송한다.
3. forwarding table :
 
### 3. Plane(폄면)
> 평면이란 네트워크에서 특정한 프로세스가 어디에서 작동하는지 추상적인 개념이다. 흔히 데이터 평면, 제어 평면 두 가지가 있다.

#### Control plane
제어 평면은 데이터 패킷들을 어떤 방법으로 포워딩하는지 제어하는 네트워크의 한 부분이다. 다른 네트워크 상으로 데이터를 보내기 위해 라우터는 라우팅 테이블, 포워딩 테이블을 구성하여 데이터 평면 기능이 작동할 수 있도록 한다. 

#### data plane
control plane의 로직에 기반하여 다른 네트워크로 패킷을 전송하는 기능을 가지고 있다. 라우팅 테이블, 포워딩 테이블, 라우팅 로직들은 데이터 평면 기능을 구성한다. 

### 2. 캡슐화
네트워크 계층은 전송계층(4계층)에서 받은 데이터(TCP: segment, UDP : datagram)에 IP Header를 추가하여 Packet을 만들어 Link Layer(2계층)에 전달한다. 

### 3. Network Header
<img width="787" alt="image" src="https://user-images.githubusercontent.com/56042451/199282011-e2cd58d9-a473-4a4b-a986-c50908cf70ba.png">

* version : 4bits로 해당 IP 버전을 나타낸다. 대부분 IPv4를 사용하여 0100로 표시된다. 만약 라우터가 버전을 지원하지 않는다면 패킷은 드랍된다.
* Internet Header Length : 인터넷 헤더의 가로 길이는 32bits words로 이루어져있다. Header 크기는 최소 20bytes이기 때문에 IHL은 최소 5(4bytes\*5 = 20bytes)이다. 최대 4비트로 표현하므로 네트워크 헤더의 길이는 20 ~ 60 까지 표현될 수 있다.
* type of service(8bits) : 
* Total Length(16bits) : data(segment or datagram) 크기와 network header 크기를 합친 값이다. (application data + tcp header + network header). 16비트이므로 최대 65,535바이트까지 가능하다.   
    Total Length is the length of the datagram, measured in octets,
    including internet header and data.  This field allows the length of
    a datagram to be up to 65,535 octets.  Such long datagrams are
    impractical for most hosts and networks.  All hosts must be prepared
    to accept datagrams of up to 576 octets (whether they arrive whole
    or in fragments).  It is recommended that hosts only send datagrams
    larger than 576 octets if they have assurance that the destination
    is prepared to accept the larger datagrams.

    The number 576 is selected to allow a reasonable sized data block to
    be transmitted in addition to the required header information.  For
    example, this size allows a data block of 512 octets plus 64 header
    octets to fit in a datagram.  The maximal internet header is 60
    octets, and a typical internet header is 20 octets, allowing a
    margin for headers of higher level protocols.
* Identification(16bits) : 송신자가 보낸 조깨진 데이터그램들을 수신자측에서 조립하기 위한 식별자이다.
* Flag(3bits) : 다양한 제어 플래그  
    bit 0은 예약된 비트로 반드시 0으로 설정되어야 한다.
    bit 1은 1이면 조각으로 나뉘어져 있음을 표현
    bit 2는 0이면 마지막 조각, 1이면 조각이 더 있음을 표현
* Fragment Offset(13bits) : 데이터 그램에서 조각이 어디에 위치해 있는지 표현
* Time to Live(8bits) : 데이터 그램이 인터넷 시스템에서 남아 있을 수 있는 최대 시간을 표현한다. 만약 필드가 0으로 설정되면 데이터그램은 바로 파괴된다. 이 필드는 인터넷 헤더 처리에 의해 변경된다. 시간은 초단위로 설정된다. This field indicates the maximum time the datagram is allowed to
    remain in the internet system.  If this field contains the value
    zero, then the datagram must be destroyed.  This field is modified
    in internet header processing.  The time is measured in units of
    seconds, but since every module that processes a datagram must
    decrease the TTL by at least one even if it process the datagram in
    less than a second, the TTL must be thought of only as an upper
    bound on the time a datagram may exist.  The intention is to cause
    undeliverable datagrams to be discarded, and to bound the maximum
    datagram lifetime.

## IPv4의 데이터그램 단편화(Datagram Fragmentation)
### 데이터그램 단편화란?
각 프로토콜 마다 최대로 전송할 수 있는 데이터 크기가 있기 때문에 전송할 수 있는 프로토콜의 데이터크기보다 더 크다면 데이터를 분할 해야한다. MTU(Maximum Transmmision Unit)로 불리는 링크 레이어의 최대 데이터 전송 크기는 1500bytes이다. IP 데이터 그램을 링크계층에서 캡슐화하여 프레임을 만들어 다른 라우터로 전송하게 된다. 예를 들어 큰 데이터를 보냈는데 MTU가 데이터그램보다 작다면 어떻게든 전송하기 위해 데이터를 축소하든 해야한다. 데이터 축소, 압축은 원본을 훼손하여 불가능하므로 데이터그램을 잘게 나누어 MUT가 수용할 수 있도록 만들어서 전송한다.

### 단편화된 데이터처리는 어디서?
분할된 데이터는 원래 데이터로 다시 재조립되어야 한다. 이 재조립은 어디서 일어날까? 바로 종단 끝에서 transport layer로 진입하기 전에 일어나게된다. 만약 재조립을 라우터에서 하게되면 라우팅을 위한 속도에 저하가 일어날 것이고 라우터는 매우 복잡하게 될 것이다. 이래서 초기 설계자들은 조각난 데이터를 종단에서 하는 것으로 결정했다.

### 재조립 방법
IPv4 디자이너들은 재조립을 위해 IP header에 identification, flag, fragmentation 필드를 두었다. 식별자는 단편화 되기 전 원본 패킷과 동일하고, flag는 마지막 패킷인 경우 0이며 fragmenttation은 재조립을 위한 오프셋을 의미한다. 만약TCP 세그먼트의 크기가 8968바이트라고 가정하면 링크레이어의 최대 수용 데이터 크기는(MTU) 보통 1500이므로 IP헤더크기 20바이트를 제외하고 세그먼트를 1480크로 나눈다음 각각 IP헤더를 추가하면된다. 이때 단편화된 모든 패킷은 처음 단편화되기 전과 같은 Identification을 가지고 뒤에 붙여져야할 데이터가 있으면 flag의 2번째 비트가 1로 설정된다. 마지막 패킷은 0으로 설정된다. 그리고 각 데이터가 재조립되기 위해 파편화된 데이터 크기를 오프셋으로 사용된다.
```
원본 패킷 
  data(segment) = 898바이트, IP header size = 20바이트, flag = 0, Identification = 0x01, framentation = 0

세그먼트 크기가 8968 이므로 단편화 시키면
1번째 패킷
  data = 1480바이트, IP header size = 20바이트, flag = 010(마지막패킷이 아님), Identification = 0x01(원본과 동일), fragmentation = 0

2번째 패킷
  data = 1480바이트, IP header size = 20바이트, flag = 010(마지막패킷이 아님), Identification = 0x01(원본과 동일), fragmentation = 1480

3번째 패킷
  data = 1480바이트, IP header size = 20바이트, flag = 010(마지막패킷이 아님), Identification = 0x01(원본과 동일), fragmentation = 2960
  
4번째 패킷
  data = 1480바이트, IP header size = 20바이트, flag = 010(마지막패킷이 아님), Identification = 0x01(원본과 동일), fragmentation = 4440
  
5번째 패킷
  data = 1480바이트, IP header size = 20바이트, flag = 010(마지막패킷이 아님), Identification = 0x01(원본과 동일), fragmentation = 5920
  
6번째 패킷
  data = 1480바이트, IP header size = 20바이트, flag = 010(마지막패킷이 아님), Identification = 0x01(원본과 동일), fragmentation = 7400
  
7번째 패킷
  data = 88바이트, IP header size = 20바이트, flag = 000(마지막패킷), Identification = 0x01(원본과 동일), fragmentation = 8880
```

#### wireshark
<img width="1245" alt="image" src="https://user-images.githubusercontent.com/56042451/200484779-435d9475-6bd1-4f34-ab64-1092509b93de.png">   

```
구글 서버로 ping 메시지를 보냄, 서버에서 응답메시지를 받진 못해도 단편화 과정을 볼 수 있음
ping의 s 옵션으로 패킷의 사이즈를 사용자가 지정할 수 있다.
> ping -s 8960 8.8.8.8
```

## 라우터
### 1. 라우터란?
Router는 네트워크 사이 또는 네트워크와 서브네트워크 사이에 패킷 스위칭을 지원하는 장치이다. 각 회사나 지역에서는 소규모의 LAN을 구성하고 각 LAN망을 연결하기 Switch나 라우터로 연결하여 대규모의 WAN을 구성한다. 
* switch : 같은 네트워크상에 존재하는 그룹이나 장치사이에서 데이터 패킷을 전달한다.
* Router : 다른 네트워크 상에서 데이터 패킷을 전달한다.

### 2. 라우터의 구조
<img width="1275" alt="image" src="https://user-images.githubusercontent.com/56042451/199918733-0dc116e9-390b-4dd1-9400-3302227b5050.png">

### 3. Plane
#### Control Plane
데이터 패킷을 목적지 IP주소로 올바르고 빠르게 전달하기 위한 알고리즘 및 다양한 제어 기능이 내장되어 있다.

#### Data Plane
입력 라인으로 전송된 패킷을 올바른 출력 포트로 전송하는 역할을 한다. 출력 포트로 전송하기 위해 Control plane에서 만든 여러 테이블을 참조하여 forwarding 한다. 하드웨어로 구현되어 빠른 속도를 보장한다.

### 4. Line Card
데이터 평면 중 하나로 물리 계층, 링크계층 역할을 수행한다. 물리적 포트마다 라인카드가 존재하고 물리적 포트는 물리계층 역할 수행, 라인카드에서 링크계층 역할 수행 후 실질적인 forwarding 역할을 수행하는 Packet processor 과정을 수행한다. 

### 5. Switching Module
데이터 평면 중 하나로 입력 라인 처리된 패킷을 출력라인으로 forwarding하기 위한 중간 가교역할을 수행한다. forwarding의 핵심은 스위칭 모듈이다.
#### 메모리를 통한 스위칭(Switching via memory)
OS의 입출력 장치와 같이 동작한다. 패킷이 입력라인카드는 라우팅 프로세서(Control plane)에 인터럽트를 보내고 메모리에 패킷 데이터를 저장한다. 메모리에 저장된 패킷은 라우팅 프로세서에 의해서 적절한 출력라인카드의 출력 버퍼에 저장된다. 최신 라우터는 이 과정을 라우팅 프로세서가 처리하는 것이 아닌 입력라인카드에서 처리한다.  
<img width="289" alt="image" src="https://user-images.githubusercontent.com/56042451/199925908-62d16c28-a527-4446-8db5-a149d59e9db5.png">

#### 버스를 통한 스위칭(Switching via a bus)
라우팅 프로세서(Control plane)에 개입없이 입력라인카드에 의해서 출력라인카드로 패킷을 전송한다. 공유버스 하나에 모든 출력라인카드가 연결되기 때문에 하나의 입력라인카드에 의해 보내진 패킷 데이터는 모든 출력라인으로 전송된다. 전송된 패킷은 해당 헤더 데이터와 일치하는 출력라인카드만 패킷을 유지하고 나머지 출력라인카드에서는 버려지게된다. 모든 라인카드들은 하나의 버스만 이용하므로 하나의 라인카드가 버스를 사용하고 있으면 다른 라인카드는 사용이 불가능하다. 전송 속도는 버스에 대역폭에 의해 제한된다.  
<img width="289" alt="image" src="https://user-images.githubusercontent.com/56042451/199927228-8c9ac600-886e-4cd6-9b38-b2a96cd4a853.png">

#### 인터커넥션 네트워크를 통한 교환(Switching via an interconnection entwork)
공유 버스의 대역폭 제한을 극복하는 방법 중에 하나이다. 크로스바 스위치라고도 불리는 이 방법은 N개의 입력포트, N개의 출력포트를 2N개의 버스로 연결시킨다. 이 방법은 메모리와 버스를 통한 스위칭 방식과는 다르게 병렬로 데이터를 전달 할 수 있다. 단 서로다른 입력라인에서 같은 출력라인으로 데이터를 전달 시 단 하나의 패킷만 버스로 전달될 수 있기 때문에 하나의 입력라인의 패킷은 대기해야한다.  
<img width="231" alt="image" src="https://user-images.githubusercontent.com/56042451/199930418-c4a5e16c-254c-4196-b3e3-ecf2688c327e.png">

### 6. Queuing Delay
네트워크에서 데이터 전달에 딜레이를 처리하기 위해 많은 시간을 쏟는 부분이 큐잉 딜레이이다. 각 라인카드에는 큐를 가지고 있다. 
#### 라인카드에서 입력 큐의 딜레이
switching fabric이 충분히 빠르지 않다면 라인카드의 입력 큐에서 대기하는 데이터 패킷들이 증가하고 만약 입력 큐가 가득찬다면 라우터 입력 회선으로부터 들어온 데이터 패킷은 큐에 저장되지 못하고 손실 처리 된다. switching fabric에서 입력 큐의 데이터 증가를 야기하는 경우는 HOL(Head-of-the-line)차단 문제로 인해 발생하다. 같은 입력큐에 들어있는 2개의 패킷이 서로다른 출력큐에 들어가길 원할 때 뒤에 있는 패킷이 앞에있는 패킷에 대기 때문에 자신이 원하는 출력 큐가 비어있음에도 불구하고 전달되지 못하는 상황때문에 입력 큐에 용량이 점점 줄어든다.

#### 라인카드에서 출력 큐의 딜레이
만약 회선의 속도가 R이고 회선의 개수는 출력 입력 각각 N개씩 있다고 가정하고 스위칭 속도는 회선속도의 N배라고라고 가정한다. 각 입력 회선으로 1개씩 패킷이 들어오고 같은 출력 회선으로 들어간다고 할 때 스위칭 속도는 회선 속도의 N배이므로 N개에 회선에 도착한 1개의 패킷들은 모두 같은 출력 회선으로 전송될 것이다. 같은 출력 회선에는 한번에 N개의 패킷이 큐에 저장되고 이를 다 처리하기도 전에 스위칭이 똑같은 N개의 패킷을 전송한다면 출력 큐에는 순식간에 패킷들이 저장될 것이고 빠르게 큐에 공간은 부족해질 것이다. 이렇게 되면 출력 큐에서는 패킷을 수용할 수 없을 때 도착한 패킷들을 손실 처리하거나 기존에 대기하고 있던 패킷을 버리고 새로운 패킷들을 저장해야한다.  

## 용어
### 1. TCP Segment
TCP segment는 tcp 메세지의 데이터의 길이를 뜻한다.
### 1. MSS(Maximum segment size)
MSS는 receiver가 수신가능한 TCP segment의 최대 사이즈를 일컫는다. MSS는 바이트단위로 측정된다. 보통 1460 bytes로 설정되어있다.
```
MTU - IP header - TCP header = MSS
```
