# 리눅스 명령어와 vim 에디터 매크로
### 1) 리눅스 명령어: top
* top 명령어는 현재 OS의 상태를 나타내주는 CLI 어플리케이션으로서. 메모리 사용량, CPU 사용량 등을 나타내주며 top를 실행하는 동안에는 주기적인 업데이트로 실시간에 근접한 내용을 보여준다. 리눅스에서 top 명령어를 실행하면 아래와 같이 출력된다.  
* ![top실행결과](https://user-images.githubusercontent.com/106875780/172044299-2c54124a-1985-4c39-a85d-477c753ad6d1.PNG)
* **[top - 13:39:10 up 2:27,]** 의 의미는 필자의 컴퓨터기준으로 현재 13시39분10초이며 OS가 실행된지 2시간27분이 지났다는 의미이다.  
* **[load average : 0.08,   0.03,   0.00]** 의 의미는 앞에서 부터 1분, 5분, 그리고 15분에 대한 CPU Load의 평균값입니다. CPU Load란 CPU가 수행하는 작업의 양 입니다. 리눅스에서는 실행되거나 대기중인 프로세스의 평균입니다. 싱글 코어일 경우 1.0의 값이 CPU 100%를 사용하고 있다는 의미입니다. 멀티 코어라면 해당 코어수 만큼 * N을 한 값이 CPU 100%를 사용한다는 의미가 됩니다.  
* **[Tasks: 10 total,   1 running,   9 sleepin,   0 stopped,   0 zombie]** 의 의미는 현재 필자의 컴퓨터에는 1 running은 1개의 프로그램이 CPU에 의해 명령어를 실행중이라는 것을 의미하고 9 sleeping은 9개의 프로그램은 명령어를 대기중 0 stopped는 종료된 프로그램 0 zombie는 좀비상태의 프로그램을 의미한다.  
|이름|하는일|
|---|---|
|실행(Runnable)|CPU에 의해서 명령어가 실행중인 Process|
|준비(Ready)|CPU의 명령어 실행을 기다리는 Process|
|대기(Waiting)|I/O operation이 끝나기를 기다리는 Process|
|종료(Terminated)|Ctrl + Z 등의 signal로 종료된 Process|

* **[%Cpu(s): 0.0  us,   0.2 sy,   0.0 ni,   99.8 id,   0.0 wa,   0.0 hi,   0.0 si,   0.0 st]** 의 의미는CPU가 어떻게 사용되고 있는지 그 사용율을 보여주는 영역으로 모든 값의 총 합은 100% 이며 이를 백분율로 나누어서 보여줍니다. 각 요소는 아래와 같다.
|이름|하는일|
|---|---|
|us|프로세스의 유저 영역에서의 CPU 사용률|
|sy|프로세스의 커널 영역에서의 CPU 사용률|
|ni|프로세스의 우선순위(priority) 설정에 사용하는 CPU 사용률|
|id|사용하고 있지 않는 비율|
|wa|IO가 완료될때까지 기다리고 있는 CPU 비율|
|hi|하드웨어 인터럽트에 사용되는 CPU 사용률|
|si|소프트웨어 인터럽트에 사용되는 CPU 사용률|
|st|CPU를 VM에서 사용하여 대기하는 CPU 비율|

* **[MiB Mem :   6235.3 total,   4751.8 free,   545.1 used,    938.4 buff/cahe]**
* **[MiB Swap:   2048.0 total,   2048.0 free,     0.0 used.   5109.3 avail Mem]** 의 의미는 첫번째 줄은 RAM의 메모리 영역으로 Mem이라 표시되어있는 부분입니다. 그리고 아랫줄은 디스크를 메모리 처럼 이용하는 Swap 메모리 영역입니다. 일반적으로 Mem의 사용량이 거의 가득 찼을때 Swap 메모리 영역을 사용하며 Swap는 디스크이기 때문에 RAM 메모리보다 속도가 많이 느린 단점을 가진다.  

|이름|하는일|
|---|---|
|total|총 메모리 양|
|free|사용가능한 메모리 양|
|used|사용중인 메모리 양|
|buff/cache|커널 버퍼에서 사용되는 메모리|
|cache|Disk의 페이지 캐시|
|avail Mem|swap 메모리를 사용하지 않고 사용할 수 있는 메모리의 크기|  

* **[PID USER ~}** 의 의미는 디테일 영역으로서 디테일 영역에는 각 프로세스에 대한 상세한 내용이 나오며 위 이미지에서 PID USER~부터 끝까지 입니다.  

|이름|하는일|
|---|---|
|PID|프로세스 ID이며 프로세스를 구분하기 위한 겹치지않는 고유한 값|
|USER|해당 프로세스를 실행한 USER 이름 또는 효과를 받는 USER의 이름|
|PR|커널에 의해서 스케줄링되는 우선순위|
|NI|PR에 영향을 주는 nice라는 값|
|VIRT|프로세스가 소비하고 있는 총 메모리|
|RES|RAM에서 사용중인 메모리의 크기|
|SHR|다른 프로세스와의 공유메모리|
|%MEM|RAM에서 RES가 차지하는 비율|
|S|프로세스의 현재 상태|
|TIME+|프로세스가 사용한 토탈 CPU 시간|
|COMMAND|해당 프로세스를 실행한 커맨드|

---
### 2) 리눅스 명령어: ps  
ps는 현재 실행중인 프로세스의 목록을 보는 명령어 이다.
사용법은 다음과 같다.  
> ps [옵션]
아래의 표는 ps에서 사용가능한 옵션이다.  

|이름|하는일|
|---|---|
|-e|실행중인 모든 프로세스의 정보를 출력한다.|
|-f|프로세스에 대한 자세한 정보룰 출력한다.( PPID 확인 가능 )|
|-u [사용자이름]|특정 사용자에 대한 모든 프로세스의 정보를 출력|
|-p pid|pid로 지정한 프로세스의 정보를 출력|
|u|프로세스 소유자의 이름, CPU 사용량, 메모리 사용량 등 상세 정보를 출력|
|a|터미널에서 실행한 프로세스의 정보를 출력|
|x|실행 중인 모든 프로세스의 정보를 출력|
###### 실제 출력까지 해보았다.

* ### ps
ps명령을 옵션 없이 사용하면 현재 셸이나 터미미널에서 실행한 사용자 프로세스에 대한 정보를 출력한다.  
(일시적인) PID는 프로세스번호 / TTY는 터미널 번호 / TIME은 해당 프로세스가 사용한  CPU 시간의 양 / CMD는 프로세스가 실행중인 명령이 무엇인지 알려준다.  
![ps](https://user-images.githubusercontent.com/106875780/172045493-0d8fbf05-2126-43f8-8e63-d6f1c75c75a3.PNG)


* ### ps -e  
ps -e명령어를 이용하여 명령어을 실행하고 ps 명령어로 time이 올라가는 것을 확인 가능하다.
![ps -e](https://user-images.githubusercontent.com/106875780/172045056-78345c95-79c0-42a2-8e5b-aed9097eccb6.PNG)

* ### ps -f  
ps -f 명령으로 ps명령에 비해서 보다 상세한 정보를 확인가능하며. -f 옵션은  PPID 확인가능하다.(PPID는 부모 프로세스의 PID이다.)
![ps -f](https://user-images.githubusercontent.com/106875780/172045096-976fbcc4-61ce-45d8-a31f-1c14e8968089.PNG)
아래의 표는  
> ps -f [옵션]  
으로 사용가능한 옵션 목록이다.  

|이름|하는일|
|---|---|
UID - 프로세스를 실행한 사용자 ID|
|PID - 프로세스 번호|
|PPID - 부모프로세스 번호|
|C - CPU 사용량( %값 )|
|STIME - 프로세스의 시작 시간|
|TTY - 프로세스가 실행된 터미널의 종류와 번호|
|TIME - 프로세스 실행 시간 ( 실행되기전에는 0)|
|CMD -이런 명령어를 사용했다.|

 * ###ps -u [사용자명]  
 ps -u [사용자명] 옵션을 이용하여 user1 계정의 ps를 확인가능하다.  
 ![ps -u](https://user-images.githubusercontent.com/106875780/172045215-81810248-3a6a-4960-801a-2cce9436226f.PNG)

* ### ps -p pid
ps -p pid 명령어를 이용하여 1번 프로세스 출력가능하다.  
![ps -p](https://user-images.githubusercontent.com/106875780/172045304-5b200853-7951-4088-b40f-184c4bea023a.PNG)

* ### ps a  
ps a 명령으로 터미널에서 실행한 프로세스 정보를 출력가능하다.

|상태|하는일|
|---|---|
|R|실행중|
|S|인터럽트 가능한 대기(sleep)상태|
|T|작업 제어에 의해 정지된(stopped) 상태|
|Z|좀비프로세스|  

![ps a](https://user-images.githubusercontent.com/106875780/172045376-d135bd6f-0bb4-4af3-af16-b0c7e333f154.PNG)

* ### ps x
ps x 명령으로 실행중인 모든 프로세스를 출력가능하다.
![ps x](https://user-images.githubusercontent.com/106875780/172045420-b975cb66-629e-4503-ab2b-ff7c356fe6e6.PNG)


---
### 3) 리눅스 명령어: jobs
* jobs 명령어는 작업의 상태를 표시하는 명령어다.  
현재 쉘 세션에서 실행시킨 백그라운드 작업의 목록이 출력되며, 각 작업에는 번호가 붙어 있다.  
사용법은 다음과같다. 
> jobs [옵션] [작업번호]  

사용가능한 옵션은 다음과 같다.  
|옵션|설명|
|---|---|
|-l|프로세스 그룹 ID를 state 필드 앞에 출력|
|-n|프로세스 그룹 중에 대표 프로세스 ID를 출력|
|-p|각 프로세스 ID에 대해 한 행씩 출력|
|command|지정한 명령어를 실행|

jobs로 출력되는 백그라운드 작업의 상태값은 다음과 같다.  

|상태|설명|
|---|---|
|Running|작업이 계속 진행중임|
|Done|작업이 완료되어 0을 반환|
|Done(code)|작업이 종료되었으며 0이 아닌 코드를 반환|
|Stopped|작업이 일시 중단|
|Stopped(SIGTSTP)|SIGTSTP 시그널이 작업을 일시 중단|
|Stopped(SIGSTOP)|SIGSTOP 시그널이 작업을 일시 중단|
|Stopped(SIGTTIN)|SIGTTIN 시그널이 작업을 일시 중단|
|Stopped(SIGTTOU)|SIGTTOU 시그널이 작업을 일시 중단|

---
### 4) 리눅스 명령어: kill
* 리눅스에서 작업을 할 때 응용프로그램이나 프로세스가 멈추는 것을 볼 수 있는데 이 때의 유일한 해결책은 그 프로세스를 종료하는 것입니다.  
리눅스는 이러한 상황에서 사용할 수 있는 kill 명령어를 제공한다.  
kill 명령어 사용법은 다음과같다.
> kill -(보낼 시그널) PID  
시그널에는 후술할 숫자외에도 SIG를 제외한 이름을 넣어도 작동한다.
Ex)
> kill -term PID
> kill -15 PID    

> kill -l  
을 이용하여 아래 이미지처럼 사용가능한 kill 옵션을 확인해볼수있다.  
![kill -l](https://user-images.githubusercontent.com/106875780/172046017-cf77a7d0-60b2-4e2e-847d-eb0117221c1b.PNG)

이를 응용해보자면 파이프와 grep, awk, 역따옴표을 조합하면 특정 이름의 프로세스를 모두 찾아서 종료 시킬 수 있다.
> kill `ps -ef | grep 프로세스이름 | grep -v grep | awk '{print $2}'`  

---
### 5) vim 에디터 메크로
* vim 에디터 메크로는 내가 했던 동작을 길이에 상관없이 단축키 만으로 한번더 같은 행동을 할수있는 명령어이다.

* 먼저 매크로 기록하는 법.  
vim의 중립모드에서 q를 누른 다음 매크로 이름으로 사용할 알파벳을 눌러준다.  
예를들어 qa 라고 누르면 a라는 매크로를 기록하기 시작한다.  
밑에 -- recording -- 이라고 나오면 원하는 매크로를 입력한다.  
기록이 끝났으면 다시 중립모드에서 q를 눌러준다.

* 매크로 재생하는 법.  
중립모드에서 @a 라고 누르면 매크로 a가 재생된다.  
@@를 누르면 제일 마지막에 재생된 매크로, 그러니까 가장 최근에  @e 를 했다면 @@때 재생되는 매크로는 e가 된다.

