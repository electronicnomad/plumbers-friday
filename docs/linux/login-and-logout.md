# 로그인과 로그아웃

> login and logout

어떤 호스트에 정상적인 접근[^1]을 시도하면 처음 만나는 대화형 화면이
login prompt 로그인 프롬프트입니다. 대략 아래와 같이 표현됩니다.

[^1]:
  접근이라는 방식에 원격 접속과 직접 장치에 연결된 키보드 마우스 모니터를
  사용하는 방법, 두 가지 모두를 의미하겠다. 전자(前者) 경우에는
  사용할 수 있는 방식이 `rlogin`, `telnet`, `ssh`, `ftp`, `sftp` 등이 있습니다.
  이 중에서 `rlogin`, `telnet`, `ftp`는 보안의 문제로 이제는 사용하지 않는 것이 상식이 되었습니다.
  후자(後者)의 경우는 여러분이 익숙한 윈도나 맥 컴퓨터를 사용하는 것과
  다름이 없습니다.

```bash
host0 login:
```

위 예시에서 `host0`는 해당 호스트의 이름, 즉 hostname 호스트 네임입니다.
이 로그인 프롬프트에 user name 사용자 이름(account name 계정 이름이라고 부르기도 합니다)과
password 암호를 대화식으로 입력하여 다음 단계인, shell prompt 쉘 프롬프트를 만나게 되는 과정을
login 로그인이라고 합니다. 그 반대의 작업이 즉, 접속을 종료하는 과정을
logout 로그아웃이라고 합니다. 대략 아래와 같은 화면을 만날 수 있습니다.

```bash
host0 login: user1
password:
Linux bastion 5.10.11-v7+ #1399 SMP Thu Jan 28 12:06:05 GMT 2021 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Mon Feb 22 13:22:31 2021 from 192.168.1.91
user1@node0:~$
```

Windows 윈도 계열에서 logon 로그온 그리고 logoff 로그오프라고 표현하는
그것과 같은 것입니다.

## 메시지

UNIX 계열 OS들은 사용자가 로그인할 때 환영(혹은 경고) 메시지를 터미널에 출력하는
기능을 가지고 있습니다. 다중 사용자들에게 관리자가 알리고 싶은 메시지를 강제
화면 출력을 하게 하는 것이 목적이었습니다. 그 메시지는 두 가지로 나뉘어 다루어지게 됩니다.
하나는 `/etc/motd`이고 나머지는 `/etc/issue`라는 파일로 존재합니다.

### motd - message of the day

`/etc/motd`에 기록된 메시지는 사용자가 로그인할 때마다 터미널에 출력됩니다.
message of th day의 약자가 motd인 만큼, 앞서 설명한 것과 같이 시스템 관리자가
사용자들에게 공람 가능한 게시판을 운영하는 것과 같습니다.

위 예제에서 `password:` 라인 아랫쪽에서 `user1@node0:~$` 윗쪽까지의
내용이 이에 해당됩니다. 만약 이 메시지를 변경하고 싶다면,
(대부분의 UNIX/Linux 배포판에서) `/etc/motd` 파일을 편집하면 됩니다.

#### Ubuntu의 경우

Ubuntu의 경우에는 이 부분이 조금 복잡하게 구성되어 있습니다.
한 번 들여다 보겠습니다.

```bash
ubuntu@node1:~$ cd /etc/
ubuntu@node1:/etc$ ls -al |grep motd
drwxr-xr-x  2 root root       4096 Feb  1 11:17 update-motd.d
ubuntu@node1:/etc$ cd ./update-motd.d/
ubuntu@node1:/etc/update-motd.d$ ls
00-header     50-landscape-sysinfo  85-fwupd              91-release-upgrade      95-hwe-eol      98-fsck-at-reboot
10-help-text  50-motd-news          90-updates-available  92-unattended-upgrades  97-overlayroot  98-reboot-required
ubuntu@node1:/etc/update-motd.d$
```

위에서와 같이 `/etc/motd`는 존재하지 않고, 대신 `/etc/update-motd.d`라는
디렉토리가 있습니다. 그 속에 여러 파일들이 `/etc/motd`의 단순한 기능을
확장하여 제공하고 있습니다. 이 파일들은 shell scripts 쉘 스크립트들로 이루어 져 있으며,
파일 이름에서 지정된 수자(數字) 순서로 실행되는 것 뿐입니다.
이제는 과거의 유물이 되어버린 - 하지만, 제가 사랑하는 - rc scripts와 같습니다.

Ubuntu에서 제공하는 동적 message of the day는 아래와 같습니다.

```bash
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-1029-raspi aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Mar  6 15:50:36 UTC 2021

  System load:  0.08              Temperature:            48.7 C
  Usage of /:   8.6% of 29.05GB   Processes:              132
  Memory usage: 3%                Users logged in:        0
  Swap usage:   0%                IPv4 address for wlan0: 192.168.1.21

 * Introducing self-healing high availability clusters in MicroK8s.
   Simple, hardened, Kubernetes for production, from RaspberryPi to DC.

     https://microk8s.io/high-availability

0 updates can be installed immediately.
0 of these updates are security updates.


Last login: Sat Mar  6 11:29:09 2021 from 192.168.1.91
ubuntu@node1:~$
```

Canonical/Ubuntu에서는 이것을 'dynamic MOTD generation'이라고 설명하고 있는데,
그 구성은 그렇게 복잡하지는 않습니다. 이에 대한 상세한 설명은
`$ man update-motd`를 터미널에서 입력하여 man page 맨 페이지를 읽는 것을
추천합니다.

### /etc/issue

이 파일도 메시지를 출력하는 것입니다. login prompt 로그인 프롬프트가 출력되기
직전에 이 파일의 내용을 읽어드려 접속하려는 사용자의 터미널에 출력해 줍니다.

한 때, host 호스트의 OS 이름과 버전을 출력해 내는 `/etc/issue`는
시스템에 부정하게 침입하려는 자들에게 불필요한 정보를 제공한다는 사유로
`# cat /dev/null > /etc/issue`라는 명령을 root 루트 사용자가 입력함으로써,
0 byte 바이트 파일로 만들기도 했습니다.

요즈음처럼, rlogind와 telnetd가 배제되고 ssh/sshd를 이용하여
원격 host 호스트에 접근하는 경우 로그인 프롬프트를
볼 기회조차 없어서 거의 용도가 폐기 상태에 가깝다 할 수 있습니다.

#### /etc/issue.net

지금 이 글을 읽는 분께서 터미널을 열어 자신이 로그인할 수 있는 UNIX/Linux 머신에서
`$ ls /etc/issue.net`이라는 명령을 내렸을 때 이 파일을 발견할 수 있다면,
그 머신은 (혹은 그 운영체제는) 상당히 오래된 것이거나,
오래된 전통을 존중하는 패키징을 가지고 있을 것입니다.

과거 UNIX 시스템은 `/etc/issue.net`은 `/etc/issue`와 구분되어 관리되었고 그 역할도 달랐습니다.
파일 이름 마지막에 `net`이라는 단어가 붙어진 것에서 유추할 수 있듯이, `/etc/issue.net`은
네트워크를 통하여 접근[^2]하는 사용자에게 알림 전하기 위해서 존재했고,
`/etc/issue`는 네트워크로 접근하지 않는 - 다른 방식의 터미널이나 콘솔도 여기에 해당 -
사용자에게 전달할 메시지가 수록됩니다.

[^2]:
  당시 네트워크를 통한 접근이라고 해 봐야, telnet과 rlogin이 전부였습니다. 하지만,
  후기 UNIX에서는 ssh를 통해 접근하였을 때에도 해당 파일을 읽어들여 출력하는 경우도 있었습니다.

이제는 `/etc/issue`라는 것을 활요하는 경우는 거의 없으며, 과거 UNIX에서도
OS의 버전이나 패치 레벨을 알리는 것에 머물기도 했습니다.

### 그래서

`/etc/motd`는 게시판을 통한 '공람' 혹은 '회람'의 역할을 한다면,
`/etc/issue`는 '거의' 고정된 메시지 전달 역할을 하는데,
어떤 건물이나 캠퍼스의 정문에 걸려 있는 안내 - 화장실은 오른쪽 10미터 전방에 있습니다 -
와 후문에 걸려 있는 안내 - 고객 접견실은 정문에 위치해 있습니다 - 정도의 역할이라고
할 수 있겠습니다.

모두 다중 사용자 환경에서 `write`, `talk`와 더불어 정겨운 장치들입니다.

## Prompt 프롬프트

프롬프트는 사용자의 입력을 기다리는 무엇입니다. 앞서 설명한 것처럼
로그인을 위하여 사용자 이름과 암호를 입력하는 곳은 로그인 프롬프트
로그인 이후 명령어 등을 입력하는 곳이 shell prompt 쉘 프롬프트입니다.

이미 설명을 하지는 못 해도 알고 있다는 느낌이 있을 겁니다.
프롬프트는 컴퓨터와 사용자 간의 입력을 할 수 있는 그 무엇을 뜻 합니다.

보통의 일반 사용자의 프롬프트를 `$`로 표현하며, 루트 사용자를 `#`로
표현하는 것이 일반적인 방식이었는데, 요즈음은 루트 사용자로 로그인하거나
전환하는 경우는 거의 없기 때문에 UNIX나 Linux의 프롬프트는 `$`로
통일되어 표현하는 것이 요즈음의 쓰임입니다.

루트 사용자에 관한 이야기는 따로 다루도록 하겠습니다.

## 쉘 초기 설정

사용자가 로그인을 성공적으로 마치면, 위에서 설명한 쉘 프롬프트가 화면에 나타나기 직전에
자동적으로 수행되는 일이 사용자의 쉘 환경에 따른 여러 정의를 읽어 들이는 것입니다.
이런 정의들은 shell initialization files 쉘 초기화[^3] 파일들에 수록되게 됩니다.

[^3]:
    initialization과 초기화(初期化)는 서로 의미하는 바가 같지는 않지만,
    이와 같이 널리 번역되는 것을 존중하여 초기화라는 표현을 사용합니다.

이 쉘 초기화 파일들을 활용하여 사용자는 터미널에서의 작업환경을 자신의 구미에 맞게
변경하여 작업 효율을 높일 수도 있고, 특정 소프트웨어가 요구하는 조건을 맞춰 낼 수도 있게 됩니다.

이런 초기화 설정은 system wide 전역 설정 파일이 존재하고,
개별 사용자가 정의하는 것이 존재합니다.
먼저 사용자가 로그인을 하게 되면 전역 설정이 먼저 적용되고, 후차적으로
개별 사용자의 정의가 적용됩니다.

아래에서부터는 대표적인 사용자 쉘인, `bash`를 기준으로 설명합니다.

### 전역 설정 파일 System wide initialization files

system wide 전역(全域)이라는 단어 그대로 여기에 수록된 정의는
시스템에 접근하는 모든 `bash` 쉘이 기본인 사용자에게 적용됩니다.
system administrator 시스템 관리자가 강제할 수 있는 조건들을
손쉽게 적용할 수 있습니다. 아래의 설명은 `bash`, `ksh`, 그리고 `ash`에 공통됩니다.
다른 쉘일 경우에는 다른 파일에 정의가 있습니다.
이를 알기 위해서는 각 쉘의 맨 페이지를 참조하는 것도 방법일 수 있습니다.

시스템 기본값은 다음의 파일들이 정의를 가지고 있습니다.
`/etc/profile`
그리고 이 파일의 내용을 보면, 무엇을 더 읽어 들이는지 알 수 있습니다.

```bash
:/etc $ cat ./profile
# /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
# and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).

if [ "`id -u`" -eq 0 ]; then
  PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
else
  PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games"
fi
export PATH

if [ "${PS1-}" ]; then
  if [ "${BASH-}" ] && [ "$BASH" != "/bin/sh" ]; then
    # The file bash.bashrc already sets the default PS1.
    # PS1='\h:\w\$ '
    if [ -f /etc/bash.bashrc ]; then
      . /etc/bash.bashrc
    fi
  else
    if [ "`id -u`" -eq 0 ]; then
      PS1='# '
    else
      PS1='$ '
    fi
  fi
fi

if [ -d /etc/profile.d ]; then
  for i in /etc/profile.d/*.sh; do
    if [ -r $i ]; then
      . $i
    fi
  done
  unset i
fi
:/etc $ 
```

다음에 설명할 기회가 있을지 모르겠지만, 위 예시에서 처음 등장하는 구문은,
`/etc` 디렉토리에서 현재 디렉토리에 있는 `profile` 이라는 파일의 내용을 보자(`cat`)!라는 명령입니다.
앞서 '미리 알면 좋을 것들'에서 설명한 것처럼, `cat` 명령에 대한 궁금한 점이 있으시면
`$ man cat`으로 맨 페이지를 읽어 봅시다.

`/etc/profile` 파일의 내용을 보면, 가장 먼저 경로에 대한 변수를 정의하고 `$PATH`
`/etc/bash.bashrc`라는 파일이 있으면 읽어 들이도록 하고 있습니다.
이후 쉘 프롬프트를 정의 `PS1` 하는데, user id가 0, 즉 root user이면 `#`를
아니라면, `$`를 부여하고, 마지막으로 `/etc/profiled.d`라는 디렉토리 아래에 있는 `*.sh`
파일들을 읽어 들입니다.


### 사용자 설정 파일 user initialization files

사용자 설정은 (당연히) 홈 디렉토리에 있습니다. 여기의 설정 파일들도 전역 설정과 비슷한 느낌이 있습니다.
먼저 사용자가 로그인을 하게 되면, `~/.profile` 파일을 쉘이 읽게 됩니다.
그리고 `~/.profile` 파일의 내부를 보면, 아래와 같습니다.

```bash
$ cat ~/.profile
# ~/.profile: executed by the command interpreter for login shells.
# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
# exists.
# see /usr/share/doc/bash/examples/startup-files for examples.
# the files are located in the bash-doc package.

# the default umask is set in /etc/profile; for setting the umask
# for ssh logins, install and configure the libpam-umask package.
#umask 022

# if running bash
if [ -n "$BASH_VERSION" ]; then
    # include .bashrc if it exists
    if [ -f "$HOME/.bashrc" ]; then
	. "$HOME/.bashrc"
    fi
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$HOME/.local/bin:$PATH"
fi
$ 
```

`$ cat ~/.profile` 이라는 구문은, `cat` 명령으로 홈 디렉토리(`~`) 아래에 있는
`.profile`이라는 파일을 화면에 출력하라! 라고 설명할 수 있겠습니다.

내용을 보면, `~/.bashrc_profile` 혹은 `~/.bashrc_login` 파일이 있으면
`~/.profile`은 무시된다는 설명과 함께, 사용자의 홈 디렉토리 아래에 `.bashrc` 파일이 있으면
읽어 들이고, 경로 설정을 `$PATH`하고 끝나는 것을 알 수 있습니다.

앞으로 Linux를 사용하면서 이런 사용자 쉘 초기화 파일을 자주 들여다 볼 일이 있을 것입니다.
여기에 각자 취향에 맞는 설정을 집어 넣고, 새로운 환경에 맞는 설정을 하게 됩니다.

전, `$PATH`에 이것저것 추가하는 것 이외에는 `set -o vi`라는 정의 한 줄을 추가합니다.
`bash`나 `ksh`에서 (혹은 유사 계열에서) 쉘 환경에서 vi 편집기 명령어를 사용할 수 있게
하는 것입니다. 전 vi 편집기를 많이 좋아합니다.

## logout 로그아웃

로그아웃을 하는 방법은 `$ exit`라고 명령을 내리는 것과 `$ logout`이라고 명령을 내리는 방법
그리고 대부분의 터미널에서 지원하는 `^D`가 있습니다. `^D`는 키보드의 control 키와 d 키를 함께
누르는 것을 말합니다. `^D`는 EOF를 standard input으로 받아들이게 되어서 해당 터미널이 닫히게 됩니다.

그리고 `exit`와 `logout`은 따로 명령어가 존재하지 않습니다.
쉘에서 입력을 받아서 처리합니다. `export`나 `set`과 성격이 같다고 볼 수 있습니다.
그리고 이 두 명령이 비슷한 목적으로 존재하는 이유는 아래와 같습니다.

```bash
$ logout --help
logout: logout [n]
    Exit a login shell.

    Exits a login shell with exit status N.  Returns an error if not executed
    in a login shell.
$ exit --help
exit: exit [n]
    Exit the shell.

    Exits the shell with a status of N.  If N is omitted, the exit status
    is that of the last command executed.
$ bash
$ logout
bash: logout: not login shell: use 'exit'
$ exit
$ exit
logout
```

### logout

`logout`은 로그인 쉘에서 일을 끝내고 나가는 것이다. `login`의 반대 개념입니다.
위의 예시를 보면, 로그인 쉘에서 `bash`을 실행했습니다.
이 때 실행한 `bash`는 로그인 쉘이 아니라 (같은 놈을 실한 한 것이지만 성격이) 다른
쉘입니다. 이 때는 `logout`을 할 수 없습니다. 왜? `login`을 안 했으니까요.

### exit

빠져 나다는 것입니다. 쉘을 종료하는 것입니다. login shell 로그인 쉘은 사용자가 로그인하면서
획득하는 쉘이고, 프로그래밍이나 다른 목적으로 쉘을 획득하거나 실행시킬 수 있습니다.
그 쉘을 종료하는 선언입니다.

그래서, 로그인 쉘에서는 `logout`대신 `exit`를 사용할 수도 있습니다. 어쨌든 종료이니까요.
