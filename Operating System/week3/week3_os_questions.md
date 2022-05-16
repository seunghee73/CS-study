# ✨ week3_os_questions ✨

#### ✏ 사용자 프로세스 영역의 물리적 메모리 할당 방법 2가지를 간략하게 설명하시오

- 연속할당은 각각의 프로세스가 메모리의 연속적인 공간에 적재도록 하는 것이고 불연속 할당은 하나의 프로세스가 메모리의 여러 영역에 분산되어 올라가도록 하는 것이다.

<br>

#### ✏ Paging 기법에서 속도 향상을 위해 쓰는 방법 2가지에 대해서 설명하시오.

- associative register: pararell search
- TLB (translation look-aside buffer) 
  - 빈번히 접근하는 주소를 TLB에 저장 
  - 주소 변환하기 전에 TLB에 있다면 TLB에서 찾고, 없다면 page table에 접근


<br>

#### ✏ CPU가 보는 주소는?

-  logical address

<br>

#### ✏ Deadlock 발생의 4가지 조건을 설명해주세요.

- \* Mutual exclusion(상호 배제) * No preemption(비선점) * Hold and wait(보유 대기) * Circular wait(순환 대기)

<br>

#### ✏ 주소변환을 하는데 os의 역할은?

- 없음(전부 하드웨어가 함)

<br>

#### ✏ Paging system에서 protection의 의미

- 다른 프로세스가 테이블을 아예 못 보게 하는 것이 아니다
- read, write, read-only 등의 접근 권한을 부여

<br>

#### ✏ two-level page table을 사용하는 이유는?

- 주소 공간을 줄이기 위해

<br>

#### ✏ 가변 분할 방식에서는 Hole이 발생합니다. 사용 중인 메모리 영역을 한군데로 몰고 hole들을 다른 한 곳으로 몰아 큰 block을 만드는 것을 무엇이라고 할까요?

- Compaction

<br>

#### ✏ segmentation이 paging보다 나은 점

- segment는 의미 단위이기때문에 공유와 보안에 있어 paging보다 우수함

<br>

#### ✏ 물리적 메모리의 연속 할당을 할 때 가변 분할 방식의 문제점은 무엇일까요?

- 외부 단편화가 발생할 수 있다.

<br>

#### ✏ Deadlock avoidance에서 자원당 단일 인스턴스에서 사용되는 알고리즘은?

- 자원 할당 그래프 알고리즘

<br>

#### ✏ Dynamic Loading과 Overlays의 차이점?

- Dynamic Loading의 경우에는 OS의 지원이 되어 상대적으로 간편하게 구현이 가능하지만 Overlays는 수작업으로 구현해야하기 때문에 복잡


<br>

#### ✏ 데드락 처리 방법 중 Deadlock Avoidance 에서 Multiple instance per resource types일 때 사용하는 알고리즘은? 간략한 설명

- Banker's Algorithm
- 총 요청 자원의 수가 가용 자원의 수보다 적은 프로세스를 선택하는 알고리즘

<br>

#### ✏ 페이징기법과 세그먼트 기법 중 테이블을 위한 메모리 낭비가 심한 것은?

- 페이징 기법

<br>

#### ✏ MMU(memory management unit)는 왜 필요하고, 어떤 register로 구성되어 있나?

- runtime binding에서는 프로그램 수행 중 계속해서 메모리 위치가 변한다. 그렇기 때문에 지속적으로 맨 처음 binding한 시점 이후에도, 계속해서 메모리 상의 주소를 확인하고 변환해주는 기술이 필요하다. 이러한 주소 변환은 하드웨어의 도움을 받게 되는데, MMU가 대표적인 하드웨어 장치이다.
- MMU는 limit register와 base register(relocation register) 두 개로 구성된다.

<br>

#### ✏ 가변 분할 방식에서 size가 n 이상인 것 중 최초로 찾아지는 hole에 할당하는 것을 무엇이라고 하나요?

- first fit

<br>

#### ✏ Inverted Page Table 나오게 된 배경

- 모든 프로세스 별로 그 논리적 주소에 대응하는 모든 page에 대해 page table entry가 존재하기 때문에, 대응하는 page가 메모리에 있든 없든 간에 page table에는 entry로 존재하는데 이를 막기 위한 취지에서 Inverted Page Table이 나옴

<br>

#### ✏ 데드락 처리 방법 중에, 데드락을 방지하기 위해서 미리 어떤 것을 하지 않고 데드락이 생기면 처리하는 방식 두 가지는?

- Deadlock Detection and recovery, Deadlock Ignorance

<br>

#### ✏ 페이지 테이블 내 각 entry마다 주소 변환 정보 외에 저장하는 부가적인 bit 두 가지

- protection bit
- valid/invalid bit

<br>

#### ✏ 자원할당 그래프를 왜 그릴까요?

- 데드락이 발생했는지 알아보기 위해서


<br>

#### ✏ Deadlock 예방 방법에 대해 설명해주세요

- 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것

<br>