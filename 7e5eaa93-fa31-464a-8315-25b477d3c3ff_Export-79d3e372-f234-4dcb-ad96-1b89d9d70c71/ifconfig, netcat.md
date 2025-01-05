# ifconfig, netcat

<aside>
💡 author : agamtt 2023-11-30

</aside>

# 실습 환경 만들기

<aside>
💡 **여기를 참고합니다.**

[실습용 컨테이너 두개 만들기](https://www.notion.so/2a2d1af41ff440f2b16513d5561bcb3c?pvs=21)

</aside>

# ifconfig

ifconfig 는 interface config 의 약자로, 현재 리눅스 컴퓨터에 설치된 인터페이스와 IP 정보를 볼 수 있습니다.

우선 두개의 컨테이너에 모두 ifconfig 를 설치합니다.

ifconfig 는 net-tools 라는 프로그램 모음 중 하나입니다.

```bash
apt update
apt install net-tools
```

컨테이너에서 ifconfig 명령어를 사용하면 IP 를 확인할 수 있습니다.

```bash
ifconfig
```

ifconfig 는 인터페이스의 목록과 이름, 그리고 인터페이스에 부여된 IP 번호를 보여줍니다.

이전에 배웠듯, 네트워크 인터페이스는 실제로 존재하는 기계장치입니다.

네트워크 인터페이스 하나 당 하나의 IP주소를 가지고 있습니다.

실제로 입력해서 확인해보면 eth0 라는 인터페이스가 있고 lo 라는 인터페이스가 있습니다.

![Untitled](Untitled%20201.png)

이는 컴퓨터에 장착된 네트워크 카드를 ifconfig 가 조회한 것 입니다.

보통, eth0, eth1, eth2 … 처럼 이름 붙여집니다.

아래의 카드 하나가 eth0 하나 입니다.

![Untitled](Untitled%20202.png)

lo 는 localhost 를 나타내는 것으로, 저 인터페이스에 패킷을 보내면 다시 자신에게 돌아옵니다.

이는 컴퓨터 내부통신에 쓰입니다.

![Untitled](Untitled%20203.png)

## 실제 서버의 IP주소 확인

client 와 server 에서 ifconfig 명령을 사용해서 IP주소를 확인합니다. port 는 docker desktop 에서 확인할 수 있습니다.

- lo 는 컴퓨터가 자기자신과 통신할때 쓰는거니, 그거 말고 다른 인터페이스를 골라야합니다.
- inet 이라고 되어있는 부분 뒤의 숫자가 인터페이스에 부여된 IP 주소입니다.

![Untitled](Untitled%20204.png)

확인해보니, client 의 IP는 172.17.0.3 이고  server 의 IP는 172.17.0.2 임을 알 수 있습니다.

docker desktop 을 확인해보면 아까 docker container 를 생성했을때와 같이

server 의 port 는 8001, client 의 port 는 8002 인 것을 알 수 있습니다.

![Untitled](Untitled%20205.png)

따라서 다음과 같은 접속정보를 얻었습니다.

컴퓨터 통신을 하려면 반드시 IP 와 port 번호가 있어야합니다. 이를 **접속정보(connection details)**라고 합니다.

```
client ip : 172.17.0.3
client port : 8002

server ip : 172.17.0.2
server port : 8001
```

이 접속정보는 컴퓨터마다 다를 수 있습니다. 직접 확인해보고 본인 것을 이용해야합니다.

## netcat

이제 두 컴퓨터의 전화번호를 알았으니, 한쪽에서 전화를 걸 수 있습니다.

netcat 은 network cat 의 약자로, cat 명령어를 network 에서 쓸 수 있게 해주는 통신 프로그램입니다. 리눅스에서 가장 널리 쓰이는 네트워크 프로그램 입니다.

각각의 컨테이너에서 netcat 을 설치합니다.

```python
apt update
apt install netcat
```

컴퓨터 통신은 편지나 택배를 보내는 것처럼 보내는쪽과 받는쪽이 정해져 있습니다.

한번 연결이 수립되면 계속 통신을 주고 받을 수 있습니다. 이를 TCP 통신이라고 부릅니다.

netcat 도 보내는 쪽과 받는쪽이 정해져 있습니다.

편지는 보내는사람만 받는사람의 주소를 입력합니다. netcat 에서도 동일합니다.

보내는 쪽에서는 이렇게 합니다. 실제 IP주소와 실제 port 번호로 변경해야합니다. 

```bash
nc {상대방의 IP_address} {상대방의 port}
```

받는 쪽에서는 -l 옵션을 입력해야합니다. listen(듣다)의 약자입니다.

```
nc -l -p {나의 port}
```

저의 컴퓨터에서는 아래와 같습니다. 

server 에서는 아래를 입력합니다.

```bash
nc -l -p 8001
```

client 에서는 아래를 입력합니다.

```bash
nc 172.17.0.2 8001
```

단,  server 에서 먼저 입력해야합니다.

캐치볼에서 받는 사람이 받을 준비를 하고 있을 때 던져야하는 것처럼,

컴퓨터 통신도 상대방이 받을 준비를 하고 있어야 연결할 수 있습니다. 

컴퓨터 통신에서 “받을 준비” 를 **리슨(Listen)** 이라고 부릅니다.

그렇지 않으면 오류가 생깁니다. 아래는 그런 오류입니다.

![Untitled](Untitled%20206.png)

![Untitled](Untitled%20207.png)

서버에서 nc -l -p 8001 을 입력하면,

netcat 이 8001 포트를 열고 listen 상태로 대기합니다.  이 상태에서는 명령어를 입력해도 아무 반응이 없습니다.

Ctrl + C 를 누르면 nc 가 listen 을 멈추고 꺼집니다. port 도 다시 반납합니다.

![Untitled](Untitled%20208.png)

이제 클라이언트에서 nc {서버의 주소} {서버의 포트} 를 입력하면 서버로 연결됩니다.

아래 처럼, 바로 다시 명령어를 입력할 수 있게 되면 연결에 실패한 것 입니다. netcat 은 연결에 실패하여도 별도의 오류를 출력하지 않습니다.

![Untitled](Untitled%20209.png)

만일 정상적으로 연결에 성공하면 둘다 아무것도 출력되지 않는 연결 성공 상태가 됩니다.

이때, 양쪽 중 어딘가에서 텍스트를 입력하면 상대방에게 그대로 전송됩니다.

카카오톡 갠톡처럼 작동하는 컴퓨터 통신을 할 수 있게 됩니다.

![Untitled](Untitled%20210.png)

![Untitled](Untitled%20211.png)

우리는 해커로써, 컴퓨터 앞에 앉아서 전세계의 모든 컴퓨터에 접속할 수 있습니다.

또한 바이러스에 감염시킨 컴퓨터를 우리의 서버에 접속시켜야합니다.

따라서 컴퓨터 네트워크를 잘 이해하고 있어야 합니다.

이번에는 거꾸로 서버가 클라이언트에 접속하고, 클라이언트가 Listen 하려면 어떻게 nc 명령어를 입력해야하는지 생각해서 연결을 수립해보세요.

이 거꾸로 연결을 성공시키지 못하면 컴퓨터 네트워크를 이해하지 못한 것이니 이 페이지를 처음부터 다시 시작하세요. 

계속