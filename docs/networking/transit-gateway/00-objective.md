# 실습 목표

Transit Gateway를 학습하기 위한 본 실습의 목표는 아래의 그림으로 설명합니다.

![학습 목표](/images/networking/transit-gateway/00-target-architecture.svg)

AWS Cloud 내에 존재하는 각기 다른 region의 vpc 간의 통신을 Transit Gateway를 통해서 완성하는 것입니다.

1. Region A의 VPC A에 종속된 Subnet A 내에 위치한 Instance가 transit gateway를 통하여 Region B의 VPC B의 subnet B에 위치하는 instance와 교신을 할 수 있게 합니다. (굵은 적색 화살표)

2. 1.과 같은 위치에 있는 instance가 역시 transit gateway를 거쳐 Region B의 VPC C 내에 있는 private subnet C를 경유, public subnet C의 NAT gateway를 통과, VPC C의 internet gateway를 거쳐 인터넷으로 통신을 할 수 있게 합니다. (굵은 황색 화살표)

Transit gateway가 제공하는 유용한 것들 중에 서로 다른 region의 VPC를 쉽게 연결 구성한다는 것과,
각 VPC가 가지고 있는 여러 중복 서비스(internet gateway, NAT gateway 등)를 제거할 수 있다는
것 - 이 두가지에 대한 장점을 직접 구성해 보는 것입니다.
