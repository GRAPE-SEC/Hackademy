# C언어 개발환경 : gcc 설치

<aside>
⚠️ author : 전재호(agamtt) 2024-03-12

</aside>

gcc 는 C언어를 컴파일하여 실행파일로 만드는 리눅스 패키지 입니다.

C언어로 코딩을 해야하니, 설치합니다.

```bash
apt update
apt install -y gcc
```

이제 잘 되는지 테스트합니다. hello.c 를 만들고, 아래를 코딩합니다.

```c
#include <stdio.h>
int main(){
	puts("Hello C World!\n");
	return 0;
	}
```

아래 명령어로 컴파일 합니다.

```c
gcc -o hello hello.c
```

![Untitled](Untitled%20282.png)

이러면 준비 완료입니다.

끝