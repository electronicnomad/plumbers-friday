# NGINX 웹 데몬 설치 및 설정

## 잡설 雜說

[CERN](https://home.cern/)에서 웹 서비스를 선보인 뒤,
[CERN httpd](https://www.w3.org/Daemon/)가 사람들을 매료시켰습니다.
Netscape(이후, Sun Netscape Alliance, 이후 Oracle)의
[iPlanet](https://en.wikipedia.org/wiki/IPlanet)이 화려하게 등장했지만,
한정된 영역에서 잠시 주목받기만 했습니다. 하지만, iPlanet은 당시 세상을 바꿔가던 전자상거래 서비스에
보안과 모니터링 등의 편의를 제공하면서 중요한 역할을 수행했습니다. CERN httpd를 잇는
오픈소스 진영의 [Apache HTTP Server](https://httpd.apache.org/)는
오랫동안 왕좌를 지켜왔습니다. 물론, 이런 경쟁 속에서
[Microsoft의 Internet Information Services](https://en.wikipedia.org/wiki/Internet_Information_Services)는
착실히 자신의 영역을 지켜내었지만, 세상을 점령하지는 못 했고 지금은 잊혀지고 있습니다.

[![the birth of the web](./the-birth-of-the-web.png)](https://home.cern/science/computing/birth-web)

Apache HTTP Server의 아성에도 변화가 생겨나는데, NGINX의 등장입니다.
여기에서는 새로운 대세인, NGINX를 설치하고 설정해 보겠습니다.

## 시작하기 전에

만약 Debian/Ubuntu를 설치하고 지금 처음 부팅한 것이라면, 아주 특별한 이유가 여러분께 있지 않다면,
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
이 명령을 입력했을 때, `nginx`라는 항목이 보일 것입니다.

현대의 리눅스는 서비스를 `service`라는 명령으로 제어합니다. 이 명령은 아래와 같은 방법으로 사용됩니다.

```bash
Usage: service < option > | --status-all | [ service_name [ command | --full-restart ] ]
```

Unix/Linux를 접하면서 익숙해져야 하는 몇 가지 표현이 있습니다. 위 설명에서 사용되는 것들을 중심으로 설명하겠습니다.
`|`는 앞선 것과 뒤에 오는 것 중에 선택하는 것입니다. 아무것도 선택하지 않을 수는 없습니다.
`--`는 명령의 옵션을 뜻하는 경우가 많은데, `--`이렇게 두개의 대쉬를 사용하면 우리에게 익숙한
풀어서 적는 '단어'혹은 '단어'의 조합으로 사용되며, `-` 이렇게 한개의 대쉬를 사용하면 두문자를
따서 사용하는 경우가 많습니다.
'많습니다' 라고 설명하는 이유는, 이제 이런 규칙은 반드시 지켜지지 않기 때문입니다.

이에 대한 정확한 설명을 얻고자 한다면 (다시 한 번) `$ man man`으로 man page를 정독하는 것을 추천합니다.
혹시 지금 macOS를 사용하고 있고, 터미널을 열어서 man에 대한 man page를 보고자 한다면 - 그만 두시는 것이 좋습니다. 리눅스로 로그인하세요.

