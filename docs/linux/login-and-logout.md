# 로그인과 로그아웃

> login and logout

어떤 호스트에 정상적인 접근[^1]을 시도하면 처음 만나는 대화형 화면이
login prompt 로그인 프롬프트입니다. 대략 아래와 같이 표현됩니다.

[^1]:
    접근이라는 방식에 원격 접속과 직접 장치에 연결된 키보드 마우스 모니터를
    사용하는 방법, 두 가지 모두를 의미하겠다. 전자(前者) 경우에는
    사용할 수 있는 방식이 `telnet`, `ssh`, `ftp`, `sftp` 등이 있겠다.
    이 중에서 `telnet`, `ftp`는 보안의 문제로 거의 사용하지 않는다.
    후자(後者)의 경우는 여러분이 익숙한 윈도나 맥 컴퓨터를 사용하는 것과
    다름이 없다.

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

요즈음처럼, ssh/sshd를 이용하여 host 호스트에 접근하는 경우 로그인 프롬프트를
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

[^2]: 당시 네트워크를 통한 접근이라고 해 봐야, telnet과 rlogin이 전부였다. 하지만,
후기 UNIX에서는 ssh를 통해 접근하였을 때에도 해당 파일을 읽어들여 출력하는 경우도 있었다.

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
