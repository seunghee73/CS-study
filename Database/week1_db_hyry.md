# 1. 관계대수와 SQL

## 1. 관계 대수

1. 관계 데이터 모델에서 지원되는 두 가지 언어

   1. **관계 해석 (relational calculus)**

      1. 원하는 데이터만 명시하고 (what) 질의를 어떻게 수행할지는 명시 X
      2. 선언적 언어

   2. **관계 대수 (relational algebra)**

      1. 어떻게 질의를 수행할 것인지 명시하는 절차적 언어

      2. 기존의 relation으로부터 새로운 relation 생성

      3. 산술 연산자와 유사하게 단일 relation이나 두 개의 relation을 입력으로 받아 하나의 결과 relation 생성

         

2. 관계 대수 연산자

   1. 필수 연산자: 이를 갖고 있는 질의어는 relationally complete

      1. selection (∂)
      2. projection (π)
      3. union (∪)
      4. difference (-)
      5. cartesian product (x)

   2. 편의를 위해 유도된 연산자 

      1. intersection (∩)

      2. join (natural join, theta join, semijoin, equijoin) 

      3. division (%)

         

3. **Selection**

   1. relation에서 어떤 조건을 만족하는 tuple들의 부분집합 생성 (행)

   2. 조건 = predicate

   3. = where 절

      

4. **Projection**

   1. relation에서 attribute 단위로 선택 (열)

   2. 한 attribute에서 중복을 삭제하는 것은 시간이 오래 걸리기 때문에, 보통은 중복이 허용되게 검색

   3. = select 절

      

5. 집합 연산자

   1. 집합 연산자로 사용되는 relation은 <u>합집합 호환(union compatible)</u>이어야 한다.

   2. 즉, attribute 순서와 숫자가 같아야 한다. domain이 같아야 한다.

   3. 합집합

      1. 말 그대로 합집합
      2. 결과 릴레이션에서 중복된 결과는 제거 된다
      3. or

   4. 교집합, 차집합

   5. cartesian product

      1. 차수는 두 relation의 차수를 합하고, cardinality는 두 realtion의 cardinality 값을 곱한다

      2. 두 relation의 tuple 간 모든 가능한 조합으로 이루어진다

      3. join을 하기 위한 연산

         

6. **join**

   1. theta join : cartesian product + 비교 연산자 

   2. equijoin : theta join 중 = 를 이용한 join

   3. natural join : = 조인 중 중복되는 값을 자연스럽게 제외시킨 것: 결과 값에서 join에 사용되는 attribute가 중복되지 않도록

   4. 조인 조건에 아무 것도 없다면 cartesian product가 된다

   5. self join : relation 자기 자신에 join -> alias 지정이 필수

      

7. **division**

   1. 빼는 쪽의 값을 모두 가지고 있는(모든 정보를 가진) tuple만 출력하는데, 기준이 된 attribute는 제외
      

8. 외부 조인

   1. right outer join: 오른쪽은 모두 포함, 오른쪽에 없는 값은 모두 null
   2. left outer join
   3. full outer join

   

9. 관계 대수의 한계

   1. 산술 연산 불가 (+, -, ...)
   2. tuple을 개별적으로 보는 것은 되지만, 집단; aggregate function은 지원하지 않는다 → extension으로 집단 함수 제공
   3. 정렬 X
   4. 데이터 베이스 수정 불가
   5. 중복을 나타내지 못한다





## 2. SQL 개요

1. SQL (Structured Query Language; SEQUEL)

   1. IBM에서 1974년 **System R**이라는 RDBMS 시제품 탄생

   2. 처음에는 SEQUEL이라고 불렸기 때문에, 지금도 그렇게 부르기도 한다

   3. SQL은 비절차적 언어 (선언적 언어)

   4. what은 명시하며 how는 아주 일부만 명시 가능

   5. RDBMS은 SQL문을 번역하여 사용자가 요구한 데이터를 찾는데 필요한 모든 과정 담당

   6. 자연어에 가까운 구문을 사용하여 질의를 표현 가능

      

2. 데이터 정의어 (DDL)

   1. schema 정의

   2. CREATE

      1. DOMAIN, TABLE, VIEW, INDEX
      2. 무결성 제약 조건
         1. restrict : 지우지 못하게 막기
         2. cascade : 연결된 것들도 모두 함께 지우기
         3. on delete/update no action/cascade/set null/set default
      3. 데이터 타입
         1. CHAR (n 바이트 문자열)
         2. VARCHAR (최대 n 바이트까지 가변 길이)

   3. ALTER

      1. TABLE
      2. alter table ... add/drop constraint ... 도 가능

   4. DROP

      1. DOMAIN, TABLE, VIEW, INDEX

         

3. DML 

   1. select

      1. 관계 대수의 selection, projection, join, cartesian product 등을 결합
      2. distinct -> 중복 제거
      3. as -> alias
      4. *, like + %(simililar match)
      5. 비교 연산자 > Not > And > Or
      6. In
      7. null 값에 연산을 하면 그 결과도 null
      8. null 인지 여부는 is null로 확인
      9. order by, group by, having
      10. 집합 연산은 기본적으로 중복된 값을 하나로 치지만, all이 붙으면 중복된 값을 별개로 취급
      11. subquery
          1. in, any, all, exists 사용 가능
          2. relation이 subquery의 값으로 return되는 경우, exists를 통해 이 relation 결과가 있는지 여부 테스트 가능 (조건을 만족하는 튜플이 하나라도 있는지) -> T/F로 결과 
      12. correlated nested query(상관 중첩 질의) : 중첩 질의의  where 절에 있는 predicate에서 외부 질의에 선언된 relation의 일부 attribute를 참조하는 질의

   2. insert, delete, update

      1. 참조 무결성 제약 조건 위배하는지 항상 체크
      2. insert into 테이블명 values ();
      3. insert into 테이블명 (col1, col2, ...) select .. from .. where .. ;
      4. update 테이블 set col = 값/식 where .. ;
      5. delete from 테이블명 where ...;

      

4. Trigger & Assertion

   1. **Trigger**
      1. 어떤 이벤트(DB 갱신)가 발생할 때마다 DBMS가 자동으로 수행하는 사용자가 정의한 문 (procedure)
      2. 너무 많이 만들면 overhead가 크다
      3. ECA 규칙. event가 발생했을 때 condition을 만족하면 action
      4. 연쇄적으로 트리거가 수행될 수도 있다
   2. **Assertion**
      1. DB 컨텐츠가 반드시 정해야만 하는 조건 정의
      2. trigger느느 제약 조건을 위반했을 때 수행할 동작을 명시, assertion은 조건을 위반하는 연산이 수행되지 않도록 한다
      3. trigger보다 좀 더 일반적인 제약 조건



## 3. Embeded SQL

- SQL이 host 언어의 완전한 표현력을 갖고 있지 않기 때문에, 모든 질의를 SQL로 표현 불가
- So, 프로그래밍 언어에 SQL문을 삽입된 것을 embedded SQL 이라고 한다
- 데이터 구조가 불일치 하는 문제 (impedance mismatch 문제)
- 별도의 precompiler가 필요
- 프로그래밍과 SQL간에 데이터를 주고 받도록 조정하는 변수 = **host variable** (SQL 문에 포함된 프로그래밍 변수) -> EXEC SQL

