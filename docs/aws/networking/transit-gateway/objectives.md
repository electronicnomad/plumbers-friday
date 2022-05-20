# 목표

![target architecture](../objective.svg)

이제 여러번 보았을 위 아키텍처에서 Region A의 Transit gateway와 Region B의 transit gateway를
연결하고, Region B의 transit gateway는 VPC 2의 subnet 2-1과 VPC 4의 subnet 4-1을 연결합니다.

![target architecture - flow](./objective-flow-tgw.svg)

그렇게 구성하여 위 그림과 같이 적색선의 데이터 흐름을 꾀하는 것이 본 실습의 목표입니다.

AWS Cloud 내에 존재하는 각기 다른 region의 vpc 간의 통신을 Transit Gateway를 통해서 완성하는 것입니다.

Transit gateway가 제공하는 유용한 것들 중에 서로 다른 region의 VPC를 쉽게 연결 구성한다는 것과,
각 VPC가 가지고 있는 여러 중복 서비스(internet gateway, NAT gateway 등)를 제거할 수 있다는
것 - 이 두가지에 대한 장점을 직접 구성해 보는 것입니다.
