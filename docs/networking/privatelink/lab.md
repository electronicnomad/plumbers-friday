# 실습

## VPC endpoint service 생성

준비 단계에서 생성한 EC2 instance과 Network Load Balancer를 사용합니다.

VPC > 가상 프라이빗 클라우드 - 엔드포인스 서비스를 선택해서 들어갑니다.
그리고 화면 상단의 '엔드포인트 서비스 생성'을 클릭하여 엔드포인트를 생성합니다.

그리고 화면에서 미리 만들어 놓은, NLB를 선택합니다.

![create endpoint service](../../images/networking/privatelink/create-endpoint-service.png)

기본으로 지정되어 있는, '엔드포인트 수락 필수' 항목은 선택하지 않습니다.

![create endpoint service](../../images/networking/privatelink/created-endpoint-service.png)

## VPC endpoint 생성

제일 먼저, 조금 전 생성해 놓은 VPC endpoint service의 이름을 클립보드에 복사해 둡니다.

(켭쳐 화면 간 설명 보충 예정)

![copy endpoint service name](../../images/networking/privatelink/copy-endpoint-service-name.png)

![copy endpoint](../../images/networking/privatelink/create-endpoint.png)

![status standby](../../images/networking/privatelink/endpoint-status-standby.png)

![service connections](../../images/networking/privatelink/endpoint-service-connections.png)

![service connections actions](../../images/networking/privatelink/endpoint-service-connections-actions.png)

![service connections actions accept request](../../images/networking/privatelink/endpoint-service-connections-accept-request.png)

![service connections available](../../images/networking/privatelink/endpoint-service-connections-available.png)

여기까지 오셨다면 설정은 모두 끝났습니다.
이제 VPC 2에 배포되어 있는 instance 2-1로 접속해서 (session manager를 통해서)
마지막에 생성한 VPC endpoint로 웹 접속을 해 봅니다. VPC endpoint의 주소는 아래의 그림과 같이
해당 endpoint의 '세부 정보'의 'DNS 이름'[^1]으로 알 수 있습니다. 

[^1]: 'DNS 이름'이라는 명칭은 혼란을
    줄 수 있다는 생각인데, FQDN(Full Quantified Domain Name)이 맞지 않나 싶습니다.
    엄밀히 말해서 DNS 이름은, Domain Name Server(Service)의 이름으로, DNS-01, NS-WEST-BAY17
    뭐 이런 식이어야 할 듯 합니다.

![copy endpoint's FQDN](../../images/networking/privatelink/copy-endpoints-fqdn.png)

VPC4에 있는 instance 4-1에 접속해서, 간단한 HTML 파일을 만들어 둡니다.
plain text 문서를 index.html로 작성해 두어도 아무런 문제가 되지 않습니다.

현재 해당 EC2 instance는 외부와의 접속이 되지 않습니다. private subnet에
위치해 있고, 다른 연결 방식은 없습니다. 따라서 웹 서버를 설치하기가 용이하지 않습니다.
그리하여, 기본으로 설치되어 있는 python의 기능 하나를 활용하기로 합니다.

아래의 내용은, python으로 간단 웹 데몬을 하나 동작시키는 것입니다.
반드시 index.html이 있는 디렉토리에서 명령 내려야 합니다.

```bash
$ sudo su - ubuntu
ubuntu@ip-10-40-1-50:~$ cd web
ubuntu@ip-10-40-1-50:~/web$ cat ./index.html
<html>
        <head><title>Test from Instance 4-1</title></head>
        <body>
                Hello World; this is Instance 4-1

        </body>
</html>
ubuntu@ip-10-40-1-50:~/web$ sudo python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
```
이제 세션 메니저로 VPC 2에 배포해 둔, EC2 instance 2-1에 접속합니다.
그리고 VPC 2에 만들어 놓은 그리고 VPC 4의 NLB를 품고 있는 VPC Endpoint Service와
연결해 둔, VPC endpoint의 주소로 `curl` 명령을 내려 모든 것이 제대로 동작하는지
확인합니다.

문제가 없다는 아래의 예와 같이 화면 출력을 보실 수 있습니다.

```bash
$ sudo su - ubuntu
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-10-20-1-37:~$ curl vpce-0b10227a244aa9730-dybhspou.vpce-svc-036f46495a6cabc80.ap-northeast-2.vpce.amazonaws.com
<html>
        <head><title>Test from Instance 4-1</title></head>
        <body>
                Hello World; this is Instance 4-1

        </body>
</html>
ubuntu@ip-10-20-1-37:~$ 
```
instance 4-1에 띄워 놓은 python 웹 데몬에서 아래와 같이 접속이 이루어졌음을 확인할 수 있습니다.

```bash
$ sudo su - ubuntu
ubuntu@ip-10-40-1-50:~$ cd web
ubuntu@ip-10-40-1-50:~/web$ cat ./index.html
<html>
        <head><title>Test from Instance 4-1</title></head>
        <body>
                Hello World; this is Instance 4-1

        </body>
</html>
ubuntu@ip-10-40-1-50:~/web$ sudo python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.40.1.80 - - [20/Jun/2020 11:31:05] "GET / HTTP/1.1" 200 -
10.40.1.80 - - [20/Jun/2020 11:31:28] "GET / HTTP/1.1" 200 -
10.40.1.80 - - [20/Jun/2020 11:31:53] "GET / HTTP/1.1" 200 -
```
