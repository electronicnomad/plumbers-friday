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

### 패키지 매니저가 없던 시절

패키지 단위의 소프트웨어 설치 및 관리 등이 등장하기 이전에는 사용자가 직접 소스 수준에서 컴파일하여
사용자가 원하는 위치에 가져다 놓는 것으로 '설치'에 필요한 모든 과정을 처리하였으며,
판매목적으로 만들어진 소프트웨어 같은 경우에는 바이너리들과 라이브러리들을 `tar`와 같은 묶음으로
테이프나 플로피 나중에는 CD-ROM 그리고 DVD-ROM에 넣어서 사용자에게 전달되었습니다.

[![storage tape](./tapes-and-floppy-diskette.jpg)](https://en.wikipedia.org/wiki/Tape_drive)

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

`$ sudo apt install $packageName`으로 설치할 수 있습니다.
`apt` 명령은 인터넷에 있는 리포지토리에 해당 패키지를 다운로드하고 설치하는 과정을 거칩니다.
따라서, 인터넷에 연결되지 않는 시스템에서는 대체로 사용할 수 없습니다.

#### 로칼 리포지토리

사설망에 리눅스 시스템이 있고 외부망과 연결할 계획이 없다면, 로칼 네트워크에 리포지토리를
설치해서 운영할 수 있습니다. 물론, 리포지토리가 있는 서버는 외부와 일시적이든 영구적이든
통신이 가능해야 합니다.

이에 대한 설명은 [Ubuntu Wiki](https://help.ubuntu.com/community/Repositories/Personal)에
잘 설명되어 있습니다.

### 패키지 제거

`$ sudo apt remove $packageName`으로 원하는 패키지를 제거할 수 있습니다.

### 리포지토리 업데이트 내역 검색

`$ sudo apt update` 명령은 현재 시스템의 지정된 리포지토리에 갱신 내역이 있는지 확인하는 명령입니다.
이를 통하여 업그래이드 가능한 패키지가 있는지 확인할 수 있습니다. 아래의 예시를 보시죠.

```bash
$ sudo apt update
Hit:1 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:3 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-backports InRelease [101 kB]       
Hit:4 https://apt.releases.hashicorp.com focal InRelease                                           
Get:5 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [1031 kB]
Get:6 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 c-n-f Metadata [13.5 kB]
Get:7 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [781 kB]
Get:8 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/universe amd64 c-n-f Metadata [17.7 kB]
Get:9 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]                          
Hit:10 https://cli.github.com/packages focal InRelease                                
Fetched 2172 kB in 1s (1918 kB/s)                                               
Reading package lists... Done
Building dependency tree       
Reading state information... Done
11 packages can be upgraded. Run 'apt list --upgradable' to see them.
$ 
```

### 패키지 업그래이드

`$ sudo apt upgrade` 명령은 현재 설치된 패키지들을 최신 상태로 업그래이드를 진행시켜 줍니다.
이 명령 수행 이전에 앞서 설명한 `$ sudo apt update`가 수행되는 것이 좋습니다.
이 명령을 수행하면 대체로 아래와 비슷하게 업그래이드가 진행됩니다.

```bash
$ sudo apt upgrade
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  cloud-init gh grub-common grub-pc grub-pc-bin grub2-common python-apt-common python3-apt snapd
  terraform update-notifier-common
11 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 74.8 MB of archives.
After this operation, 7475 kB of additional disk space will be used.
Do you want to continue? [Y/n] 
Get:1 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 python-apt-common all 2.0.0ubuntu0.20.04.5 [17.1 kB]
Get:2 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 python3-apt amd64 2.0.0ubuntu0.20.04.5 [154 kB]
Get:3 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 update-notifier-common all 3.192.30.8 [132 kB]
Get:4 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 grub-pc amd64 2.04-1ubuntu26.12 [125 kB]
Get:5 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 grub2-common amd64 2.04-1ubuntu26.12 [591 kB]
Get:6 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 grub-pc-bin amd64 2.04-1ubuntu26.12 [971 kB]
Get:7 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 grub-common amd64 2.04-1ubuntu26.12 [1875 kB]
Get:8 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 snapd amd64 2.49.2+20.04 [30.6 MB]
Get:9 https://apt.releases.hashicorp.com focal/main amd64 terraform amd64 1.0.0 [33.0 MB]          
Get:10 https://cli.github.com/packages focal/main amd64 gh amd64 1.11.0 [6908 kB]                  
Get:11 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 cloud-init all 21.2-3-g899bfaa9-0ubuntu2~20.04.1 [456 kB]
Fetched 74.8 MB in 1s (66.8 MB/s)                                                                  
Preconfiguring packages ...
(Reading database ... 138770 files and directories currently installed.)
Preparing to unpack .../00-python-apt-common_2.0.0ubuntu0.20.04.5_all.deb ...
Unpacking python-apt-common (2.0.0ubuntu0.20.04.5) over (2.0.0ubuntu0.20.04.4) ...
Preparing to unpack .../01-python3-apt_2.0.0ubuntu0.20.04.5_amd64.deb ...
Unpacking python3-apt (2.0.0ubuntu0.20.04.5) over (2.0.0ubuntu0.20.04.4) ...
Preparing to unpack .../02-update-notifier-common_3.192.30.8_all.deb ...
Unpacking update-notifier-common (3.192.30.8) over (3.192.30.7) ...
Preparing to unpack .../03-gh_1.11.0_amd64.deb ...
Unpacking gh (1.11.0) over (1.10.3) ...
Preparing to unpack .../04-grub-pc_2.04-1ubuntu26.12_amd64.deb ...
Unpacking grub-pc (2.04-1ubuntu26.12) over (2.04-1ubuntu26.11) ...
Preparing to unpack .../05-grub2-common_2.04-1ubuntu26.12_amd64.deb ...
Unpacking grub2-common (2.04-1ubuntu26.12) over (2.04-1ubuntu26.11) ...
Preparing to unpack .../06-grub-pc-bin_2.04-1ubuntu26.12_amd64.deb ...
Unpacking grub-pc-bin (2.04-1ubuntu26.12) over (2.04-1ubuntu26.11) ...
Preparing to unpack .../07-grub-common_2.04-1ubuntu26.12_amd64.deb ...
Unpacking grub-common (2.04-1ubuntu26.12) over (2.04-1ubuntu26.11) ...
Preparing to unpack .../08-snapd_2.49.2+20.04_amd64.deb ...
Unpacking snapd (2.49.2+20.04) over (2.48.3+20.04) ...
Preparing to unpack .../09-cloud-init_21.2-3-g899bfaa9-0ubuntu2~20.04.1_all.deb ...
Unpacking cloud-init (21.2-3-g899bfaa9-0ubuntu2~20.04.1) over (21.1-19-gbad84ad4-0ubuntu1~20.04.2) ...
Preparing to unpack .../10-terraform_1.0.0_amd64.deb ...
Unpacking terraform (1.0.0) over (0.15.4) ...
Setting up snapd (2.49.2+20.04) ...
Installing new version of config file /etc/apparmor.d/usr.lib.snapd.snap-confine.real ...
snapd.failure.service is a disabled or a static unit, not starting it.
snapd.snap-repair.service is a disabled or a static unit, not starting it.
Setting up cloud-init (21.2-3-g899bfaa9-0ubuntu2~20.04.1) ...
Installing new version of config file /etc/cloud/templates/chef_client.rb.tmpl ...
Setting up grub-common (2.04-1ubuntu26.12) ...
update-rc.d: warning: start and stop actions are no longer supported; falling back to defaults
Setting up gh (1.11.0) ...
Setting up python-apt-common (2.0.0ubuntu0.20.04.5) ...
Setting up terraform (1.0.0) ...
Setting up grub2-common (2.04-1ubuntu26.12) ...
Setting up python3-apt (2.0.0ubuntu0.20.04.5) ...
Setting up update-notifier-common (3.192.30.8) ...
Setting up grub-pc-bin (2.04-1ubuntu26.12) ...
Setting up grub-pc (2.04-1ubuntu26.12) ...
Sourcing file `/etc/default/grub'
Sourcing file `/etc/default/grub.d/40-force-partuuid.cfg'
Sourcing file `/etc/default/grub.d/50-cloudimg-settings.cfg'
Sourcing file `/etc/default/grub.d/init-select.cfg'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-5.8.0-1035-aws
Found initrd image: /boot/microcode.cpio /boot/initrd.img-5.8.0-1035-aws
Found linux image: /boot/vmlinuz-5.4.0-1048-aws
Found initrd image: /boot/microcode.cpio /boot/initrd.img-5.4.0-1048-aws
Found linux image: /boot/vmlinuz-5.4.0-1045-aws
Found initrd image: /boot/microcode.cpio /boot/initrd.img-5.4.0-1045-aws
Found Ubuntu 20.04.2 LTS (20.04) on /dev/nvme0n1p1
done
Processing triggers for install-info (6.7.0.dfsg.2-5) ...
Processing triggers for mime-support (3.64ubuntu1) ...
Processing triggers for rsyslog (8.2001.0-1ubuntu1.1) ...
Processing triggers for systemd (245.4-4ubuntu3.6) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for dbus (1.12.16-2ubuntu2.1) ...
$ 
```

#### 다른 방식의 업그래이드

`apt upgrade`는 기존의 설치되었던 이전 버전의 패키지를 보존합니다.
때에 따라서 되돌아가야 할 일이 있기도 하기 때문입니다. 그런 반면 `apt full-upgrade`라는
옵션을 사용하면, 이전 패키지는 제거되고 새로운 패키지를 설치하게 됩니다.
지나온 길은 덮어버리고, 새로운 길로 진입하는 것입니다.

## 실습

이제, [실습](../tutorials/)을 통해서 지금까지 익힌 것들을 복습해 보도톡 하겠습니다.
