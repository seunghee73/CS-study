# **Chapter 4. Process Management**

> 프로세스 생성, 종료

------

### **프로세스 생성 (Process Creation)**

Copy-On-Write (COW) : 자식은 부모 자원을 그대로 공유하여 사용하고 있다가 write 발생할 경우 복사

- 부모 프로세스(Parent process)가 자식 프로세스(children process)를 생성
- 프로세스의 트리(계층 구조) 형성
- 프로세스는 자원을 필요로 함
  - 운영체제로부터 받는다
  - 부모와 공유한다
- 자원의 공유
  - 부모와 자식이 모든 자원을 공유하는 모델
  - 일부를 공유하는 모델
  - 전혀 공유하지 않는 모델
- 수행 (Execution)
  - 부모와 자식은 공존하며 수행되는 모델
  - 자식이 종료(terminate)될 때까지 부모가 기다리는(wait-블럭된 상태) 모델 - wait() 시스템 콜
- 주소 공간(Address space)
  - 자식은 부모의 공간을 복사함 (binary and OS data) -fork
  - 자식은 그 공간에 새로운 프로그램을 올림 (복제 생성 후 덮어 씌움-exec)
- UNIX의 예
  - **fork()** 시스템 콜이 새로운 프로세스를 생성
    - 부모를 그대로 복사 (OS data except PID + binary)
    - 주소 공간 할당
  - fork 다음에 이어지는 **exec()** 시스템 콜을 통해 새로운 프로그램을 메모리에 올림



### fork() 시스템 콜

- A process is created by the fork() system call.

  - creates a new address space that is a duplicate of the caller.

  - 부모 프로세스

    ```c
    int main()
    {	int pid;
    	pid = fork();
    	if (pid == 0) /* this is child */
    		printf("\n Hello, I am child!\n");
    	else if (pid > 0) /* this is parent */
    		printf("\n Hello, I am parent!\n");
    }
    ```

    위에서부터 실행하다가 fork를 만나면 새로운 프로세스를 만듦

    아래와 같은 자식 프로세스가 생기고, 

    함수 실행이 끝나면 부모 프로세스는 아래쪽의 코드를 계속 실행함

    자식 프로세스는 main 함수의 시작부터 실행하는 것이 아니라 fork 이후부터 실행함

  - 자식 프로세스

    ```c
    int main()
    {	int pid;
    	pid = fork();
    	if (pid == 0) /* this is child */
    		printf("\n Hello, I am child!\n");
    	else if (pid > 0) /* this is parent */
    		printf("\n Hello, I am parent!\n");
    }
    ```

    프로세스의 fork를 통한 복제 생성은 부모 프로세스의 문맥을 그대로 복사함. 부모 입장에서는 프로그램 카운터가 fork를 가리키고 있음. fork가 끝난 시점에 문맥이 이르고, 자식도 그대로 copy 하므로 main 함수의 제일 윗부분부터 실행하는 것이 아닌, 부모가 fork한 것을 그대로 이어 다음 코드 부분을 실행함.

  - 자식과 부모의 혼동 문제를 막기 위해, 구분 해줌

    부모 프로세스는 pork의 결과 값이 양수, 자식 프로세스는 0

    **Parent process**

    pid > 0

    **Child process**

    pid = 0



### exec() 시스템 콜

- A process can execute a different program by the exec() system call.

  - replaces the memory image of the caller with a new program.

    ```c
    int main()
    {	int pid;
    	pid = fork();
    	if (pid == 0) /* this is child */
        {	printf("\n Hello, I am child! Now I'll run date \n");
        	execlp("/bin/date", "/bin/date", (char *) 0);
        }	
    	else if (pid > 0) /* this is parent */
    		printf("\n Hello, I am parent!\n");
    }
    ```

    execlp 함수가 결국 exec() 시스템 콜을 하게 됨. 

    새로운 시스템으로 덮어 씌움. 

    /bin/date 는 리눅스에서의 커맨드, 프로그램임. 

    부모 프로세스 실행 후 자식은 "\n Hello, I am child! Now I'll run date \n"를 출력하고 이후 date라는 새로운 시스템으로 덮어 쓰는 식으로 실행이 되는 것.

    exec 한 뒤에는 다시 돌아갈 수 없음.

  - 꼭 자식을 만들어서 exec 할 필요는 없음

    ```c
    int main(){
    	printf("\n Hello, I am child! Now I'll run date \n");
        execlp("/bin/date", "/bin/date", (char *) 0);
    	printf("\n Hello, I am parent!\n");
    }
    ```

    

### wait() 시스템 콜

- 프로세스 A가 wait() 시스템 콜을 호출하면

  - 커널은 child가 종료될때 까지 프로세스 A를 sleep시킨다 (block 상태) - 잠들게 만듦
  - Chile process가 종료되면 커널은 프로세스 A를 깨운다 (ready 상태) 

  ```c
  main {
      int childPID;
      S1;
      
      childPID = fork(); /* fork를 한 뒤에 결과값이 0이면, */
      if(childPID == 0)
          <code for child process>
      else { /* 결과값이 0이 아니라면 (부모 프로세스라면) */ 
          
          wait(); /* wait을 넣어줘서 잠들게 됨. */
      } /* cpu를 얻지 못하고 자식 프로세스가 종료될 때까지 기다림 */
      
      S2;
  }
  ```




### **프로세스 종료 (Process Termination)**

- 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려줌 

  (exit)

  - 자식이 부모에게 output data를 보낸다 (via **wait**)
  - 프로세스의 각종 자원들이 운영체제에게 반납됨

- 부모 프로세스가 자식의 수행을 종료시킴 

  (abort)

  - 자식이 할당 자원의 한계치를 넘어설 때
  - 자식에게 할당된 테스크가 더 이상 필요하지 않을 때
  - 부모가 종료(exit) 하는 경우
    - 운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 두지 않는다
    - 단계적인 종료 (딸려있는 모든 자식을 종료시킨 후 부모를 죽임)



### exit() 시스템 콜

- 프로세스의 종료
  - 자발적 종료
    - 마지막 statement 수행 후 exit() 시스템 콜을 통해
    - 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어줌
  - 비자발적 종료
    - 부모 프로세스가 자식 프로세스를 강제 종료시킴
      - 자식 프로세스가 한계치를 넘어서는 자원 요청
      - 자식에게 할당된 태스크가 더 이상 필요하지 않음
    - 키보드로 kill, break 등을 친 경우
    - 부모가 종료하는 경우
      - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨



### 프로세스와 관련한 시스템 콜

- fork() : create a child (copy)
- exec() : overlay new image
- wait() : sleep until child is done
- exit() : frees all the resources, notify parent



### 프로세스 간 협력

- 독립적 프로세스 (Independent process)
  - 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함
- 협력 프로세스 (Cooperating process)
  - 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음
- 프로세스 간 협력 메커니즘 (IPC : Interprocess Communication)
  - 메시지를 전달하는 방법
    - **message passing** : 커널을 통해 메시지 전달
  - 주소 공간을 공유하는 방법
    - **shared memory** : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 있음 (처음에는 커널의 도움을 받지만 이후에는 프로세스 간 공유함-서로 신뢰할 수 있는 관계여야 함)
    - thread : thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능 (주소 공간 자체를 thread 자체가 완전 공유하기 때문에)



### Meassage Passing

- Message system

  - 프로세스 사이에 공유 변수(shared variable)를 일체 사용하지 않고 통신하는 시스템
  - 메시지를 직접 보낼 수 없고 운영체제 **커널**을 통해서 전달 가능

- Direct Communication

  - 통신하려는 프로세스의 이름(Q)을 **명시적**으로 표시

    ​		Process P 		=> 		Process Q

    Send (Q, message)	Receive (P, message)

- Indirect Communication

  - mailbox (또는 port)를 통해 메시지를 **간접** 전달

    ​		Process P 	=>	Mailbox M	=>	Process Q

    Send (M, message)							Receive (P, message)

- shared memory : 주소 공간을 공유하는 방법

  - shared memory : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘
  - thread : thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능



### Interprocess Communication

---



# Chapter 5. CPU Scheduling

### CPU and I/O Bursts in Program Execution

- 프로그램이 실행되면 어떤 프로그램이든 간에 아래의 path를 진행함

... load store / add store / read from file (CPU burst (Running) 

=> wait for I/O (I/O burst) 

=> store increment / index / read from file (CPU burst) 

=> wait for I/O (I/O burst) ... 

- 프로그램은 CPU burst(CPU를 연속적으로 쓰는 시간)와 I/O burst의 연속이지만 프로그램의 종류에 따라서 빈도나 길이가 달라짐



### CPU-burst Time의 분포

- 여러 종류의 job (=process)이 섞여 있기 때문에 CPU 스케줄링이 필요하다 (- I/O bound job 때문)
  - Interactive job에게 적절한 response 제공 요망
  - CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용
  - 공평한 것보다 효율적 매커니즘이 중요. intractive job에게 너무 오랜 시간을 기다리게 하면 안됨.
  - 누구에게 우선적, 언제 뺏을 것인가 등이 중요한 이슈

![CPU-burst Time의 분포](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/CPU-burst Time의 분포.JPG)

- CPU burst가 짧은 경우가 빈번하고 긴 경우도 간혹 나타남

### 프로세스의 특성 분류

- 프로세스는 그 특성에 따라 다음 두 가지로 나눔
  - I/O-bound process
    - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
    - (many short CPU bursts)
  - CPU-bound process
    - 계산 위주의 job
    - (few very long CPU bursts.)



### CPU Scheduler & Dispatcher

- CPU Scheduler

  - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다
  - 운영체제 안에서 CPU 스케줄링을 하는 커널 코드

- Dispatcher

  - CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다
  - 이 과정을 context switch(문맥 교환)라고 한다

- CPU 스케줄링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다

  1. Running -> Blocked (예: I/O 요청하는 시스템 콜) : CPU가 어떤 프로세스를 잡고 있다가 오래 걸리는 작업을 하러 간 경우, 자진해서 CPU를 내어 놓아서 다른 프로세스에게 CPU가 넘어감
  2. Running -> Ready (예: 할당시간말료로 timer interrupt) : CPU를 강제로 빼앗아서 원래 있던 애를 줄서게 하고 다른 프로세스에게 CPU를 넘겨줌
  3. Blocked -> Ready (예: I/O 완료후 인터럽트) : 오래걸리는 작업이 끝나면 CPU를 얻을 수 있는 권한을 줌. 바로 CPU를 주지는 않고 Ready 상태로 보내는 것. 하지만 곧바로 넘겨야되는 경우도 있음. 
  4. Terminate : 프로세스가 종료되어서 다른 프로세스에게 넘겨야하는 경우

  - 비선점형 : 1, 4에서의 스케줄링은 nonpreemptive (=강제로 빼앗지 않고 자진 반납)
  - 선점형 : All othef scheduling is preemptive (=강제로 빼앗음)



### Scheduling Criteria

> Performance Index (= Perfomance Measure, 성능 척도)

- CPU utilization (이용률)
  - keep the CPU as busy as possible (CPU 바쁘게 일을 시켜라)
- Throughout (처리량) - 프로세스 입장에서의 CPU 성능 척도
  - \# of processes that complete their execution per time unit
- Turnaround time (소요시간, 반환시간) - 프로세스 입장에서의 CPU 성능 척도
  - amount of time to execute a particular process (CPU를 쓰러 들어와서 다 쓰고 I/O로 나갈 때까지의 걸린 시간)
- Waiting time (대기 시간) - 프로세스 입장에서의 CPU 성능 척도
  - amount of time a process has been waiting in the ready queue (ready queue에서 기다린 시간-선점형 스케줄링의 경우, CPU를 얻었다가 뺏겨서 다시 줄 서서 기다릴 수 있음. 이를 모두 합친 시간)
- Response time (응답 시간)
  - amount of time it takes from when a request was submitted until the first response is produced, not output (for time-sharing environment) (ready queue에 들어와서 처음으로 CPU를 얻기까지 걸린 시간-처음으로 얻을 때까지 기다린 시간)



### FCFS (Fisrt-Come First-Served) - 비선점형 스케줄링

- Example: 

  - Process P1 P2 P3
  - Busrt Time 24 3 3

- 프로세스의 도착 순서 P1, P2, P3

  스케줄 순서를 Gantt Chart로 나타내면 다음과 같다

  ![The Gantt chart for the schedule 1](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/The Gantt chart for the schedule 1.JPG)

- Waiting time for P1 = 0; P2 = 24; P3 = 27

- Average waiting time : (0+24+27)/3 = 17

- 그렇게 좋은 스케줄링 방법이 아님

  - 똑같이 0초에 도착했을 때, CPU 사용시간이 긴 프로세스가 먼저 도착하면 오래 기다려야 함.

- 프로세스의 도착 순서가 다음과 같다고 할 때,

  P2, P3, P1

  - The Gantt chart for the schedule is:

    ![The Gantt chart for the schedule 2](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/The Gantt chart for the schedule 2.JPG)

  - wating time for P1 = 6; P2 = 0; P3 = 3

  - Average waiting time: (66+0+3)/3 = 3

  - Much better than previous case.

  - **Convoy effect**: short process behind long process 큐에서 오래 기다리는 현상을 의미



### SJF(Shortest-Job-First)

- 각 프로세스의 다음번 CPU burst time을 가지고 스케줄링에 활용

- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄
- Two schemes:
  - Nonpreemptive
    - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점(preemption) 당하지 않음
  - Preemptive
    - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김
    - 이 방법을 Shortest-Remaining-Time-Fisrt(SRTF) 이라고도 부른다
- SJF is optimal
  - 주어진 프로세스들에 대해 minimum average waiting time을 보장 - preemptive 버전



### Example of Non-Preemptive SJF

- SJF (non-preemptive)

  ![SJF (C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/SJF (non-preemptive).JPG)](os_05_CPU Scheduling.assets/SJF (non-preemptive).JPG)

- Average waiting time = (0+6+3+7)/4=4



### Example of Preemptive SJF

- SJF (preemptive)

  - 더 짧은 시간이 도착하면 빼앗길 수 있음

  ![SJF (C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/SJF (preemptive).JPG)](os_05_CPU Scheduling.assets/SJF (preemptive).JPG)

- Average waiting time = (9+1+0+2)/4=3 (optimal(minimum))

- Non-preemptive 버전보다 웨이팅 시간이 짧음



### Priority Scheduling

- A priority number (integer) is associated with each process

- highest priority를 가진 프로세스에게 CPU 할당

  (smallest integer = highest priority). (작은 숫자가 우선순위가 높음)

  - Preemptive
  - nonpreemptive

- SJF는 일종의 priority scheduling이다

  priority = predicted next CPU burst time

- Problem

  - Starvation(기아 현상) : low priority processes may never execute.

    우선순위가 낮은 프로세스가 지나치게 오래 기다려서 영원히 CPU를 얻지 못할 수도 있다는 문제점이 있음

- Solution

  - Aging(노화) : as time progresses increase the priority of the process.

    오래 기다리면 우선순위를 조금씩 높여주자는 의미



### 다음 CPU Busrt Time의 예측

- 다음번 CPU burst time을 어떻게 알 수 있는가? (input data, branch, user ...)

- 추정(estimate) 만이 가능하다

- 과거의 CPU burst time을 이용해서 추정

  (exponential averaging)

  ![(C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/(exponential averaging).JPG)](os_05_CPU Scheduling.assets/(exponential averaging).JPG)

  - n+1 번째 CPU 사용 예측 시간은 n번째 실제 CPU 사용시간과 n번째 실제 CPU 예측 시간을 일정 비율 곱해서 더해준 값.



### Exponential Averaging

![Exponential Averaging](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/Exponential Averaging.JPG)

- a와 (1-a)가 둘 다 1 이하이므로 후속 term은 선행 term보다 적은 가중치 값을 가진다



### Round Robin (RR)

- 각 프로세스는 동일한 크기의 할당 시간(**time quantum**)을 가짐 (일반적으로 10-100 milliseconds)

- 할당 시간이 지나면 프로세스는 선점(preempted)당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다

- n개의 프로세스가 ready queue에 있고 할당 시간이 **q time unit**인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.

  => 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.

  - 응답시간이 빨라진다는 것이 RR의 장점

- Performance

  - q large => FCFS
  - q small => context switch 오버헤드가 커진다 (q를 너무 작게 주면 오버헤드가 커져서 시스템 성능이 나빠지는 문제 발생)
    - 적당한 규모의 time quantum을 주는 것이 바람직 (10-100 milliseconds)



### Example: RR with Time Quantum = 20

​		![RR with Time Quantum = 20](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/RR with Time Quantum = 20.JPG)

- 일반적으로 SJF보다 average turnaround time이 길지만 **response time**은 더 짧다.



### Turnaround Time Varies With Time Quantum

![Turnaround Time Varies With Time Quantum](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/Turnaround Time Varies With Time Quantum.JPG)



### Multilevel Queue

![Multilevel Queue](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/Multilevel Queue.JPG)

system processes : 시스템 프로세스

interactive processes : 사람과 인터렉션하는 프로세스

interactive editing processes

batch processes : CPU만 오랫동안 사용하는 job

student processes

=> 우선순위는 변하지 않음.



### Multilevel Queue

- Ready Queue를 여러 개로 분할
  - Foreground(Interactive) - RR
  - Background(batch - no human Interactive) - FCFS
- 각 큐는 독립적인 스케줄링 알고리즘을 가짐
  - Foreground - RR
  - Background- FCFS
- 큐에 대한 스케줄링이 필요
  - Fixed Priority Scheduling
    - serve all from foreground then from background.
    - Possibility of starvation.
  - Time Slice 
    - 각 큐에 CPU Time 적절한 비율로 할당 
    - ex) 80% to Foreground in RR, 20% to Background in FCFS
- 우선순위는 변하지 않기 때문에, 높은 것에만 주면 낮은 프로세스는 starvation의 문제가 생김. 여러 줄로 줄서기를 하면서 경우에 따라서 줄 간에 이동을 할 수 있는 스케줄링을 할 수 있음.



### MultiLevel FeedBack Queue

- 프로세스가 다른 큐로 이동 가능.
- 에이징(Aging)을 이와 같은 방식으로 구현할 수 있다.
- MultiLevel-FeedBack-Queue Scheduler를 정의하는 파라미터들
  - Queue의 수
  - 각 큐의 Scheduling Algorithm
  - Process를 상위 큐로 보내는 기준
  - Process를 하위 큐로 내쫓는 기준
  - 프로세스가 CPU 서비스를 받으려 할때, 들어갈 큐를 결정하는 기준



### Multilevel Feedback Queue

![image-20220509153849698](C:/Users/YoonJuhye/TIL/cs/os/os_05_CPU Scheduling.assets/image-20220509153849698.png)

- CPU burst가 짧은 프로세스에게 우선순위를 더 많이 주고 긴 프로세스는 점점 밑으로 쫓겨나서 우선순위를 낮춰주는 방법
- 처음에 들어온 프로세스에게는 무조건 짧은 시간을 주기 때문에, CPU 사용 시간이 긴지 짧은지의 예측이 필요 없음. 



### Example of Multilevel Feedback Queue

- Three queues:
  - Q0 - time quantum 8 milliseconds
  - Q1 - time quantum 16 milliseconds
  - Q2 - FCFS
- Scheduling
  - new job이 queue Q0로 들어감
  - CPU를 잡아서 할당 시간 8 milliseconds 동안 수행됨
  - 8 milliseconds 동안 다 끝내지 못했으면 queue Q1으로 내려감
  - Q1에 줄서서 기다렸다가 CPU를 잡아서 16 ms 동안 수행됨
  - 16 ms에 끝내지 못한 경우 queue Q2로 쫓겨남



### Multiple-Processor Scheduling

- CPU가 여러 개인 경우 스케줄링은 더욱 복잡해짐.
- Homogeneous Processor인 경우
  - Queue에 한 줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
  - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐.

- Load Sharing
  - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘이 필요
  - 별개의 큐를 두는 방법 vs. 공동 큐를 사용하는 방법

- Symmentic Multiprocessing(SMP)
  - 각 프로세서가 각자 알아서 스케줄링 결정 (모든 CPU가 대등)

- Asymmentic Multiprocessing
  - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름. (하나의 CPU가 전체적인 컨트롤을 담당)\



### Real-Time Scheduling

- Hard real-time Systems
  - Hard real-time task는 정해진 시간 안에 반드시 끝내도록 스케줄링 해야 함

- Soft real-time Systems
  - Soft real-time task는 일반 프로세스에 비해 높은 Priority를 갖도록 해야 함 



### Thread Scheduling

- Local Scheduling
  - User Level Thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케줄할지 결정 (사용자 프로세스가 직접 thread를 관리하고 운영체제는 thread를 모름-그 프로세스에게 CPU를 줄지 안줄지 프로세스 내부에서 결정)
- Global Scheduling
  - Kernel level thread의 경우 일반 프로세스와 마찬가지로 커널의 단기 스케줄러가 어떤 thread를 스케줄할지 결정 (운영체제가 thread를 이미 알고 있는 상황-운영체제가 결정)



### Algorithm Evaluation

- Queueing Models

  - 확률 분포로 주어지는 arrival rate(도착률)와 service rate(처리율) 등을 통해 각종 performance index 값을 계산 (이론적)

- Implementation (구현) & Measurement(성능 측정)

  - 실제 시스템에 알고리즘을 구현하여 실제 작업(workload)에 대해 성능을 측정 비교 

    (실제 시스템에 구현해서 돌려보고 성능 측정해 봄(실측))

- Simulation(모의 실험)

  - 알고리즘을 모의 프로그램으로 작성 후 trace를 입력으로 하여 결과를 비교 

    (trace : 시뮬레이션 프로그램에 인풋으로 들어갈 데이터.)

---

# Chapter 6. Process Synchronization

### 데이터의 접근

![image-20220509162106291](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509162106291.png)

- 데이터가 저장된 위치에서 데이터를 읽어와서 연산을 한 후 그 결과를 저장된 위치에 다시 저장

- 누가 먼저 읽어왔느냐에 따라서 결과가 달라질 수 있음. 그렇게해서 생기는 문제 = Synchronization



### Race Condition

![image-20220509162236441](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509162236441.png)

- 여러 주체가 하나의 데이터를 동시에 접근하려고 할 때를 Race Condition(경쟁상태)라고 함

- 이를 조율해주는 방법이 필요할 것

- 프로세스는 일반적인 경우라면 자기 주소공간만 접근하기 때문에  Race Condition 문제가 발생하는 경우가 없지만 본인이 직접 실행할 수 없는 부분, 운영체제에게 대신 요청해야하는 경우엔 시스템콜을 해서 커널의 코드가 실행됨. CPU를 뺏겨서 또 시스템콜을 하면  커널의 코드를 건드리기 때문에 Race Condition의 문제가 발생할 수 있음.



### OS에서 race condition은 언제 발생하는가?

1. kernel 수행 중 인터럽트 발생 시
2. Process가 system call을 하여 kernel mode로 수행 중인데 context switch가 일어나는 경우
3. Multiprocessor에서 shared memory 내의 kernel data



### OS에서의 race condition - interrupt handler v.s. kernel

![image-20220509170624311](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509170624311.png)

- 커널모드 running 중 interrupt가 발생하여 인터럽트 처리루틴이 수행

  => 양쪽 다 커널 코드이므로 kernel address space 공유



### If you preempt CPU while in kernel mode ...

![image-20220509171012229](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509171012229.png)

- 해결책 : 커널 모드에서 수행 중일 때는 CPU를 preempt 하지 않음, 커널 모드에서 사용자 모드로 돌아갈 때 preempt



### OS에서의 race condition - multiprocessor

![image-20220509171438110](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509171438110.png)

- 자주 등장하지는 않음
- 어떤 CPU가 마지막으로 count를 store했는가? -> race condition
- multiprocessor의 경우 interrupt enable/disable로 해결되지 않음
- (방법 1) 한번에 하나의 CPU만이 커널에 들어갈 수 있게 하는 방법 (비효율적아게 됨)
- (방법 2) 커널 내부에 있는 각 공유 데이터에 접근할 때마다 그 데이터에 대한 lock(다른 CPU가 접근할 수 없게 함) / unlock을 하는 방법 - 이 방식이 더 좋음



### Process Synchronization 문제

- 공유 데이터(shared data)의 동시 접근(concurrent access)은 데이터의 불일치 문제 (inconsistency)를 발생시킬 수 있다
- 일관성 (consistency) 유지를 위해서는 협력 프로세스 (cooperating process)간의 실행 순서 (orderly execution)를 정해주는 메커니즘 필요
- Race condition
  - 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황
  - 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐
- race condition을 막기 위해서는 concurrent process는 동기화(synchronize)되어야 한다



### Example of a Race Condition

![image-20220509171959901](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509171959901.png)

- 사용자 프로세스 P1 수행중 timer interrupt가 발생해서 context switch가 일어나서 P2가 CPU를 잡으면?
- 두 개의 프로세스가 하나의 공유 데이터에 접근해서 P1은 증가시키고 P2는 감소시키는 경우에, consistency가 깨어지는 불일치 문제 발생



### The Critical-Section Problem

- n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우

- 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 critical section (임계 구역)이 존재

- Problem

  - 하나의 프로세스가 critical section에 있을 때 다른 모든 프로세스는 critical section에 들어갈 수 없어야 한다

    ![image-20220509173246559](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509173246559.png)



### 프로그램적 해결법의 충족 조건

- Mutual Exclusion (상호 배제)
  - 프로세스 Pi가 Critical Section 부분을 수행 중이면 다른 모든 프로세스들은 그들의 Critical Section에 들어가면 안된다
  - 배타적으로 접근해야 한다.

- Progress (진행)
  - 아무도 Critical Section에 있지 않은 상태에서 Critical Section에 들어가고자 하는 프로세스가 있으면 Critical Section에 들어가게 해줘야 한다
  - 둘 다 없는데 아무도 못들어가게 막는 경우가 있기 때문.

- Bounded Waiting (유한 대기)
  - 프로세스가 Critical Section에 들어가려고 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 Critical Section에 들어가는 횟수에 한계가 있어야 한다 
  - 기다리는 시간이 유한해야 한다. 특정 프로세스 입장에서 지나치게 오래 기다리면 안된다. (Starvation이 생기면 안된다)

- 가정
  - 모든 프로세스의 수행 속도는 0보다 크다
  - 프로세스들 간의 상대적인 수행 속도는 가정하지 않는다



### Initial Attempts to Solve Problem

- 두 개의 프로세스가 있다고 가정 P0, P1

- 프로세스들의 일반적인 구조

  ```c
  do {
      entry section /* 락을 걸어서 여러 프로세스가 critical section에 들어가는 것을 막음 */
      critical section
      exit section
      remainder section
  } while (1);
  ```

- 프로세스들은 수행의 동기화(synchronize)를 위해 몇몇 변수를 공유할 수 있다 -> synchronization variable



### Algorithm 1

- Synchronization variable

  int turn;

  initially turn = 0; => Pi can enter its critical section if (turn == i)

- Process P0

  ```c
  do {
      while (turn != 0);  /* My turn? */
      critical section
      turn = 1;           /* Now it's your turn */
      remainder section
  } while (1);
  ```

  - 본인 차례가 아닌 동안 while문을 돌면서 기다리고 차례가 되면 critical section에 들어감. 상대방이 빠져나가면 turn이 돌아서 자기 차례가 되어 들어감

  Satisfies mutual exclusion, but not progress

  즉, 과잉양보: **반드시 한번씩 교대로 들어가야만 함** (swap-turn)

  그가 turn을 내값으로 바꿔줘야만 내가 들어갈 수 있음

  특정 프로세스가 더 빈번히 critical section을 들어가야 한다면?

  - 상대방이 turn을 바꿔주지 않기 때문에 영원히 못 들어가게 됨. (progress 조건을 만족하지 못함)



### Algorithm 2

- Synchronization variables

  - boolean flag[2];

    initially flag[모두] = false; /* no one is in CS */

  - "Pi ready to enter its critical section" if (flag [i] == true)

- Process Pi

  ```c
  do {
      flag[i]=true;  /* Pretend I am in 들어간다는 의사 표시*/
      while (flag[i]); /* Is he also in? then wait 상대방도 의사표시 하고 있다면 기다림*/
      critical section
     	flag[i] = false; /* I am out now 상대방이 기다리고 있으면 들어갈 수 있게 표시*/
      remainder section
  } while (1);
  ```

- Satisfies mutual exclusion, but not progress requirement.

- 둘 다 2행까지 수행 후 끊임 없이 양보하는 상황 발생 가능 (둘다 True인 상태이고, 일단 들어가야 깃발을 내려놓을 수가 있어짐 아무도 못 들어감)



### Algorithm 3 (Peterson's Algorithm)

- Combined synchronization variables of algorithms 1 and 2.

- Process Pi

  ```c
  do {
          flag[i] = true;  /* My intention is to enter ...깃발 들어서 의사표시 */
      	turn = j;        /* Set to his turn turn을 상대방에게 바꿔놓음 */
          while (flag[i] && turn == j); /* wait only if ... 상대방이 깃발 들고, 상대방 차례면 기다림*/
          critical section
          flag[i] = false; 
          remainder section
  	} while (1);
  ```

- Meets all three requirements; solves the critical section problem for two processes.(세 가지 조건을 모두 만족하는 코드임)

- Busy Waiting! (=spin lock) (계속 CPU와 memory를 쓰면서 wait) - 이미 들어가 있는 상태에서 CPU를 잡으면 할당 시간 동안까지 계속 체크함. 상대방을 체크하지 않고 쓸데 없이 본인의 할당 시간을 while문에서 소비하고 반납하게 됨. (비효율적인 방법)



### Synchronization Hardware

- 하드웨어적으로 Test & modify를 atomic하게 수행할 수 있도록 지원하는 경우 앞의 문제는 간단히 해결

  ![image-20220509202646474](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509202646474.png)

  - a가 원래 0이었다면 0이 읽히고 1로 바뀜
  - Test_and_set(a)는 읽고 바꾸는 것을 atomic하게 수행함

- Mutual Exclusion with Test & Set

  ```c
  Synchronization variable:
  	boolean lock = false;
  Process Pi
      	do {
              while (Test_and_Set(lock));
              critical section
              lock = false;
              remainder section
          }
  ```

  

### Semaphores

- 앞의 방식들을 추상화시킴

- 공유자원을 획득하고 반납하는 것을 처리해줌

- Semaphore S

  - integer variable

  - 아래의 두 가지 atomic 연산에 의해서만 접근 가능

    ```c
    P(S): 	while (S<=0) do no-op; /* -> i.e. wait */
    		S--;
    ```

    - 공유자원을 획득하는 과정 (lock) - 자원이 있으면 가져가고 없으면 while문에서 대기

    if positive, decrement-&-enter.

    Otherwise, wait until positive (busy-wait)

    ```c
    V(S):
    		S++;
    ```

    - 반납하는 과정 (un-lock)



### Critical Section of n Processes

```c
Synchronization variable:
semaphore mutex; /* initially 1: 1개가 CS에 들어갈 수 있다 */

Process Pi
do {
	P(mutex); /* if positive, dec-&-enter, Otherwise, wait. */
	critical section
    V(mutex); /*Increment semaphore */
    remainder section
    }
```

busy-wait은 효율적이지 못함(=spin lock)

Block & Wakeup 방식의 구현 (=sleep lock)



### Block / Wakeup Implementation

- Semaphore를 다음과 같이 정의

  ```c
  typedef struct
  {	int value; /* semaphore */
  	struct process *L; /* process wait queue */
  } semaphore;
  ```

- block과 wakeup을 다음과 같이 가정

  - block 

    - 커널은 block을 호출한 프로세스를 suspend 시킴
    - 이 프로세스의 PCB를 semaphore에 대한 wait queue에 넣음

  - wakeup(P)

    - block된 프로세스 P를 wakeup 시킴

    - 이 프로세스의 PCB를 ready queue로 옮김

      ![image-20220509204853902](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509204853902.png)



### Implementation - Block / Wakeup version of P() & V()

- Semaphore 연산이 이제 다음과 같이 정의됨

  ```c
  P(S): 	S.value--; /* prepare to enter */
  		if (S.value < 0){ /* Oops, negative, I cannot enter 자원의 여분이 없음 */
              	add this process P to S.L;
              	block(); /* block되어 있다가 자원이 생기면 깨어날 수 있음 */
          }
  ```

  ```c
  V(S):	S.value++;
  		if (S.value <= 0){
              	remove a process P from S.L;
              	wakeup(P); /* 잠들어 있는 연산이 있으면 깨워줌 */
          }
  ```

  

### Which is better?

- Busy-wait v.s. Block/wakeup
- Block/wakeup overhead v.s. Critical section 길이
  - Critical section의 길이가 긴 경우 Block/Wakeup이 적당
  - Critical section의 길이가 매우 짧은 경우 Block/Wakeup 오버헤드가 busy-wait 오버헤드보다 더 커질 수 있음
  - 일반적으로는 Block/Wakeup 방식이 더 좋음



### Deadlock and Starvation

- Deadlock

  - 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상

- S와 Q가 1로 초기화된 semaphore라 하자.

  ![image-20220509210152437](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509210152437.png)

  - S와 Q는 배타적. Q를 쓴 이후에 S를 내놓는 데 상대방을 기다리면서 영원히 자기 것은 내어놓지 않음

  ![image-20220509210356661](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509210356661.png)

  - 자원을 획득하는 순서를 똑같이 맞춰주면 데드락 해결 가능

- Starvation

  - indefinite blocking. 프로세스가 suspend된 이유에 해당하는 세마포어 큐에서 빠져나갈 수 없는 현상
  - 자원을 얻지 못하고 무한히 기다리는 것 (데드락도 일종의 스타베이션으로 볼 수 있음)



### Classical Problems of Synchronization

- Bounded-Buffer Problem (Producer-Consumer Problem)
- Readers and Writers Problem
- Dining-Philosophers Problem



### Bounded-Buffer Problem (Producer-Consumer Problem)

- 임시로 데이터를 저장하는 공간(Buffer)이 유한한 환경에서의 생산자-소비자 문제
- Producer 프로세스, Consumer 프로세스가 여러개 존재
- 생산자 : 공유 버퍼에 데이터를 하나 만들어서 집어 넣는 역할(주황색: 데이터가 들어있는 버퍼, 나머지는 비어있는 버퍼)
- 소비자 : 데이터를 하나씩 꺼내 쓰는 역할

- 버퍼가 유한하기 때문에 생기는 문제: 생산자 입장에서는 사용할 자원이 없는 것이 됨. 소비자가 나타나서 내용을 꺼내가야 빈 버퍼가 생길 때가지 생산자는 기다려야 함. 소비자 입장에서는 꺼내갈 것이 없어서, 생산자가 내용을 만들어서 넣어줄 때까지 기다려야 함
- 둘이 동시에 공유 버퍼에 접근하는 것을 막기 위해 lock을 걸어 배타적 접근을 하도록 함. 가용 자원을 세는 변수가 필요함

![image-20220509211404469](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509211404469.png)

- Shared data
  - buffer 자체 및 buffer 조작 변수(empty/full buffer의 시작 위치)
- Synchronization variables
  - mutual exclusion -> Need binary semaphore (shared data의 mutual exclusion을 위해)
  - resource count -> Need integer semaphore (남은 full/empty buffer의 수 표시)



### Bounded-Buffer Problem

![image-20220509212040370](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509212040370.png)

- 공유 버퍼: n개



### Reader-Writers Problem

- 한 프로세스가 db에 write 중일 때 다른 process가 접근하면 안됨
- read는 동시에 여럿이 해도 됨
- Solution
  - Writer가 db에 접근 허가를 아직 얻지 못한 상태에서는 모든 대기중인 reader들을 다 db에 접근하게 해준다
  - Writer는 대기중인 reader가 하나도 없을 때 db접근이 허용된다
  - 일단 writer가 db에 접근중이면 reader들은 접근이 금지된다
  - Writer가 db에서 빠져나가야만 reader의 접근이 허용된다

- Shared data
  - DB 자체
  - readcount; /* 현재 DB에 접근 중인 Reader의 수 */

- Synchronization variables
  - mutex /* 공유 변수 readcount를 접근하는 코드(critical section)의 mutual exclusion 보장을 위해 사용 */
  - db /* Reader와 writer가 공유 DB 자체를 올바르게 접근하게 하는 역할 */



![image-20220509213005761](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509213005761.png)



### Dining-Philosophers Problem

![image-20220509210552293](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509210552293.png)

- 다섯명의 철학자가 생각하는 시간과 밥 먹는 시간이 다름. 젓가락 두 개를 다 잡아야 함.

- 공유 자원이기 때문에 옆에 있는 철학자가 잡고 있으면 못 잡고 기다려야 됨.

- 앞의 solution의 문제점

  - Deadlock의 가능성이 있다
  - 모든 철학자가 동시에 배가 고파져 왼쪽 젓가락을 집어버린 경우

- 해결 방안

  - 4명의 철학자만이 테이블에 동시에 앉을 수 있도록 한다
  - 젓가락을 두 개 모두 집을 수 있을 때에만 젓가락을 집을 수 있게 한다
  - 비대칭
    - 짝수(홀수) 철학자는 왼쪽(오른쪽) 젓가락부터 집도록

  ![image-20220509213846593](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509213846593.png)



### Monitor

- Semaphore의 문제점

  - 코딩하기 힘들다
  - 정확성 (correctness)의 입증이 어렵다
  - 자발적 협력(voluntary cooperation)이 필요하다
  - 한 번의 실수가 모든 시스템에 치명적 영향

- 예

  ![image-20220509214124669](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509214124669.png)

- 동시 수행중인 프로세스 사이에서 abstract data type의 안전한 공유를 보장하기 위한 high-level synchronization construct

  ![image-20220509214339846](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509214339846.png)



- 모니터 내에서는 한번에 하나의 프로세스만이 활동 가능

- 프로그래머가 동기화 제약 조건을 명시적으로 코딩할 필요가 없음

- 프로세스가 모니터 안에서 기다릴 수 있도록 하기 위해 condition variable 사용

  condition x, y;

- Condition variable 은 wait과 signal연산에 의해서만 접근 가능.

  x.wait();

  x.wait()을 invoke한 프로세스는 다른 프로세스가 x.signal()을 invoke 하기 전까지 suspend된다

  x.signal();

  x.signal()은 정확하게 하나의 suspend된 프로세스를 resume한다.

  Suspend된 프로세스가 없으면 아무 일도 일어나지 않는다

![image-20220509214652133](C:\Users\YoonJuhye\TIL\cs\os\os_06_Process Synchronization.assets\image-20220509214652133.png)



### Bounded-Buffer Problem

![image-20220509214554409](os_06_Process Synchronization.assets/image-20220509214554409.png)