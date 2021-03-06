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
