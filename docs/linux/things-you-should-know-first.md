# 먼저 알면 좋을 것들

## 배경 이야기

UNIX라는 인류의 역사를 변화시킨 컴퓨터 운영체제가 있습니다.
UNIX가 지금의 인류가 누리고 있는 정보화 사회를 열었고, 여전히 큰 영향을 미치고 있습니다.
[마이크로소프트도 UNIX](https://en.wikipedia.org/wiki/Xenix)를 만들었고,
세상의 모든 인터넷은 UNIX로 움직이던 시절도 있었습니다.
그리고 그 시절이 지나, Linux가 그 자리를 넘겨 받는 모습니다.

### 오픈소스

Linux은 사실, 커널 이름입니다. 운영체제의 이름은 아니었습니다. 지금은 혼용하여 사용하고
많은 사람들이 이제는 운영체제의 이름으로 사용하고 있습니다.

운영체제로서의 이름으로는 'GNU/Linux System' 이라고 부르는 것이 맞긴 합니다.
이 이름대로 Linux 커널을 가운데에 두고, GNU의 각종 프로그램들이 애워싼 모습니다.
GNU에서는 [Hurd](https://www.gnu.org/software/hurd/hurd.html)라는 오랜 염원으로만
존재하는 커널이 있는데,  Hurd의 성숙 이전에 Linux를 실험적으로 채택하고
운영체제의 모습을 꾸미기 시작한 것이 여기까지 왔다고 합니다.

GNU Hurd는 여전히 'no stable version'입니다.

Linux는 오픈소스입니다. GNU Public License, GPL의 규정을 따르고 있습니다 -
오픈소스 라이센스는 GPL 이외에도 MIT, CDDL, BSD, MPL 等 여럿이 있고
각기 서로 추구하는 방향이나 규정이나 규약이 다릅니다.
만약 흥미가 있다면, [https://opensource.org/licenses](https://opensource.org/licenses)
&larr; 이곳을 방문해 보는 것을 추천합니다.

### 배포판

리눅스는 몇 가지 배포한으로 보통의 사용자에게 전달됩니다.
대표적인 것이 Ubuntu와 Red Hat이 있는데, Ubuntu는 Debian을 기반하여 제작된 배포판입니다.
리눅스 배포판에 대한 설명은 Wikipedia <https://en.wikipedia.org/wiki/List_of_Linux_distributions> 에
잘 설명되어 있습니다. 한 번 시간 내에 정독해 볼만 합니다.

## 명령어들

리눅스 시스템의 운영은 명령어를 익히고 활용하는 데에서 시작되고 완성됩니다.
화려한 그래픽 기반의 어플리케이션이나 웹 브라우저 인터페이스로 무언가를 할 수도 있겠지만,
결국 뒤에서는 명려어들이 수행되고 그 결과를 출력합니다.
그리고 그래픽 인터페이스가 없는 경우는 있어도,
커멘드 라인 인터페이스가 없는 경우는 없을 것입니다.

리눅스를 배우는데 있어 먼저 알고 가야 할 명령어들을 정리해 봅니다.
이건 외워야 합니다. 그리고 이 명령어들은 UNIX를 기반하는 거의 모든
운영체제에서 거의 동일하게 동작합니다.

```bash
man
ls
cd
pwd
cp
mv
cat
more
sort
touch
mkdir
rmdir
rm
```

### 도움말 보기

#### man page

UNIX와 그 파생 운영체제들은 맨 페이지 man page를 기본으로 가지고 있습니다.
요즈음은 그 중요도가 약해진 것 같지만, 여전히 맨 페이지는 정보를 전달하는
중요한 역할을 하고 있습니다.

다음의 명령을 입력해 보면 맨 페이지를 이해하는 데 큰 도움이 됩니다.
`man man` 그리고, 위에 나열한 명령어들의 맨 페이지를 하나씩 읽어 봅시다.  
만약, Debian 계열의 Linux 배포판을 사용하는데, `man` 명령이 없다고 판단된다면
`sudo apt install man-db`를 통해 설치를 합시다.

맨 페이지의 man은 manual을 의미합니다.
각 명령어는 맨 페이지를 가지는 것이 기본이지만, 요즈음은 그렇지 않은
경우도 많습니다 - 개발자들이 맨 페이지를 좋아하지 않은 듯 합니다.

`man ls`라고 입력하면 `ls` 명령어의 맨 페이지를 볼 수 있습니다.
내가 UNIX를 배웠을 때, `/usr/bin` `/usr/sbin` 아래에 있는 모든 명령어의
맨 페이지를 진지하게 읽었습니다. 지루하고 인내가 필요한 일이지만
각 명령을 이해하고 활용하는 데에 큰 도움이 됩니다.

위에 나열한 명령어들의 맨 페이지를 읽어 봅시다. 그리고
학습을 이어 가면서 새로이 만나게 되는 명령의 man page는
꼭 정독해 보길 적극적으로 추천합니다.

#### help 도움말

대부분의 명령에 `--help`라고 옵션을 붙히면 도움말을 볼 수 있습니다.
어떤 명령어는 `-h` 로 혹은 `-H`일 수도 있습니다. 개발자 마음입니다.

#### info

man page가 UNIX의 전통이라면, GNU 프로젝트에서 생성된 명령어들은
info page[^1]를 기본으로 합니다. `$ man man`으로 man page에 대한 기본을
익힐 수 있듯이, `$ info info` 명령을 통해 info page에 대해 익힐 수
있습니다.

[^1]:
    GNU의 emacs라는 텍스트 편집기에서 유례되었다고 합니다.

참, 위 명령 예제에서 `$`는 쉘 프롬프트의 의미입니다.
앞으로 직접 명령어를 입력하는 구문은 이와 같이 표현합니다.

## 편집기

UNIX의 역사는 아마도 조판이나 문서를 편집하는데에서 시작되었다고 할 수 있습니다.
편집기에 대한 초창기 UNIX 개발자들은 많은 공을 들였습니다.
그 중에 vi editor, emacs는 엄청난 문서 편집의 엄청난 가능성을 제시하고
실증했습니다. 지금까지 많은 사람들이 여전히 사랑하고 있지만,
요즈음에는 멋지고 편리한 대안은 많습니다.

하지만, 터미널에서 직접 편집할 수 있는 명령이나 프로그램을 익히는 것은
여전히 중요한 일입니다. 윈도의 메모장처럼 기능이 간단하고
쉽게 익힐 수 있는 nano 라는 편집기가 있고, 앞서 언급한 vi(vim)[^2]라는
세상 모든 Plain text 문서를 작성할 수 있을 것 같은 편집기도 있습니다.
둘 중 하나는 매우 익숙하게 다룰 수 있어야 합니다.

[^2]:
    vi는 visual이라는 단어를 약칭한 것입니다.
    당시 입력하는 문자를 터미널에서 눈으로 바로 확인하는 것은 혁명과 같은 일이었습니다.
    그런 당시 상황을 잘 전달하는 이름입니다.
    vim은 vi + improved라는 의미입니다. 종례의 vi에 여러가지 기능을 강화한 것입니다.
    현재 Linux에 기본으로 설치되는 vi 편집기는 vim입니다.
    `vi`라고 명령으로 동작하며, 어떤 배포판에서는 `vim`이라는 명령이 따로 존재하기도 합니다.

### 참조

* VIM 홈페이지 <https://www.vim.org/>
* GNU nano 홈페이지 <https://www.nano-editor.org/>