# PrivateLink

![PrivateLink ICON](../../images/networking/privatelink/privatelink-icon.png){: style='width:120px; padding:5px; float:right; border-radius:8%'}
PrivateLink은 VPC endpoint를 활용한 것입니다.
VPC endpoints는 두 가지가 있는데, 하나는 gateway endpoint이고,
나머지 하나는 interface endpoint입니다.

## VPC Endpoints

### Gateway endpoint

쉬운 것부터 설명하겠습니다. 게이트웨이 엔드포인트는 간단합니다.
특정 서비스와 연결되는 한정된 엔드포인트인데, 그 특정 서비스는
S3, DynamoDB 딱 둘입니다.

네? 게이트웨이 엔드포인트로 무엇을 할 수 있냐구요?

게이트웨이 엔드포인트로 VPC와 연결된 S3나 DynamoDB는 오롯이 VPC내에 있는
리소스/서비스로부터만 연결할 수 있습니다. VPC내 CIDR 영역 내의 IP를 부여받게
됩니다. 그리고 외부로부터의 연결은 불가능하게 됩니다.

### Interface endpoint

interface endpoint는 VPC 내에 ENI - 가상 네트워크 인터페이스 카드 - 하나를
유치해서 이 ENI를 이용 endpoint를 잡는 것입니다.
그래서 이름이 interface endpoint입니다. 그리고 이 인터페이스로 연결을 완성하는 것이
PrivateLink입니다.
