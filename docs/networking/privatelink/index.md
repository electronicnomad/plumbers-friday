# 개요

![PrivateLink ICON](../../images/networking/privatelink/privatelink-icon.png){: style='width:120px; padding:5px; float:right; border-radius:8%'}
PrivateLink은 VPC endpoint를 활용한 것입니다.
그리고 VPC endpoint services라고도 부릅니다.
VPC endpoints는 기술적으로 두 가지가 있는데,
하나는 gateway endpoint이고,
나머지 하나는 interface endpoint입니다.

## VPC Endpoints

### Gateway endpoint

쉬운 것부터 설명하겠습니다. 게이트웨이 엔드포인트는 간단합니다.
특정 서비스와 연결되는 한정된 엔드포인트인데, 그 특정 서비스는
S3, DynamoDB 딱 둘입니다.

아래에 설명할 interface endpoint와는 달리 gateway endpoint는
다소 정책을 설정에 반영한다고 할 수 있겠습니다.

### Interface endpoint

interface endpoint는 VPC 내에 ENI(Elastic Network Interface)를
배포해서 사용합니다. 그래서 이름이 interface endpoint입니다.
이 인터페이스로 연결을 완성하는 것이 PrivateLink입니다.

#### PrivateLink

Interface endpoint를 이용한 서비스, VPC endpoint services가 PrivateLink입니다.  
[VPC Endpoint Services](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/endpoint-service.html)

## 실습 목표

우리 이전 단계에서 System Manager의 session manager를 통하여 EC2 instance에
접근하기 위하여 VPC endpoint를 작성해 보았습니다 그 과정과 크게 다르지 않습니다.

1. VPC endpoint services(PrivateLink)를 통해 NLB를 연결합니다.
1. VPC 2에서 VPC endpoint를 생성하고, 1.에서 작성한 VPC endpoint services와 연결합니다.
1. Instance 4-1에서 간단 웹 데몬을 동작 시키고
1. Instance 2-1에서 VPC 2에 있는 endpoint를 통하여 Instance 4-1의 웹 서비스에 접속을 합니다.
   경로는 Instance 2-1 > VPC 2 endpoint > PrivateLink > NLB > Instance 4-1
   이 됩니다.

