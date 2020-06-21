# 개요

![VPC peering ICON](../../images/networking/vpc-peering/vpc-peering-icon.png){: style='width:120px; padding:5px; float:right; border-radius:8%'}
VPC peering은 두 개의 VPC를 상호 연결하는 것입니다. 이 두 VPC는 하나의 리전에서도
서로 다른 리전에 존재하여도 문제가 되지 않습니다, 지역적 위치에 대한 제약은 없습니다.
하지만, 단 하나 설계할 때 염두해 두어야 하는 것이 있는데,

A와 B가 peering되어 있고, B와 C가 peering되어 있다 하여도, A가 C와
통신할 수 있는 것은 아닙니다. A가 C와 통시하려면, A와 C가 새로이 peering을
맺어야 합니다. A와 C가 peering을 맺지 않고, 단순히 라우팅 테이블의 설정만으로
A가 B를 거쳐 최종적으로 C로 가는 통신을 구현할 수는 없습니다.

그리고 VPC peering은 gateway도 ENI와 같은 것을 활용한 어떤 실질적인 연결도 아닙니다.
그저 정책적인 그러니까 설정에 의존하는 선언적인 연결이라고 볼 수도 있겠습니다.
