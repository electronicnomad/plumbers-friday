# 목표와 준비

이번 학습과제는 다음의 아키텍처를 구현하는데 있습니다.

## 아키텍처

[![target architecture](../../images/networking/objective.svg)](../../images/networking/objective.svg)

### 데이터 흐름

## 준비

다음의 환경을 미리 준비하여 단계별 실습의 바탕을 마련합니다.

### 웹 브라우저

웹 브라우저만 잘 갇추었다면 만족할 수 있습니다.
Microsoft Edge, Firefox, Google Chrome이 적합하다고 할 수 있습니다.
즉, Gecko 엔진과 Chromium 엔진을 사용하는 웹 브라우저라면 좋습니다.

### CLI

추가적으로 command line interface가 있으면 더 좋겠습니다.
여러분께서 사용하고 있는 운영체제에서 CLI를 지원하고 그 CLI에 AWS CLI를 설치할 수 있으면
됩니다. 만약 그런 환경이 갖추어지지 않았다거나 굳이 그런 수고를 들이고 싶지 않는다면
AWS에서 제공하는 Cloud9을 사용하는 것을 추천합니다.
혹은, EC2 instance 하나를 만들고, session manager로 접속해서 사용하셔도 무관합니다.
모두 결국에는 같은 환경입니다.

### 기초 환경 설정

VPC과 Subnet을 다음의 같이 미리 만들어 놓습니다.
서로 다른 리전에 VPC를 배치하며 각 VPC는 하나의 Subnet을 (일단) 보유하게 됩니다.  
아래의 구성은 개선의 여지가 충분히 있습니다. 특히 하나의 VPC 내 생성되는 두번째 Subnet은
사용하지 않을 가능성이 매우 높습니다.

| Region   | VPC   | Subnet   | Subnet  |
|----------|-------|----------|----------|
| Region A (Tokyo) | VPC 1 | Subnet 1-1 |  |  
|          | 10.10.0.0/16 | 10.10.1.0/24 | |
| Region A | VPC 3 | Subnet 3-1 |  |
|          | 10.30.0.0/16 | 10.30.1.0/24 | |
| Region B (Seoul) | VPC 2 | Subnet 2-1 | Subnet 2-2 |
|          | 10.20.0.0/16 | 10.20.1.0/24 | 10.20.2.0/24 |
| Region B | VPC 4 | Subnet 4-1 | |
|          | 10.40.0.0/16 | 10.40.1.0/24 | 10.40.2.0/24 |
| Region B | VPC 6 | Subnet 6-1 ||
|          | 10.60.0.0/16 | 10.60.1.0/24 |  |

