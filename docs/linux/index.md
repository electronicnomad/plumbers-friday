# 시작 (기초공사중)

부끄럽게도 리눅스를 배웁시다 - 라는 문서를 작성하고
공개하려는 결심을 하였습니다.  
현재까지 (2021-02-22) 미완성이며, 기초공사에 해당되는 작업을 하고 있습니다.
언제까지 하겠다는 - 식의 약속은 하지 못 할 듯 합니다.

## 대상

지금 계획하며 작성하는 문서는 굳이 따져보자면 '운영자'용입니다.
'개발자'를 위한 정보는 정말 드물거나 아예 존재하지 않을 것입니다.

## 기준

기준이 되는 배포판은 Debian 계열이 될 것입니다.
Ubuntu를 언급할 수도 있고, Debian을 언급할 수도 있고, Raspberry Pi OS를
혹은, WSL(Windows Subsystem for Linux)에 대한
언급이 있을 수도 있습니다.  
현재로서는 Fedora/Red Hat 계열의 특징적인 부분에 대해 기술할 계획은 없습니다.

내용은 언제든 변경이 될 수 있습니다.
그리고 적당한 오류가 포함될 확률이 존재합니다.

## 구성

아래의 구성안(構成案)은 작성 중에 변경될 수 있습니다.

1. [shell 쉘](./shell/)
1. directory 디렉토리
1. file and permissions 파일과 권한
1. login and logout 로그인과 로그아웃
1. user and groups 사용자와 그룹
    1. password and shadow 암호와 샤도우
    1. super user or root user 수퍼 유저 혹은 루트 유저
1. /var/log 로그 파일들
1. daemon and services 데몬과 서비스
1. hardware information 하드웨어 정보 조회
1. networking 네트워크
1. storage 스토리지
1. booting process 부팅 과정
1. performance monitoring 성능 모니터링
1. kernel 커널

### 참고자료

참고하면 좋을 리소스들 혹은 읽기를 추천하는 것들

* [Linux FAQ](https://tldp.org/FAQ/Linux-FAQ/general.html)


## 기타

### 갱신 내역

### 잡소리

전 SunOS/Solaris에 대한 심각한 팬이었고,
그것으로 밥벌이도 했습니다. Linux는 Solaris의 아름다움에 취한 저로서는
한낱 장난감 정도로 밖에 보이지 않았습니다.
아마도 그 당시 많은 사람들이 그렇게 생각했을 것이며,
누군가가 오픈소스 운영체제를 심각하게 고민한다면,
당연히 BSD의 후예들을 이었을 겁니다.
물론, 나중에는 OpenSolaris.org도 선택 중 하나였겠지만
그 이전에도 (Sun이 라이센스에 대한 그리고 지원 플랫폼에 대한 선언을
개삽질로 도배하기 직전까지는) Solaris x86을 염가에 구매할 수 있었기 때문에
x86 플랫폼에서 서버 OS로는 Windows Server, Solaris x86, FreeBSD가
일단 손에 꼽히고, SCO Unix가 다음으로 검토될 수 있었을 겁니다.

Linux가 이렇게 각광받는 그리고 이제 인류의 삶의 중심으로 편입되는
결정적 계기가 무엇이었는지 따져보아도 - 수많은 사람들의 각기 다른 주장이
여럿 있어도 - 도무지 모르겠습니다.
어쨌든 어느 순간 Solaris와 OpenSolaris.org의 관이 Sun의
무덤 속으로 들어가는 장면에서 한 동안 징징거리다가 정신을 차려보니
세상은 Linux가 접수했었습니다.

지금은 Linux를 가지고 놀고 있습니다. Raspberry Pi에 Ubuntu를 설치해 놓고
이런 저런 장난질도 하고, WSL로 스크립트를 짜서 Windows의 파일도 관리하고,
뭐 그러고 놀고 있습니다.
