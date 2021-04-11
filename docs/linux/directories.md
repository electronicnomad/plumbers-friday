# 디렉토리

> directory

디렉토리라는 말이 어떤 사람에게는 어색한 시대가 되었습니다.
디렉토리는 파일이 수록되는 논리적으로 구분된 공간입니다.
디렉토리는 파일을 담고 있고, 다른 디렉토리를 담을 수 있습니다.
그래서 디렉토리 아래에 디렉토리 아래에 디렉토리를 만들 수 있어 계층구조가 형성됩니다.
최상위 디렉토리는 루트 디렉토리(root directory)라고 부르며, / (슬래시) 로 표기합니다.

## 기본 사항

### 루트 디렉토리

최상위 디렉토리를 루트 디렉트로 root directory라고 부릅니다. 표기는 앞서 이야기 한 것과 같이
/ 라고 적고, slash '슬래시'라고 읽습니다.

/ 는 디렉토리의 시작을 의미합니다.
앞서 설명한 것과 같이 root directory는 / 로 표기되며
root directory 아래에 위치하는 디렉토리는 /usr 과 같이 표기할 수 있습니다.
만약 /usr 아래에 또 다른 디렉토리, bin 이 존재한다면, /usr/bin 이렇게 표기할 수 있습니다.

### 현재 디렉토리와 상위 디렉토리

디렉토리를 표기하거나 기능적으로 지칭하는 방식 중에 . 과 .. 이 있습니다.
. 은 현재 디렉토리를 의미하고 .. 은 상위 디렉토리를 의미합니다.
아래의 예제를 보겠습니다.

```bash
$ pwd
/usr/local
$ ls -al
total 0
drwxr-xr-x 1 root root 512 Feb  8 01:57 .
drwxr-xr-x 1 root root 512 Feb  8 01:57 ..
drwxr-xr-x 1 root root 512 Feb  8 01:57 bin
drwxr-xr-x 1 root root 512 Feb  8 01:57 etc
drwxr-xr-x 1 root root 512 Feb  8 01:57 games
drwxr-xr-x 1 root root 512 Feb  8 01:57 include
drwxr-xr-x 1 root root 512 Feb 16 23:00 lib
lrwxrwxrwx 1 root root   9 Feb  8 01:57 man -> share/man
drwxr-xr-x 1 root root 512 Feb  8 01:57 sbin
drwxr-xr-x 1 root root 512 Feb 15 02:13 share
drwxr-xr-x 1 root root 512 Feb  8 01:57 src
$
```

/usr/local 디렉토리에서 `$ ls -al` 명령을 수행한 결과입니다.
그 중 . 과 .. 이 보일 것입니다. 모든 디렉토리에서 . 와 .. 은 조회됩니다. 사용자가 위치하는 디렉토리를 변경하는 `cd` 명령을 사용하여 `$ cd ..`라고 명령하면 상위 디렉토리로 이동하고, `$ cd .`라고 명령하면 여전히 그 위치에 있게 됩니다.  
/usr/local에서 `$ cd ..`라고 명령을 하면, /usr로 이동하게 되고, /usr/local에서 `$cd .`라고 명령을 하면 /usr/local로 가게 됩니다, 즉 그 자리에 있게 됩니다.

### 홈 디렉토리

사용자의 공간을 home directory 홈 디렉토리 라고 합니다.
변수로는 $HOME 이라고 정의합니다.
사용자의 홈 디렉토리의 위치는 `/etc/passwd` 파일에 정의됩니다.  
`$ cat /etc/passwd | grep $USER`로 현재 사용자의
설정을 확인해 봅니다. 그리고,
`echo $HOME`이라고 명령 내려봅니다. 출력되는 결과가 지금 로그인한
사용자의 홈 디렉토리입니다.

홈 디렉토리로 이동하는 방법이 몇 가지 있습니다.

1. 직접 자신의 홈 디렉토리를 입력한다.  
  `cd /home/ubuntu`
2. 환경 변수를 활용한다.  
  `cd $HOME`
3. 다음과 같이 간단히 한다.  
  `cd ~`

UNIX/Linux에서 `~`는 사용자의 홈 디렉토리를 뜻 합니다.

## 경로

디렉토리의 위치에 접근하는 방식을 경로라고 하거나, 디렉토리의 위치 자체를 경로라고 설명하는 사람도 있습니다. 실제 우리가 사용하는 '경로'라는 단어는 經路라는 것으로 '지나는 길'이라는 뜻을 담고 있습니다. 뭔가 서로 맞지는 않지만, 언어라는 것이 널리 쓰이고 그렇게 굳어지면 자연스러운 것이 되는 법이죠.

이 '경로'를 `$PATH`라는 변수로 정의됩니다, 그리고 `$PATH`는 디렉토리의 위치를 지시합니다. 그 위치는 어떤 명령이나 참조 정보(library 라이브러리)를 조회할 때 찾아야 하는 리렉토리 위치를 정의하는 것이라고 할 수 있습니다.

예시를 먼저 보겠습니다.

```bash
$ pwd
/home/user1
$ ls -al ../../usr/local
total 40
drwxr-xr-x 10 root root 4096 Jan 11 12:50 .
drwxr-xr-x 11 root root 4096 Jan 11 12:58 ..
drwxr-xr-x  2 root root 4096 Jan 11 12:50 bin
drwxr-xr-x  2 root root 4096 Jan 11 12:50 etc
drwxr-xr-x  2 root root 4096 Jan 11 12:50 games
drwxr-xr-x  2 root root 4096 Jan 11 12:50 include
drwxr-xr-x  5 root root 4096 Jan 11 13:00 lib
lrwxrwxrwx  1 root root    9 Jan 11 12:50 man -> share/man
drwxr-xr-x  2 root root 4096 Jan 11 12:50 sbin
drwxr-xr-x  7 root root 4096 Jan 11 13:00 share
drwxr-xr-x  2 root root 4096 Jan 11 12:50 src
$ ls -al /usr/local
total 40
drwxr-xr-x 10 root root 4096 Jan 11 12:50 .
drwxr-xr-x 11 root root 4096 Jan 11 12:58 ..
drwxr-xr-x  2 root root 4096 Jan 11 12:50 bin
drwxr-xr-x  2 root root 4096 Jan 11 12:50 etc
drwxr-xr-x  2 root root 4096 Jan 11 12:50 games
drwxr-xr-x  2 root root 4096 Jan 11 12:50 include
drwxr-xr-x  5 root root 4096 Jan 11 13:00 lib
lrwxrwxrwx  1 root root    9 Jan 11 12:50 man -> share/man
drwxr-xr-x  2 root root 4096 Jan 11 12:50 sbin
drwxr-xr-x  7 root root 4096 Jan 11 13:00 share
drwxr-xr-x  2 root root 4096 Jan 11 12:50 src
$
```

`/home/user1`이라는 디렉토리에서 위엣것은 상대경로로 아랫것은 절대경로로 같은 디렉토리를 `ls -al` 명령으로 조회한 것입니다.
이미 위 예시만으로 이해를 했다면 아래의 구구절절한 설명을 굳이 읽으실 필요는 없습니다. ;-)
아니라면, 다음을 봅시다.

### 상대경로와 절대경로

#### 상대경로

위 예시에서 `../../usr/local`이 상대경로입니다. 이 의미는 현재 디렉토리의 상위 그리고 상위 디렉토리 아래에 있는
`usr` 아래에 있는 `local` 이라는 것을 지시하는 것입니다. 현재 디렉토리를 중심으로 상대적인 위치를 경로로 표시하는 것으로 '상대경로'라고 부릅니다. 이는 매우 유용합니다. 예를 들어 나의 절대적 위치를 알지 못 하더라도
내 현재 위치에서 바로 아래에 있는 어떤 디렉토리 혹은 내 현재 위치에서 상위 디렉토리에 있는 어떤 파일에 접근하는 것이 가능하기 때문입니다.
어떤 프로그램을 작성하고 실행시킬 때 그 결과(output)을 현재 디렉토리 아래에 있는 `output`이라는 디렉토리에 저장할 수 있습니다.
굳이 명령어 형식으로 표현한다면, `$ myApps -o ./output` 이렇게 되겠죠.

#### 절대경로

위 예시에서 `/usr/local`이 절대경로입니다. 절대경로는 절대적 위치를 말합니다.
그 절대적 위치의 시작은 루트 디렉토리입니다. 그래서 항상 `/`로부터 시작됩니다.
위 설명과 비슷하게 해 보자면, 어떤 프로그램을 작성하고 실행하실 때 그 프로그램의 작동기록 즉, 로그(log) 파일을
항상, 달리 지정하지 않더라도, `/var/log/myApps.log`로 저장하고 싶다면 절대경로의 사용이 적절합니다.

절대경로는 절대적 위치이기 때문에 항상 참일 것을 가정합니다. `/var/log`라는 디렉토리는 어떤 리눅스에나 있기 때문에 문제가 없습니다.
대체로, 절대경로로 어떤 것을 지정하는 건 이와 같이 널리 알려져 있고, 어떤 시스템에서도 통용되거나, 그 절대적 위치가
활용되는 시스템에서 해당 경로가 항상 존재한다는 '관리자'의 다짐(?)같은 것이 있다면 쓸모 있습니다.

## 관련 명령어들

### 디렉토리 위치 변경

명령어 중에, `cd`가 있습니다. change the shell working directory.
디렉토리를 변경할 때 사용합니다. 용례는, `cd /var/log`와 같이
목적하는 디렉토리로 변경할 수 있습니다. `cd` 명령 뒤에 아무런
선언이 오지 않으면, 사용자의 홈 디렉토리로 이동합니다.

`cd ..`라고 명령하면 상위 디렉토리로 이동하게 되며, `ls ./example`라는 명령은 '현재 디렉토리에 위치한 example이라는 파일(혹은 디렉토리)를 조회하라'가 됩니다. 응용하면, `ls ../example2`라는 명령은 '상위 디렉토리에 위치한 example2라는 파일(혹은 디렉토리)를 조회하라'가 됩니다.

### 현재 디렉토리 출력

`pwd`는 사용자가 현재 위치한 디렉토리를 화면에 출력해 줍니다.
print the name of the current working directory.

```bash
$ pwd
/usr/local/lib
$
```

### 디렉토리 생성과 삭제

`mkdir`은 디렉토리를 생성하는데 사용됩니다. make directory.  
`rmdir`은 디렉토리를 삭제하는데 사용됩니다. remove directory.

#### 생성

`mkdir`은 특별한 것은 없습니다. 다만, `mkdir -p`라고 옵션을 함께 쓰면 재미있습니다.
`mkdir -p ./a/b/c/d`라고 명령을 하면 현재 디렉토리 아래에 a라는 디렉토리를 작성하고
b, c, d까지 한 번에 만들어 줍니다. 만약 현재 디렉토리에 a라는 디렉토리가 존재하면
그 하위 b, c, d를 만들어 냅니다.

만약, `mkdir`로 이미 존재하고 있는 디렉토리와 같은 이름의 디렉토리를 생성하려고 시도하면
실패하게 됩니다.

#### 삭제

`rmdir`은 흔하게 `rm -r`로 대체되어 사용되기도 합니다. 두 명령은 같은 결과를 기대할 수 있습니다.
다만, 다른 것이 있다면, `rmdir`은 해당 디렉토리 내에 어떤 파일이나 디렉토리가 존재하면,
아래의 예시와 같이, 기본적으로 명령 수행을 멈추고 메시지를 출력합니다.

```bash
$ ls -al
total 0
drwxr-xr-x   3 jhin  staff   96 Mar 25 18:17 .
drwxr-xr-x+ 28 jhin  staff  896 Mar 25 18:16 ..
drwxr-xr-x   3 jhin  staff   96 Mar 25 18:17 a
$ rmdir ./a
rmdir: ./a: Directory not empty
$ ls -al ./a
total 0
drwxr-xr-x  3 jhin  staff  96 Mar 25 18:17 .
drwxr-xr-x  3 jhin  staff  96 Mar 25 18:17 ..
-rw-r--r--  1 jhin  staff   0 Mar 25 18:17 b
$ 
```

이와는 달리, `rm -r`은 지정하는 디렉토리 내부에 파일이나 디렉토리의 존재여부와 상관없이 모두 지워냅니다.

## Filesystem Hierachy Standard

각 리눅스 배포판을 제작하는 곳에서는 [File System Hierachy Standard(FSH)](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)라고 하는 디렉토리 구조에 대한 표준을 만들고 유지하려 합니다. FSH는 [리눅스 재단](https://www.linuxfoundation.org/)에서 관리를 하고 있는데, 지금의 표준은 3.0 판이며 지난 2015년에 재정되었다고 합니다.

FHS의 기원은, [BSD](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution)의 디렉토리 구조일 듯 하지만, 아무래도 원형의 의미로서는 [SunOS](https://en.wikipedia.org/wiki/SunOS)를 모방의 대상으로 삼은 것으로 판단할 수도 있습니다. 아무튼, 최초에 UNIX가 있었고, 학술적 목적이든 상업적인 목적이든 시간이 가져다준 합리적인 원형이 만들어졌으며 그것을 바탕으로 견주어 만들어진 것이 현재의 리눅스 배포판들이 체택하고 있는 FHS라고 말할 수 있겠습니다.

FHS는 디렉토리 (혹은 파일시스템) 구조에 대한 정의입니다. 예를 들자면,  

| 디렉토리  | 용도                       |
|-------|--------------------------|
| /     | 루트 디렉토리 모든 구조의 최상위에 위치   |
| /usr  | 사용자에게 필요한 프로그램이나 도구들이 위치 |
| /etc  | 각종 설정에 관계되는 정보가 수록       |
| /var  | 각종 로그 파일들이 쌓이는 곳         |
| /home | 사용자들의 홈 디렉토리 위치          |

등이 있겠습니다. 이에 대한 상세한 설명은, 앞에서도 링크로 안내한 [이곳, 위키피디아]((https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard))에서 찾아볼 수 있습니다. 여기에서 설명한 모든 디렉토리가 지금 의미가 있거나 유용한 것은 아닙니다. 최소한 위에 설명한 몇 가지만 기억해도 크게 도움이 됩니다.
