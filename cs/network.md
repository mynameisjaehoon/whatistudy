# 네트워크

- [OSI 7계층](#osi-7계층)
- [3 Way Handshake & 4 Way Handshake](#3-way-handshake--4-way-handshake)
    - [3 Way Handshake](#3-way-handshake)
    - [4 Way Handshake](#4-way-handshake)
- [TCP/UDP, 흐름제어, 혼잡제어](#tcpudp-흐름제어-혼잡제어)
    - [흐름제어](#흐름제어)
    - [혼잡제어](#혼잡제어)

---


## OSI 7계층
- 7계층을 나누는 이유
    - 통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생겼을 때 그 단계만 수정할 수 있기 때문이다.
- OSI 7계층
    1. 응용계층
        - 최종 목적지로, 응용서비스와 직접 관계해서 일반적인 응용서비스를 수행하는 계층이다.
    2. 표현계층
        - 데이터 표현에 대한 독립성을 제공하고 암호화하는 역할을 제공한다.
    3. 세션계층
        - 데이터가 통신하기 위한 논리적인 연결을 담당한다.
    4. 전송계층
        - TCP, UDP 프로토콜을 통해서 통신을 활성화환다.
    5. 네트워크계층
        - 데이터를 목적지까지 가장 빠르고 안전하게 전달하는 기능을 담당한다.
        - 라우터를 통해 이동한 경로를 선택하여 IP주소를 지정하고, 해당 경로에 따라서 패킷을 전달한다.
        - 라우팅, 흐름제어, 오류제어, 세그멘테이션을 수행
    6. 데이터-링크계층
        - 물리 계층으로 송수신 되는 정보를 안전하게 전달되도록 관리한다. Mac주소를 통해서 통신한다.
    7. 물리계층
        - 데이터를 전기적인 신호로 변환핸서 전송하는 역할을 한다.

## 3 Way Handshake & 4 Way Handshake
### 3 Way Handshake
TCP는 정확한 전송을 보장해야 하기 때문에 전송하기에 앞서 논리적인 접속을 활성화 하기 위해서 3 Way Handshake를 진행합니다.
1. 클라이언트가 서버에서 SYN패킷을 보낸다.
2. 서버가 SYN을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN을 보낸다.
3. 클라이언트는 서버의 응답이 ACK, SYN 을 받고, ACK를 서버로 보낸다.

### 4 Way Handshake
연결 성립 후 모든 통신이 끝났을 때 해제해야하는 것이다.
1. 클라이언트가 서버에게 연결을 종료한다는 FIN 플래스를 보낸다.
2. 서버에서 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보낸다(이때 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 된다)
3. 데이터를 모두 보냈다면 연결이 종료되었다는 FIN플래그를 클라이언트에게 보낸다.
4. 클라이언트가 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다. (클라이언트는 아직 서버에서 받지 못한 데이터가 있을 수도 있으므로 TIME_WAIT을 통해 기다린다.)
    - 서버는 ACK를 받은 이후 소켓을 닫느다.
    - TIME_WAIT시간이 끝나면 클라이언트도 닫는다.

## TCP/UDP, 흐름제어, 혼잡제어
- TCP
    - 네트워크에서 신뢰적인 통신방식
    - reliable network를 보장한다는 것은 4가지 문제점을 지원한다는 것이다.
        1. 손실: 패킷이 손실될 수 있는 문제
        2. 순서: 패킷의 순서가 바뀔수 있는 문제
        3. Congestion: 네크워크가 혼잡한 문제
        4. Overload: receiver가 overload되는 문제
- UDP
    - User Data Protocol의 약자로, 데이터를 데이터그램 단위로 처리하는 프로토콜
    - 비연결성, 신뢰성없는 전송 프로토콜이다.
    - 왜 필요함?
        1. IP의 역할은 Host to Host만을 지원한다. 장치에서 장치로의 이동은 IP로 해결되지만, 하나의 장비 안에서 여러 프로그램들이 통신해주어야 할 경우에는 IP만으로 한계가 있다.
    - UDP의 경우에는 IP가 제공하는 정도의 수준만을 제공하는 간단한 IP상위 계층의 프로토콜이다.
    - 결정적인 장점은 **데이터의 신속성**이다. 데이터의 처리가 TCP보다 빠르다.
    - 주로 실시간 방송이나 온라인 게임에서 사용된다. 
- 흐름제어, 혼잡제어란?
    - 흐름제어
        - 송신측과 수신 측의 데이터 처리속도 차이로 인해 발생하는 문제를 해결하는 것
        - 수신측에서 지나치게 많은 패킷을 받지 않도록 하는 것이다.
        - 기본 개념은 수신측에서 송신측에게 현재 자신의 상태를 피드백 함으로써 이루어진다.
    - 혼잡제어
        - 송신측의 데이터 전달과 네트워크의 데이터 전달속도 차이로 인해 발생하는 문제를 해결하는 것
### 흐름제어
- 수신측이 송신측보다 데이터처리속도가 빠를때는 상관없지만 송신측이 더 빠를때는 문제가 발생한다.
- 수신측에서 제한한 용량을 넘어서 데이터가 도착할 경우에는 데이터가 손실될 수 있고
- 이러한 위험을 줄이기 위해서 송신측에서 수신측의 상태에 따라 보내는 데이터의 양을 조절해야한다.
- 해결방법
    - `Stop And Wait`: 매번 전송한 패킷에 대해 확인을 받아야만 다음 패킷을 전송하는 방법
    - Sliding Window
        - 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인없이 세그먼트를 전송할 수 있도록 하여 데이터 흐름을 동적으로 조절하는 제어기법
        - 동작방식
            - 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전송이 확인되는 만큼 윈도우를 옆으로 옮기면서 다음 패킷들을 전송한다.
        - TCP/IP 통신을 하는 모든 호스트 들은 송신용 윈도우와 수신용 윈도우를 가지고 있다.
        - 3 way handshake를 통해서 수신 호스트의 윈도우사이즈에 송신 호스트의 윈도우사이즈를 맞추게 된다.

### 혼잡제어
- 송신측에 데이터는 지역망이나 대형 네트워크를 통해 전달되게 된다. 하나의 라우터에만 데이터가 몰릴 경우에는 라우터가 자신에게 온 데이터를 처리할 수 없게 되고 결국 호스트에서 데이터의 재전송이 이루어지는데 이것은 결국 혼잡을 가중시켜서 오버플로우나 데이터의 손실을 불러올 수 있다.
- 이러한 네트워크의 혼잡을 피하기 위해서 송신측에서 보내는 데이터의 수를 강제로 줄이게되는데 이것을 혼잡제어 라고한다.
- 해결법
    - AIMD(Additive Increase / Multiplicative Decrease)
        - 처음에 패킷을 하나 보내고, 문제없이 도착하면 window의 사이즈를 1씩 증가시키는 방법이다.
        - 패킷 전송에 실패하면 window의 사이즈를 절반으로 줄인다.
        - 공평한방법이라는 장점이 있고, 여러 호스트들이 같은 네트워크에 진입할 때 처음에는 나중에 진입하는 호소트가 불리할 수 있지만 시간이 흐르면 평형상태로 수렴한다는 특징이 있습니다.
        - 하지만 윈도우 사이즈를 1씩 증가시킨다는 점에서 처음에는 높은 대역폭을 사용하지 못한다는 점과, 네트워크의 혼잡을 미리 아는 것이 아닌 혼잡을 경험 한 다음에 window를 감소시킨다는 점에서 네트워크의 혼잡을 미리 감지하지 못한다는 단점이 있습니다.
    - Slow Start(느린 시작)
        - AIMD방식이 네트워크 수용량 주변에서는 효율적으로 동작하지만 초반에는 전송속도를 올리는데 시간이 오래 걸린다는 단점이 있습니다.
        - Slow Start 방식은 AIMD 처럼 패킷을 하나 보내면서 시작하고 패킷 하나가 문제없이 도착할 때 ACK 패킷하나마다 window의 크기를 1씩 증가 시키는 방식입니다. 즉, 한 주기가 지나면 윈도우의 크기는 2배가 됩니다.
        - 전송속도는 AIMD와 비교해서 지수함수 꼴로 증가합니다.
        - 혼잡 현상이 발생하면 window 크기를 1로 감소시킵니다.
        - 처음에는 네트워크 수용량을 예상할 수 있는 정보가 없지만 한번 혼잡 현상이 발생하고 나면 네트워크 수용량을 어느정도 예측할 수 있습니다. 
        - 혼잡 현상이 발생하였던 윈도우 크기까지는 지수함수 꼴로 증가시키고 그 이후로는 1씩 증가시킵니다.
    - Fast Recovery(빠른 회복)
        - 혼잡이 발생하였을 때 window 크기를 1로 줄이지 않고, 절반으로 줄인 다음에 선형적으로 증가시키는 방법입니다. 
        - 혼잡이 한번 발생한 이후로는 순수한 AIMD방식으로 동작하게 됩니다. 