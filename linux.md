# 1. 리눅스 : 계정, 권한
## 1.1. Sudo 설치와 사용

### 1.1.1. 리눅스 유저 설정
리눅스의 권한 관리는 복잡하지만, 보통 아래의 내용만 알면 됩니다.  
그러니 이유는 나중에 설명하고, 실습을 통해 먼저 확인해보겠습니다.  
  
docker container를 처음 실행하면 **root로 로그인** 됩니다.  
![image](https://github.com/user-attachments/assets/a86a9cb3-fd3c-4a63-b50e-83cacc811eeb)
  
  
만일 **su 명령어**를 이용해 root에서 다른 유저로 **switch user**했다면, **exit 명령어를 실행하면 다시 이전 유저로 돌아올** 수 있습니다.  
  
**adduser**로 유저를 만들고 확인해봅시다.  
```bash
adduser username
```
(password 는 표시되지 않는 것이 정상입니다.)  
![image](https://github.com/user-attachments/assets/6c9d8a0f-099a-435e-b761-13f5d8348f15)
  
  
유저가 전부 생성되고 나면 해당 유저로 su해봅시다.
- **su**는 switch user 의 약자로, 유저를 변경하는 명령어입니다.
- **exit**은 su 로 변경했다가 다시 기존 유저로 돌아오는 명령어입니다.  
![image](https://github.com/user-attachments/assets/8f683fa2-f4c0-4d7b-8929-cc808d785fa6)
  
   
### 1.1.2. sudo 설치 및 사용
  
- **sudo**
  - **root가 아닌 계정**
  - 한 줄의 명령어만 root로 su한 후 실행시키고, 다시 원래 계정으로 돌아오는 명령어
  - sudo는 일반적으로 도커 컨테이너에 설치되어있지 않으므로, **apt로 설치** 필요
  
- **apt**
  - 프로그램을 설치하는 명령어
  
sudo 를 설치하는 명령어는 아래와 같습니다.  
프로그램 설치는 root로 실행해야하므로, **root인 상태에서만 sudo를 설치**할 수 있습니다.  
다른 유저로 로그인되어있다면 exit으로 root로 변경한 후 설치합니다.  
  
```bash
apt update
# 업데이트가 끝나면, 아래 명령어를 실행
apt install sudo
```
  
![image](https://github.com/user-attachments/assets/eb8bc685-0466-43c1-873d-807435ea1f1a)
    
![image](https://github.com/user-attachments/assets/b8d8f35d-b2c8-45b1-911e-944347c9b226)
  
  
보통 apt install하기 전에는 apt update를 하므로,  
두 개의 명령어를 한 줄로 실행하기 위한 명령어인 '**&&**'를 사용하여 둘을 연결합니다.  

```bash
apt update && apt install sudo
```
  
이제 adduser로 유저를 생성하고 sudo를 사용해봅시다.  
root가 아닌 유저에게 sudo를 사용할 수 있는 권한을 부여하려면 아래와 같이 입력합니다.  
당연히 **root인 상태**에서 부여해야합니다.  

```bash
usermod -aG sudo username
```
  
![image](https://github.com/user-attachments/assets/18e8c8a1-5ef3-43ae-b411-4c58ac642eae)
  
  
ls 명령어는 user의 권한으로 실행되지만,  
```bash
ls
```
  
**sudo ls는 root 의 권한으로 실행됩니다.**  

```bash
sudo ls
```
  
whoami는 지금 누구로 로그인 되어있는지 출력하는 명령어입니다.  
sudo를 붙이면, root 라고 표시됩니다.
  
아래의 이유에서 입니다.
> 💡 sudo 는 root 가 아닌 계정으로, 한 줄의 명령어만 root 로 su 한 후 실행시키고, 다시 원래 계정으로 돌아오는 명령어입니다.
  
![image](https://github.com/user-attachments/assets/c5baca10-e0e2-4c27-85a9-a1be7219c750)
  
  
이제 왜 그런지 자세히 설명해보겠습니다.  
  
계속  


  
## 1.2. root 권한과 sudo

### 1.2.1. whoami
가장 간단한 명령어는 whoami(나는 누구인가?) 입니다.  
리눅스에게 나는 누구인지 물어봅니다.  
이것은 **“현재 로그인한 유저” 가 누구인지 표시하는 명령어**입니다.  
  
docker exec으로 접속한 경우(docker desktop 의 Exec 탭으로 접속했거나, vscode 로 접속한 경우),  
**root**라는 계정으로 접속됩니다.  
  
whoami를 입력하면 root라고 표시됨을 알 수 있습니다.  

```bash
whoami
```
  
![image](https://github.com/user-attachments/assets/bb97e6e2-eee5-45b2-9c81-e5379a1a90bd)

  
이제 컴퓨터를 사용해보겠습니다.  
  
### 1.2.2. echo
**echo**는 메아리라는 뜻으로, **입력한 것을 그대로 화면에 결과를 출력하게하는 명령어**입니다.  
  
아래 명령어를 입력해봅시다.

```bash
echo hello
```
  
![image](https://github.com/user-attachments/assets/ae0cf63e-b000-4f5c-9e41-5269abdd8976)
  
그러면 리눅스가 똑같은 말을 앵무새처럼 화면에 출력합니다.  
    
### 1.2.3. root
root는 모든 리눅스에 존재하는 유저로, **관리자 계정**을 의미합니다.  
  - Windows의 Administrator와 동일합니다.  
  - macOS에서도 root라고 부릅니다. ~~(열받게 마이크로소프트 지혼자 다르게 부릅니다.)~~
  
리눅스는 **여러 계정을 생성해서 여러명이 쓰는데에 특화**되어있습니다.  
여러 계정이 쓸 때, 갑자기 한사람이 남의 파일을 삭제해 버리거나 하면 곤란합니다.  
  
따라서, 리눅스에는 root라는 관리자 계정이 있어서  
root만 모든 명령어를 제한없이 실행할 수 있고 다른 계정은 명령어 실행이 제한됩니다.  
  
다른 계정이 어떤 명령어를 실행하고 어떤 프로그램을 쓸 수 있고 어떤 파일을 삭제할 수 있는지는 root 가 규칙을 정합니다.
  
### 1.2.4. passwd
일단 처음 리눅스를 켜면, root 유저는 비밀번호가 설정되어 있지 않습니다.  
아래 명령어를 입력하면, **root 계정의 패스워드를 설정**할 수 있습니다.  

```bash
passwd root
또는
passwd
```
  
![image](https://github.com/user-attachments/assets/aed0ac8d-77eb-4848-969a-38af5b0dcab4)

  
> 🔥 **패스워드가 입력이 안되는데요?**
> 패스워드가 입력되지 않는 것처럼 보이지만, 실제로는 입력되고 있는 것 입니다.
> 이는 리눅스의 보안기능으로, 뒤에서 누가 비밀번호가 몇 글자인지 엿보는 것을 막기 위해서 아예 표시를 안합니다.
  
passwd 명령어는 한 번 더 입력하면 다시 새로운 비밀번호를 설정합니다.  
  
### 1.2.5. 일반유저
내가 컴퓨터 주인이더라도, 리눅스에서는 root 계정으로 컴퓨터를 사용하면 안됩니다.  
명령어 중 어떤 명령어는, 컴퓨터에 있는 모든 파일을 삭제할 수 있기 때문에 위험합니다.  
또한 나 말고 다른 사람이 저장해 놓은 파일을 덮어씌우거나 삭제하면 큰일납니다.  
  
따라서 반드시 **일반유저를 생성해서 사용**하고, **꼭 필요할 때 root로 로그인**해서 쓴 후, 다시 일반유저로 로그인해야 합니다.  

> 🔥 **root , 해커가 쓰면 큰일남**
> 해킹의 대부분 목적은 해커가 root 계정으로 로그인할 수 있게 만드는 것 입니다.
> 해커는 root 계정의 비밀번호를 훔치거나, 각종 해킹 방법을 써서 root를 뺏기 위해 노력합니다.
> 반대로 컴퓨터 주인은 root 계정을 뺏기지 않도록 해야합니다.
  
#### 1) adduser
adduser 는 유저를 새로 추가하는 명령어입니다.  
**이 명령어는 root 만 수행할 수 있습니다.**  
  
새로운 유저를 만들어봅시다.
※ 참고로, '{{ }}' 로 둘러쌓인 것이 명령어 안내란에 있으면, 실제 값을 넣어서 변경하라는 뜻입니다.  

```bash
adduser {{user name}}
```
  
저는 슈퍼해커이므로, superhacker라는 계정을 만들어보겠습니다. (참고로 유저네임은 영문 알파벳만 쓸 수 있습니다.)  

```bash
adduser superhacker
```
  
![image](https://github.com/user-attachments/assets/6ebe1b7c-3ea4-4a8a-86f7-70f848ba064a)

  
패스워드를 입력하라고 하면, 패스워드를 입력하고 엔터를 칩니다.
> 🔥 **패스워드가 입력이 안되는데요?**
> 패스워드가 입력되지 않는 것처럼 보이지만, 실제로는 입력되고 있는 것 입니다.
> 이는 리눅스의 보안기능으로, 뒤에서 누가 비밀번호가 몇글자인지 엿보는 것을 막기 위해서 아예 표시를 안합니다.
> New password와 Retype new password에서 입력한 패스워드가 동일해야 유저를 생성할 수 있습니다.
  
- Full Name이나 Room Number 같은 건 입력하지 않으셔도 됩니다. 엔터를 치면 생략됩니다.
- 마지막에 Is the information correct? 라고 물어보면, 그냥 엔터를 치거나 y를 입력하고 엔터를 칩니다.  
이제 일반유저가 생성되었습니다!  
  
#### 2) home 폴더
유저를 생성하면 유저의 home 폴더가 생성됩니다.  
이것을 Windows 나 Mac에서 일종의 바탕화면의 개념입니다.  
  
root 계정이 아닌 **일반유저는 각자의 home 폴더에만 들어갈 수 있습니다.**  
  
1. 현재 로그인한 계정의 홈폴더는 **직접 입력**해서 들어가거나,
  ```bash
  cd /home/superhacker
  ```

2. 또는 그냥 **물결**을 쓰면 자동으로 리눅스가 현재 로그인한 유저의 홈폴더를 입력해줍니다.
  ```bash
  cd ~
  ```

  
#### 3) su
su는 switch user의 약자로, **다른 유저로 변경하는 명령어**입니다.  
일종의 로그아웃, 로그인의 개념입니다.  
  
root에서 superhacker로, superhacker에서 root로 전환할 수 있습니다.  
  
root 명령어가 꼭 필요할 때만 root로 전환하고,  
평소에는 superhacker나 자기 계정으로 리눅스를 사용해야합니다.  
  
아래와 같이 입력하면, 입력한 유저로 변경됩니다.

```bash
su {{username}}
```
  
root로 로그인된 상태에서 whoami를 입력하면 root 라고 나옵니다.  
그리고 su superhacker 명령어를 입력하여 superhacker로 유저를 전환할 경우,  
whoami를 입력 시 superhacker라고 나옵니다.  
  
![image](https://github.com/user-attachments/assets/cde9204e-6b7c-42db-a5e7-aa108c897d3b)

  
이제 root로 다시 바꿔봅시다.  

```bash
su root
```
  
맨 처음에, passwd root로 root 계정의 패스워드를 설정했던 걸 기억해보세요.  
그 비밀번호를 입력하면 root로 로그인할 수 있습니다.  
  
패스워드가 틀린 경우, Authentication failure라고 나옵니다.  
  
![image](https://github.com/user-attachments/assets/13001cf5-d94c-48a0-8446-9cbd50a04c8d)
  
패스워드가 맞으면, root로 전환됩니다.  
whoami 를 입력해보니 root 라고 나오네요.  

![image](https://github.com/user-attachments/assets/d80fcf05-c84b-4d33-9aba-94ab76dc07f7)


  
#### 4) 기존유저로 돌아가기 : exit
리눅스에서는 어떤 유저에서 다른 유저로 su한 경우, 다시 기존 유저로 빠져나올 수 있습니다.  
안방에 화장실이 있으면, 화장실에 있다가 나오면 안방인 것처럼 작동합니다.  
  
exit 명령어를 사용하면 됩니다.  

```bash
exit
```
  
root인 상태에서 superhacker로 su 하고, exit 하면 다시 root로 돌아옵니다.  
아래 명령어를 순서대로 입력해보면 확인할 수 있습니다.  
  
![image](https://github.com/user-attachments/assets/fa6a1f6e-2223-4578-9181-94e4102ee06d)
  
  
  
### 1.2.6. 현재 컴퓨터에 있는 모든 계정 확인 : cat /etc/passwd
이제 adduser 로 유저를 계속 추가해서 사용할 수 있습니다.  
어떤 유저가 추가되어 있는지 어떻게 확인할까요?  
  
리눅스에서는 **cat 명령어를 이용**하여 텍스트파일을 읽을 수 있습니다.  

```bash
cat {{텍스트파일이름}}
```
  
adduser로 추가된 유저 이름 목록은 **/etc/passwd**라는 파일에 적혀있습니다.  
따라서 해당 파일을 읽어서 유저 이름 목록을 확인합니다.    

```bash
cat /etc/passwd
```
  
daemon, bin, sys, sync … 이상한 문자들이 출력됩니다.  
이것은 **리눅스가 정상적으로 동작하기 위한 유저**들로,   
사람이 사용하는 것이 아니라 **리눅스가 작동하기 위해서 사용**합니다.  
  
예를 들어, mail이라는 유저는 리눅스로 메일을 보낼 때 리눅스가 사용하는 계정입니다.  
이런 계정을 **리눅스 시스템 계정**이라고 부릅니다.  
  
그리고 맨 아래에 보면 아까 adduser로 생성한 superhacker 계정이 보입니다.  
**사람이 사용하는 계정에는 /bin/bash**라는 것이 적혀 있습니다.  
그것으로 사람이 사용하는 계정과 리눅스 시스템 계정을 구분할 수 있습니다.  
  
![image](https://github.com/user-attachments/assets/f45e5e06-3b76-4369-b653-a58898d3df3e)
  
  
    
### 1.2.7. userdel
**생성한 유저를 삭제**하려면 userdel 명령어를 사용합니다.  
**user delete**의 약자입니다.  

```bash
userdel -rf {{username}}
```

아까 생성한 superhacker 계정을 삭제해보겠습니다.  
  
    
### 1.2.8. sudo
앞서 언급했듯이, 중요한 작업을 할 땐 root 유저로 수행해야 하고,  
일반 작업을 할 때에는 adduser로 일반 유저를 생성하고 컴퓨터를 사용해야 합니다.  
  
즉, 어떤 명령어를 root로 실행하면 곧 바로 일반 유저로 su 해야 합니다.  

```bash
su root
(패스워드 입력)
(root 로 전환됨)
(root 로 실행할 명령어 입력)
su superhacker
```
  
위 명령은 su를 두 번 입력해야 해서 불편합니다.   
  
그리고, 여러명의 유저가 동시에 리눅스를 사용한다고 가정해봅시다.  
  
만일 어떤 유저가 root 계정을 사용해야 한다면 root 계정을 가진 사람이 그 유저에게 root 비밀번호를 알려줘야 합니다.  
만약 특정 기간 동안만 root 계정을 사용하게 하고, 그 이후에는 사용하지 못하게 하려면 root 비밀번호를 변경하는 수 밖에 없습니다.  
  
이 경우엔, 또 다른 유저는 root 계정을 잘 사용하다가도 비밀번호가 변경되면서 사용할 수 없게 됩니다.  
  
즉, “root 계정을 사용할 수 있는 사람”을 관리할 수 없습니다.
  
이것을 **권한 관리**라고 부릅니다.  
**리눅스는 권한 관리를 위해서 sudo라는 개념을 도입했습니다.**  
sudo는 super user do(최고 권한으로 실행한다)의 약자입니다.  
  
sudo를 특정 명령어 앞에 붙이면, 자동으로 root로 su하여 해당 명령어만 실행시킨 다음 다시 원래 유저로 su 합니다.  

```bash
sudo {{command}}
```

이는 아래 명령어와 같습니다.  

```bash
su root
{{command}}
su {{원래 유저}}
```
  
예를 들어 봅시다.  
아래 _echo hello_ 명령어는 'hello'를 화면에 출력하는 명령어입니다.  
이때 sudo를 앞에 붙임으로써, 일시적으로 root가 되어 명령어를 실행한 다음, 원래 유저로 돌아옵니다.  
  
echo hello는 일반 유저도 사용할 수 있는 명령어라서 동일하게 출력되지만,  
root만 실행할 수 있는 명령어는 sudo를 앞에 붙여야만 사용할 수 있습니다.  

```bash
sudo echo hello
```
  
그리고 sudo 그룹에 속한 유저는 root 계정의 비밀번호가 아니라, 자신의 비밀번호로 root 계정을 잠시 사용할 수 있습니다.  
root가 그 유저를 sudo 그룹에서 제거함으로써 root 권한 사용을 박탈하기 때문에, 관리가 용이합니다.  
  
리눅스 개발자들이 만든 이 방식을 **권한 관리**라고 부릅니다.  
지금도 거의 모든 컴퓨터가 이런 식으로 유저를 관리합니다.  
  
> 🔥 경우에 따라, sudo 명령어가 설치돼 있지 않을 수도 있습니다.
> 그럴 때는 아래 명령어를 입력해서 sudo를 설치합니다.
> 
> (root로 로그인 한 상태)
> _apt update_
> _apt install sudo_
  
sudo는 sudo 그룹에 속한 유저 계정만 사용할 수 있습니다.  
sudo 그룹에 속했다는 것은, 해당 컴퓨터의 root를 사용할 수 있는 사람에 속한다는 뜻입니다.  
root 유저만 sudo 그룹에 새 유저를 추가할 수 있습니다.  
  
이렇게 sudo 그룹에 속해서, 자신의 패스워드로 root 를 사용할 수 있는 유저를 **sudoers**(sudo를 하는 사람)라고 부릅니다.  
  
  
#### 1) sudo 그룹에 user 를 추가하기 : usermod

usermod 명령어를 이용하여 sudo 그룹에 어떤 유저를 새로 추가할 수 있습니다.  
  
**usermod -a**G는 user modify -append Group 의 약자로, 특정 유저를 특정 그룹에 속하도록 설정합니다.  

```bash
usermod -aG {{group}} {{username}}
```
  
어떤 유저를 sudo에 속하게 하려면 다음과 같이 입력합니다.  
단, 이 명령을 일반 유저가 수행할 수 있도록 하면 자기 자신을 sudo에 포함시킬 수 있으므로,  
오직 root 만 sudo 그룹에 새로 유저를 추가할 수 있습니다.  
  
superhacker 계정을 sudo 그룹에 추가하는 명령어입니다.  

```bash
usermod -aG sudo superhacker
```
  
이렇게 sudo 그룹에 속해지면서, root나 다름없는 권한을 갖게 됩니다.  
  
만일 sudo 그룹에 속하지 않은 사람이 sudo를 사용하려고 한다면 이렇게 출력됩니다.  
_“superhacker라는 계정을 sudoers가 아닙니다. 이 시도는 기록됩니다.”_  
※ 기록을 하는 이유는 해킹 시도가 있었을때 root가 알 수 있도록 하는 기능입니다.  
  
![image](https://github.com/user-attachments/assets/3f230df2-99e6-4e40-bfc1-6863019b2908)

    
이제 root로 로그인 된 상태에서 usermod -aG sudo superhacker를 사용하여 sudoers에 추가하면, sudo echo hello를 superhacker 의 비밀번호로 사용할 수 있게 됩니다.  

![image](https://github.com/user-attachments/assets/56d3a222-11d3-4b7d-a813-642f95be8459)
  
  
  
#### 2) sudo 그룹에서 삭제하기 : deluser

이제 특정유저가 sudo를 더 이상 사용하지 못하도록 sudo를 뺏는 방법 입니다.  
  
이 명령어는 root만 수행할 수 있습니다.  
따라서 sudo 그룹에 속한 다른 사용자가 sudo를 이용하여 실행하거나, root가 실행해야 합니다.  
  
```bash
sudo deluser {{username}} sudo
root 가 실행하는 경우,
deluser {{username}} sudo
```
  
아래와 같이 명령어를 수행하고 나면 더 이상 sudo를 이용할 수 없습니다.  
  
![image](https://github.com/user-attachments/assets/cff0c9bf-b865-4d7e-b149-9c8f81699022)
  
  
sudo는 **프로그램을 설치**하거나, **설정파일을 수정**할 때 필요합니다.  
  
리눅스는 여러 사용자가 함께 사용하는데에 특화되어 있는 운영체제입니다.  
sudo에 속한 사람이나 root가 프로그램 등을 설치해 놓고 설정한 다음 adduser로 사용자 계정을 만들면,  
사용자들이 접속해서 프로그램을 사용해서 계산을 하거나, 게임을 하거나, 인터넷을 하는 등 개인 목적으로 쓸 수 있습니다.  
  
중요한 동작은 sudo를 사용할 수 없으니, 컴퓨터가 고장나지 않게 여러 명이 사용할 수 있습니다.  
또한, 관리자를 여러 명으로 지정한 후, 그 관리자를 누구로 지정할지, 해제할지 편리하게 설정할 수 있습니다.  
  
  
### 1.2.9. 해킹과 root

sudo는 양날의 검입니다.  
  
root 권한을 쉽게 관리하게 만들면서도,  
악성프로그램을 설치할 수 있게 하고 해커가 컴퓨터로 들어올 수 있도록 설정파일을 수정하는 데에도 쓰일 수 있습니다.  
  
따라서 해커는 sudo, 즉 root를 탈취하고자 다양한 방법을 시도합니다.  
  
그런 방법들에 대해서 차차 배우겠습니다.  
  
계속  
  
  
    
## 1.3. 실행권한과 소유 ugo / chmod

### 1.3.1. 실행 권한
linux에는 한 서버에 많은 인원이 들어갈 수 있습니다.  
이는 때론 매우 유용하지만, 때론 큰 문제를 일으킬 수도 있습니다.  
한 사람이 다른 사람의 파일을 임의로 바꾸거나 삭제할 수 있다는 것이죠.  
  
이를 해결하기 위해 linux에는 **실행권한**이라는 것이 주어집니다.  
이를 통해 **본인, 그룹, 모든 유저**로 나누어 **read, write, delete 할 권한을 따로 부여**할 수 있습니다.  
- read : 파일을 읽는 것을 말합니다. 이 권한이 있으면 파일을 읽을 수 있습니다.
- write : 파일을 쓰는 것을 말합니다. 원래있던 파일을 편집하거나, 새로운 파일을 생성하는 것을 말합니다.
- delete : 존재하는 파일을 삭제하는 것을 말합니다.
  
먼저 한 파일의 실행권한을 확인해보도록 합시다.  

```bash
ls -la
```

![image](https://github.com/user-attachments/assets/fc8a9f59-f597-440b-a6c9-e1c3383ee978)
  
여기서 실행권한을 의미하는 것은 맨 왼쪽, 10개의 문자 부분입니다.  
  
이를 차례대로 보도록 하죠.  
![image](https://github.com/user-attachments/assets/03a78771-7add-41bc-9dc2-59396724c75c)
  
먼저 첫 글자 d 입니다.  
이는 파일의 종류를 의미합니다.  
  
이후부터는 3글자씩 나누어서 보도록 합시다.  
차례대로 user, group, others 를 의미합니다.  

| user | u | 소유 유저 |
| --- | --- | --- |
| group | g | 그룹  |
| others | o | 모든 유저 |
  
더욱 세분화하여 rwx의 각각의 의미를 보도록 하겠습니다.  

|  read | r |
| --- | --- |
| write | w |
| execute | x |
  
위는 권한을 의미하며 -의 경우 그 권한이 없다는 의미입니다.  
  
  
  
### 1.3.2. chmod 와 sudo
chmod로 파일을 변경하는 것은 시스템에서 아주 중요한 역할을 수행합니다.  
누군가가 악성코드나 시스템을 지우는 파일을 저장하고 실행권한을 부여할 수 있으면 해당파일을 실행할 수 있게 되어서 아주 위험합니다.  
  
**따라서 리눅스에서 chmod 명령어는 root 권한으로만 실행 가능합니다.**  
  
root로 로그인된 상태에서 사용하거나, sudo를 설치해서 root 권한으로 수행해야 합니다.  
  
#### 1) chmod
이제 이 권한을 바꿔보도록 하겠습니다.  
  
먼저 r,w,x간의 binary number 값을 보겠습니다.  

|  read | r | 4 |
| --- | --- | --- |
| write | w | 2 |
| execute | x | 1 |
  
여기서 허용하고 싶은 권한에 대해서는 그 수를 더하고 아닌 경우 그대로 놔두면 됩니다.  
예를 들어 모든 user, group, others가 read only로 만들고 싶은 경우 4인 것이죠.  

```bash
chmod 444 [filename]
```
  
이는 문자를 통해서도 변경할 수 있습니다.  
  
이 경우 user가 r,w,x의 권한을 가지도록 해보죠. 

```bash
chmod u+rwx [filename]
```

이 경우 -는 권한을 없애는 것이고, +는 권한을 부여하는 것입니다.  
  
계속  
  
# 2. 프로세스
## 2.1. 프로세스란?
명령어나 프로그램이 실행되어있는 상태를 프로세르라 합니다.  
  
### 2.1.1. 프로세스 끄기 : Ctrl + c
- GUI 환경에서 프로세스를 끌려면 걍 X 를 누르면 됩니다.
    
- 리눅스 CUI 터미널에는 X 버튼 같은게 없으니 아래를 입력해서 종료합니다.
  ```php
  ctrl + c
  ```
  - c 는 cancel(취소) 의 약자입니다. 왠지 Windows 의 복사 단축키와 겹칩니다. (copy 라서)
  - 참고로 리눅스에서 복사는 이것입니다.
    ```php
    ctrl + shift + c
    ```
  
### 2.1.2. 백그라운드에 있는 프로세스 끄기
어떤 프로세스들은 뒤에서 몰래 실행됩니다. (예를 들어 웹서버)  
이런 프로그램은 Ctrl + c 로 끌 수가 없습니다. (나랑 대화 중인 프로세스만 끌 수 있다)  
  
이런 프로세스들은 **PID**(프로세스 아이디라는 뜻)을 찾아서 꺼야합니다.  
  
**ps**는 **실행 중인 프로세스들과 PID를 출력**합니다.  
ps로 끄고 싶은 프로세스의 PID를 찾은 후 **kill로 종료**시키면 됩니다.  
  
```php
ps -aux | grep {process_name}
kill -9 {PID}
```
  
  
## 2.2. ps / kill
### 2.2.1. ps
ps는 process의 약자로, 프로세스를 관리하는 명령어입니다.  
Windows의 작업관리자처럼 **프로세스를 종료하거나 확인하는 데**에 쓰입니다.  
  
아시다시피, 컴퓨터는 OS와 OS에서 실행시킨 프로세스로 작동합니다.  

![image](https://github.com/user-attachments/assets/8ae2ad35-4817-4fd1-9ffd-3d066ee0ddc6)
  
이것은 리눅스에서도 마찬가지 입니다.  

![image](https://github.com/user-attachments/assets/0550b37f-7671-4786-84a9-5044e7d1e18b)
  
리눅스는 **프로세스를 번호로 구분**합니다.  
이 번호를 프로세스 아이디, **PID**라고 부릅니다.  
  
  
프로세스의 목록, 현재 상태를 출력하겠습니다.  

```bash
ps [option]
```
  
| 옵션 | 설명 |
| --- | --- |
| -e | 모든 프로세스 정보 출력 |
| -f | 프로세스의 다양한 정보 출력 |
| -a | 실행중인 전체 사용자의 모든 프로세스 출력 |
| -u | 프로세스를 실행한 사용자 정보와 프로세스 시작 시간 출력 |
| -x | 제어 터미널을 갖지 않는 프로세스 출력  |

- PID 확인
  ```bash
  ps -ef
  ```
  ![image](https://github.com/user-attachments/assets/15a6467f-5a66-4220-8215-03a0070375ba)
  
  
### 2.2.2. kill
현재 작동하는 프로세스를 종료시킬 때 사용하는 명령어입니다.  
대괄호 없이 숫자만 입력합니다.  

```bash
kill [option] [PID]
```
  
| 옵션 | 설명 |
| --- | --- |
| -1 | 재실행 |
| -9 | 강제 종료 |
| -15 | 기다렸다가 정상 종료 (default)  |

- 프로세스 강제 종료
  대괄호 없이 숫자만 입력합니다.
  
  ```bash
  kill -9 [PID]
  ```

계속

# 3. 환경변수
## 3.1. 환경변수란
환경변수란 운영체제의 여러 설정을 저장하고 있는 변수입니다.  
  
### 3.1.1. PATH
PATH라는 환경변수는 쉘에서 명령어를 실행했을때, 그게 **어떤 실행파일을 실행할지 경로를 지정하는 환경변수**입니다.  
예를 들어, python을 설치하고 python이라는 명령어를 터미널에 입력하면 자동으로 python이 실행됩니다.  

![image](https://github.com/user-attachments/assets/c8c82311-53db-4fa7-a61c-27d54b0cf26e)
  
그것은 쉘이 python이라는 명령어를 실행했을때, python.exe가 어디있는지 기억하고 있기 때문입니다.  
아래를 보면 PATH라는 환경변수에 Python이 어디에 설치되어있는지 저장되어있습니다.  

![image](https://github.com/user-attachments/assets/9ab44d8b-f44b-4c16-a68d-649afe4e1938)  
해당 경로로 가보면 실제로 python 의 실행파일이 저장되어있는 것을 확인할 수 있습니다.
  
![image](https://github.com/user-attachments/assets/a0c39c2c-9eda-4f2d-a9ae-96d84f32162d)  
  
환경변수를 확인하는 방법은 운영체제마다 다릅니다.  
구글에 검색하면 나옵니다.  
그러나 환경변수가 작동하는 방식은 모든 운영체제에서 동일합니다.  
  
  
### 3.1.2. 환경변수를 인간이 설정해야하는 경우
보통 수동으로 환경변수를 바꿔야하는 경우는 잘 없지만, 종종 발생합니다.  
Python을 설치할때, Add Python 3.7 to PATH 를 체크하면 Python 설치 프로그램이 자동으로 PATH에 Python을 추가합니다.  
  
![image](https://github.com/user-attachments/assets/fc5dc6a7-b686-4cc6-9c66-efe6acdd68f3)
  
그러나 만약 예를 들어, Python 3.11 버전과 Python 2.7 버전 두 개를 설치하면,   
쉘에 python 명령어를 실행했을 때 어떤 python을 실행해야하는지 컴퓨터가 알지 못합니다.  
  
주로, Python 2.7을 설치할 때 Add Python 2.7을 체크하고, Python 3.11 을 설치할 때 체크를 안하면 체크한 녀석을 기준으로 PATH가 설정되니, 터미널로 Python 3.11 을 실행할 수 없게 됩니다.  
  
vscode도 PATH를 읽어서 python을 integrated Terminal에서 실행하므로, vscode도 python 2.7로 코드를 실행합니다.  
(코딩 실습실에서 자주 일어나는 문제인데, 사실 내막은 거의 이렇습니다.)  
  
_우리는 해커이니, 이런 문제를 스스로 잘 해결하도록 합시다._  
  
또한 환경변수를 수동으로 편집하면, 본인만의 터미널 명령어를 설정할 수도 있습니다.  
PATH에 추가하고 그 곳에 직접 만든 프로그램을 위치시키면 됩니다.  
  
  
## 3.2. 각 운영체제 별 환경변수 설정법

### 3.2.1. Windows

![image](https://github.com/user-attachments/assets/b6fce7f6-4b33-4d7b-aed1-06206b43cab9)
  
![image](https://github.com/user-attachments/assets/aa2f0bb1-51a9-4cee-9c8c-c4bb9789b316)
  
  
### 3.2.2. MacOS
애플의 맥OS 의 환경변수를 설정하는 방법은 아래와 같습니다.  
[[번역] PATH (MacOS) : Mac OS에서 PATH 환경 변수 모범 사례](https://lovejaco.github.io/posts/path-macos-best-practice-for-path-environment-variables-on-mac-os/)
  
  
### 3.2.3. Linux
리눅스에서는 아래 명령어로 환경변수 확인이 가능합니다.  

```bash
env
```
  
리눅스는 **모든 명령어 즉 실행파일이 /bin 안**에 있으니, 해당 경로가 PATH에 저장되어있는 것을 확인할 수 있습니다.
![image](https://github.com/user-attachments/assets/6cde8edd-4d0c-471b-9a6a-f22f8a7209c0)
  
계속
  
# 4. 쉘스크립트
# 4.1. 쉘스크립트 : sh 파일로 명령어 여러개 자동으로 실행하기
