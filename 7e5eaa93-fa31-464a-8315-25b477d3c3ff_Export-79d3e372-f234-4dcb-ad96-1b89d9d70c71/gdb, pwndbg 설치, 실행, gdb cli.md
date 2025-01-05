# gdb, pwndbg 설치, 실행, gdb cli

<aside>
💡 author : 전재호(agamtt) 2024-03-15
contributor : 손기민(KiminSon) 2024-04-07

</aside>

디버깅을 하기 위해, 디버거를 설치하겠습니다.

리눅스용 바이너리 디버거로는 gdb 가 가장 유명합니다.

다른 디버거의 사용법도 gdb 와 매우 유사하므로, gdb 를 배우면 다른 디버거도 쉽게 배울 수 있습니다.

# gdb 설치

gdb 는 gnu debugger 의 약자로, 리눅스용 디버거 입니다.

```bash
apt update && apt install -y gdb
```

# pwndbg 설치

pwndbg 는 gdb 용 플러그인입니다.

[GitHub - pwndbg/pwndbg: Exploit Development and Reverse Engineering with GDB Made Easy](https://github.com/pwndbg/pwndbg)

주의할 점은, root 권한으로 setup.sh 를 실행해서는 안됩니다. 그러면 root 만 pwndbg 를 사용할 수 있고, 일반 유저가 pwndbg 를 쓸 수 없습니다.

```bash
apt install -y git

su user # user 는 실제 리눅스 유저명으로 대체, root 로 아래를 실행하면 안됨.

# git clone 할 디렉토리는 자유롭게 선택, /home/user 안이 적당.
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```

- git clone 을 하고 나면 pwndbg 라는 폴더에 pwndbg 설치파일을 내려 받습니다.
- pwndbg 폴더로 들어가서, setup.sh 를 실행하면 설치됩니다.

# pwndbg 유니코드 설정

pwndbg 로 한글과 같은 다국어를 사용하려면 아래 설정을 해야합니다.

두개의 명령어를 실행하면 됩니다. (root 가 아니라 user 의 권한으로 해야함)

```bash
echo 'export LANG=C.UTF-8' >> ~/.bashrc
source ~/.bashrc
```

# gdb 와 pwndbg 가 잘 작동하는지 확인

이제 grappe_story 실행파일이 있는 디렉토리에서 아래의 명령을 수행합니다.

gdb 안에서 게임이 실행됩니다.

```bash
gdb ./grapple_story_2
```

이제, gdb 안에서 명령어를 내릴 수 있습니다.

이제 게임이 제대로 실행되는지 확인해봅시다. r 은 실행(run) 의 약자입니다.

```bash
pwndbg > r 
# 또는
pwndbg > run
```

![Untitled](Untitled%20313.png)

게임을 종료하고 나가려면 quit 명령어를 입력합니다.

```bash
pwndbg > quit
```

# gdb shell

아래는 리눅스의 쉘 입니다. 리눅스 쉘 커맨드를 실행합니다.

![Untitled](Untitled%20314.png)

gdb 를 실행하면 gdb 명령어를 실행할 수 있는 쉘이 새로 열립니다.

이것은 리눅스 쉘이 아니고, gdb 에 명령을 내리는 gdb shell 입니다.

이것을 앞으로 **gdb shell** 이라고 부르겠습니다.

![Untitled](Untitled%20315.png)

아래와 같이 쓰고 명령어를 쓰면, gdb 안에서 gdb shell 에 입력하라는 의미로 이해하면 됩니다.

```bash
pwndbg>
# 또는
gdb>
```

예시) gdb shell 에서 quit 입력하세요

```bash
gdb> quit
```

계속