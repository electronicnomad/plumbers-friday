# 새 디스크 추가하기

새로운 디스크를 추가하기 - 라고 적었지만, 사실 여기에서는 다음의 과정을 다룹니다.

1. 새로운 디스크를 운영체제/리눅스에서 찾아내기
1. 찾은 디스크를 리눅스에서 사용할 수 있게 초기화 및 파티션 나누기
1. 파일 시스템 작성하기
1. 리눅스에 마운트하기

그리고 아래의 설명은 정확히 위의 순서에 따른 설명이 되겠습니다.

## 새로운 디스크 장착

서버를 끄고 디스크를 넣습니다. 빈자리에 적당히 넣으면 요즈음은 잘 작동합니다. 참 쉬운 세상이 되었습니다.
클라우드에 대포한 VM에 디스크를 추가하는 건 더욱 쉬운 일이 되겠죠?

## 새 디스크 찾기

장착된 새로운 디스크를 찾는 방법은 여러가지가 있습니다. 그러니 누군가가 '이렇게 해야만 해'라고 주장한다면,
그 사람과 멀리하시는 것이 좋습니다. 세상 모든 일이 그렇겠지만, 특히나 오픈소스 세상에서는
'반드시', '꼭', '절대적으로' 라는 것은 대부분, 성립되지 않습니다.

### `/var/log/syslog`를 관찰하기

```bash
Jul 24 22:31:40 bastion kernel: [2862970.027539] usb 1-1.3: new high-speed USB device number 5 using dwc_otg
Jul 24 22:31:41 bastion kernel: [2862970.158911] usb 1-1.3: New USB device found, idVendor=090c, idProduct=1000, bcdDevice=11.00
Jul 24 22:31:41 bastion kernel: [2862970.158938] usb 1-1.3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
Jul 24 22:31:41 bastion kernel: [2862970.158955] usb 1-1.3: Product: Flash Drive FIT
Jul 24 22:31:41 bastion kernel: [2862970.158970] usb 1-1.3: Manufacturer: Samsung
Jul 24 22:31:41 bastion kernel: [2862970.158986] usb 1-1.3: SerialNumber: 0373221010008377
Jul 24 22:31:41 bastion kernel: [2862970.162706] usb-storage 1-1.3:1.0: USB Mass Storage device detected
Jul 24 22:31:41 bastion kernel: [2862970.167001] usb-storage 1-1.3:1.0: Quirks match for vid 090c pid 1000: 400
Jul 24 22:31:41 bastion kernel: [2862970.168031] scsi host0: usb-storage 1-1.3:1.0
Jul 24 22:31:41 bastion mtp-probe: checking bus 1, device 5: "/sys/devices/platform/soc/3f980000.usb/usb1/1-1/1-1.3"
Jul 24 22:31:41 bastion mtp-probe: bus: 1, device: 5 was not an MTP device
Jul 24 22:31:41 bastion mtp-probe: checking bus 1, device 5: "/sys/devices/platform/soc/3f980000.usb/usb1/1-1/1-1.3"
Jul 24 22:31:41 bastion mtp-probe: bus: 1, device: 5 was not an MTP device
Jul 24 22:31:45 bastion kernel: [2862974.397768] scsi 0:0:0:0: Direct-Access     Samsung  Flash Drive FIT  1100 PQ: 0 ANSI: 6
Jul 24 22:31:45 bastion kernel: [2862974.400294] sd 0:0:0:0: Attached scsi generic sg0 type 0
Jul 24 22:31:45 bastion kernel: [2862974.404295] sd 0:0:0:0: [sda] 501253132 512-byte logical blocks: (257 GB/239 GiB)
Jul 24 22:31:45 bastion kernel: [2862974.404727] sd 0:0:0:0: [sda] Write Protect is off
Jul 24 22:31:45 bastion kernel: [2862974.404765] sd 0:0:0:0: [sda] Mode Sense: 43 00 00 00
Jul 24 22:31:45 bastion kernel: [2862974.405074] sd 0:0:0:0: [sda] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
Jul 24 22:31:45 bastion kernel: [2862974.450013]  sda: sda1
Jul 24 22:31:45 bastion kernel: [2862974.452257] sd 0:0:0:0: [sda] Attached SCSI removable disk
Jul 24 22:31:45 bastion kernel: [2862974.787033] exfat: Deprecated parameter 'namecase'
Jul 24 22:31:46 bastion systemd[1]: Started Clean the /media/pi/256 mount point.
Jul 24 22:31:46 bastion udisksd[398]: Mounted /dev/sda1 at /media/pi/256 on behalf of uid 1000
```

위 예시는 리눅스 장비의 USB 포트에 USB 메모리 장치를 삽입한 후 시스템에서 출되는 메시지입니다.
위 메시지는 우리가 필요한 거의 모든 정보를 제공하고 있는데, 이는 `/var/log/syslog`에 기록되고
어딘가로 지정된 콘솔에도 출력되게 됩니다.

우리가 필요한 정보는 새로운 디스크(저장 매체)의 위치 정보입니다.  
위 메시지에서 우리가 주목해야 할 부분은 `/dev/...`로 시작되는 곳입니다.
`/dev`는 디바이스에 대한 정보가 있는 디렉토리이고, 여기에 디스크의 정보도 수록됩니다.
위 예시에서는 `/dev/sda1`이라는 새로운 디스크의 파티션이 `/media/pi/256`로 마운트된 것까지 알 수 있습니다.

장착한 새로운 디스크에 대한 정보를 얻는 방식으로 가장 흔하게 사용되는 것은, 다음의 명령을 수행하는 것입니다.

!!! note "알려두기"
    위 syslog에 반영된 정보와 아래의 출력된 정보는 같은 시스템에서 가져온 것이라 대략적으로 같은 내용일 수 있지만
    이 문서의 작성 시점에 수 개월의 공백에 있는 탓에 같은 하드웨어 구성인지 확신은 없습니다. 따라서 위의
    syslog 출력문을 아래의 예제와 1:1로 견주어 생각하시지는 않기를 바랍니다.

`sudo lsblk` 이 명령을 수행하면, 다음과 같은 결과가 화면에 출력됩니다.

```bash
$ sudo lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    1  239G  0 disk 
`-sda1        8:1    1  239G  0 part /media/pi/256
mmcblk0     179:0    0 29.8G  0 disk 
|-mmcblk0p1 179:1    0  256M  0 part /boot
`-mmcblk0p2 179:2    0 29.6G  0 part /
$ 
```

`lsblk`는 'list block devices'라는 의미로 해당 시스템에 논리적이든 물리적이든 연결된 block device(s)를
화면에 출력해 주는 역할을 합니다. block device 블록 디바이스란, filesystem 파일 시스템을 작성해서 데이터를 직접 작성하거나 기록에 활용하는 저장 장치를 말합니다. 우리가 익히 알고 있는, HDD, SDD 같은 것이 여기에 해당됩니다.

`lsblk`로 현재 시스템에 연결된 디스크/저장장치를 살펴볼 수 있고, 이 때 획득한 정보 - 디스크의 주소 - 를 통해서
다음의 과정으로 나아갈 수 있습니다. 위 출력 내용을 보면, `sda`라는 새로운 디바이스가 있고, `sda1`이라는
파일 시스템이 `/dedia/pi/256`에 마운트 되어 있다는 걸 알 수 있습니다.

!!! info "마운트란?"
    마운트는 mount라는 명령 혹은 그 명령으로 꾀하는 행위를 말합니다. 영단어 mount가 지니고 있는
    의미가 가미되긴 했으리라 보지만, 아무튼, '마운트'는 저장 매체를 시스템에 인식시키고 
    파일 시스템을 작성했거나 이와 유사한 형태로 (프로그램을 포함하는)
    사용자가 '마운트 된 저장매체에' 파일을 생성하고 
    조작할 수 있는 상태라고 할 수 있습니다.

현대의 사용자가 직접 (쉽게) 설치할 수 있는 리눅스 배포한들은 대체로 이동식 저장장치가 시스템에 연결되면
자동 마운트를 해 줍니다. 위 예시에서 보이는 `/media/pi/256`로 그와 같은 경위로 우리가 조회할 수 있습니다.

자동 마운트된 USB 메모리는 다음과 같은 정보를 가지고 있다는 것을 알 수 있습니다.

```bash
$ mount |grep pi\/256
/dev/sda1 on /media/pi/256 type exfat (rw,nosuid,nodev,relatime,uid=1000,gid=1000,fmask=0022,dmask=0022,iocharset=utf8,errors=remount-ro,uhelper=udisks2)
$ 
```

mount라는 명령은 후속 옵셥이 없으면, 시스템에 현재 마운트된 저장장치의 정보를 보여줍니다.
위 출력값의 5번째 행을 보면, `exfat`이라고 나타나는데, 이는 Extended FAT입니다.
이 파일 시스템이 좋다면 이대로 사용해도 되겠습니다. 그런데, 다른 파일 시스템으로 작성하고자 한다면
이 또한 가능합니다.

