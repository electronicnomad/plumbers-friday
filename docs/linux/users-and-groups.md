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

man page에서는 아래와 같이 설명되어 있습니다. 사실 상 man page가 가장 좋은 설명이라 판단하고 여기에 옮깁니다.

```html
NAME
       passwd - the password file

DESCRIPTION
       /etc/passwd contains one line for each user account, with seven fields delimited by colons
       (":"). These fields are:

       o   login name

       o   optional encrypted password

       o   numerical user ID

       o   numerical group ID

       o   user name or comment field

       o   user home directory

       o   optional user command interpreter

       The encrypted password field may be blank, in which case no password is required to
       authenticate as the specified login name. However, some applications which read the
       /etc/passwd file may decide not to permit any access at all if the password field is
       blank. If the password field is a lower-case "x", then the encrypted password is actually
       stored in the shadow(5) file instead; there must be a corresponding line in the
       /etc/shadow file, or else the user account is invalid. If the password field is any other
       string, then it will be treated as an encrypted password, as specified by crypt(3).

       The comment field is used by various system utilities, such as finger(1).

       The home directory field provides the name of the initial working directory. The login
       program uses this information to set the value of the $HOME environmental variable.

       The command interpreter field provides the name of the user's command language
       interpreter, or the name of the initial program to execute. The login program uses this
       information to set the value of the $SHELL environmental variable. If this field is empty,
       it defaults to the value /bin/sh.
FILES
       /etc/passwd
           User account information.

       /etc/shadow
           optional encrypted password file

       /etc/passwd-
           Backup file for /etc/passwd.

           Note that this file is used by the tools of the shadow toolsuite, but not by all user
           and password management tools.
```

### 로그인 이름

로그인 이름(login name)은 인터넷 서비스에서 여러분께서 사용하는 ID와 같은 역할을 합니다.
알파벳으로 시작해야 하며, 숫자와 알파벳을 혼용하여 사용할 수 있으며 최대 8자까지 가능합니다.

### 로그인 암호 정의

(아주) 예전에는 이 컬럼에 암호환된 암호가 나열되어 있었다. 지금은 `x`라는 표식으로 `/etc/shadow`에
암호가 저장되어 있다라는 의미로만 쓰입니다. UNIX에서는 이 항목에 `NP`와 `LK`와 같은 방식으로 표현하기도 합니다.
`NP`는 no password, `LK`는 locked를 의미합니다. 사용자가 login해야 하거나 password가 발급되어 있어야
하는 계정이 아닌 경우에, 이와 같이 password 정의를 하지 않거나, 못 하게 하는 것입니다.

`/etc/passwd`에 기록되던 암호가 `/etc/shadow`로 옮겨기게 된 이유는 역시 보안 때문이었습니다.
한 시스템에 로그인 할 수 있는 모든 사용자가 다른 사용자의 암호도 볼 수 있었고, 이를 통해서 허용되지 않은
방법으로 권한을 획득할 수 있었기 때문입니다. `/etc/passwd`는 모든 사용자가 읽어 들일 수 있습니다.

```bash
$ ls -al /etc/passwd
-rw-r--r-- 1 root root 1950 Apr 30 12:57 /etc/passwd
```

그래서 이 부분을 분리하여 `/etc/shadow`로 암호를 저장하고, `/etc/shadow` 파일은 root user 권한으로만
read write 권한을 주고, (경우에 따라서는 아래 예제와 같이 shadow group의 권한에 read 권한을 주어서)
내용을 볼 수 있는 권한을 축소하여 임의의 (손쉬운) 탈취를 막았습니다.

```bash
$ ls -al /etc/shadow
-rw-r----- 1 root shadow 1077 Apr 30 12:57 /etc/shadow
```

`/usr/bin/passwd`라는 명령, 즉 암호를 변경하거나 지정하는 명령에 [Set UID](https://en.wikipedia.org/wiki/Setuid)를 지정하여 운영하도록 했습니다.
Set UID의 지정은 아래의 예제에서 보이는 것처럼, `s` 기호가 user permission 부분에 `x`대신 표식으로 보이고, 이를 실행하는 사용자에게 일시적으로
root user의 권한을 허용하는 것입니다.

```bash
$ ls -al /usr/bin/passwd
-rwsr-xr-x 1 root root 45612 Jul 27  2018 /usr/bin/passwd
```


## 참조

[Managing Users & Groups, File Permissions & Attributes and Enabling sudo Access on Accounts – Part 8](ttps://www.tecmint.com/manage-users-and-groups-in-linux/)