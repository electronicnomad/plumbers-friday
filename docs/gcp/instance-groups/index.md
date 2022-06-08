# 개요

Google Cloud에서는 GCP 서비스로서 managed instance groups를 가지고 있습니다.
managed instance groups는 instance groups의 한 종류인데,
instance groups는 다음의 세 가지로 나누어 볼 수 있습니다.

* Managed instance group (stateless)  
  대두분의 경우에 적합니다.
* Managed instance group (stateful)  
  전통적인 데이터베이스와 같은 stateful 워크로드를 위해 마련되어 있습니다.  
  autoscaling이 불가능합니다.
* Unmanaged instance group  
  VM instances의 묶음입니다. 단일 서브넷에 nic0가 연결되어 있는
  VM instances를 하나의 묶음으로 만들 수 있습니다, 서로 VM type이 달라도 무관합니다.  
  load balancer를 연결할 수 있습니다.

여기에서는 위의 셋 중, managed instance group을 다룹니다.

## 우리는 왜 managed instance group을 신경 쓰는가?

가장 클라우드 다운 워크로드의 관리는 탄력적인 자원의 즉시 확보와 적절 시간 내에 task/job을 완료하는 것일 수 있습니다.
managed instance group은 autoscaling을 통해서 이 부분을 가능하게 합니다.

### 가장 클라우드 컴퓨팅 다운 리소스 활용

기대하지 않았던, 순간에 몰려드는 request를 처리를 대비하기 위하여
우리는 머리를 굴려가며 설계를 할 필요가 없게 됩니다.
놓아두면, 알아서 scale-out이 되고, task/job이 끝나면 자동으로 scale-in이
되는 건 가장 클라우드 다운 컴퓨팅이라고 할 수 있습니다.

이러한 기능에 autohealing이 추가되고, updating의 방법이 다양하게 제공되어서, managed instance group으로 rolling update, canary 배포와 같은 응용이 가능하게 됩니다. 물론, managed instance group과 친구처럼 여겨지는 load balancer를 통한 A/B testing도 설게할 수 있습니다.
