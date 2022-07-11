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
신뢰적인 데이터 전송을 하기 위해서는 패킷의 에러, 패킷 유실 두 가지를 해결해야한다. 패킷 에러는 패킷 전송 중에 일어날 수 있는 데이터 변이를 뜻하고, 패킷 유실은 패킷 전송 중 패킷이 목적지까지 제대로 도착을 못하는 것을 의미한다. 패킷 유실의 대표적으로 라우터의 큐가 가득차 패킷이 버리는 것이다.

#### RDT 1.0 (reliable transfer over a perfect channel)
RDT 1.0은 채널이 완전하게 안정적이라고 생각한다. 즉 신뢰적 전송의 중요 요소 에러와 유실이 발생하지 않는다고 보는 것이다. 에러와 유실이 발생하지 않으면 무조건적으로 데이터가 송신측에서 수신측으로 올바르게 전송된다는 것을 의미하므로 패킷 송신자와 수신자는 패킷의 에러와 유실을 걱정할 필요가 없다. **현실적으로 이런 상황은 매우 비현실적이다.**

### RDT 2.0 (channel with packet error)
RDT 2.0은 패킷에 유실은 일어나지 않지만 **에러**가 발생 할 수 있다는 가정을 한다. 에러를 발생 유무를 확인하기 위해서는 일단 패킷에 **Error detection**을 체크하는 항목과 에러를 확인하고 다시 재전송 해줄것을 요청하는 feedback(Acknowledgements(acks), negative acknowledgements(naks)) 마지막으로 재전송하는 메카니즘이 필요하다.

* Error detection 방법
  + 패킷 내부에 에러를 탐지할 수 있는 요소를 삽입하고 Transfer Layer에서 확인한다. 대표적으로 Checksum bit
* feedback 방법
  + ACKs : 수신측이 송신측에 패킷을 제대로 받았다고 응답한다.
    - 송신측에서 ACKs가 담긴 패킷을 받으면 연결을 종료한다.
  + NAKs : 수신측이 송신측에 패킷에 오류가 있다고 응답한다.
    - 송신측에서 NAKs가 담긴 패킷을 받으면 해당 패킷을 재전송한다.

<font color="green"> **RDT 2.0의 결함**</font>  
ACKs와 NAKs도 패킷에 담겨저 보내지기 때문에 패킷 오류 또는 손실이 발생할 수 있다. 오류 또는 손실이 발생할 경우 송신측에서는 NAK신호라고 가정하여 다시 데이터를 보내는데 이 데이터가 재전송된 데이터인지 새로운 데이터인지 수신측에서는 확인이 불가능하다.

### RDT 2.1(handles garbled ack/naks)
RDT 2.0의 결함을 제거하기 위해 고안된 방법. 패킷마다 sequence 번호를 붙여서 송수신을 한다. 
* 가정
  + packet error가 발생가능성이 있음
  + loss는 일어나지 않음
보내는 packet에 체크섬 비트를 추가한다.(부가적인 정보 추가), receiver 측에서 패킷 수신 여부를 전송한다.(feedback) -> ack 또는 nak, 송신자는 nak일 때 패킷을 재전송한다.  
피드백, 즉 수신 여부 패킷에 에러가 있다면? 일단 checksum 이 있어야함.  
handling duplicate packets 

### RDT 2.1
### RDT 2.2
### RDT 3.0
timer 

### RDT 3.
## Application Layer
