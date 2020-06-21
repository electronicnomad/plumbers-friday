# 개요

![transit gateway](/images/networking/transit-gateway/icon.png){: style='width:120px; padding:5px; float:right; border-radius:8%'}
Transit Gateway는 다수개의 VPC를 운영하고 상호 통신을 해야 하는 상황이 있을 경우
종례의 복잡한 구성을 통해서 구현했어야 했는데, 이를 Transit Gateway가 해결해 줍니다.

Transit Gateway가 보다 용이하게 네트워크 구성을 해결해 주는 대상은
VPC peering과 Transit VPC라고 할 수 있겠습니다.
VPC sharing은 용례가 달라 해당 되지 않겠습니다.

Transit Gateway는 단순히 VPC 간의 연결 뿐만 아니라,
site-to-site VPN도 수용하고 있습니다.

다음의 단계에서부터 수행하면서 알게 되는 부분이기도 하지만,
Transit Gateway는 VPC 간의 연결을 꾀하지만, 연결의 단위는 subnet입니다.
즉, 서로 다른 VPC 내에 있는 subnet을 서로 연결함으로써
VPC 간의 연결을 가져오게 됩니다 - 어쩌면 당연한 것이기도 합니다.

AWS 공식 한국어 문서에서는 Transit Gateway를 전송 게이트웨이로 번역하고 있습니다.
본 문서에서는 원어 그대로인, Transit Gateway 혹은 약어, TGW로 표기합니다.
