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


## Application Layer

### Socket
OS 수준에서 제공하는 API, Application program, OS 에 transport layer가 구현되어있음(TCP, UDP만), tcp socket과 udp소켓은 아예다른 소켓이다.  
tcp => data stream, udp -. data gram

