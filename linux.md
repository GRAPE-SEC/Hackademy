# 1. 리눅스 : 계정, 권한
## 1.1. Sudo 설치와 사용

### 1.1.1. 리눅스 유저 설정
리눅스의 권한 관리는 복잡하지만, 보통 아래의 내용만 알면 됩니다.  
그러니 이유는 나중에 설명하고, 실습을 통해 먼저 확인해보겠습니다.  
  
docker container를 처음 실행하면 **root로 로그인** 됩니다.  
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/21baff1b-b0cd-4c84-939e-53b0141cf33a/Untitled.png)
  
만일 **su 명령어**를 이용해 root에서 다른 유저로 **switch user**했다면, **exit 명령어를 실행하면 다시 이전 유저로 돌아올** 수 있습니다.  
  
**adduser**로 유저를 만들고 확인해봅시다.  
```bash
adduser username
```
(password 는 안 표시되는 것이 정상입니다.)  
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/3afc48d1-7ea5-49b2-8e75-02aadb4db468/Untitled.png)
  
유저가 전부 생성되고 나면 해당 유저로 su해봅시다.
- **su**는 switch user 의 약자로, 유저를 변경하는 명령어입니다.
- **exit**은 su 로 변경했다가 다시 기존 유저로 돌아오는 명령어입니다.  
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/c60f52ab-8956-4347-991a-ea0331f231c2/Untitled.png)

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
  
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/2713cf76-d074-443f-8e20-0ebe838b35af/Untitled.png)
  
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/d5714555-f3c2-4f62-a168-ac59dff5b1a3/Untitled.png)
  
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
  
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/899cc790-fc48-4763-88e1-f9b53f86053e/Untitled.png)
  
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
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/f2f55f1e-9ff1-4225-b4dd-f9379b66113d/Untitled.png)
  
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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/e69d83da-e04c-46f4-a42c-677f026b1bac/Untitled.png)
  
이제 컴퓨터를 사용해보겠습니다.  
  
### 1.2.2. echo
**echo**는 메아리라는 뜻으로, **입력한 것을 그대로 화면에 결과를 출력하게하는 명령어**입니다.  
  
아래 명령어를 입력해봅시다.

```bash
echo hello
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/cf7a96f6-26bd-4a1f-ad41-237586ee1a6a/Untitled.png)

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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/322ffef0-578d-40ed-981b-846c2b9fcee7/Untitled.png)
  
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
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/05d3f8bd-230a-4b4f-8fd8-6ae3d71c8c11/Untitled.png)
  
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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/316919cc-e3e5-4e35-a6c6-bd39bd2cba3a/Untitled.png)
  
이제 root로 다시 바꿔봅시다.  

```bash
su root
```
  
맨 처음에, passwd root로 root 계정의 패스워드를 설정했던 걸 기억해보세요.  
그 비밀번호를 입력하면 root로 로그인할 수 있습니다.  
  
패스워드가 틀린 경우, Authentication failure 라고 나옵니다.  

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/8a3f54bb-c2fa-4b71-887f-38e48e943a56/Untitled.png)
  
패스워드가 맞으면, root로 전환됩니다.  
whoami 를 입력해보니 root 라고 나오네요.  

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/5eb47674-97e0-41c8-8c01-94f58fd5e5cb/Untitled.png)

  
#### 4) 기존유저로 돌아가기 : exit
리눅스에서는 어떤 유저에서 다른 유저로 su한 경우, 다시 기존 유저로 빠져나올 수 있습니다.  
안방에 화장실이 있으면, 화장실에 있다가 나오면 안방인 것처럼 작동합니다.  
  
exit 명령어를 사용하면 됩니다.  

```bash
exit
```
  
root인 상태에서 superhacker로 su 하고, exit 하면 다시 root로 돌아옵니다.  
아래 명령어를 순서대로 입력해보면 확인할 수 있습니다.  

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/cca2cec7-1bcb-4fa3-b744-2c10a94b1a8b/Untitled.png)

  
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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/72cd2ed5-ff81-49c5-af3b-34f8a162780b/Untitled.png)

  
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
  
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/1f659d12-2598-44fe-a44d-3bd1be13a54d/Untitled.png)
  
이제 root로 로그인 된 상태에서 usermod -aG sudo superhacker를 사용하여 sudoers에 추가하면, sudo echo hello를 superhacker 의 비밀번호로 사용할 수 있게 됩니다.  

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/96f6cfd1-756b-4a1f-b159-2b11951abce1/Untitled.png)
  
  
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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6617195d-3c86-450f-920c-3551e4bc12a3/df7ca1cc-e854-4d26-a87d-496bdaead8de/Untitled.png)
  
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

# 2. 프로세스
## 2.1. ps / kill

# 3. 환경변수
## 3.1. 환경변수란

# 4. 쉘스크립트
# 4.1. 쉘스크립트 : sh 파일로 명령어 여러개 자동으로 실행하기
