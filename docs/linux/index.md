# 리눅스를 배워 봅시다

리눅스라는 운영체제에 이제 막 진입하는 사람들을 위하여 만들었습니다.
기술적 깊이는 매우 얕고, 주변 이야기가 많습니다. 기능적인 정보를 전달하는 것보다
이해를 할 수 있는 내용으로 채우고 싶은 욕심이 있습니다.

초보자에게 좋은 리눅스 안내서 - 가 목표라고 할 수 있겠습니다.

## 방향

여기 수록되는 문서는 처음 리눅스를 접하는 분들께 하나의 길라잡이 역할을 하려 작성되었습니다.
다른 리눅스 문서 프로젝트와는 조금 다른 방향을 가지고 있습니다.
그래서 깊이 있는 이해와 폭넓은 응용은 다루어지지 않을 수도 있습니다.

### 기준

(아직까지는) 기준이 되는 배포판은 Debian 계열이 될 것입니다.
Ubuntu에서 수행한 결과를 예제로 삼을 수도 있고,
Debian을 언급할 수도 있고, Raspberry Pi OS를
혹은, WSL(Windows Subsystem for Linux)에 대한 이야기가 있을 수도 있습니다.  
Debian 계열을 기준으로 잡는 이유는 매우 간단합니다. 지금 이 글을 적고 있는
사람이 이것에 익숙하기 때문입니다. 다른 예를 추가해 주시거나 차이점을 설명해 줄 수 있는
기회가 있기를 바랍니다.

### 구성

아래의 목차는 구성안(構成案)입니다. 그저 안(案)이어서 언제든 변경될 수 있습니다.

- login and logout 로그인과 로그아웃
- shell 쉘
- directory 디렉토리
- file and permissions 파일과 권한
- users and groups 사용자와 그룹
- package management 패키지 관리
- /var/log 로그 파일들
- daemon and services 데몬과 서비스
- hardware information 하드웨어 정보 조회
- networking 네트워크
- storage 스토리지
- booting process 부팅 과정
- performance monitoring 성능 모니터링
- kernel 커널

## 기타

### 읽기를 권하는 문서들

다음의 문서들은 리눅스를 배우고자 하는 사람들에게 좋은 참고 자료가 됩니다.

- [Linux FAQ](https://tldp.org/FAQ/Linux-FAQ/general.html)
- [Learn Linux, 101: Boot the system](https://developer.ibm.com/tutorials/l-lpic1-101-2/)
- [BEGINNER’S GUIDE FOR LINUX – Start Learning Linux in Minutes](https://www.tecmint.com/free-online-linux-learning-guide-for-beginners/)

### 당부

내용은 언제든 변경이 될 수 있습니다.  
그리고 적당한 오류가 포함될 확률이 존재합니다.
