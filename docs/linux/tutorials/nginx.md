# NGINX 웹 데몬 설치 및 설정

## 잡설 雜說

[CERN](https://home.cern/)에서 웹 서비스를 선보인 이후,
[CERN httpd](https://www.w3.org/Daemon/)가 사람들을 매료시켰습니다.
이 후 대세는, Netscape(이후, Sun Netscape Alliance, 이후 Oracle)의
[iPlanet](https://en.wikipedia.org/wiki/IPlanet)이 화려하게 등장했지만,
협소한 영역에서 잠시 반짝했하고 말았습니다. 하지만, iPlanet은 당시 세상을 바꿔가던 전자상거래를
서비스하는 중요한 역할을 수행했습니다. CERN httpd를 잇는
오픈소스 진영의 [Apache HTTP Server](https://httpd.apache.org/)는
오랫동안 왕좌를 지켜왔습니다. 물론, 이런 경쟁 속에서
[Microsoft의 Internet Information Services](https://en.wikipedia.org/wiki/Internet_Information_Services)는
착실히 자신의 영역을 지켜내었지만, 세상을 점령하지는 못 했고 지금은 잊혀지고 있습니다.

Apache HTTP Server의 아성에도 변화가 생겨나는데, NGINX의 등장입니다.
여기에서는 새로운 대세인, NGINX를 설치하고 설정해 보겠습니다.

## 시작하기 전에

만약 Ubuntu를 설치하고 지금 처음 부팅한 것이라면, 아주 특별한 이유가 여러분께 있지 않다면,
`$ sudo apt update && sudo apt upgrade` 를 수행하기 바랍니다. 앞으로 생길 귀찮은 일을 한 번에 해소할 수 있습니다.
쉘 명령에서 `&&`는 선행(先行) 명령이 성공적으로 완료되면 후행(後行) 명령을 이어 수행한다는 의미입니다.

## 설치

Ubuntu의 리포지토리에서는 고마웁게도 상당히 많은 '일반적'인 혹은 '필수적'인 소프트웨어들이
패키지로 존재합니다. NGINX도 예외는 아니어서, `apt` 명령으로 쉽게 설치할 수 있습니다.

`$ sudo apt install nginx`

## 시작 재시작 종료

설치된 NGINX는 리눅스 시스템의 서비스에 등록됩니다. '서비스'는 그 단어 의미 그대로
서비스를 시작하고 멈추고 하는 역할을 합니다.

`$ service --status-all` 명령을 통해, 서비스가 제어하는 개별 서비스 목록을 화면으로 출력해 볼 수 있습니다.