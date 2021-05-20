# 사용자와 그룹

user 사용자와 group 그룹은 단순히 리눅스에 로그인하고
신분을 증명하는 용도 이외에도 다양한 구분을 위하여 사용됩니다.
대표적이 사용은 아무래도 파일과 디렉토리 그리고 프로세스에 권한에 관한 권한 구분이라고 할 수 있겠습니다.
먼저 여기에서는 사용자와 그룹에 대한 기본적인 내용을 설명하겠습니다.

## /etc/passwd

사용자에 대한 개별 설정은 `/etc/passwd`에 모두 담겨 있습니다. `/etc/passwd`를 열어 보겠습니다.

```bash
ubuntu@node0:~$ cat /etc/passwd |grep $USER
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
ubuntu@node0:~$ 
```

위 예시는 /etc/passwd 파일에서 ubuntu라는 계정을 찾아 그 행을 본 것입니다. /etc/passwd 파일은 이렇게 사용자 정보가 기록되어 있습니다.
`:`으로 구분된 이 행은 한 사용자에 한 줄씩 정의 되어 있는데, 다음의 순서로 이루어져 있습니다.

`로그인 이름:로그인 암호 정의:UID:GID:사용자 이름(혹은 설명):홈 디렉토리 위치:쉘`  
이를 아래와 대칭해서 본다면, 이해가 쉬울 수 있습니다.  
`ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash`  

### 로그인 이름

로그인 이름(login name)은 
입니다.

만약 `finger`가 미리 설치되어 있지 않다면, 다음과 같이 설치할 수 있습니다.

```bash
$ sudo apt install finger
```