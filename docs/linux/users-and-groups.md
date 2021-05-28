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

`로그인 이름:로그인 암호 정의:UID:GID:사용자 이름(혹은 설명):홈 디렉토리 위치:로그인 쉘`  
이를 아래와 대칭해서 본다면, 이해가 쉬울 수 있습니다.  
`ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash`  

man page에서는 아래와 같이 설명되어 있습니다. 사실 상 man page가 가장 좋은 설명이라 판단하고 여기에 옮깁니다.

```html
NAME
       passwd - the password file

DESCRIPTION
       /etc/passwd contains one line for each user account, with seven fields delimited by colons
       (":"). These fields are:

       -   login name
       -   optional encrypted password
       -   numerical user ID
       -   numerical group ID
       -   user name or comment field
       -   user home directory
       -   optional user command interpreter

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

`/etc/passwd`에 있는 7가지 컬럼(필드)에 대한 설명을 하겠습니다.

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

### UID

UID는 User ID의 약어입니다. 모든 사용자는 고유의 ID 값을 가집니다. 그것은 숫자로 기록되고
`/etc/passwd`에 저장됩니다. 보통의 Linux에서는 사용자를 위한 UID를 1000번부터 시작합니다.

아래는 사용자의 UID를 확인하는 방법입니다. 그냥 `/etc/passwd`를 열어봐도 무관합니다.

```bash
jhin@home:~$ id
uid=1000(jhin) gid=1000(jhin) groups=1000(jhin),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev)
jhin@home:~$
```

`id`라는 명령을 내리면 현재 명령을 내린 사용자의 UID와 GID를 알려줍니다. GID는 Group ID로 아래 항목에서 설명합니다.

아래 예시와 같이 `id` 명령 뒤에 특정 사용자 이름을 지정하면, 해당 사용자의 UID와 GID를 화면에 출력합니다.

```bash
jhin@home:~$ id nobody
uid=65534(nobody) gid=65534(nogroup) groups=65534(nogroup)
jhin@home:~$
```

UID는 사용자를 생성할 때 시스템에서 자동으로 1000번부터 부여하지만, 임의로 지정할 수도 있습니다.
하지만, 사용하지 말아야할 영역(range)이 있는데, 그 영역은 0번부터 99번입니다. 이 영역은
시스템에서 사용하고, privilege, 특권 사용자를 위해 남겨둡니다. UNIX에서는 여기까지가 제한인데,
대부분의 Unix-like 시스템들에서는 그리고 Linux에서도 100번부터 499번까지도 일반 사용자가 사용하지 않도록 하고 있습니다. 이 또한 여러 용도에 따른 시스템에서 동적으로 사용자를 배정하여 사용하는 영역입니다.  

Debian 계열 (Ubuntu도 여기에 해당됩니다) Linux에서는 다음과 같은 사용자 제한 영역이 있습니다.  
0 - 99, 100 - 999, 60000 - 64999, 65000 - 65533.  
그래서, Linux에서는 사용자를 생성할 때 1000번부터 부여하여 (일반적으로는) 59999번까지 사용합니다.

어떤 UNIX에서는 UID에를 관리할 때 15bit 값을 사용하여 32,767개의 값을 지정할 수 있었는데,
실상 Solaris/SunOS를 비롯한 대부분의 UNIX에서는 16bit 값을 사용하여 65,536개의 고유값을 부여할 수 있었습니다. 하지만, Solaris 2.0/SunOS 5.0 이후 32bit 체계를 갖추면서 32bit 값을 UID에서도 사용하여 2^32개, 4,294,967,296개의 고유한 UID값을 지정할 수 있게 되었습니다. Solaris는 1990년부터 이것이 적용되었고, Linux는 2001년, 21세기가 되어서야 가능하게 되었습니다.

`root`사용자는 UID `0`로 지정됩니다. `nobody`는 UID `65534`를 가지게 됩니다. 이 두 사용자는 UNIX, Unix-like 시스템에서 상징적인 로그인 이름입니다.

### GID

GID는 Group ID입니다. 그룹에 부여하는 고유의 숫자값입니다. UID는 하나의 사용자에게 하나만 (원칙적으로) 부여할 수 있지만,
하나의 사용자는 여러게의 그룹에 속할 수 있습니다. 앞서 본 예시를 다시 보면,

```bash
jhin@home:~$ id
uid=1000(jhin) gid=1000(jhin) groups=1000(jhin),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev)
jhin@home:~$
```

와 같이, `jhin`이라는 사용자는 그룹, `jhin, adm, cdrom, sudo, dip, plugdev`에 동시 속해 있습니다.
GID 배정은 `/etc/passwd`에 있지 않습니다. `/etc/group` 파일에 정의되어 있습니다.

`/etc/group`은 권한있는 사용자가 편집할 수도 있고, `addgroup`, `groupadd`로 추가, `delgroup`, `groupdel`으로 삭제하는 명령도 마련되어 있습니다.

다중 그룹 지정은 매우 간단합니다.  
아래는 `/etc/group` 내용 중 일부를 발췌한 것입니다.

```bash
lightdm:x:114:
rdma:x:115:
rtkit:x:116:
lpadmin:x:117:root,pi
ssl-cert:x:118:
pulse:x:119:
```

위의 예시에서 `lpadmin:x:117:root,pi` 와 같이 마지막 컬럼에 로그인 이름을 추가해 주면 됩니다.

### 사용자 이름

이 부분에는 본디, 각 사용자의 이름을 적어 넣는 용도였습니다. 사람이 구분하기 위함입니다. 최근에는 그저 comment를 남기는 용도로도 사용됩니다. `finger`라는 명령으로 로그인 이름을 찾아보면 이 부분이 출력되는 걸 볼 수 있습니다.

```bash
$ finger gnats
Login: gnats          			Name: Gnats Bug-Reporting System (admin)
Directory: /var/lib/gnats           	Shell: /usr/sbin/nologin
Never logged in.
No mail.
No Plan.
$
```

### 홈 디렉토리

해당 사용자의 홈 디렉토리를 지정합니다. 끝 :-)

### 로그인 쉘

해당 사용자가 사용할 로그인 쉘을 지정합니다. 일반적으로는 `/bin/bash`가 지정됩니다. `/etc/passwd`를 살펴보면,
`/usr/sbin/nologin`으로 지정되어 있는 것을 볼 수 있습니다. 이 지정은 해당 사용자가 쉘을 획득하는 것을 방지합니다.