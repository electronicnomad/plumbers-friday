# 파일과 권한

파일은 이유없이 디렉토리와 묶어야 될 것 같지만, 따지자면 디렉토리도 시스템에서는 파일과 별반 다를 것이 없기에 '파일과 권한'을 묶어 하나의 챕터를 만들었습니다.

## 확장자라는 개념은 버리자

확장자라는 개념은 DOS 시절에 매우 중요했고, 여전히 개인 컴퓨팅 환경에서는 이것이 중요한 구분자가 됩니다.
확장자라는 것은 점(.)으로 구분되는 앞과 뒤에 이름 중에 뒤의 이름을 정형화 하여 그 파일의 성격을 규정하는 것입니다.

### 도스와 윈도

만약, DOS와 Windows에서 .exe .com .bat 과 같이 끝나는 파일 이름이 있다면 이들은 실행이 가능한 것들일 것이며,
.doc .docx 는 Microsoft Office Word라는 프로그램이 생성하는 고유의 파일 이름이라고 할 수 있겠습니다.

### 웹 서비스

사실 이것은 운영체제의 범위를 벗어나면 또 다른 유용함이 있는데, 바로 웹 서비스에서 입니다.
웹 서버/데몬은 서비스에 필요한 파일들을 이 확장자를 기준으로 하여 그 유형을 구분합니다.
.html .md .jpeg .png .gif 등이 대표적이라고 할 수 있겠습니다.

### 그럼 리눅스에서는?

사실 확장자라는 개념은 사람이 구분하기 쉽게 하기 위한 장치이기도 합니다.
하지만, 리눅스에서는 .exe 라든지 .com 으로 끝나는 파일이 있다고 하여도 이는 실행된다고 장담할 수는 없습니다.
확장자는 그저 이것으로 구분이 필요한 누군가 혹은 어떤 것의 편의에 의한 것이기 때문입니다.

```bash
$ ls -al /bin/bash
-rwxr-xr-x 1 root root 1215072 Jun 18  2020 /bin/bash
$ 
```

위 예시와 같이 실행이 가능한 파일은 첫번째 컬럼에서 보이는 것과 같이 `x`라는 특별한 구분자가 보입니다. 예시에서는 3번 반복되어 나타나고 있습니다. 이를 아래에서 설명하겠습니다.

## 권한 permission

앞선 예시에서 첫번째 컬럼은 해석이 필요한 어떤 기호처럼 보입니다. 그 해석하는 법은 아래와 같습니다.

예시의 첫번째 컬럼은 총 7개의 비트 혹은 문자로 이루어져 있습니다. 표현은 `----------`이 되겠습니다. 풀어서 보면, 네가지로 구분됩니다.

### - r w x

```sh
-  r  w  x 
|  |  |  |
|  |  |  +------ execute
|  |  +--------- write
|  +------------ read
+--------------- 'd' is directory
                 '-' is file
```

첫번째는 `-` 하나의 비트입니다. 이는 `-`과 `d` 둘 중 하나로 표현되는데, `-`는 파일, `d`는 디렉토리라는 의미입니다.

두번째는 `---` 세개의 비트입니다. 이 세 자리에는 차례대로 `r`, `w`, `x`,가 올 수 있습니다. 이는 서로 조합이 되는데, `rwx`, `rw-`, `r--`, `-w-`, `--x`, `-wx` 등 표현이 가능합니다. `r`은 read, `w`는 write, `x`는 execute입니다.

세번째와 네번째는 두번째와 동일한 표현 형식을 따릅니다. (물론 예외적인 상황도 있습니다. s나 T로 표현되는 경우도 있는데, 이에 대한 설명은 따로 하겠습니다)

기본적으로 리눅스에서는 이렇게 rwx로 파일에 대한 접근 권한을 정의하고 실행 가능하다는 정의도 내리게 됩니다. 그런데, 위 설명에 따르면 같은 정의를 세 차례 반복하는 느낌입니다. 이에 대한 설명을 바로 아래에서 하겠습니다.

### owner, group, other

```sh
- rwx rwx rwx
   |   |   |
   |   |   +---- other
   |   +-------- group
   +------------ owner
```

앞서 설명한 `- --- --- ---` 중 뒷쪽 세 가지는 owner(소유권자), group(그룹), 그리고 other(그 외 모든 경우)에 대한 설정입니다. 다시 첫 예시를 보자면,

```bash
$ ls -al /bin/bash
-rwxr-xr-x 1 root root 1215072 Jun 18  2020 /bin/bash
$ 
```

| 권한 | 대상 | 설명 |
|--|--|--|
| rwx | owner(root) | 읽기, 쓰기, 실행 |
| r-x | group(root) | 읽기, 쓰기, 실행 |
| r-x | other | 읽기, 실행 |

`/bin/bash`라는 파일에 대하여  
소유권을 가지고 있는 사용자, root는 읽고, 쓰고, 실행할 수 있습니다.  
그룹 이름, root 에 속한 사용자들은 읽고, 쓰고, 실행할 수 있습니다.
그외 시스템에 있는 모든 사용자들은, 읽고, 실행할 수 있습니다.

라는 해석이 가능합니다.  
여기까지 이해를 했으면, 결론적으로 리눅스에서는 owner, group, other로 권한을 관리한다는 것을 파악했을 것입니다. 이는 기본적인 적용입니다. 만약 이보다 복잡한 권한 설정이 필요하면 ACL이라는 것을 사용합니다. Access Control List의 약어인, ACL은 기회가 있으면 따로 설명하도록 하겠습니다. 그 이전에 이에 대한 정보가 필요하다면, <https://www.redhat.com/sysadmin/linux-access-control-lists> 이 문서가 좋은 안내가 될 수 있습니다.

### 권한에 대한 상세 해설

#### 파일에 대하여

w, 즉 쓰기(write)에 대한 권한은 해당 파일을 편집할 수 있는 권한과 지울 수 있는 권한을 같이 가져 갑니다.

#### 디렉토리에 대하여

w, 즉 쓰기(write) 관한은 디렉토리 아래에서는 새로운 파일을 생성할 수 있는 권한을 의미합니다.
x, 즉 실행(execute) 권한은 디렉토리에 접근할 권한을 의미합니다. 즉, x가 표기되지 않은 곳으로는 들어가 볼 수가 없습니다.

## 권한 설정

### chown

`chown`은 지정하는 대상의 소유자를 변경하는 것입니다. change ownership.

```bash
$ ls -al ./test.sh
-rwxr-xr-x 1 jhin jhin 111 Apr 23 20:24 ./test.sh
$
```

위 예시는 현재 디렉토리에 있는, `test.sh`이라는 파일을 `ls -al` 명령으로 조회한 것입니다.
여기에서 3번째에 해당되는 것이 소유자, owner에 대한 표시입니다. `test.sh`이라는 파일은
`jhin`이라는 사용자가 소유하고 있는 것이죠.

이 소유자를 변경할 수 있는 명령이 `chown`입니다. 다음과 같이 사용할 수 있습니다.

```bash
$ sudo chown nobody ./test.sh
[sudo] password for jhin:
$ ls -al ./test.sh
-rwxr-xr-x 1 nobody jhin 111 Apr 23 20:24 ./test.sh
$
```

`sudo`라는 명령으로 root 권한을 일시적으로 확득합니다. `sudo`는 그런 역할을 하는 용도가 단순하고 아주
강력한 명령입니다. 그 뒤에 따르는 명령, `chown nobody ./test.sh`은 `nobody`라는 사용자로 `./` 현재
디렉토리에 위치하고 있는 `test.sh`이라는 파일의 소유권을 변경하라. 라는 뜻입니다.

`sudo`라는 root 권한을 획득하는 과정이 필요한 이유는, 단순합니다. 당신의 소유물을 타인에게 권리를 양도하는 것
또한, 타인의 허락이 필요하기 때문입니다. 당신이 임의로 할 수 없다는 것이죠. 이건 현실 세계와 다르지 않습니다.
그래서, 이것을 강제할 수 있는 초월적 권한이 필요한 것입니다.

`chown`은 소유자를 변경할 때, 그룹도 변경할 수 있습니다. `chown --help`라고 하면
조회할 수 있는 도움말의 첫 번째 행을 보면... `chown [OPTION]... [OWNER][:[GROUP]] FILE...`와 같이 되어 있습니다.  
`$ chown nobody:sys ./test.sh`과 같은 명령을 하게 되면, 현재 디렉토리에 있는 test.sh이라는 파일의
소유자를 nobody로 변경함과 동시에 그룹을 sys로 변경하는 것입니다.  
그룹 변경에 대한 더 상세한 내용은 아래에서 다루겠습니다.

### chgrp

`chgrp`는 지정하는 대상의 그룹을 변경하는 것입니다. change group.

`chgrp`는 `chgrp [OPTION]... GROUP FILE...`와 같은 문법으로 사용하게 됩니다.  
`chgrp`는 `chown`과 달리, 반드시 root 사용자의 권한을 필요로 하지 않습니다.
다만, 번경하려는 그룹은 그 명령을 내리는 사용자도 속해 있어야 합니다.

`id`라는 명령을 내려보면, 해당 사용자가 속한 그룹을 쉽게 찾아 볼 수 있습니다. 혹은
`/etc/group` 파일을 열어 보아도 됩니다.

```bash
ubuntu@node0:~$ id
uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),115(netdev),118(lxd),998(microk8s)
ubuntu@node0:~$ 
```

아래는 `chgrp`를 활요하여 그룹을 변경하는 예시입니다.

```bash
ubuntu@node0:~/workspace/test$ ls -al
total 8
drwxrwxr-x 2 ubuntu ubuntu 4096 Apr 24 16:42 .
drwxrwxr-x 4 ubuntu ubuntu 4096 Apr 24 16:42 ..
-rw-rw-r-- 1 ubuntu ubuntu    0 Apr 24 16:42 test.file
ubuntu@node0:~/workspace/test$ id
uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),115(netdev),118(lxd),998(microk8s)
ubuntu@node0:~/workspace/test$ chgrp cdrom ./test.file 
ubuntu@node0:~/workspace/test$ ls -al
total 8
drwxrwxr-x 2 ubuntu ubuntu 4096 Apr 24 16:42 .
drwxrwxr-x 4 ubuntu ubuntu 4096 Apr 24 16:42 ..
-rw-rw-r-- 1 ubuntu cdrom     0 Apr 24 16:42 test.file
ubuntu@node0:~/workspace/test$ chgrp sys ./test.file 
chgrp: changing group of './test.file': Operation not permitted
ubuntu@node0:~/workspace/test$ sudo chgrp sys ./test.file 
ubuntu@node0:~/workspace/test$ ls -al ./test.file 
-rw-rw-r-- 1 ubuntu sys 0 Apr 24 16:42 ./test.file
ubuntu@node0:~/workspace/test$ 
```

위 예시에서 앞서 설명한 것과 같이, 사용자가 속한 그룹, `cdrom`으로는 자유롭게 변경이 가능했지만,
속하지 않은 그룹, `sys`로 변경할 때는 `sudo` 명령을 통한 root 사용자의 권한이 필요했습니다.

### chmod

`chmod`는 파일이나 디렉토리이 모드를 변경하는 것입니다. change mode.

모드의 변경은, 해당 파일이나 디렉토리에 대한 '읽기', '쓰기', '실행'에 대한 허용여부와
그 허용을 '사용자', '그룹', '모든 사용자'로 범위를 설정하는 것을 포함합니다.
이 바탕은 위에서 설명한 것과 같습니다.

```bash
ubuntu@node1:~/test$ ls -al ./test.file
-rw-r--r-- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ chmod a-rwx ./test.file
ubuntu@node1:~/test$ ls -al ./test.file 
---------- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ chmod u+rw,g+r,o+r ./test.file 
ubuntu@node1:~/test$ ls -al ./test.file
-rw-r--r-- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ chmod g+w ./test.file 
ubuntu@node1:~/test$ ls -al ./test.file 
-rw-rw-r-- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
```

위 예시는 `chmod`를 통해서 파일의 권한을 변경해 본 것입니다.
최초의 파일 상태는, `-rw-r--r--`였습니다.
즉, 사용자는 '읽기'와 '쓰기', 그룹은 '읽기', 그 외에는 '읽기'가 가능한 상태였습니다.

이 상태에서 `chmod a-rwx`를 통해, 모든 권한을 제거하였습니다. 그 뒤에 다시 원상태로 돌렸습니다.
예시에서 유추해 볼 수 있는 것과 같이, a(ll)는 사용자 그룹 그 외를 모두 한 번에 설정할 수 있으며,
u(ser)는 사용자를, g(roup)는 그룹을 뜻 합니다.
여기에 +와 -를 사용해서 r(ead), w(rite), (e)x(ecute)의 권한을 추가하거나 제거할 수 있는 직관적은 방식입니다.

이렇게, 기호를 통한 모드의 변경도 가능하지만, 수를 통해서도 가능합니다.
익숙해지면, 아래의 방식을 더 많이 사용하게 될 것입니다. 일단, 아래의 도표를 살펴보겠습니다.

| 기호표시 | 이진표시 Binary | 팔진표시 Octal | 설명                    |
|-----|-----------|------------|--------------------------|
| --- | 000       | 0          | Nothing                  |
| --x | 001       | 1          | Execute                  |
| -w- | 010       | 2          | Write                    |
| -wx | 011       | 3          | Write and execute        |
| r-- | 100       | 4          | Read                     |
| r-x | 101       | 5          | Read and execute         |
| rw- | 110       | 6          | Read and write           |
| rwx | 111       | 7          | Read, write, and execute |

rwx의 표기를 이진법으로 치환하여 생각하면 위 표의 두 번째 열처럼 됩니다.
이를 팔진법으로 표기하면 세 번째 열이 됩니다. 이것을 user group other로 엮어 나타내면 아래와 같습니다.

```bash
-rw-r--r-- = 0644
-rwxr-xr-x = 0755
```

위에 인용한 예시를 수를 이용한 모드 변경으로 아래와 같이 동일한 결과를 얻을 수 있습니다.

```bash
ubuntu@node1:~/test$ ls -al ./test.file 
-rw-rw-r-- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ chmod 0000 ./test.file 
ubuntu@node1:~/test$ ls -al ./test.file 
---------- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ chmod 0644 ./test.file 
ubuntu@node1:~/test$ ls -al ./test.file 
-rw-r--r-- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ chmod 000 ./test.file 
ubuntu@node1:~/test$ ls -al ./test.file 
---------- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ chmod 644 ./test.file 
ubuntu@node1:~/test$ ls -al ./test.file 
-rw-r--r-- 1 ubuntu ubuntu 0 Apr 24 18:51 ./test.file
ubuntu@node1:~/test$ 
```

각 비트에 대응하는 자릿수는 4자리이기 때문에 형식상 0644 0755 등이 맞지만,
첫번째 비트는 특별한 정의가 없으면, 0으로 간주되기 때문에 3자리만 정의해서 사용할 수 있습니다.

#### 참조

`chmod`에 대한 상세하고 친절한 설명은,
[위키피디아 - File system permissions](https://en.wikipedia.org/wiki/File-system_permissions)를 참조하면 좋겠습니다.
위 링크에 있는 문서를 읽다보면, setuid와 setguid 그리고 sticky bit에 대한 설명도 관심있게 읽어 보시길 바랍니다.
그리고 `umask`라는 부분이 실재 파일의 모드와 직접적인 관련이 있는데, 이 또한
[위키피디아 - umask](https://en.wikipedia.org/wiki/Umask)를 찾아보길 추천합니다.
