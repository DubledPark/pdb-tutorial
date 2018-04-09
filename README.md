# `pdb` 튜토리얼

이 튜토리얼의 목적은 [Python 2](https://docs.python.org/2/library/pdb.html)와 [Python 3](https://docs.python.org/3/library/pdb.html)를 위한 파이썬 디버거인  `pdb` (**P**ython **D**e**B**ugger)의 기초 사용법을 알려주는 것입니다.
또한 디버깅중에 활용할 수 있는 유용한 트릭들 또한 소개하고 있습니다. 이 튜토리얼은 Python 2.7과 Python 3.4에 최적화 되어 있으며 버전간 `pdb` 명령어가 다른 경우에는 차이점을 강조하여 표시하고 있습니다. 다음 명령어를 통해 여러분의 머신에 설치되어 있는 파이썬의 버전을 확인할 수 있습니다.

```shell
python --version
```

버전을 확인했다면 튜토리얼을 시작해봅시다!


## 디버거의 목적

코드로 넘어가기 전에 디버깅과 디버깅 도구 활용의 중요성에 대해 간단한 브리핑을 해보겠습니다. 필자가 생각하는 디버거의 중요성 세 가지는 다음과 같습니다.

디버거를 사용하면 다음의 것들을 할 수 있습니다:

* 실행중인 프로그램의 상태를 들여다 볼 수 있다
* 적용하기 전에 구현 코드를 테스트 해볼 수 있다
* 프로그램의 실행 로직을 따라갈 수 있다

디버거를 사용하면 프로그램의 모든 지점에서 [중단점(breakpoint)](https://en.wikipedia.org/wiki/Breakpoint)를 설정하여 프로그램을 중단하고 위 세 가지를 적용할 수 있습니다. 디버거는 아주 강력한 도구이며 디버거를 사용하면 사방에 `print()`문을 사용하는 것보다 디버깅 과정을 훨씬 빠르게 처리할 수 있습니다.

베테랑 프로그래머 여러분이라면, 최고의 프로그래머와 효과적으로 디버깅하는 방법을 알고있는 프로그래머 사이에 상관 관계가 있다는 것에 동의할 것입니다. 효과적으로 디버깅을 한다는 것은 프로그램의 문제를 진단하고 최소한의 어려움으로 오류를 처리 할 수 있다는 것을 의미합니다.
디버거를 사용하고 디버거를 올바르게 사용하는 방법을 배우면 효과적인 디버깅을 할 수 있는 프로그래머가 될 수 있습니다. 디버깅 환경에서 어려움없이 편하게 코드를 탐색할 수 있는데까지는 어느 정도 시간이 필요할테지만 이 튜토리얼의 목적은 여러분의 코드베이스에서 직접 `pdb`를 사용해보기전에 디버거 활용에 대한 발판을 마련해주는 것입니다.


## 게임 플레이

우리는 디버거의 목적에 대해서 알게되었고 이제 이를 실행에 옮길 시간입니다. 우선, 이 레포지토리를 클론합니다. `git`이 설치되어 있지 않다면 [여기](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)에서 `git`을 다운로드 받으십시오. 다른 소스 컨트롤 시스템을 사용해도 무관하나 `git`을 추천합니다. `git`이 설치되었다면 다음 명령어를 통해 레포지토리를 클론합니다.

```shell
git clone https://github.com/spiside/pdb-tutorial
```

**참고**: 위 명령어가 동작하지 않는다면 Github의 [클론 튜토리얼](https://help.github.com/articles/cloning-a-repository/)을 따라해보세요.

이제 레포지토리를 클론 했으므로 프로젝트의 루트로 이동하여 지침서(`instructions.txt`)를 살펴보겠습니다.

```shell
cd /path/to/pdb-tutorial
```

`file: instructions.txt`
```
Your boss has given you the following project to fix for a client. It's supposed to be a simple dice game where the object of the game is to correctly add up the values of the dice for 6 consecutive turns.

The issue is that a former programmer worked on it and didn't know how to debug effectively. It's now up to you to fix the errors and finally make the game playable.

To play the game you must run the main.py file.

여러분은 보스로부터 다음 프로젝트를 수정하라는 임무를 받았습니다. 이 프로그램은 간단한 주사위 게임이며 게임의 목적은 6번의 연속턴에서 나온 주사위 값들의 합을 맞추는 것입니다.

문제는 이 프로그램을 작성한 전 프로그래머가 디버깅을 효과적으로하는 방법을 몰랐다는겁니다. 이제 여러분이 이를 고쳐야하며 최종적으로 정상적인 게임 플레이가 가능하도록 만들어야합니다.

게임을 플레이하기 위해선 main.py 파일을 실행해야합니다.
```

문제는 쉬워 보입니다! 시작하기 전에 무엇이 잘못되었는지 확인하기 위해 게임을 플레이해봅시다. 프로그램을 실행하려면 터미널에서 다음 명령어를 실행하세요:

```shell
python main.py
```

다음과 같은 결과를 보게 될 것입니다.

```
Add the values of the dice
It's really that easy
What are you doing with your life.
Round 1

---------
|*      |
|       |
|      *|
---------
---------
|*      |
|       |
|      *|
---------
---------
|*     *|
|       |
|*     *|
---------
---------
|*     *|
|       |
|*     *|
---------
---------
|*     *|
|   *   |
|*     *|
---------
Sigh. What is your guess?: 
```

주사위 값들의 합이 17이므로 17을 입력해봅시다.

```
Sigh. What is your guess?: 17
Sorry that's wrong
The answer is: 5
Like seriously, how could you mess that up
Wins: 0 Loses 1
Would you like to play again?[Y/n]: 
```

이상합니다. 답이 5라고 말하고 있지만 확실히 잘못되었습니다 .. 합산이 잘못된 것 같지만 한 번 더 플레이를 해봅시다. 다시 플레이 하라는 메시지가 `Y`인 것 같으니 한 번 입력해봅시다.

```
Would you like to play again?[Y/n]: Y
Traceback (most recent call last):
  File "main.py", line 12, in <module>
    main()
  File "main.py", line 8, in main
    GameRunner.run()
  File "/Users/Development/pdb-tutorial/dicegame/runner.py", line 62, in run
    i_just_throw_an_exception()
  File "/Users/Development/pdb-tutorial/dicegame/utils.py", line 13, in i_just_throw_an_exception
    raise UnnecessaryError("You actually called this function...")
dicegame.utils.UnnecessaryError: You actually called this function...
```

음 이상합니다. 우리가 입력한 값이 유효한 값인 것 같지만 프로그램은 예외를 발생시키고 있습니다. 프로그램이 망가졌다고 말해도 괜찮을 것 같으니 이제 디버깅을 시작해봅시다!


## PDB 101: `pdb` 소개

이제 드디어 파이썬의 고유 디버거인 `pdb`로 작업할 차례입니다. 디버거는 파이썬의 표준 라이브러리에 포함되어 있으며 다른 파이썬 라이브러리와 동일한 방법으로 사용하면 됩니다. 먼저, `pdb` 모듈을 임포트하고 프로그램에 디버깅 중단점을 추가하기 위한 메서드를 호출해봅시다. 이를 수행하는 일반적인 방법은 중지하려는 줄에서 임포트를 추가하고 같은 줄에서 동시에 메서드를 호출하는 것입니다. 이 한 줄이 포함시키려는 전체 명령입니다.

```python
import pdb; pdb.set_trace()
```

[`set_trace()`](https://docs.python.org/3/library/pdb.html#pdb.set_trace) 메서드는 명령어가 위치하는 줄에서 중단점을 하드 코딩합니다. 이제 `main.py` 파일을 열고 8번째 줄에 중단점을 추가해봅시다.

`file: main.py` 
```python
from dicegame.runner import GameRunner


def main():
    print("Add the values of the dice")
    print("It's really that easy")
    print("What are you doing with your life.")
    import pdb; pdb.set_trace() # add pdb here
    GameRunner.run()


if __name__ == "__main__":
    main()
```

훌륭합니다. `main.py`를 다시 실행하고 결과를 살펴봅시다.

```shell
python main.py
```
```
Add the values of the dice
It's really that easy
What are you doing with your life.
> /Users/Development/pdb-tutorial/main.py(9)main()
-> GameRunner.run()
(Pdb) 
```

도착했습니다! 우리는 지금 실행중인 프로그램의 한가운데에 있고 프로그램을 추적할 수 있게 되었습니다. 우리가 해결해야할 첫 번째 문제는 주사위 값들의 올바른 합산 문제일 것 같습니다.

파이썬 인터프리터에 익숙하다면, 많은 인터프리터 지식을 pdb 디버거에서도 활용할 수 있습니다. 그러나 몇 가지 문제가 발생할 것입니다. 이에 대해서는 고급 섹션에서 살펴보겠습니다. 어쨌든 합산 문제를 해결하는데 도움이 되는 몇 가지 명령을 배워봅시다.

## 5가지 `pdb` 명령어로 여러분은 말문이 막힙니다

다음은 `pdb` 문서에서 바로 가져온 다섯 가지 명령어인데, 일단 한 번 배우기만 하면 이 명령어들 없이 어떻게 살았을까 할겁니다.

1. `l(ist)` - 현재 줄 주위의 11개의 줄을 표시하거나 이전 목록을 계속 표시합니다.
2. `s(tep)` - 현재 줄을 실행하고 현재 함수의 다음 줄에서 멈춥니다. 단, 다음 줄이 함수 호출인 경우, 호출된 함수로 들어갑니다.
3. `n(ext)` - 현재 함수의 다음 줄에 도달할 때까지 실행을 계속하거나 반환합니다.
4. `b(reak)` - 중단점을 설정합니다 (인자로 옵션을 줄 수 있음)
5. `r(eturn)` - 현재 함수의 리턴 직전까지 실행을 계속합니다.

> step과 next의 차이점: 다음 줄이 함수 호출인 경우 step은 해당 함수 안으로 들어가서 멈추지만, next는 호출된 함수를 모두 실행하고 현재 함수의 다음 줄에서만 멈춤

모든 키워드의 마지막 부분에 있는 괄호는 명령어 단어의 나머지 부분을 나타내며 `pdb` 프롬프트에서 *선택적으로* 사용할 수 있습니다. 위 단축 명령어는 타이핑을 줄여주지만 프로그램에 `l`이나 `n`과 같은 변수명이 있는 경우엔 `pdb`의 명령어가 더 우선시됩니다. 즉, 프로그램에 `c`라는 변수가 있는 경우 `c`의 값을 알아내고자 `pdb`에서 `c`를 타이핑하면 중단점을 만날때까지 계속 실행되는 `c(ontinue)` 키워드가 실행됩니다.

**참고**: 필자를 포함한 많은 프로그래머들은 `a`, `b`, `gme`와 같은 짧은 변수명의 사용을 지양합니다. 이런 변수명은 의미도 부실하며 코드를 읽기 어렵게 만듭니다. 필자는 짧은 변수명이 있는 상태에서 `pdb`를 사용할 때 발생할 수 있는 문제만 설명합니다.

**참고**: 도움 명령어도 있습니다: `h(elp) - 인자 없이 사용하면 사용가능한 명령어의 리스트를 보여줍니다. 명령어를 인자로 사용하면 명령어에 대한 도움말을 출력합니다. `

튜토리얼의 나머지 부분에서는 단축 명령어를 사용하겠습니다. 위에서 소개하지 않은 명령어를 사용할 경우 부연 설명을 붙일겁니다. 먼저 첫번째 명령어부터 시작해보겠습니다.

### 1. l(ist) - 소스 파일 열기가 귀찮습니다

```
l(ist) [first [,last]]
    List source code for the current file. Without arguments, list 11 lines around the current line
    or continue the previous listing. With one argument, list 11 lines starting at that line.
    With two arguments, list the given range; if the second argument is less than the first, it is a count.
```

**참고**: 위 설명은 `list`에 대해 `help`를 호출한 결과입니다. `pdb` 실행기에서 `help l`을 입력하면 됩니다.

`list`를 사용하면 현재 파일의 소스 코드를 검사할 수 있습니다. `list`의 인자로 출력할 줄의 갯수를 지정할 수 있습니다. 이는 서드파티 패키지를 디버깅할 때 특히 유용합니다.

**참고**: Python 3.2 이상에서는 현재 함수 혹은 프레임의 소스 코드를 보여주는 `ll` (long list) 명령어가 추가되었습니다. 필자는 항상 `l` 대신 `ll`을 사용하는데 현재 줄 주변의 임의의 11개의 줄을 보는것보다 훨씬 더 유용하기 때문입니다.

이제 `l`을 한 번 사용해봅시다. 이미 열려있는 `pdb` 프롬프트에서 `l`을 입력하고 출력값을 확인해보세요.

```
(Pdb) l
  4     def main():
  5         print("Add the values of the dice")
  6         print("It's really that easy")
  7         print("What are you doing with your life.")
  8         import pdb; pdb.set_trace()
  9  ->     GameRunner.run()
 10     
 11     
 12     if __name__ == "__main__":
 13         main()
[EOF]
```

전체 파일 내용을 보고 싶다면 1에서 13까지의 범위를 지정하여 list 함수를 호출할 수 있습니다:

```
(Pdb) l 1, 13
  1     from dicegame.runner import GameRunner
  2     
  3     
  4     def main():
  5         print("Add the values of the dice")
  6         print("It's really that easy")
  7         print("What are you doing with your life.")
  8         import pdb; pdb.set_trace()
  9  ->     GameRunner.run()
 10     
 11     
 12     if __name__ == "__main__":
 13         main()
```

안타깝게도 이 파일만으로는 많은 정보를 얻지 못했지만 `GameRunner` 클래스에서 `run()` 메서드를 호출하고 있음을 볼 수 있습니다. 이 부분에서, 여러분은 "오, 그럼 `dicegame/runner.py` 파일의 run 메서드에서 `pdb`를 설정하면 되겠네"라고 생각할 수도 있습니다. 그래도 되긴 하지만, 다음 섹션에서 다룰 `step` 커맨드를 사용하는 더 쉬운 방법이 있습니다.

### 2. `s(tep)` - 메서드가 무슨 일을 하는지 알고 싶습니다

```
s(tep)
    Execute the current line, stop at the first possible occasion
    (either in a function that is called or in the current
    function).
```

현재 실행중인 줄은 `9`번째 줄입니다. 현재 위치는 `list` 명령어의 출력에서 `->`을 통해 알 수 있습니다.

`step` 명령어를 호출하여 어떤일이 일어나는지 살펴봅시다.

```
(Pdb) s
--Call--
> /Users/Development/pdb-tutorial/dicegame/runner.py(22)run()
-> @classmethod
```

잘했습니다! `> /Users/Development/pdb-tutorial/dicegame/runner.py(21)run()` 이 줄을 통해 우리가 현재 `runner.py` 파일의 22번째 줄에 있음을 알 수 있습니다.

문제는 컨텍스트 정보가 별로 없기 때문에 `list` 명령어를 실행해 메서드를 확인해봅시다.

```
(Pdb) l
 16             total = 0
 17             for die in self.dice:
 18                 total += 1
 19             return total
 20     
 21  ->     @classmethod
 22         def run(cls):
 23             # Probably counts wins or something.
 24             # Great variable name, 10/10.
 25             c = 0
 26             while True:
```

와우! 이제 `run()` 메서드에 대한 컨텍스트를 조금 얻었지만, 우리는 아직 `22`번째 줄에 있습니다. 메서드에 진입하기 위해 한 번 더 step을 실행하고 현재 위치를 알 수 있도록 list를 실행해봅시다.

```
(Pdb) s
> /Users/Development/pdb-tutorial/dicegame/runner.py(25)run()
-> c = 0
(Pdb) l
 20     
 21         @classmethod
 22         def run(cls):
 23             # Probably counts wins or something.
 24             # Great variable name, 10/10.
 25  ->         c = 0
 26             while True:
 27                 runner = cls()
 28     
 29                 print("Round {}\n".format(runner.round))
 30  
```

보시는 바와 같이, 호출시 문제를 일으킬 수 있는 `c`라는 끔찍한 이름의 변수를 가지고 있습니다. (이전에 `c(ontinue)` 명령어와 관련된 코멘트를 생각해보세요) 우리는 현재 `while` 루프 바로 앞에 있으므로 루프로 들어가 우리가 발견할 수 있는 다른 것들을 살펴봅시다.

### 3. `n(ext)` - 현재 줄에서 예외가 발생하지 않길 바랍니다

```
n(ext)
    Continue execution until the next line in the current function
    is reached or it returns.
```

현재 줄에서 `n(ext)`와 `list` 명령어를 실행하여 무슨 일이 일어나는지 살펴봅시다.

```
(Pdb) n
> /Users/Development/pdb-tutorial/dicegame/runner.py(27)run()
-> while True:
(Pdb) l
 21         @classmethod
 22         def run(cls):
 23             # Probably counts wins or something.
 24             # Great variable name, 10/10.
 25             c = 0
 26  ->         while True:
 27                 runner = cls()
 28     
 29                 print("Round {}\n".format(runner.round))
 30     
 31                 for die in runner.dice:
```

이제 현재 줄은 `whilte True` 명령문에 위치하게 되었습니다! 이제 프로그램이 예외를 발생시키거나 종료될때까지 `next`를 계속 실행할 수 있습니다. `for` 루프로 들어가기 위해 `next`를 3번 더 호출한 뒤 `list`를 연달아 실행해봅시다.

```
(Pdb) n
> /Users/Development/pdb-tutorial/dicegame/runner.py(27)run()
-> runner = cls()
(Pdb) n
> /Users/Development/pdb-tutorial/dicegame/runner.py(29)run()
-> print("Round {}\n".format(runner.round))
(Pdb) n
Round 1

> /Users/Development/pdb-tutorial/dicegame/runner.py(31)run()
-> for die in runner.dice:
(Pdb) l
 26             while True:
 27                 runner = cls()
 28     
 29                 print("Round {}\n".format(runner.round))
 30     
 31  ->             for die in runner.dice:
 32                     print(die.show())
 33     
 34                 guess = input("Sigh. What is your guess?: ")
 35                 guess = int(guess)
```

현재 위치에서, `next` 명령어를 호출하면`runner.dice`의 길이만큼 `for` 루프를 순회하게 됩니다. `pdb` 실행기에서 `len()` 함수를 호출하여 `runner.dice`의 길이를 확인할 수 있습니다. 호출 결과는 5를 반환할 것입니다.

```
(Pdb) len(runner.dice)
5
```

길이가 5밖에 안되므로, `next`를 5회만 호출하면 루프를 순회할 수 있지만, 반복해야할 횟수가 50회, 10,000회라고 가정해보면 다른 방법이 필요해 보입니다. 보다 더 좋은 옵션은 중단점을 설정하고 중단점까지 `continue`를 호출하는 것입니다.


### 4. `b(reak)` - `n`을 반복적으로 호출하고 싶지 않습니다

```
b(reak) [ ([filename:]lineno | function) [, condition] ]
    Without argument, list all breaks.

    With a line number argument, set a break at this line in the
    current file.  With a function name, set a break at the first
    executable line of that function.  If a second argument is
    present, it is a string specifying an expression which must
    evaluate to true before the breakpoint is honored.

    The line number may be prefixed with a filename and a colon,
    to specify a breakpoint in another file (probably one that
    hasn't been loaded yet).  The file is searched for on
    sys.path; the .py suffix may be omitted.
```

이 튜토리얼에선 `b(reak)`의 설명중 첫 두 문장에만 주의를 기울이면 됩니다. 이전 섹션에서 언급했듯이, `for` 루프 다음에 중단점을 설정하여  `run()` 메서드를 계속 탐색할 수 있도록 하려고 합니다. 유저의 입력을 기다리는 입력 함수가 있는 `34`번째 줄에 중단점을 설정합니다. `b 34`를 호출해 중단점을 설정하고 `continue`를 호출해 중단점까지 실행합니다.

```
(Pdb) b 34
Breakpoint 1 at /Users/Development/pdb-tutorial/dicegame/runner.py(34)run()
(Pdb) c

[...] # prints some dice

> /Users/Development/pdb-tutorial/dicegame/runner.py(34)run()
-> guess = input("Sigh. What is your guess?: ")
```

아무 인자 없이 `break`를 호출하면 설정한 중단점들을 볼 수 있습니다.

```
(Pdb) b
Num Type         Disp Enb   Where
1   breakpoint   keep yes   at /Users/Development/pdb-tutorial/dicegame/runner.py:34
    breakpoint already hit 1 time
```

위 출력의 가장 왼쪽열에서 볼 수 있는 중단점 번호와 함께 `cl(ear)` 명령어를 사용하면 중단점을 삭제할 수 있습니다. `clear` 명령어로 1번 중단점을 삭제해봅시다.

**참고**: `clear` 명령어에 아무 인자도 주지 않으면 모든 중단점들을 삭제합니다.

```
(Pdb) cl 1
Deleted breakpoint 1 at /Users/Development/pdb-tutorial/dicegame/runner.py:34
```

`next`를 호출하면 `input()` 함수를 실행할 수 있습니다. 추측값으로 10을 입력하고 `pdb` 실행기로 돌아가 다음 줄들을 보기 위해 `list`를 호출합니다.

```
(Pdb) n
Sigh. What is your guess?: 10
> /Users/Development/pdb-tutorial/dicegame/runner.py(35)run()
-> guess = int(guess)
(Pdb) l
 30     
 31                 for die in runner.dice:
 32                     print(die.show())
 33     
 34                 guess = input("Sigh. What is your guess?: ")
 35  ->             guess = int(guess)
 36     
 37                 if guess == runner.answer():
 38                     print("Congrats, you can add like a 5 year old...")
 39                     runner.wins += 1
 40                     c += 1


```

우리는 지금 왜 우리의 추측이 첫 플레이에서의 결과와 맞지 않았는지를 알아내고 있음을 기억하세요. 아마 비교 조건문인 `guess == runner.answer()`에 문제가 있을 것 같습니다. 여기서 오류가 발생하는 것 같으니 `runner.answer()` 메서드가 어떻게 동작하는지 확인 해봐야합니다. `next`를 호출해 다음 라인으로 이동한 뒤 `step`을 통해 `runner.answer()` 메서드로 들어가봅시다.

```
(Pdb) s
--Call--
> /Users/spiro/Development/mobify/engineering-meeting/pdb-tutorial/dicegame/runner.py(15)answer()
-> def answer(self):
(Pdb) l
 10         def reset(self):
 11             self.round = 1
 12             self.wins = 0
 13             self.loses = 0
 14     
 15  ->     def answer(self):
 16             total = 0
 17             for die in self.dice:
 18                 total += 1
 19             return total
 20  
```

필자는 문제를 찾은 것 같습니다! 18번 줄에서 `total` 변수가 주사위의 값들을 제대로 합산하고 있지 않습니다. `die`가 주사위 값을 가지고 있는지 확인하여 이를 해결할 수 있는지 확인해봅시다. 18번 줄로 가기 위해 중단점을 설정하고 첫 순회까지 `next`를 호출합니다. `18`번 줄에 도착하면, `die` 인스턴스에서 `dir()` 함수를 호출하여 어떤 메서드와 속성값들을 가지고 있는지 확인합니다.

```
-> total += 1
(Pdb) dir(die)
['__class__', '__delattr__', [...], 'create_dice', 'roll', 'show', 'value']
```

리스트의 마지막에 `value` 속성값이 있습니다! value 값을 호출하여 어떤 값을 반환하는지 확인해 봅시다. (값은 랜덤이기 때문에 필자와 다를 수도 있다) 재미삼아, `show()` 메서드를 호출하여 주사위의 값과 주사위의 출력 모양이 같은지도 확인해봅시다.

```
(Pdb) die.value
2
(Pdb) die.show()
'---------\n|*      |\n|       |\n|      *|\n---------'
```

**참고**: `\n`을 개행 문자로 출력하려면 `print()` 함수로 `die.show()`를 호출하세요.

우리가 원하는대로 잘 동작하는 것 같습니다. 이제 answer 메서드를 수정할 준비가 되었습니다. 그러나 우리중 일부는 디버깅을 계속 유지하면서 한 번에 모든 문제를 다 찾고 싶을수도 있습니다. 안타깝게도 우리는 또 다시 for 루프에 갇혔습니다. 여러분은 `19`번 줄에 중단점을 설정하고 `continue`를 호출하면 될거라고 생각할테지만 이 경우에 사용할 수 있는 더 좋은 방법이 있습니다.

### 5. `r(eturn)` - 이 함수를 빠져 나가고 싶습니다

```
r(eturn)
    Continue execution until the current function returns.
```

`return`은 함수의 최종 결과값을 검사할 수 있는 아주 *강력한* 명령어입니다. 반환문 호출시 중단점을 설정할 수 있지만 `return` pdb 명령어는 단일 반환에 대한 실행 경로를 따르기 때문에 단일 함수에 다중 반환 ㅁ명령문이 있는 경우에 유용합니다. `return` 명령어를 호출해 함수의 마지막 부분으로 이동합니다.

```
(Pdb) r
--Return--
> /Users/Development/pdb-tutorial/dicegame/runner.py(19)answer()->5
-> return total
(Pdb) l
 14     
 15         def answer(self):
 16             total = 0
 17             for die in self.dice:
 18                 total += 1
 19  ->         return total
 20     
 21         @classmethod
 22         def run(cls):
 23             # Probably counts wins or something.
 24             # Great variable name, 10/10.
(Pdb) 
```

반환된 `total` 변수의 값을 확인하려면 현재 위치에서 `total` 을 호출하거나 `--Return--` 출력줄 아래에 있는 최종값을 확인하면 됩니다. 이제 `run()` 메서드를 빠져나오기 위해 `next` 명령어를 호출하면 이전 위치로 돌아가게 됩니다.

이제 `exit()` 또는 `CTRL+D` (파이썬 실행기에서와 동일)를 호출하여 `pdb` 디버거를 종료할 수 있습니다. 이러한 다섯 가지 명령어를 사용하면 여러가지 버그를 찾고 좀 더 고급의 pdb 예제를 따라할 수 있습니다.


## `pdb` 고급 주제

다음은 여러분이 사용할 수 있는 몇 가지 고급 `pdb` 명령어입니다.

### ! (뱅) 명령어

```
!
  Execute the (one-line) statement in the context of the current stack frame.
```

뱅 커맨드 (`!`)는 다음의 명령문이 `pdb` 명령어가 아닌 Python의 명령인 것으로 해석할 수 있도록 해줍니다.이는 `c` 변수를 가진 `run()` 메서드에서 유용합니다. 튜토리얼의 초반부에서 언급했듯이, `pdb`에서 `c`를 호출하면 `continue` 명령어가 실행되는 문제가 있습니다. `pdb` 실행기에서 코드를 탐색하다가 `runner.py` 파일의 `26`번째 줄에서 멈추고 `c` 앞에 `!`을 붙인 명령어를 실행하면 다음과 같은 결과가 나옵니다.

```
(Pdb) !c
0
```

`25`번째 줄에서 `c = 0`으로 설정했으므로 우리 의도에 맞는 결과값을 얻었습니다!


### `pdb` 포스트 모템

```
pdb.post_mortem(traceback=None)
    Enter post-mortem debugging of the given traceback object. If no traceback is given, it uses the one of the exception that is currently being handled
    (an exception must be being handled if the default is to be used).

pdb.pm()
    Enter post-mortem debugging of the traceback found in sys.last_traceback. 
```

두 메서드는 동일해 보이지만, `post_mortem()`과 `pm()`은 주어진 트랙백(trackback)에 따라 다릅니다. 필자는 `except` 블록에서 주로 `post_mortem()`을 사용합니다.

그러나, `pm()` 메서드가 좀 더 강력하다고 생각하기 때문에 이에 대해서 다루고자 합니다. 이 메서드가 실제로 어떻게 동작하는지 살펴봅시다.

프로젝트의 루트 디렉토리의 셸에서 `python`으로 파이썬 실행기를 엽니다. 그 다음 실행기에서 `main` 모듈에서 `main` 메서드와 `pdb`를 임포트합니다. 게임을 계속하기 위해 `Y`를 입력해 예외가 발생할 때까지 게임을 진행합니다.

```
>>> import pdb
>>> from main import main
>>> main()
[...]
Would you like to play again?[Y/n]: Y
Traceback (most recent call last):
  File "main.py", line 12, in <module>
    main()
  File "main.py", line 8, in main
    GameRunner.run()
  File "/Users/Development/pdb-tutorial/dicegame/runner.py", line 62, in run
    i_just_throw_an_exception()
  File "/Users/Development/pdb-tutorial/dicegame/utils.py", line 13, in i_just_throw_an_exception
    raise UnnecessaryError("You actually called this function...")
dicegame.utils.UnnecessaryError: You actually called this function...
```

이제 `pdb` 모듈에서 `pm()` 메서드를 호출하여 무슨일이 일어나는지 살펴봅시다.

```
>>> pdb.pm()
> /Users/Development/pdb-tutorial/dicegame/utils.py(13)i_just_throw_an_exception()
-> raise UnnecessaryError("You actually called this function...")
(Pdb) 
```

보세요! 우리는 마지막 예외가 발생한 지점으로부터 프로그램을 복구하고 pdb 프롬프트에 들어왔습니다. 여기서 우리는 크래시 나기 전의 프로그램의 상태를 검사할 수 있습니다.

**참고**: 익셉션이 발생하기 전까지 `python -m pdb main.py`와 `continue`를 사용해 `main.py`를 실행할 수도 있습니다. 파이썬은 포착되지 않은 예외가 발생한 지점에서 자동으로 `post_mortem` 모드로 들어갑니다.


## 마무리

튜토리얼을 마친걸 축하드리며 여기까지 따라와줘서 감사합니다! 의견, 잘못된 점 또는 추가 고급 예제가 있으면 풀 리퀘스트를 날려주세요.