### 1. Transaction

- 수백, 수천 명 이상의 사용자들이 DB의 서로 다른 부분이나 동일한 부분을 동시에 접근하기 때문에 일관성 유지 문제가 발생
- 1s에 몇 개의 transaction을 수행하는지(TPS)가 정보 처리 시스템의 성능의 기준
- **동시성 제어 (concurrency control)**
    - 동시에 수행되는 트랜잭션들이 DB에 미치는 영향을 트랜잭션이 순차적으로 수행했을 때와 동일하게 보장함으로써, 데이터의 일관성 유지
- **회복 (recovery)**
    - 데이터 베이스를 갱신하는 도중에 에러가 나도 DB의 일관성 유지
    - **원자성**: DBMS가 하나의 트랜잭션(단위)를 수행이 전혀 안 되거나 완전히 완료 되도록 보장 → 하나의 SQL문처럼 취급
    - DBMS가 고장난 후 복구를 위해 **로그(log)**를 추가적으로 유지
        - log = 어떤 사용자가 어떤 튜플의 값을 어떻게 수정했는지에 대한 정보
    - 비휘발성 저장 장치에 저장
        - 두 개 이상의 비휘발성 저장 장치가 동시에 고장날 가능성이 매우 낮으므로 두 군데 복사해서 저장
    - 로그를 사용한 즉시 갱신
        - 트랜잭션이 DB를 갱신한 사항을 주 기억장치의 버퍼에 유지하다가 트랜잭션이 완료되기 전이라도 디스크의 DB에 기록 가능
        - 철회된 수행 결과도 반영 가능
        - DB 항목에 영향을 미치는 모든 트랜잭션의 연산에 대해 로그 레코드 기록
        - 각 로그 레코드는 로그 순서 번호(LSN : log sequence number)로 식별
- **Abort (철회)**
    - DB의 원자성을 보장하기 위해, 어떠한 이유로 일부만 반영된 트랜잭션의 경우 철회
    - ROLLBACK
- **Commit (완료)**
    - commit된 데이터에 대해서는 DBMS가 데이터를 보장
    - COMMIT

### 2. 트랜잭션의 특성 (ACID)

- **Atomicity (원자성)**
    - all or nothing
    - 모든 명령문을 다 수행하거나 모두 수행하지 않거나
    - 에러가 발생하면 DBMS는 부분적으로 갱신된 트랜잭션을 모두 취소해 원자성 보장
    - 완료된 트랜잭션인 경우(commit 완료) 트랜잭션을 재수행함으로써 트랜잭션의 원자성 보장
- **Consistency (일관성)**
    - 트랜잭션이 수행되기 전와 후에 일관성 유지
    - 트랜잭션이 수행되는 도중에는 일시적으로 불일치 상태일 수 있다
- **Isolation (고립성)**
    - 한 트랜잭션이 데이터를 갱신하는 동안 다른 트랜잭션은 해당 데이터에 접근 불가하도록 해야 한다
    - 모든 트랜잭션이 동시에 수행되더라도 결과는 순차 실행과 동일해야 한다
    - DBMS의 동시성 제어 모듈이 고립성을 보장
- Durability (지속성)
    - 한 트랜잭션이 완료되면 이 트랜잭션이 갱신한 것은 시스템 고장이 발생하더라도 손실되지 않는다
    - 완료된 트랜잭션의 효과는 시스템이 고장난 경우에도 DB에 반영된다

### 3. 동시성 제어

- 여러 사용자들이 다수의 트랜잭션을 동시에 수행하는 환경에서 트랜잭션 간 간섭이 발생하지 않도록 방지
- 직렬 스케줄(serial schedule)
    - 트랜잭션의 집합을 순차적으로 실행
- 비직렬 스케줄(non-serial schedule)
    - 여러 트랜잭션을 동시에 수행
- serializable
    - 비직렬 스케줄로 실행하지만 직렬 스케줄의 수행 결과와 동일하도록
- 동시성 제어가 되지 않을 경우 발생할 수 있는 문제
    - 갱신 손실(lost update) : 수행 중인 트랜잭션이 갱신한 내용을 다른 트랜잭션이 덮어 써 무효화
    - dirty read : 완료되지 않은 트랜잭션이 갱신한 데이터를 읽는 것
    - unrepeatable read : 한 트랜잭션이 동일한 데이터를 두 번 읽을 때 서로 다른 값을 읽는 것

### 4. locking

- 트랜잭션 동시성 제어하기 위해 널리 사용되는 기법
    - 로크(lock)는 DB 내 각 데이터 항목과 연관된 하나의 변수
    - 각 트랜잭션이 수행을 시작하여 데이터 항목에 접근할 때마다 요청한 lock에 관한 정보는 lock table에 유지된다
    - 트랜잭션이 갱신을 목적으로 데이터 항목을 접근할 때는 **독점 로크(X-lock, exclusive lock)**을 요청
    - 읽을 목적으로 데이터 항목을 접근할 때는 **공유 로크(S-lock, shared lock)**을 요청
    - 트랜잭션이 데이터 항목에 대한 접근을 끝낸 후에 **lock**을 해제
- **2단계 로킹 프로토콜 (2-phase locking protocol)**
    - 로크를 요청하고 해제하는 두 단계로 구성
    - 로크 확장 단계가 지난 후에 로크 수축 단계에 들어간다
    - 로크를 한 개라도 해제하면 로크 수축 단계에 들어간다
    - 로크 확장 단계
        - 로크 확장 단계에서는 트랜잭셔닝 데이터 항목에 대해 새로운 로크를 요청할 수 있지만 보유하고 있던 로크를 하나라도 해제할 수 없음
    - 로크 수축 단계
        - 로크 수축 단계에서는 보유하고 있던 로크를 해제할 수 있지만 새로운 로크를 요청할 수 없다
        - 일반적으로 로크를 조금씩 해제하기 보다는 한 번에 해재한다
    - 로크 포인트
        - 한 트랜잭션에서 필요로 하는 모든 로크를 걸어 놓은 시점
    - deadlock 발생할 수 있다
        - 데드록 방지 기법이나 탐지해 희생자를 선정하는 방법 등을 사용
- 다중 로크 단위(multiple granularity)
    - lock을 설정하는 단위가 여러 개 존재
    - tuple < block < relation < DB
    - DBMS에서 각 트랜잭션에 접근하는 튜플 수에 따라 자동으로 조절

## Hashing

### 1. hashing 방법

- 다른 레코드의 참조 없이 목표 레코드의 접근을 직접 지원
- key와 레코드 주소 간 mapping 관계를 함수로 설정
- hashing function (해싱 함수)
    - key 값으로부터 레코드 주소를 계산
    - mapping function: key → 주소
    - 삽입, 검색에 모두 이용
    

### 2. bucket hashing (버킷 해싱)

- bucket: 하나의 주소를 가지지만 하나 이상의 레코드를 저장하는 파일의 한 구역 단위
- 버킷 크기: 저장 장치의 물리적 특성과 한 번 접근으로 채취 가능한 레코드 수 고려
- synonym : 같은 bucket에 있는 레코드
- collision (충돌) : 상이한 레코드들을 같은 주소 (bucket)으로 변환
    - 특정한 bucket에만 레코드가 몰리는 현상
    - linked list로 overflow를 연결
    - hash function의 잘못된 설계로 인해
    

### 3. extendible hashing (확장성 해싱)

- 레코드가 나중에 얼마나 들어올지 모르기 때문에, extendible이 중요
- 충돌에 대처하기 위해 제안된 기법
- 모조 키(pseudo key)
    - 확장성 해시 함수
    - key 값을 일정 길이의 비트 스트링(2의 배수- depth-)로 pseudo key로 변환