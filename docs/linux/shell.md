# 쉘

쉘(Shell)은 일종의 사용자 인터페이스이라고 말할 수 있습니다.

우리가 사용하는 컴퓨터는 하드웨어가 있고, 그 위에 운영체제가 동작하는 구조입니다.
운영체제에서 하드웨어에 가장 가까운 것이 kernel 커널이라는 존재가 있으며,
그 상위에 devices 디바이스나 기타 library 라이브러리나 명령어들이 존재하게 됩니다.
그리고 shell 쉘이 있는데, 이 쉘을 통해서 사람은 컴퓨터와 정보를 주고받게 됩니다.

Linux에서는 현재 `bash`이 가장 보편적이며, `bash`와 같은 같은 `sh`을 바탕으로 만들어진 `zsh`이 macOS Mojave 이후 기본 쉘이 되면서 주목받고 있습니다. 한 때 UNIX에서는 `csh`과 `tcsh`이 중심을 이루었는데,
아무래도 그 때는 C 프로그래밍이 대세였기 때문은 아니었을까? 라는 나름의 추측을 해 봅니다.  
아무튼, 본 문서에서는 우선, `bash`를 다룹니다.

Windows에서 explorer.exe도 cmd.exe도 쉘이라고 할 수 있습니다.

## bash

`bash`는 '배쉬'라고 읽으며 현재 가장 보편적인 쉘입니다.  
`bash`는 [GNU](https://www.gnu.org/)에서 만들어낸 Unix-like 시스템을 위한 쉘입니다. `bash`는 'Bourne-Again SHell'이라는 이름을 가지고 있는데, Bourne Shell(`sh`)을 발전적으로 계승한다는 의미라고 해석할 수 있겠습니다. `sh`, Bourne Shell은 UNIX의 고향, Bell Lab의 Stephen Bourne이 개발했고, UNIX Version 7의 기본 쉘로 채택되었습니다.  
`sh`은 1979년 세상에 소개되었습니다.  
`bash`는 1989년에 첫 릴리즈가 되었습니다.

그리고, `bash`에 대하여 깊이 있는 이해를 원한다면, [공식 홈페이지](https://www.gnu.org/software/bash/)에 있는 [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)을 정독해 보시길 권합니다.

### 초기 설정 파일들

이 부분에 대해서는 이전 챕터, '[로그인과 로그아웃 - 쉘 초기 설정](/linux/login-and-logout/#_4)'에서 다루었습니다.

### 환경 변수

`bash`는 고유의 환경 변수를 가지고는 있지만, 대부분 중요한 변수들은 `sh`에서 상속되었습니다.
이에 대한 설명은 (간단하지만) [공식 메뉴얼](https://www.gnu.org/software/bash/manual/html_node/Shell-Variables.html)을 참조하면 좋겠습니다.

지금부터 설명에서 `$`와 맞붙어 `$ENV`와 같이 표현하는 건 환경변수입니다.
`$ ls -al /opt/SUNWccon`과 같이 표현되는 것은 쉘 프롬프트에서 명령을 뜻하는 것처럼 말입니다.

#### $PATH

이전 챕터에서 언급하긴 했지만, 구체적인 설명은 없었습니다.
`$PATH`는 디렉토리 정보를 가지고 있습니다. 기본적으로 리눅스에서는 어떤 명령을 수행하려면 그 명령이 있는 곳(디렉토리)에 가서 명령을 내려 실행시킵니다. 아래와 같습니다.

```bash
$ cd /usr/bin
$ ./du
73848    .
$
```
이런 작업이 빈번히 일어나면 매번 해당 명령이나 프로그램이 있는 곳을 `cd`로 찾아 들어가야 하고, 또한
그 명령이나 프로그램이 있는 위치를 정확하게 기억해야 하는 번거러움이 있습니다.  
`$PATH`는 이런 반복되고 번거러운 작업을 하지 않아도 현재 나의 위치가 어느 디렉토리 속에 있다하여도
원하는 명령이나 프로그램을 실행시킬 수 있게 합니다.

현재 나의 `$PATH` 설정을 확인하고 싶다면, `$ echo $PATH`와 같이 명령을 내리면 됩니다.

만약, 새로운 디렉토리를 그 변수에 추가하고 싶다면, `$ export PATH='/home/user1/myapps/bin:$PATH`와 같이 명령을 내리면 됩니다. 이렇게 하면, 기존의 `$PATH` 변수에 `/home/user1/myapps/bin`이 추가되게 됩니다.

지난 챕터에서 설명한 것과 같이, `/etc/profile`이나 자신의 홈 디렉토리에 있는 `.profile`을 열어보면
`$PATH`의 정의를 찾아 볼 수 있을 것입니다. 특별한 것은 없으며, `:`으로 각 디렉토리 정보를 구분하여 입력한다는 것만 기억하면 활용하는데에 문제는 없을 것입니다.

#### $HOME

현재 사용자의 홈 디렉토리입니다.

#### $PS1

`$PS1`은 사용자의 쉘 프롬프트를 꾸며주는 변수입니다. ` $ ` 로그인 프롬프트가 이와 같이 보인다면 `$PS1`에 꾸밈이 들어간 것이죠, 이 꾸임에는 나름의 문법이 있는데, <https://access.redhat.com/solutions/505983>와 <https://wiki.archlinux.org/index.php/Bash/Prompt_customization>를 읽어보면 많은 도움이 됩니다. 하지만, 일단 배우고 익히는 건 뒤로 하고 내 쉘 프롬프트를 이쁘게 꾸미고 싶다! 라는 생각을 하신다면,
<https://ezprompt.net/> 이 곳에서 쉽게 만들 수 있답니다.

## Shell built-in commands

관련 번역서 등에서는 '쉘 내장 명령어'라고 한국어로 표현하고 있습니다.
내장은 內藏으로 적을 수 있고, 우리가 아는 동물의 몸 속에 있는 장기들을 뜻 합니다.
그 의미의 확대에 따라서, 밖으로 드러나지 않게 안에 간직하다-라고도 쓰인다고 합니다.
하지만, 뭔가 많이 어색하긴 합니다.

아무튼, 각종 쉘들은 보통의 명령어들과 같은 수준의 명령을 스스로 지니고 있는데,
해당 쉘에서 `$ help`라고 명령을 내리면 화면에 관련된 것들을 출력합니다.

```bash
$ help
GNU bash, version 5.0.3(1)-release (x86_64-pc-linux-gnu)
These shell commands are defined internally.  Type 'help' to see this list.
Type 'help name' to find out more about the function 'name'.
Use 'info bash' to find out more about the shell in general.
Use 'man -k' or 'info' to find out more about commands not in this list.

A star (*) next to a name means that the command is disabled.

 job_spec [&]                                                history [-c] [-d offset] [n] or history -anrw [filename]>
 (( expression ))                                            if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMAN>
 . filename [arguments]                                      jobs [-lnprs] [jobspec ...] or jobs -x command [args]
 :                                                           kill [-s sigspec | -n signum | -sigspec] pid | jobspec .>
 [ arg... ]                                                  let arg [arg ...]
 [[ expression ]]                                            local [option] name[=value] ...
 alias [-p] [name[=value] ... ]                              logout [n]
 bg [job_spec ...]                                           mapfile [-d delim] [-n count] [-O origin] [-s count] [-t>
 bind [-lpsvPSVX] [-m keymap] [-f filename] [-q name] [-u >  popd [-n] [+N | -N]
 break [n]                                                   printf [-v var] format [arguments]
 builtin [shell-builtin [arg ...]]                           pushd [-n] [+N | -N | dir]
 caller [expr]                                               pwd [-LP]
 case WORD in [PATTERN [| PATTERN]...) COMMANDS ;;]... esa>  read [-ers] [-a array] [-d delim] [-i text] [-n nchars] >
 cd [-L|[-P [-e]] [-@]] [dir]                                readarray [-d delim] [-n count] [-O origin] [-s count] [>
 command [-pVv] command [arg ...]                            readonly [-aAf] [name[=value] ...] or readonly -p
 compgen [-abcdefgjksuv] [-o option] [-A action] [-G globp>  return [n]
 complete [-abcdefgjksuv] [-pr] [-DEI] [-o option] [-A act>  select NAME [in WORDS ... ;] do COMMANDS; done
 compopt [-o|+o option] [-DEI] [name ...]                    set [-abefhkmnptuvxBCHP] [-o option-name] [--] [arg ...]
 continue [n]                                                shift [n]
 coproc [NAME] command [redirections]                        shopt [-pqsu] [-o] [optname ...]
 declare [-aAfFgilnrtux] [-p] [name[=value] ...]             source filename [arguments]
 dirs [-clpv] [+N] [-N]                                      suspend [-f]
 disown [-h] [-ar] [jobspec ... | pid ...]                   test [expr]
 echo [-neE] [arg ...]                                       time [-p] pipeline
 enable [-a] [-dnps] [-f filename] [name ...]                times
 eval [arg ...]                                              trap [-lp] [[arg] signal_spec ...]
 exec [-cl] [-a name] [command [arguments ...]] [redirecti>  true
 exit [n]                                                    type [-afptP] name [name ...]
 export [-fn] [name[=value] ...] or export -p                typeset [-aAfFgilnrtux] [-p] name[=value] ...
 false                                                       ulimit [-SHabcdefiklmnpqrstuvxPT] [limit]
 fc [-e ename] [-lnr] [first] [last] or fc -s [pat=rep] [c>  umask [-p] [-S] [mode]
 fg [job_spec]                                               unalias [-a] name [name ...]
 for NAME [in WORDS ... ] ; do COMMANDS; done                unset [-f] [-v] [-n] [name ...]
 for (( exp1; exp2; exp3 )); do COMMANDS; done               until COMMANDS; do COMMANDS; done
 function name { COMMANDS ; } or name () { COMMANDS ; }      variables - Names and meanings of some shell variables
 getopts optstring name [arg]                                wait [-fn] [id ...]
 hash [-lr] [-p pathname] [-dt] [name ...]                   while COMMANDS; do COMMANDS; done
 help [-dms] [pattern ...]                                   { COMMANDS ; }
$
```

위에서 설명된 각 명령들 중에 아래의 것들은 매일 필요한 것이기에, 각 명령에 `--help`를 붙혀 용법을 익히는 것이 좋습니다. 예를 들면,

```bash
$ alias --help
alias: alias [-p] [name[=value] ... ]
    Define or display aliases.

    Without arguments, 'alias' prints the list of aliases in the reusable
    form 'alias NAME=VALUE' on standard output.

    Otherwise, an alias is defined for each NAME whose VALUE is given.
    A trailing space in VALUE causes the next word to be checked for
    alias substitution when the alias is expanded.

    Options:
      -p        print all defined aliases in a reusable format

    Exit Status:
    alias returns true unless a NAME is supplied for which no alias has been
    defined.
```

처럼 말이죠 :-) 그럼, 먼저 익혀두면 좋을 것들을 아래에 나열해 보겠습니다. 몇몇은 다른 챕터에서 이미 다룬 것도 있습니다.

```bash
alias, unalias, bg, fg, history, jobs, logout, pwd, cd, set, unset, echo, exit, for, while, kill, time, true, umask, ulimit, until, while,
```

이 중 몇가지에 대하여 아래에 설명을 하겠습니다.

### alias와 unalias

`alias`는 단어 뜻 그대로 별칭을 지정하는 것입니다. 이는 매우 유용한데, 명령과 그것에 따르는 옵션을 하나로 묶어서 사용자 스스로 정의하는 새로운 명령으로 사용하거나 기존의 명령을 대체할 수 있습니다.

`$ alias ls='ls -al'`  
위 명령은 `ls`라는 명령어를 실행하면 `ls -al`와 같이 수행되는 것을 의미합니다. 기존의 `/bin/ls`를 수행하기 전에 `alias`로 정의된 것을 먼저 수행하기 때문에 사용자는 `ls` 명령어를 실행하면 `ls -al`을 실행한 것과 같은 수행결과를 얻게 됩니다, 아래처럼 말이죠.

```bash
$ alias ls='ls -al'
$ ls
total 36
drwxr-xr-x 1 jhin jhin   512 Mar 11 15:41 .
drwxr-xr-x 1 root root   512 Feb 15 01:12 ..
-rw------- 1 jhin jhin 25852 Mar 22 12:55 .bash_history
-rw-r--r-- 1 jhin jhin   220 Feb 15 01:12 .bash_logout
-rw-r--r-- 1 jhin jhin  3526 Feb 15 01:12 .bashrc
drwxr-xr-x 1 jhin jhin   512 Feb 15 02:32 .bundle
drwxr-xr-x 1 jhin jhin   512 Feb 16 23:16 .cache
drwxr-x--x 1 jhin jhin   512 Mar  6 21:24 .config
drwxr-xr-x 1 jhin jhin   512 Feb 15 02:16 .gem
drwxr-xr-x 1 jhin jhin   512 Mar 16 10:01 .gems
lrwxrwxrwx 1 jhin jhin    29 Feb 15 01:56 git -> /mnt/d/data/Documents/GitHub/
-rw-r--r-- 1 jhin jhin    68 Feb 25 02:00 .gitconfig
drwx------ 1 jhin jhin   512 Feb 25 02:04 .local
-rw-r--r-- 1 jhin jhin  1017 Mar  6 21:28 .profile
drwx------ 1 jhin jhin   512 Feb 27 06:21 .ssh
$ unalias ls
$ ls
git
$
```

위 예시에서, `unalias`라는 명령이 쓰였습니다. 이는 `alias`로 정의한 것을 해제한다는 의미입니다.

### 작업 관리

'작업 관리'라고 하니, 윈도의 '작업 관리자'가 이유없이 연상되기도 하지만,
제가 번역하고 싶었던 문장은 'job control'이었습니다. 윈도의 그것은 'task manager'이죠.

리눅스는 다중 사용자를 위한 시스템이고, 멀티 태스킹이 가능한 시스템이기도 합니다.
요즈음이야, 손에 들고 있는 전화기도 멀티 태스킹이 되는 시대이니 신기할 것은 없지만,
여러 해 전에는 이것이 가능한 운영체제는 대중적이지도 않았고, 되더라도 제대로 되는 것이
많이 없었습니다.

#### bg

`bg`는 background의 약자입니다. 실행시키는 프로그램이나 명령을 background로 동작시키는 것입니다.
인터넷에 검색되는 많은 예제들이 `$ sleep 500`과 같은 명령으로 이 것을 설명하던데, 저도 그렇게 하겠습니다. :-) 참, `sleep`은 단어 그대로 잠드는 것(혹은 지연)을 명령하는 것(아무런 일도 일어나지 않습니다)이고, 뒤이어 오는 `500`은 '500초 동안'이라는 의미입니다. `$ sleep 500`은 '500초 동안 잠시 멈춤'이라고 해석할 수 있겠습니다.

```bash
$ sleep 500
^Z
[1]+  Stopped                 sleep 500
$ jobs
[1]+  Stopped                 sleep 500
$ bg
[1]+ sleep 500 &
$ jobs
[1]+  Running                 sleep 500 &
```

위의 예시는, `$ sleep 500`을 명령한 후, Ctrl+Z(키보드의 control 키와 z 키를 동시에 누름)로
명령을 잠시 중단(suspend)시키고, `jobs` 명령으로 어떤 프로세스(실행 중인 프로그램이나 명령)가 현재 터미널에서 수행되고 있는지 확인했습니다.
그 뒤에, `bg` 명령으로 잠시 중단되었던 명령을 백그라운드에서 계속 수행하게 하였습니다. 그 결과를
`jobs` 명령을 통해 확인합니다.

#### fg

`fg`는 foreground의 약자입니다.
지정하는 background로 동작 중인 프로그램이나 명령을 foreground로 전환하는 명령입니다.
아래의 예시를 보겠습니다. 위 예시에서 이어집니다.

```bash
$ fg
sleep 500
^Z
[1]+  Stopped                 sleep 500
$ jobs
[1]+  Stopped                 sleep 500
$ sleep 1000 &
[2] 416
$ jobs
[1]+  Stopped                 sleep 500
[2]-  Running                 sleep 1000 &
$ fg %1
sleep 500
```

백그라운드로 동작 중인, 한 프로세스(sleep 500)를 `fg` 명령으로 포그라운드로 전환했습니다.
그 이후에 Ctrl+Z로 다시 일시 정지(suspend)시키고, `jobs`로 프로세스를 확인했습니다.
그리고 `$ sleep 1000 &`으로 하나의 작업을 더 동작시켰습니다. 이 번에는 이전과는 달리
숫자만 달라진 것이 아니라, `&`라는 기호를 덧붙혔습니다. `&`를 명령이나 프로그램의 실행과 함께 입력하면
해당 명령이나 프로그램은 즉시 백그라운드로 전환됩니다. 이 때, 이전 프로세스가 `[1]`이라는 수를 할당받았고
이번에는 `[2]`라는 수를 할당받는 걸 보실 수 있을 것입니다. 해당 터미널에서 수행되는 프로세스의 순서입니다.
당 시점에서 고유합니다. `[1]`과 `[2]` 뒤에 이어오는 수, `[2] 416`에서의 `416`은 PID, Process ID라고 하는
해당 OS내에서 부팅 시점 이후 고유한 값을 가지는 수이어서, ID와 같이 식별이 가능합니다. 다른 챕터에서
이에 대한 자세한 설명을 하겠습니다.
이어서, `jobs`로 백그라운드에 있는 프로세스를 확인합니다. 그리고 `fg %1`로 첫번째 프로세스를
포그라운드로 전환시킵니다.

#### 용어

한국어로는, 혹은 전산용어로는, background를 배경(背景) 그리고 foreground를 전경(前景)이라고 사용하고 있습니다. 대부분의 외국어를 한국어로 바꾼 것들이 어색하듯 이 또한 익숙해지기가 쉽지 않습니다.
외국어를 외래어로 인정하는 것도 나쁘지 않은 결정이라고 생각합니다, 괜한 노력을 쏟지 않고 말이죠.

## 기타

### 쉘 프로그래밍

Shell script 쉘 스크립트라는 것이 있습니다. 짧게, script 스크립트라고 말하기도 합니다.
이 쉘 스크립트를 가운데에 놓고 마치 정교한 프로그램처럼 동작시키는 것을 가능하게 하는 어떤 행위를 shell programming 쉘 프로그래밍이라고 부릅니다.

쉘 프로그래밍에 대해서는 따로 구분된 글을 적고 싶은 마음도 있는데, 그 만큼 크고 넓고 유용합니다.

쉘은 스스로 프로그래밍 언어의 환경을 가지고 있습니다.
변수가 있고, 정의가 가능합니다.
하지만, 대부분은 UNIX/Linux 내에 있는 명령어들을 잘 나열하는 것으로 완성될 수도 있습니다.
리눅스 시스템 내에 존재하는 대부분의 제어 목적을 가진 명령어 수행은 쉘 스크립트로 작성되어 있습니다.

다음의 웹 사이트가 쉘 프그래밍/스크립트를 익히는데에 도움이 될 것입니다.
<https://www.javatpoint.com/bash>
