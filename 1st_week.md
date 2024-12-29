## 1. 기본 지식


### 1.1. 운영체제 (OS)
- HW-SW 사이 중간 다리 역할

#### 1.1.1. Kernel
- Memory, CPU, file system, device, network 등 시스템 자원을 관리 & 프로세스 간의 통신과 스케줄링을 담당

#### 1.1.2. Shell
- 사용자와 운영체제 사이의 인터페이스
- 사용자가 shell을 통해 kernel에게 명령을 내림.   

  → shell이 사용자의 명령을 해석해서 kernel이 이해할 수 있는 형태로 변환


- 쉘 유형
  1. 명령어 기반 쉘

     : **CLI**(Command Line Interface)

     : 사용자가 터미널에 명령어를 입력해서 시스템과 상호작용하는 방식

        ex. Bash, Zsh

  3. **GUI**(Graphical User Interface)

     : 사용자가 아이콘이나 메뉴를 클릭해서 시스템과 상호작용


### 1.2. Terminal
-  CLI(Command Line Interface) : 텍스트로 명령어를 입력하여 운영체제와 상호작용하는 방식

-  Terminal 사용 이유
  1. 빠른 작업 처리
  2. GUI로는 할 수 없는 고급 시스템 관리 작업 수행
  3. 스크립트를 사용해서 반복적인 작업을 자동화

- 터미널 명령어 vs 프로그래밍 코드
  | **특징** | **터미널 명령어** | **프로그래밍 코드** |
  | --- | --- | --- |
  | **목적** | 즉각적이고 단순한 작업 수행 (파일 관리, 시스템 명령) | 복잡한 연산 및 로직 구현, 다양한 기능 제공 (프로그램 개발) |
  | **실행 방식** | 즉시 실행, 명령어 하나씩 독립적으로 실행 | 프로그램 전체가 논리적으로 흐름을 따라 실행 |
  | **조건문/반복문 지원** | 기본적으로 없음 (스크립트로 가능) | 조건문, 반복문, 함수, 클래스 등 다양한 구조를 지원 |
  | **사용 예시** | `ls`, `cd`, `cp` 등 파일 관리, 시스템 설정 | 웹사이트 개발, 데이터 분석, 인공지능 모델 개발 등 |
  | **언어** | 셸 명령어 (Bash, PowerShell 등) | Python, C++, Java, JavaScript 등 다양한 프로그래밍 언어 |
  | **스크립트 가능성** | 스크립트로 여러 명령어를 조합 가능 | 코드를 구조화하여 복잡한 프로그램 작성 가능 |


## 2. Docker
- 애플리케이션을 **컨테이너**라는 가상화된 환경에서 실행할 수 있도록 하는 컨테이너 기반 가상화 플랫폼
- 개발자가 애플리케이션을 어디서나 실행할 수 있도록 일관된 환경을 제공

###### Container
- 애플리케이션과 그에 필요한 모든 의존성을 하나의 패키지로 묶어 제공하는 일종의 가상화 기술
- 기존의 virtual machine과 달리 운영체제를 완전히 가상화하지 않으며, 대신 호스트 운영체제의 커널을 공유하여 가벼운 가상화 환경을 제공

###### 가상환경(Virtual Environment)
- 물리적 하드웨어와는 독립적으로 애플리케이션이나 소프트웨어를 실행할 수 있는 논리적 공간을 만드는 기술

###### Hyper-V
- Windows 운영체제에서 제공하는 가상화 플랫폼
- Docker Desktop에서 컨테이너를 구동할 때 필수적인 가상화 기능
- Windows Home 에디션은 기본적으로 Hyper-V를 지원하지 않기 때문에 Docker Desktop을 바로 사용할 수 없음.

  ⇒ Windows Subsystem for Linux 2 (WSL 2)를 사용

###### WSL2(Windows Subsystem for Linux 2)
- Windows 환경을 유지하면서 Linux 실제 커널을 사용할 수 있게 해주는 시스템
- 관리자 권한으로 Windows PowerShell 실행 후 명령어 입력

```PowerShell
# Windows Subsystem for Linux (WSL) 기능을 활성화하기 위한 명령
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

#  Virtual Machine Platform (VMP) 기능을 Windows에서 활성화하는 명령어
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## 3. VSCode 
### 3.1. VSCode의 기본 구조
1. **탐색기(Explorer)**
   - 폴더 안에 있는 모든 파일과 디렉토리를 탐색 가능
   
2. **편집기(Editing Area)**
  - 코드를 작성하는 곳
  - 여러 파일을 동시에 열 수 있음. → 각 파일은 탭(tab)으로 표시
  
3. **터미널(Terminal)**
    - 내장된 터미널 → 명령어를 직접 입력하고 실행 가능
    - **Ctrl + `**(틸드)
    
4. **확장(Extensions)**
   - 다양한 **확장 기능**을 설치해 개발 환경을 맞춤 설정
   - 언어 지원, 디버깅, 코드 완성 등의 기능을 추가 가능

### 3.2. VSCode 확장 기능
- **VSCode에서 Python 설정**
  1. Python 확장을 설치
  2. **Ctrl + Shift + P**를 눌러 명령 팔레트(Command Palette)를 열고, "Python: Select Interpreter"를 선택 →  사용할 Python 버전을 선택
  3. VSCode 내장 터미널에서 `python [파일명.py](http://파일명.py)` 또는 **"Run Python File in Terminal"** 옵션을 선택해 터미널에서 실행

- **VSCode에서 C 설정**
    1. **C/C++ 확장**을 설치 → C와 C++ 코드 작성 및 디버깅 가능
    2. **C compiler 설치**   

       **→  MinGW**(Minimalist GNU for Windows)
       
        → Linux/Mac - 기본적으로 **gcc**(GNU Compiler Collection) 설치
        
    3. 터미널에서 **gcc** 명령어로 C 파일을 컴파일하고, **실행 파일**을 생성한 후, 그 파일을 실행
 

- **VSCode와 Linux 명령어**
  - VSCode의 내장 터미널은 기본적으로 **Linux 명령어**를 사용할 수 있는 환경
    - Windows - WSL(Windows Subsystem for Linux) 필요
  - 리눅스 서버에 원격 접속
    - **Remote - SSH** 확장
