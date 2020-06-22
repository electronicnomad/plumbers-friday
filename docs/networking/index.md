# 시작

여기에서는 AWS networking에 대하여 알아봅니다.
네트워킹이라고 하면 참 그 범위가 넓습니다. 그리고 보안과 함께 생각 것도 많습니다.
이렇게 범위가 넓은 이유는, 그렇습니다, 기반 서비스이기 때문입니다.
그래도 전통적인 인프라에 비하면, 클라우드 컴퓨팅 세상에서는 정말 단순화
되었기에 '그 때'와 비교하면 초라해 보이기까지 합니다.

하지만, AWS에서의 네트워킹에 대한 개념은 나름 제대로 익혀야 합니다.
모든 on-premises와 다른 점도 분명 존재하고 모든 클라우드 서비스 사업자들이
같은 개념을 사용하지도 않습니다. 만약 여러분께서 on-premises의 네트워크
설계와 운영에 경험이 있다면, 다른 클라우드 사업자의 그것보다
분명 AWS 네트워킹은 이해하기 쉬울 수 있습니다.

## 기본 개념

AWS에서는 네트워킹에 기본이 되는 개념을 VPC, Virtual Private Cloud가
지니고 있습니다. 여기에서 출발합니다. 이 과정을 시작하기 전에 VPC에 대한
개념을 확실히 가지고 있지 않다고 판단이 되시면,
[공식 문서](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/what-is-amazon-vpc.html)를 읽어보시기 바랍니다.

AWS networking이 찾이하는 그 모든 부분에 대해서 다루는 것은 어려운 일이기도 하고
효용가치가 있는 일이라고 생각하지는 않습니다.
다만, 특정 서비스를 중심으로 어떤 예제를 상정하고 배워 간다면 괜찮다고 봅니다.

### VPC 핵심 개념

VPC를 이해하고 이를 기반으로 AWS 전반의 네트워크를 설계하고 운영하는데 기본이 되는
핵심은 다음의 다섯가지라고 할 수 있겠습니다.

- [VPC](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/what-is-amazon-vpc.html) (위에서 소개한 공식 문서를 재차 링크 걸어 두었습나다)
- Subnet (여러분께서 익히 알고 있는 그 subnet과 같습니다)
- Routing table (여러분께서 익히 알고 있는 그 routing table과 같습니다)
- Internet gateway (VPC가 AWS Cloud 밖, 그러니까 쉽게 말해서 인터넷으로 연결하기 위한 게이트웨이입니다)
- VPC endpoint (이제부터 알아가 보십시다)

지금 준비된 주제는 다음과 같습니다.

- PrivateLink (VPC Endpoint Services)
- VPC Peering
- Transit Gateway

## 다음에 대해서는 다루지 않습니다

- IPv6
- 네트워크 보안
- 타 AWS 계정과 VPC 연결
- On-premises와 연결하는 하이브리드 네트워크 구성
- 타 클라우드 서비스 제공자와 연결하는 멀티 클라우드 연결
- 무엇보다 기초적인 내용: 세상에서 가장 어려운 것이 기초를 설명하고 이해하는 일이라고 합니다.
  가장 어려운 것은 가장 나중에 하는 것이라는 원칙을 입시를 치루면서 우리는 배웠습니다 ;-)
