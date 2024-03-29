## 3.1 운영체제와 컴퓨터



#### 운영체제의 역할과 구조

- 운영체제의 역할
  
  - CPU 스케줄링과 프로세스 관리
  
  - 메모리 관리
  
  - 디스크 파일 관리
  
  - I/O 디바이스 관리



- 운영체제의 구조
  
  - 유저 프로그램 > (GUI > 시스템콜 > 커널 > 드라이버 : 운영체제) >하드웨어
  
  - GUI : 사용자가 전자장치와 상호 작용할 수 있도록 하는 사용자 인터페이스의 한 형태
  
  - 드라이버 : 하드웨어를 제어하기 위한 소프트웨어
  
  - CUI : 그래픽이 아닌 명령어로 처리하는 인터페이스



- 시스템콜
  
  - 운영체제가 커널에 접근하기 위한 인터페이스
  
  - 유저 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출할 때 사용
  
  - I/O 요청으로 트랩이 발동하면 올바른 I/O 요청인지 확인한 후 유저 모드가 시스템콜을 통해 커널 모드로 변환되어 실행
  
  - modebit를 참고해서 유저 모드와 커널 모드 구분, 유저 모드인 경우에는 시스템콜을 못하게 막아서 한정된 일만 가능



- 커널
  
  - 운영체제의 핵심 부분이자 시스템콜 인터페이스를 제공하며 보안, 메모리, 프로세스, 파일 시스템, I/O 디바이스, I/O 요청 관리 등 운영체제의 중추적인 역할



#### 컴퓨터의 요소

- CPU(Central Processing Unit)
  
  - 산술논리연산장치, 제어장치, 레지스터로 구성되어 있는 컴퓨터 장치
  
  - interrupt에 의해 단순 메모리에 존재하는 명령어를 해석해서 실행
  
  - interrupt : 어떤 신호가 들어왔을 때 CPU를 잠시 정지시키는 것
    
    - hardware interrupt : 키보드나 마우스를 통해 발생하는 interrupt
    
    - software interrupt : trap이라고도 한다. 프로세스 오류 등으로 프로세스가 시스템콜을 호출할 때 발동한다.

- 제어 장치(CU, Control Unit)
  
  - 프로세스 조작을 지시하는 CPU의 한 부품
  
  - 입출력 장치 간 통신을 제어하고 명령어들을 읽고 해석하며 데이터 처리를 위한 순서 결정

- 레지스터
  
  - CPU 안에 있는 매우 빠른 임시기억장치

- 산술논리연산장치(ALU, Arithmetic Logic Unit)
  
  - 산술 연산과 배타적 논리합, 논리곱과 같은 논리 연산을 계산하는 디지털 회로

- CPU의 연산처리
  
  1. 제어장치가 메모리에 계산할 값을 로드, 레지스터에도 로드
  
  2. 제어장치가 레지스터에 있는 값을 계산하라고 산술논리연산장치에 명령
  
  3. 제어장치가 계산된 값을 다시 '레지스터에서 메모리로' 계산한 값을 저장

- DMA 컨트롤러
  
  - I/O 디바이스가 메모리에 직접 접근할 수 있도록 하는 하드웨어
  
  - CPU 부하를 막아주며 CPU의 일을 보조
  
  - 하나의 작업을 CPU와 DMA 컨트롤러가 동시에 하는 것을 방지

- 메모리
  
  - 테이터나 상태, 명령어 등을 기록하는 장치
  
  - RAM(Random Access Memory)를 일컬어 메모리라고도 한다.
  
  - CPU는 계산을 담당하고 메모리는 기억을 담당

- 타이머
  
  - 몇 초 안에 작업이 끝나야 한다는 것을 정하고 특정 프로그램에 시간 제한을 다는 역할

- 디바이스 컨트롤러
  
  - 컴퓨터와 연결되어 있는 IO 디바이스들의 작은 CPU



#### 메모리

- 메모리 계층
  
  - (속도가 빠름, 용량이 작음) 레지스터 > 캐시 > 메모리 > 저장장치 (속도가 느림, 용량이 큼)
  
  - 레지스터 : CPU 안에 있는 작은 메모리, 휘발성, 속도 가장 빠름, 기억 용량이 가장 작음
  
  - 캐시 : 휘발성, 속도 빠름, 기억 용량이 작음
  
  - 주기억장치 : RAM, 휘발성, 속도 보통, 기억 용량 보통
  
  - 보조기억장치 : HDD, SDD를 일컬으며 휘발성, 속도 낮음, 기억 용량 많음

- 캐시
  
  - 데이터를 미리 복사해 놓는 임시 저장소이자 빠른 장치와 느린 장치에서 속도 차이에 따른 병목 현상을 줄이기 위한 메모리
  
  - 메모리와 CPU 사이의 속도 차이가 크기 때문에 그 중간에 레지스터 계층을 둬서 해결
  
  - 캐싱 계층 : 속도 차이를 해결하기 위해 계층과 계층 사이에 있는 계층
  
  - 캐시 계층 외에는 **자주 사용하는데이터**를 기반으로 캐시를 설정
    
    - 시간 지역성 : 최근 사용한 데이터에 다시 접근하려는 특성
    
    - 공간 지역성 : 최근 접근한 데이터를 이루고 있는 공간이나 그 가까운 공간에 접근하는 특성
  
  - 캐시히트 : 원하는 데이터를 찾은 것
  
  - 캐시미스 : 해당 데이터가 캐시에 없어 주 메모리로 가 데이터를 찾아오는 것
  
  - 캐시매핑 : 캐시가 히트되기 위해 매핑하는 방법
    
    - 직접 매핑 
    
    - 연관 매핑
    
    - 집합 연관 매핑
  
  - 쿠키
    
    - 만료 기한이 있는 키-값 저장소
  
  - 로컬 스토리지
    
    - 만료 기한이 없는 키-값 저장소
    
    - 웹 브라우저를 닫아도 유지되고 도메인 단위로 저장, 생성
  
  - 세션 스토리지
    
    - 만료 기한이 없는 키-값 저장소
    
    - 탭 단위로 세션 스토리지를 생성하고 탭을 닫으면 데이터가 삭제

- 메모리 관리
  
  - 가상 메모리 : 컴퓨터가 실제로 이용 가능한 메모리 자원을 추상화하여 이를 사용하는 사용자들에게 매우 큰 메모리로 보이게 만드는 것, 가상 주소와 실제 주소가 매핑 되어 있고 프로세스의 주소 정보가 들어 있는 페이지 테이블로 관리, 속도 향상을 위해 TLB 사용
    
    - 가상 주소(logical address) : 가상 메모리에서 가상으로 주어진 주소
    
    - 실제 주소(physical address) : 실제 메모리상에 있는 주소
    
    - TLB : 메모리와 CPU 사이에 있는 주소 변환을 위한 캐시
  
  - 스와핑 : 당장 사용하지 않는 영역을 하드디스크로 옮겨 필요할 때 다시 RAM으로 불러와 올리고, 사용하지 않으면 다시 하드디스크로 내림을 반복하여 RAM을 효과적으로 관리하는 것
  
  - 페이지폴트 : 프로세스의 주소 공간에는 존재하지만 지금 이 컴퓨터 RAM에는 없는 데이터에 접근했을 경우 발생

- 스레싱(thrashing)
  
  - 메모리의 페이지 폴트율이 높은 것
  
  - 메모리에 너무 많은 프로세스가 동시에 올라가 스와핑이 많이 일어나게 되어 발생
  
  - 페이지 폴트가 발생하면 CPU 이용률이 낮아지고 이를 통해 운영체제는 가용성을 높이기 위해 더 많은 프로세스를 메모리에 올리는 악순환을 반복하여 스레싱 발생
    
    - 메모리를 늘리기, HDD를 SDD로 바꾸기, 작업 세트와 PFF 등으로 해결
    
    - 작업 세트 : 프로세스의 과거 사용 이력을 통해 페이지 집합을 만들어 미리 메모리에 로드하는 것
    
    - PFF(Page Fault Frequency) : 페이지 폴드 빈도의 하한선과 상한선을 만드는 것. 하한선에 도달하면 페이지를 줄이고 상한선에 도달하면 페이지를 늘린다.

- 메모리 할당
  
  - 연속 할당
    
    - 메모리에 연속적으로 공간을 할당
    
    - 고정 분할 방식 (fixed partition allocation) : 메모리를 미리 나누어 관리하는 방식, 내부 단편화의 문제
      
      - 내부 단편화 : 메모리를 나눈 크기보다 프로그램이 작아서 들어가지 못하는 공간이 많이 발생하는 현상
      
      - 외부 단편화 : 메모리를 나눈 크기보다 프로그램이 커서 들어가지 못하는 공간이 많이 발생하는 현상
      
      - 홀 : 할당할 수 있는 비어있는 메모리 공간
    
    - 가변 분할 방식(variable partiton allocation) : 매 시점 프로그램의 크기에 맞게 동적으로 메모리를 나눠 사용
      
      - 최초적합 : 위쪽이나 아래쪽부터 시작해서 홀을 찾으면 바로 홀딩
      
      - 최적적합 : 프로세스의 크기 이상인 공간 중 가장 작은 홀부터 할당
      
      - 최악적합 : 프로세스의 크기와 가장 많이 차이가 나는 홀에 할당
  
  - 불연속 할당
    
    - 페이징 : 동일한 크기의 페이지 단위로 나누어 메모리의 서로 다른 위치에 프로세스 할당, 홀의 크기가 균일하지 않은 문제가 없어지지만 주소 변환이 복잡해지는 문제
    
    - 세그멘테이션 : 세그먼트 단위로 나누는 방식, 공유와 보안 측면에서 좋지만 홀 크기가 균일하지 않은 문제
    
    - 페이지드 세그멘테이션 : 공유나 보안을 의미 단위의 세그먼트로 나누고 물리적 메모리는 페이지로 나누는 것 



- 페이지 교체 알고리즘
  
  - 오프라인 알고리즘 
    
    - 먼 미래에 참조되는 페이지와 현재 할당하는 페이지를 바꾸는 알고리즘
  
  - FIFO(First In First Out) 
    
    - 가장 먼저 온 페이지를 교체 영역에 가장 먼저 놓는 방법
  
  - LRU(Least Recentle Used) : 
    
    - 참조가 가장 오래된 페이지를 바꾸는 알고리즘
    
    - 오래된 것을 파악하기 위해 페이지마다 계수기, 스택을 두어야 하는 문제
    
    - 해시 테이블과 이중 연결 리스트를 사용
  
  - NUR(Not Used Recently) 
    
    - clock 알고리즘
    
    - 1은 최근에 참조 0은 참조되지 않음
    
    - 시계 방향으로 돌면서 0을 찾고 0을 찾은 순간 해당 프로세스 교체, 해당 부분 1로
  
  - LFU(Least Frequently Used)
    
    - 가장 참조 횟수가 적은 페이지 교체
















