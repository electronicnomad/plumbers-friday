# 리눅스를 배워 봅시다

잘 알지도 못 하는데, 전문가도 아닌데 이런 걸 시작했습니다.
일하면서 돈벌어 먹고 살면서 알게 된 것들을 나열해 보겠습니다.
누군가에게는 도움이 되지 않을까요?

*** (( 여전히 슬금슬금 만들고 있습니다 - 완성의 시간은 멀고도 멉니다 )) ***

## 방향

리눅스라는 운영체제에 이제 막 진입하는 사람들을 위하여 만들고 있습니다.
그리고 그래픽 사용자 인터페이스(GUI)에 대한 설명은 ~~아마도~~ 없을 것입니다.
콘솔이든, 원격 접속이든 커맨드 라인(command line)으로 사용하고
제어하는 방법을 알려드립니다.

### 기준

모든 설명에 기준이 되는 배포판은 Debian 계열이 될 것입니다.
Ubuntu에서 수행한 결과를 예제로 삼을 수도 있고,
Debian을 언급할 수도 있고, Raspberry Pi OS를
혹은, WSL(Windows Subsystem for Linux)에 대한 이야기가 있을 수도 있습니다.

Debian 계열을 기준으로 잡는 이유는 매우 간단합니다. 지금 이 글을 적고 있는
사람이 이것에 익숙하기 때문입니다. 다른 예를 추가 되거나, 차이점을 설명해 줄 수 있는
기회가 있기를 바랍니다.

### 구성

아래의 목차는 구성안(構成案)입니다.  
그저 안(案)이어서 언제든 변경될 수 있습니다.

- [x] login and logout 로그인과 로그아웃
- [x] shell 쉘
- [x] directory 디렉토리
- [x] file and permissions 파일과 권한
- [x] users and groups 사용자와 그룹
- [ ] package management 패키지 관리
- [ ] /var/log/* 로그 파일들
- [ ] daemon and services 데몬과 서비스
- [ ] hardware information 하드웨어 정보 조회
- [ ] networking 네트워크
- [ ] storage 스토리지
- [ ] booting process 부팅 과정
- [ ] performance monitoring 성능 모니터링
- [ ] kernel 커널

## 기타

### 읽기를 권하는 문서들

다음의 문서들은 리눅스를 배우고자 하는 사람들에게 좋은 참고 자료가 됩니다.

- [Linux FAQ](https://tldp.org/FAQ/Linux-FAQ/general.html)
- [Learn Linux, 101: Boot the system](https://developer.ibm.com/tutorials/l-lpic1-101-2/)
- [BEGINNER’S GUIDE FOR LINUX – Start Learning Linux in Minutes](https://www.tecmint.com/free-online-linux-learning-guide-for-beginners/)

### 당부

내용은 언제든 변경이 될 수 있습니다.  
그리고 적당한 오류가 포함될 확률이 존재합니다.
