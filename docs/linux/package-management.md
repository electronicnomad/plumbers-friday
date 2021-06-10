# 패키지 관리

## 구분 되는 두 가지

리눅스의 '계열'을 나누는 기준으로 패키지 관리를 꼽을 수 있습니다.
Fedora - Red Hat Enterprise Linux - CentOS로 대표되는 이 계열은
RPM, Red hat Package Manager를 통해서 패키지를 관리합니다.
**RPM**의 패키지들의 이름은 `.rpm`으로 끝납니다.

Ubuntu를 포함하는 Debian 계열은, dpkg, Debian Package Manager로
패키지를 관리합니다.
**dpkg**의 패키지들의 이름은 통상, `.deb`으로 끝납니다.

## 패키지란?

패키지는 어떤 소프트웨어가 동작하기에 필요한 여러가지(바이너리들과 라이브러리들과 설정에 필요한 여럿 파일들) 것들을
꾸러미로 묶어 놓은 것이라고 할 수 있습니다. 이러한 꾸러미는 보통의 컴퓨팅 환경에서 매우 일반적인 것이 되어서,
윈도(Windows)와 맥오에스(macOS)는 물론이거니와 iOS나 안드로이드(Android)와 같은 스마트폰 운영체제에서도
이러한 체계를 사용합니다.

이런 체계에서 가장 늦게 진입한 운영체제는 아무래도 리눅스와 같은 쪽이겠습니다.

### 패키지 매니저가 없던 시절

패키지 단위의 소프트웨어 설치 및 관리 등이 등장하기 이전에는 사용자가 직접 소스 수준에서 컴파일하여
사용자가 원하는 위치에 가져다 놓는 것으로 '설치'에 필요한 모든 과정을 처리하였으며,
판매목적으로 만들어진 소프트웨어 같은 경우에는 바이너리들과 라이브러리들을 `tar`와 같은 묶음으로
테이프나 플로피 나중에는 CD-ROM 혹은 DVD-ROM에 넣어서 사용자에게 전달되었습니다.

> ![storage tape](https://upload.wikimedia.org/wikipedia/commons/thumb/3/39/Dds_tape_drive_01.jpg/600px-Dds_tape_drive_01.jpg)  
> DDS tape drive. Above, from left to right: DDS-4 tape (20 GB), 112m Data8 tape (2.5 GB), QIC DC-6250 tape (250 MB), and a 3.5" floppy disk (1.44 MB) - [Tape drive
From Wikipedia](https://en.wikipedia.org/wiki/Tape_drive), 그리고 바닥에 깔려 있는 건, Sun UniPack DDS Tape Drive.

이렇게 배포되는 소프트웨어의
설치 과정은 동봉된 쉘 스크립트를 사용하여 각 바이너리들과 라이브러리와 설정에 관계되는 편집 가능한
파일들을 소프트웨어 제작자가 의도한 디렉토리로 복사하는 것으로 모든 과정을 마치는 형식이었습니다.

### 패키지 단위의 관리가 필요한 이유

일단 설치가 쉽습니다. 그리고 제거도 쉽습니다. 업데이트/업그래이드도 쉽고, 사실상
소프트웨어의 설치 관리 제거에 특별히 시스템 관리자가 신경 쓸 일이 거의 없습니다.

이 중에 가장 중요한 건, 업데이트일 것인데, 과거의 소프트웨어 업데이트는
제거 - 설치를 반복하는 과정과 다를바가 없었습니다. 그리고 앞서 설명한 것과 같이
어려운 설치는 당연히 어려운 제거가 따라왔는데, 어느 디렉토리에 어느 파일이 어떻게
배치되었는지 정확히 알지 못 하면 완전한 제거가는 거의 불가능했습니다.

업데이트에 신경 써야 할 이유는 '최신의 소프트웨어'를 사용하는 목적보다는
시스템 관리자 입장에서는 '보안'에 신경쓰는 측면이 더 강하다고 할 수 있습니다.

그리고 중앙에서 일괄적으로 데이터 센터나 클라우드에 배포된 다수의 OS를 관리한다면
패키지 관리 시스템이 없이는 매우 복잡하고 어려울 수 있다. 이 것이 또 하나의 이유라고 할 수 있습니다.

## 패키지 관리자

직접 패키지 파일(.deb)을 설치할 때에는 `dpkg` 명령을 사용합니다. 그리고 온라인에 있는 리포지토리를 활용하여 패키지를 다운로드 받아 설치까지 마치는 방식이 있습니다. 이 때에는 `apt`라는 명령을 사용하게 됩니다. 

### dpkg

앞서 설명한 것과 같이, 패키지 파일을 대상으로 설치하는데 사용됩니다. 물론, 제거하고 설치된 패키지를 조회하는 등의 관리도 가능합니다. 이러한 것 뿐만 아니라, `dpkg`는 패키지를 만드는데에도 사용됩니다.
만약, `dpkg` 명령에 대한
관심이 있다면, 맨 페이지 `man dpkg`를 정독해 보는 것을 추천합니다. 그리 길지는 않습니다.

#### 세부 명령들

흔하게 사용되는 옵션들을 설명해 보겠습니다.

`dpkg -l`

이 명령은 dpkg의 기본 데이터베이스에 있는 모든 패키지 목록을 보여 줍니다.
설치된 것과 설치 할 수 있는 것 모두 포함하여 화면에 출력합니다.

### apt

apt 명령은 최근에 소개되었습니다. 그 이전까지는 `apt-get`이라는 명령과 `apt-cache`라는 명령이 따로 역할 하던 것을 `apt` 하나로 갈음하게 되었습니다.

`apt-get`과 `apt-cache`라는 명령도 여전히 존재합니다.

#### /etc/apt

`/etc/apt`에는 `apt` 명령을 위한 설정들이 있습니다.
그 중에 빈도 있게 들여다 볼 기회가 있는 설정 파일은, 아무래도
`/etc/apt/source.list`입니다.

`/etc/apt/source.list`는 `apt` 명령이 수행되면서 참조하는 것인데, 이 곳에 패키지가 있을 곳을 정의해 둡니다.
사용자가 추가할 수도 있으며, 기본적으로 배포판 제막한 곳에서
제공하는 리포지토리들이 수록되어 있습니다. 기본값은 아래와 같습니다.

```bash
$ more /etc/apt/sources.list
deb http://deb.debian.org/debian buster main
deb http://deb.debian.org/debian buster-updates main
deb http://security.debian.org/debian-security/ buster/updates main
$
```

### 패키지 검색

`$ apt search`로 패키지를 검색할 수 있습니다.

`apt` 명령으로 패키지를 찾아서 설치하는 것이
무언가 자연스러운 시스템 관리의 방법인 듯 하지만 ;-)
원하는 패키지를 찾는 방법은 시대에 맞게 웹 브라우저에서도 가능합니다.
[Debian Packages Search](https://packages.debian.org/index).
물론, 구글링이 더 나은 결과를 얻을 때도 있습니다 :-)

```bash
$ apt search nginx-full
Sorting... Done
Full Text Search... Done
nginx/stable 1.14.2-2+deb10u4 all
  small, powerful, scalable web/proxy server

nginx-full/stable 1.14.2-2+deb10u4 armhf
  nginx web/proxy server (standard version)

$
```

위 예시는 `nginx-full`이라는 패키지를 검색한 것입니다.
이 때, 연관된 그러니까, dependancy라고 부르는 상호 필요한 패키지도 함께 출력합니다.
이 경우에는 `nginx`가 되겠습니다.

`$ apt search nginx`라고 명령을 내리면 상당히 많은 패키지 목록을 볼 수 있습니다.
`nginx`라는 패키지에 dpendancy가 있는 것들입니다.

`apt` 명령으로 패키지를 설치하면, 관계된 그래서 필요한 다른 패키지도 함께 자동으로 설치해 줍니다.
그래서 이 부분에 대한 걱정을 덜 수 있습니다.

### 패키지 설치

`$ apt install 패키지_이름`으로 설치할 수 있습니다.
`apt` 명령은 인터넷에 있는 리포지토리에 해당 패키지를 다운로드하고 설치하는 과정을 거칩니다.
따라서, 인터넷에 연결되지 않는 시스템에서는 대체로 사용할 수 없습니다.

#### 로칼 리포지토리

사설망에 리눅스 시스템이 있고 외부망과 연결할 계획이 없다면, 로칼 네트워크에 리포지토리를
설치해서 운영할 수 있습니다. 물론, 리포지토리가 있는 서버는 외부와 일시적이든 영구적이든
통신이 가능해야 합니다.

이에 대한 설명은 [Ubuntu Wiki](https://help.ubuntu.com/community/Repositories/Personal)에
잘 설명되어 있습니다.

### 패키지 제거

`$ apt remove 패키지_이름`으로 원하는 패키지를 제거할 수 있습니다.