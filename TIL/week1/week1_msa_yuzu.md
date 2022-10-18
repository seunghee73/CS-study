# MSA


### Sofrware Architecture

- The History of IT System
    - 1960-1980: Fragile, Cowboys
        - Mainframe, Hardware
        - 서비스 자체가 고가여서 변경이 쉽지 않음
    - 1990-2000: Robust, Distributed
        - Changes
        - 분산화로 인해 어느정도 성능이 높은 서비스를 안정적으로 운영
    - 2010-: Resilient/Anti-Fragile, Cloud Native
        - Flow of value의 지속적인 개선
        - DevOps
        - Anti-Fragle
            - Auto scaling
            - Microservices
            - Chaos engineering
            - Continuous deployments
        - Cloud Native Architecture
            - 확장 가능
                - 시스템의 수평적 확장에 유연
                - 확장된 서버로 시스템의 부하 분산, 가용성 보장
                - 시스템 또는 서비스 애플리케이션 단위의 패키지
                - 모니터링
            - 탄력적
                - 서비스 생성-통합-배포, 비즈니스 환경 변화에 대응 시간 단축
                - 분할된 서비스구조
                - 무상태 통신 프로토콜
                - 서비스의 추가와 삭제 자동으로 감지
                - 변경된 서비스 요청에 따라 사용자 요청 처리
            - 장애 격리
                - 특정 서비스에 오류가 발생해도 다른 서비스에 영향 주지 않음

### Clound Native Application

- CI/CD
    - 지속적인 통합
        - 통합 서버, 소스 관리, 빌드 도구, 테스트 도구
        - ex) Jenkins, Team CI, Travis CI
    - 지속적인 배포
        - Continuous Delivery
        - Continuous Deployment
        - Pipe line
    - 카나리 배포
        - 95% 사용자는 이전 버전 서비스 이용, 5% 사용자는 새 버전 서비스 이용
    - 블루그린 배포
        - 100% 사용자가 이전 버전에서 새 버전으로 점진적 이동
- DevOps
- Microservices
- containers
- 12 Factors + 3
    - One codebase, one application
    - API first
    - Dependency management
    - Design, build, release, and run
    - Configuration, credentials, and code
    - Logs
    - Disposability
    - Backing services
    - Environment parity
    - Administrative processes
    - Port binding
    - Stateless processes
    - Concurrency
    - Telemetry
    - Authentication and authorization

### Monolithic VS MSA

- Monolith Architecture
    - 모든 업무 로직이 하나의 애플리케이션 형태로 패키지 되어 서비스
    - 애플리케이션에서 사용하는 데이터가 한곳에 모여 참조되어 서비스되는 형태
    - 구조가 간단하고 빠른 설계 가능
    - 클라우드의 빠른 확장성과 유연한 인프라 환경구조에는 부적합
- MSA
    - HTTP통신을 이용해서 resource API에 통신할 수 있는 작은 규모 여러 서비스의 묶음
    - 종속성이 낮음
        - 한 서비스의 장애가 다른 서비스의 장애로 이어지지 않음
        - 새로운 기술을 적용하고 싶으면 개별 서비스의 단위로 언제든지 적용 가능
    - 구조가 복잡하고 관리하기 어려움
    - 데이터 정합성을 확보하기 어려움
    - Challenges
    - Small Well Chosen Deployable Units
    - Bounded Context
    - RESTful
    - Configuration Management
    - Cloud Enabled
    - Dynamic Scale Up And Scale Down
    - CI/CD
    - Visibility

### SOA VS MSA

- 서비스의 공유 지향점
    - SOA - 재사용을 통한 비용 절감, 서비스 공유 최대화
    - MSA - 서비스 간의 결합도를 낮추어 변화에 능동적으로 대응, 서비스 공유 최소화
- 기술 방식
    - SOA - 공통의 서비스를 ESB에 모아 사업 측면에서 공통 서비스 형식으로 서비스 제공
    - MSA - 각 독립된 서비스가 노출된 REST API를 사용

### Spring Cloud

- + Spring Boot
- Netflix Eureka

### 국내 MSA 적용 사례

- 배달의 민족
- 11번가