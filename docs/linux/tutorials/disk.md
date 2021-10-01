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
Jul 24 22:31:46 bastion systemd[1]: Started Clean the /media/pi/Samsung USB mount point.
Jul 24 22:31:46 bastion udisksd[398]: Mounted /dev/sda1 at /media/pi/Samsung USB on behalf of uid 1000
```

위 예시는 리눅스 장비의 USB 포트에 USB 메모리 장치를 삽입한 후 시스템에서 출되는 메시지입니다.
위 메시지는 우리가 필요한 거의 모든 정보를 제공하고 있는데, 이는 `/var/log/syslog`에 기록되고
어딘가로 지정된 콘솔에도 출력되게 됩니다.

우리가 필요한 정보는 새로운 디스크(저장 매체)의 위치 정보입니다.  
위 메시지에서 우리가 주목해야 할 부분은 `/dev/...`로 시작되는 곳입니다.
`/dev`는 디바이스에 대한 정보가 있는 디렉토리이고, 여기에 디스크의 정보도 수록됩니다.
위 예시에서는 `/dev/sda1`이라는 새로운 디스크의 파티션이 `/media/pi/Samsung USB`로 마운트된 것까지 알 수 있습니다.

장착한 새로운 디스크에 대한 정보를 얻는 방식으로 가장 흔하게 사용되는 것은, 다음의 명령을 수행하는 것입니다.

`sudo lsblk`
