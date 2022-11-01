# [프로그래머스] SQL LEVEL 1

- `=` : equal은 하나만! `!=`
- 테이블 alias는 테이블 명 바로 옆에 적으면 된다 (e.g. TABLE T)
    - 이후 칼럼 명은 `테이블alias.칼럼명` 으로 표기 (e.g. T.col)
- `as` : 칼럼 명 alias
- `IN` : 파이썬 `in` operator와 동일

- `DATE` 함수
    - MySQL의 `DATE` 함수는 `00-00-00` 형태로 변환해 주지만, 프로그래머스에서는 적용되지 않고 뒤에 `00:00:00` 이 남아 있다.
    - [MySQL 공식 사이트]([https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date))
- `DATE_FORMAT` 함수
    - `DATE` 대신 사용 가능
    - 첫 번째 인자: 날짜
    - 두 번째 인자: 형식 (따옴표 필요: "%Y-%m-%d")
    - 시간 %H, 분 **%i**, 초 %S
    - 달 1~12 → %c

- NULL 값을 다른 값으로 대체하기
    - `IFNULL` 함수
        - 첫 번째 인자: NULL이 아닌 경우 반환할 값 → 칼럼 명을 그대로 적으면 된다.
        - 두 번째 인자: 첫 번째 인자의 칼럼이 NULL일 경우 대체할 값
    - `COALESCE` 함수
        - 맨 처음 나오는 NULL이 아닌 값을 반환
        - 첫 번째 인자: NULL
        - 두 번째 인자: 칼럼 명
        - 세 번째 이후: 대체하기 원하는 값

- `IS NULL` → NULL 체크
- `IS NOT NULL` → NOT NULL 체크

- `LIKE` 연산자
    - `%` : 자리 수 상관 X (e.g. 2021% → 2021-08-09)
    - `_` : 자리 수와 `_` 의 개수 일치 (e.g. 2021% → 2021-)

- `BETWEEN A AND B`
    - between은 A도 포함하고 B도 포함한다 (inclusive)

- 날짜의 최근/최신 → 내림차순에서 가장 위 (큰 쪽)

- `LIMIT` : 반환하는 행의 개수를 제한

---

- 과일로 만든 아이스크림 고르기

```sql
# 상반기 (출하 번호, 맛, 총 주문량)
# 아이스크림정보 (맛, 재료 종류)

# (조건1) 상반기 아이스크림 총주문량이 3,000보다 높으면서
# (조건2) 아이스크림의 주 성분이 과일인
# (핵심) 아이스크림의 맛을
# (부가) 총주문량이 큰 순서대로 조회
# 조건 1과 조건 2가 다른 테이블에 FLAVOR로 연결 -> JOIN해서 보는 것이 편할 듯

SELECT F.FLAVOR
FROM FIRST_HALF F
JOIN ICECREAM_INFO I
ON F.FLAVOR = I.FLAVOR
WHERE I.INGREDIENT_TYPE = 'fruit_based' AND F.TOTAL_ORDER > 3000
ORDER BY F.TOTAL_ORDER DESC
;
```

- 인기 있는 아이스크림

```sql
# 상반기 (출하 번호, 아이스크림 맛, 총 주문량)

SELECT FLAVOR
FROM FIRST_HALF
ORDER BY TOTAL_ORDER DESC, SHIPMENT_ID ASC
;

# ASC는 없어도 가능

```

- 흉부외과 또는 일반외과 의사 목록 출력하기

```sql
# 의사 (의사 이름, 의사 ID, 면허 번호, 고용일자, 진료과코드, 전화번호)
# (조건) 진료과가 흉부외과(CS)이거나 일반외과(GS)인
# (핵심) 의사의 이름, 의사ID, 진료과, 고용일자를 조회
# (부가1) 고용일자를 기준으로 내림차순 정렬
# (부가2) 고용일자가 같다면 이름을 기준으로 오름차순 정렬

SELECT DR_NAME, DR_ID, MCDP_CD, DATE_FORMAT(HIRE_YMD, "%Y-%m-%d") AS HIRE_YMD
FROM DOCTOR
WHERE MCDP_CD = "CS" OR MCDP_CD = "GS"
ORDER BY HIRE_YMD DESC, DR_NAME ASC
;

# IN도 가능
WHERE MCDP_CD IN ("CS", "GS")
```

- 12세 이하인 여자 환자 목록 출력하기

```sql
# 환자 정보 (환자 번호, 이름, 성별, 나이, 전화번호)

# (조건) 12세 이하인 여자환자의
# (메인) 환자이름, 환자번호, 성별코드, 나이, 전화번호를 조회
# (부가 1) 전화번호가 없는 경우, 'NONE'으로 출력
# (부가 2) 나이를 기준으로 내림차순 정렬
# (부가 3) 나이 같다면 환자이름을 기준으로 오름차순 정렬

SELECT PT_NAME, PT_NO, GEND_CD, AGE, IFNULL(TLNO, "NONE") AS TLNO
FROM PATIENT
WHERE GEND_CD = "W" AND AGE <= 12
ORDER BY AGE DESC, PT_NAME ASC
;
```

- 가장 비싼 상품 구하기

```sql
# 상품 (상품 ID, 상품 코드, 판매가)

# (메인) 가장 높은 판매가를 출력
# (부가) 컬럼명은 MAX_PRICE

SELECT MAX(PRICE) AS MAX_PRICE
FROM PRODUCT
;
```

- 조건에 맞는 회원 수 구하기

```sql
# 회원 정보 (회원 ID, 성별, 나이, 가입일)

# (조건 1) 2021년에 가입한 회원 중 
# (조건 2) 나이가 20세 이상 29세 이하인 회원이 
# (핵심) 몇 명인지 출력

SELECT COUNT(*)
FROM USER_INFO
WHERE JOINED LIKE "2021%" AND AGE >= 20 AND AGE <= 29
;

# 날짜에 DATE_FORMAT도 가능
DATE_FORMAT(JOINED, "%Y") = "2021"

# AGE에 BETWEEN도 가능
AGE BETWEEN 20 AND 29
```

- 나이 정보가 없는 회원 수 구하기

```sql
# 회원 정보 (회원 ID, 성별, 나이, 가입일)

# (조건) 나이 정보가 없는 회원이 
# (핵심) 몇 명인지 출력
# (부가) 컬럼명은 USERS

SELECT COUNT(*) AS USERS
FROM USER_INFO
WHERE AGE IS NULL
;
```

- 경기도에 위치한 식품 창고 목록 출력하기

```sql
# 식품창고 (창고 ID, 이름, 주소, 전화번호, 냉동시설)

# (조건) 경기도에 위치한 창고의
# (메인) ID, 이름, 주소, 냉동시설 여부를 조회
# (부가 1) 냉동시설 여부가 NULL인 경우, 'N'으로 출력
# (부가 2) 창고 ID를 기준으로 오름차순 정렬

SELECT WAREHOUSE_ID, WAREHOUSE_NAME, ADDRESS, IFNULL(FREEZER_YN, "N") AS FREEZER_YN
FROM FOOD_WAREHOUSE 
WHERE ADDRESS LIKE "경기도%"
ORDER BY WAREHOUSE_ID
;

# COALESCE
COALESCE(NULL, FREEZER_YN, "N") AS FREEZER_YN
```

- 강원도에 위치한 생산 공장 목록 출력하기

```sql
# 식품 공장 (ID, 이름, 주소, 전화번호)

# (조건) 강원도에 위치한 식품공장의 
# (메인) 공장 ID, 공장 이름, 주소를 조회
# (부가) 공장 ID를 기준으로 오름차순 정렬

SELECT FACTORY_ID, FACTORY_NAME, ADDRESS
FROM FOOD_FACTORY 
WHERE ADDRESS LIKE "강원도%"
ORDER BY FACTORY_ID
;
```

- 최댓값 구하기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# 가장 최근에 들어온 동물은 = MAX
# 언제 들어왔는지 조회

SELECT DATETIME AS "시간"
FROM ANIMAL_INS
WHERE DATETIME = (SELECT MAX(DATETIME) FROM ANIMAL_INS)
;

# 간단하게
SELECT MAX(DATETIME) AS "시간"
FROM ANIMAL_INS
;

# LIMIT 
SELECT DATETIME AS "시간"
FROM ANIMAL_INS
ORDER BY DATETIME DESC
LIMIT 1
;
```

- 이름이 있는 동물의 아이디

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (조건) 이름이 있는 동물의 
# (메인) ID를 조회
# (부가) ID는 오름차순 정렬

SELECT ANIMAL_ID
FROM ANIMAL_INS 
WHERE NAME IS NOT NULL
ORDER BY ANIMAL_ID
;
```

- 상위 n개 레코드

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (조건) 동물 보호소에 가장 먼저 들어온 -> 가장 날짜가 오래된 (MIN)
# (메인) 동물의 이름을 조회

SELECT NAME
FROM ANIMAL_INS
WHERE DATETIME = (SELECT MIN(DATETIME) FROM ANIMAL_INS)
;

SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1
;
```

- 여러 기준으로 정렬하기

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (메인) 모든 동물의 아이디와 이름, 보호 시작일을
# (부가 1) 이름 순으로 조회
# (부가 2) 이름이 같은 동물 중에서는 보호를 나중에 시작한 동물을 먼저

SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS 
ORDER BY NAME, DATETIME DESC
;
```

- 동물의 아이디와 이름

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (메인) 모든 동물의 아이디와 이름을 
# (부가) ANIMAL_ID순으로 조회

SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
;
```

- 이름이 없는 동물의 아이디

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (조건) 이름이 없는 채로 들어온 동물의 
# (메인) ID를 조회
# (부가) ID는 오름차순 정렬

SELECT ANIMAL_ID
FROM ANIMAL_INS 
WHERE NAME IS NULL
ORDER BY ANIMAL_ID
;
```

- 어린 동물 찾기

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (조건) 젊은 동물의
# (핵심) 아이디와 이름을 조회
# (부가) 아이디 순으로 조회

SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS 
WHERE INTAKE_CONDITION != "Aged"
ORDER BY ANIMAL_ID
;
```

- 아픈 동물 찾기

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (조건) 아픈 동물의
# (핵심) 아이디와 이름을 조회
# (부가) 아이디 순으로 조회

SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS 
WHERE INTAKE_CONDITION = "Sick"
ORDER BY ANIMAL_ID
;
```

- 역순 정렬하기

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (핵심) 이름과 보호 시작일을 조회
# (부가) ANIMAL_ID 역순

SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC
;
```

- 모든 레코드 조회하기

```sql
# 동물 (ID, 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별/중성화)

# (핵심) 모든 동물의 정보를
# (부가) ANIMAL_ID순으로 조회

SELECT *
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
;
```

# [프로그래머스] SQL LEVEL 2

- A에 대한 B, B를 A 별로 계산
    - A로 그룹지어 B를 계산 (SUM, AVG, COUNT …)
    - GROUP BY A
    - group by는 여러 칼럼을 하나로 묶어서도 가능하다 → 이때 `()` 로 묶지 않는다.
- HAVING
    - GROUP에 대한 조건
    - (e.g. 그룹은 count가 1보다 커야 한다 → HAVING COUNT(*) > 1)

- A `INNER JOIN` B `ON` A.col = B.col
    - A `JOIN` B `ON` A.col = B.col
    - 교집합
    - 조건에 부합하는 행만 RETURN
    - 기본적으로 LEFT JOIN과 RIGHT JOIN은 INNER JOIN보다 느리다.
        - 왜냐하면 INNER JOIN에 추가 작업을 하기 때문
- A `LEFT OUTER JOIN` B `ON`  조인 조건
    - A `LEFT JOIN` B `ON`  조인 조건
    - A는 그대로 가져가고
    - B 중에 (조인 조건을 통해) A와 연결될 수 있는 게 있다면 연결
    - 그렇기 때문에 A 중에 B와 연결될 수 있는 것이 없는 행이 있다면, NULL로 표시된다.
- A `RIGHT OUTER JOIN` B `ON`  조인 조건
    - A `RIGHT JOIN` B `ON`  조인 조건
    - B는 그대로 가져가고
    - A 중에 (조인 조건을 통해) B와 연결될 수 있는 게 있다면 연결
    - 그렇기 때문에 B 중에 A와 연결될 수 있는 것이 없는 행이 있다면, NULL로 표시된다.

- `count(distinct 칼럼명)` → 칼럼에서 distinct한 것만 셀 때 사용
- `count(distinct col1, col2)` → 칼럼에서 (col1, col2) 조합을 distinct하게 센다
    - `count(col1, col2)` 는 불가능

- `IF(조건, true값, false값)` : if문
- 같은 칼럼으로 구간이 나뉘는 경우
    - CASE문 자체가 하나의 COL으로 역할
    
    ```sql
    SELECT # 여기에 COL 집어넣으면 에러 발생
    	CASE # CASE문 시작
    	WHEN 조건 THEN 조건충족하는경우값
    	WHEN 조건 THEN 조건충족하는경우값
    	...
    	ELSE # 조건을 충족하지 않는 경우 (DEFAULT)
    	END # CASE문 종료 # 이때 다른 COL명 원하는 경우 <END AS 새칼럼명>
    FROM
    ```
    

- `FLOOR` 함수 → 숫자 내림 + 정수화
- `TRUNCATE`
    - 첫 번째 인자 → 값
    - 두 번째 인자 → 값의 몇 번째 자리까지 0처리 할 것인지
        - -3 → ~100자리

- `SUTSTR`
    - 첫 번째 인자 : 칼럼명
    - 두 번째 인자 : 문자 자르기 시작할 위치
    - 세 번째 인자 : 시작 위치부터 문자를 얼마나 길게 자를지
    - 1-INDEXING이니 주의!

---

- 성분으로 구분한 아이스크림 총 주문량

```sql
# 상반기 (출하 번호, 맛, 총 주문량)
# 성분 (맛, 성분)
# 성분에 있는 값이 상반기에 없을 수 있음 상반기 기준으로 JOIN
# => LEFT JOIN으로도 가능

# (핵심 1) 각 아이스크림 성분 타입과 
# (조건) 성분 타입에 대한 아이스크림의 (핵심 2) 총주문량을
# (부가 1) 총주문량이 작은 순서대로 조회
# (부가 2) 총주문량을 나타내는 컬럼명은 TOTAL_ORDER

# 하나의 테이블로 합쳐서 하기
SELECT INGREDIENT_TYPE, SUM(TOTAL_ORDER)
FROM FIRST_HALF F
INNER JOIN ICECREAM_INFO I
ON F.FLAVOR = I.FLAVOR
GROUP BY INGREDIENT_TYPE
ORDER BY TOTAL_ORDER
;

# GROUP BY가 없다면 SUM이 맨 처음 그룹만 나타나게 된다.
# SUM과 같은 함수는 기본적으로는 하나의 행만 출력
```

- 진료과별 총 예약 횟수 출력하기

```sql
# 예약 (예약일시, 예약번호, 환자번호, 진료과코드, 의사 ID, 예약취소여부, 취소 날짜)

# (조건) 2022년 5월에 예약한
# (핵심 1) 환자 수를 -> count
# (핵심 2) 진료과코드 별로 조회 -> group by
# (부가 1) 컬럼명은 '진료과코드', '5월예약건수'로 지정
# (부가 2) 진료과별 예약한 환자 수를 기준으로 오름차순 정렬
# (부가 3) 예약한 환자 수가 같다면 진료과 코드를 기준으로 오름차순 정렬

SELECT MCDP_CD AS "진료과코드", COUNT(*) AS "5월예약건수"
FROM APPOINTMENT 
WHERE APNT_YMD LIKE "2022-05%"
GROUP BY MCDP_CD
ORDER BY 5월예약건수, 진료과코드
;
```

- 재구매가 일어난 상품과 회원 리스트 구하기

```sql
# 상품 판매 (판매 ID, 회원 ID, 상품 ID, 판매량, 판매일)

# (조건) 동일한 회원이 동일한 상품을 재구매한 데이터 
#	-> 회원과 상품이 동일한 레코드가 두 개 이상(HAVING), 회원과 상품은 GROUP
# (핵심) 재구매한 회원 ID와 재구매한 상품 ID를 출력
# (부가 1) 회원 ID를 기준으로 오름차순 정렬
# (부가 2) 회원 ID가 같다면 상품 ID를 기준으로 내림차순 정렬

SELECT USER_ID, PRODUCT_ID
FROM ONLINE_SALE
GROUP BY USER_ID, PRODUCT_ID
HAVING COUNT(*) > 1
ORDER BY USER_ID, PRODUCT_ID DESC
;
```

- 상품 별 오프라인 매출 구하기

```sql
# 상품 (상품 ID, 상품코드, 판매가) P
# 오프라인 판매 (판매 ID, 상품 ID, 판매량, 판매일) O

# (그룹) 상품코드 별 
# (핵심) 매출액(판매가 * 판매량) 합계를 출력 => P join O (상품 ID로 조인) => Right join도 가능
# (부가 1) 매출액을 기준으로 내림차순 정렬
# (부가 2) 매출액이 같다면 상품코드를 기준으로 오름차순 정렬

SELECT PRODUCT_CODE, SUM(P.PRICE * O.SALES_AMOUNT) AS "SALES"
FROM PRODUCT P
RIGHT JOIN OFFLINE_SALE O
ON P.PRODUCT_ID = O.PRODUCT_ID
GROUP BY P.PRODUCT_CODE
ORDER BY SALES DESC, PRODUCT_CODE ASC
;

# inner join
RIGHT JOIN OFFLINE_SALE O
```

- 가격대 별 상품 개수 구하기

```sql
# 상품 (상품 ID, 상품코드, 판매가)

# (부가 1) 만원 단위의 가격대 별로 => GROUP BY
# (핵심) 상품 개수를 출력
# (부가 2) 컬럼명은 각각 PRICE_GROUP, PRODUCTS
# (부가 3) 가격대 정보는 각 구간의 최소금액
#	        (10,000원 이상 ~ 20,000 미만인 구간인 경우 10,000)으로 표시
# (부가 4) 가격대를 기준으로 오름차순 정렬

# 위는 MAX값을 알고 범위를 모두 하드 코딩
SELECT
    CASE
    WHEN PRICE < 10000 THEN 0
    WHEN PRICE BETWEEN 10000 AND 19999 THEN 10000
    WHEN PRICE BETWEEN 20000 AND 29999 THEN 20000
    WHEN PRICE BETWEEN 30000 AND 39999 THEN 30000
    WHEN PRICE BETWEEN 40000 AND 49999 THEN 40000
    WHEN PRICE BETWEEN 50000 AND 59999 THEN 50000
    WHEN PRICE BETWEEN 60000 AND 69999 THEN 60000
    WHEN PRICE BETWEEN 70000 AND 79999 THEN 70000
    WHEN PRICE BETWEEN 80000 AND 89999 THEN 80000
    END AS PRICE_GROUP, COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP  # 위는 COL을 새로 만든 것이므로 여전히 직접 GROUP BY 필요
ORDER BY PRICE_GROUP
;

# 숫자 계산으로 구역 나누기
SELECT FLOOR(PRICE / 10000) * 10000 AS PRICE_GROUP, COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP
;

# TRUNCATE
SELECT TRUNCATE(PRICE, -4) AS PRICE_GROUP, COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP
;
```

- 카테고리 별 상품 개수 구하기

```sql
# 상품 (ID, 상품코드, 판매가)

# (부가 1) 상품 카테고리 코드(PRODUCT_CODE 앞 2자리) 별
# (핵심) 상품 개수를 출력
# (부가 2) 상품 카테고리 코드를 기준으로 오름차순 정렬

SELECT SUBSTR(PRODUCT_CODE, 1, 2) AS CATEGORY, COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY CATEGORY
;
```

- 3월에 태어난 여성 회원 목록 출력하기

```sql
# 회원 정보 (회원 ID, 이름, 연락처, 성별, 생년월일)

# (조건 1) 생일이 3월인 
# (조건 2) 여성 회원의 
# (핵심) ID, 이름, 성별, 생년월일을 조회
# (부가 1) 전화번호가 NULL인 경우는 출력대상에서 제외
# (부가 2) 회원ID를 기준으로 오름차순 정렬

SELECT MEMBER_ID, MEMBER_NAME, GENDER, 
	DATE_FORMAT(DATE_OF_BIRTH, "%Y-%m-%d") AS DATE_FORMAT
FROM MEMBER_PROFILE
WHERE DATE_FORMAT(DATE_OF_BIRTH, "%m") = "03" AND GENDER = "W" AND TLNO IS NOT NULL
ORDER BY MEMBER_ID
;
```

- 가격이 제일 비싼 식품의 정보 출력하기

```sql
# 식품 (식품 ID, 이름, 코드, 분류, 가격)

# (조건) 가격이 제일 비싼 식품의 
# (핵심) 식품 ID, 식품 이름, 식품 코드, 식품분류, 식품 가격을 조회

SELECT *
FROM FOOD_PRODUCT 
WHERE PRICE = (SELECT MAX(PRICE) FROM FOOD_PRODUCT)
;
```

- DATETIME에서 DATE로 형 변환

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (핵심) 각 동물의 아이디와 이름, 들어온 날짜를 조회
# (부가) 아이디 순으로 조회

SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, "%Y-%m-%d") AS "날짜"
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID
;
```

- 입양 시각 구하기(1)

```sql
# 입양 보냄 (ID, 종, 입양일, 이름, 성별/중성화)

# (조건1) 09:00부터 19:59까지,
# (부가 1) 각 시간대별로
# (핵심) 입양이 몇 건이나 발생했는지 조회
# (부가 2) 시간대 순으로 정렬

SELECT DATE_FORMAT(DATETIME, "%H") AS HOUR, COUNT(*) AS COUNT
FROM ANIMAL_OUTS
WHERE DATE_FORMAT(DATETIME, "%H:%i") BETWEEN "09:00" AND "19:59"
GROUP BY HOUR
ORDER BY HOUR
;
```

- NULL 처리하기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (핵심) 종, 이름, 성별 및 중성화 여부를
# (부가 1) 아이디 순으로 조회
# (부가 2) 이름이 없는 동물의 이름은 "No name"으로 표시

SELECT ANIMAL_TYPE, IFNULL(NAME, "No name") AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID
;

# COALESCE
COALESCE(NULL, NAME, "No name") AS NAME
```

- 중성화 여부 파악하기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (핵심) 동물의 아이디와 이름, 중성화 여부를
# (부가 1) 아이디 순으로 조회
# (부가 2) 중성화가 되어있다면 'O', 아니라면 'X'라고 표시

SELECT ANIMAL_ID, NAME, 
    IF(SEX_UPON_INTAKE LIKE "Neutered%" OR SEX_UPON_INTAKE LIKE "Spayed%","O","X") AS "중성화"
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
;
```

- 중복 제거하기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (핵심) 동물의 이름은 몇 개인지 조회
# (조건) 이름이 NULL인 경우는 집계하지 않으며
# (부가) 중복되는 이름은 하나로 친다.

SELECT COUNT(DISTINCT NAME) AS count
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
;
```

- 동물 수 구하기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# 동물이 몇 마리 들어왔는지 조회

SELECT COUNT(*) AS "count"
FROM ANIMAL_INS
;
```

- 이름에 el이 들어가는 동물 찾기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (조건1) 이름에 "EL"이 들어가는 
# (조건 2) 개의 
# (핵심) 아이디와 이름을 조회
# (부가 1) 이름 순으로 조회해주세요. 
# (부가 2) 이름의 대소문자는 구분하지 않습니다.

SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS 
WHERE NAME LIKE "%EL%" AND ANIMAL_TYPE ="Dog"
ORDER BY NAME
;
```

- 루시와 엘라 찾기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (조건) 이름이 Lucy, Ella, Pickle, Rogan, Sabrina, Mitty인 동물의 
# (핵심) 아이디와 이름, 성별 및 중성화 여부를 조회

SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS 
WHERE NAME IN ("Lucy", "Ella", "Pickle", "Rogan", "Sabrina", "Mitty")
;

# in 안에 찾는 string은 반드시 ""로 감싼다. 
```

- 동명 동물 수 찾기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (조건) 동물 이름 중 두 번 이상 쓰인 
# (핵심) 이름과 해당 이름이 쓰인 횟수를 조회
# (부가 1) 이름이 없는 동물은 집계에서 제외
# (부가 2) 이름 순으로 조회

SELECT NAME, COUNT(*) AS "COUNT"
FROM ANIMAL_INS 
WHERE NAME IS NOT NULL
GROUP BY NAME
HAVING COUNT(*) > 1
ORDER BY NAME
;
```

- 고양이와 개는 몇 마리 있을까

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (부가 1) 고양이와 개가 
# (핵심) 각각 몇 마리인지 조회
# (부가 2) 고양이를 개보다 먼저 조회

SELECT ANIMAL_TYPE, COUNT(*) as count
FROM ANIMAL_INS 
WHERE ANIMAL_TYPE IN ("Cat", "Dog")
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE
;
```

- 최솟값 구하기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)

# (조건) 동물 보호소에 가장 먼저 들어온 동물은 
# (핵심) 언제 들어왔는지 조회

SELECT DATETIME AS "시간"
FROM ANIMAL_INS
WHERE DATETIME = (SELECT MIN(DATETIME) FROM ANIMAL_INS)
;
```

# [프로그래머스] SQL LEVEL 3

- **GROUP BY에 쓰이지 않은 COL을 SELECT에 쓸 수 없다.**
    - DBMS에서 어떤 COL의 값을 써야 할지 몰라 에러 발생
    - SUM, AVG 등은 가능 → 하나만 RETURN하니까

- FROM에 DERIVED TABLE, 즉, 다른 테이블에서 뽑아내 만든 테이블을 사용한다면 ALIAS가 필요

---

- 즐겨찾기가 가장 많은 식당 정보 출력하기

```sql
# 식당 (ID, 이름, 음식 종류, 조회수, 즐겨찾기, 주차장, 주소, 전화번호)

# (부가 1) 음식종류별로
# (조건) 즐겨찾기수가 가장 많은 식당의
# (핵심) 음식 종류, ID, 식당 이름, 즐겨찾기수를 조회 O
# (부가 2) 음식 종류를 기준으로 내림차순 정렬 O

# FROM에 들어갈 내용
# SELECT FOOD_TYPE, MAX(FAVORITES)
# FROM REST_INFO
# GROUP BY FOOD_TYPE
# ;

SELECT A.FOOD_TYPE, B.REST_ID, B.REST_NAME, A.FAVORITES
FROM (
    SELECT FOOD_TYPE, MAX(FAVORITES) AS FAVORITES
    FROM REST_INFO
    GROUP BY FOOD_TYPE
) A  #=> 꼭 ALIAS 필요
LEFT JOIN REST_INFO B
ON A.FOOD_TYPE = B.FOOD_TYPE AND A.FAVORITES = B.FAVORITES
ORDER BY FOOD_TYPE DESC
;
```

- 조건 별로 분류하여 주문 상태 출력하기

```sql
# 식품 공장 (주문 ID, 제품 ID, 주문양, 생산일자, 입고일자, 출고일자, 공장 ID, 창고 ID)

# () 주문 ID, 제품 ID, 출고일자, 출고여부를 조회
# () 출고여부는 5월 1일까지 출고완료로 이후 날짜는 출고 대기로 미정이면 출고미정으로 출력
# () 주문 ID를 기준으로 오름차순 정렬 O

SELECT ORDER_ID, PRODUCT_ID, DATE_FORMAT(OUT_DATE, "%Y-%m-%d") AS OUT_DATE,
    CASE
        WHEN OUT_DATE IS NULL THEN "출고미정"
        WHEN OUT_DATE <= "2022-05-01" THEN "출고완료"
        WHEN OUT_DATE > "2022-05-01" THEN "출고대기"
    END AS "출고여부"
FROM FOOD_ORDER
ORDER BY ORDER_ID
;
```

- 헤비 유저가 소유한 장소

```sql
# 공간 (ID, 이름, 보유유저)

# (조건) 공간을 둘 이상 등록한 
# (핵심) 공간의 정보를
# (부가) 아이디 순으로 조회

SELECT ORIGINAL.ID, ORIGINAL.NAME, DERIVED.HOST_ID
FROM (
    SELECT HOST_ID, COUNT(*)
    FROM PLACES
    GROUP BY HOST_ID
    HAVING COUNT(*) > 1
    ) DERIVED
LEFT JOIN PLACES ORIGINAL
ON DERIVED.HOST_ID = ORIGINAL.HOST_ID
ORDER BY ORIGINAL.ID
;
```

- 오랜 기간 보호한 동물(2)

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)
# 입양 보냄 (ID, 종, 입양일, 이름, 성별/중성화)

# (조건 1) 입양을 간 동물 중 => 양쪽 레코드가 모두 존재 -> INNER JOIN으로 쉽게 해결
# (조건 2) 보호 기간이 가장 길었던 
# (부가 1) 동물 두 마리의 
# (핵심) 아이디와 이름을 조회
# (부가 2) 보호 기간이 긴 순으로 조회

SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_INS I
JOIN ANIMAL_OUTS O
ON I.ANIMAL_ID = O.ANIMAL_ID
ORDER BY (O.DATETIME - I.DATETIME) DESC
LIMIT 2
;
```

- 오랜 기간 보호한 동물(1)

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)
# 입양 보냄 (ID, 종, 입양일, 이름, 성별/중성화)

# (조건 1) 아직 입양을 못 간 동물 중 => IN LEFT JOIN OUT && OUT IS NULL
# (조건 2) 가장 오래 보호소에 있었던 => MIN
# (부가 1) 동물 3마리의 => LIMIT
# (핵심) 이름과 보호 시작일을 조회
# (부가 2) 보호 시작일 순으로 조회

SELECT I.NAME, I.DATETIME
FROM ANIMAL_INS I
LEFT JOIN ANIMAL_OUTS O
ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE O.DATETIME IS NULL
ORDER BY I.DATETIME
LIMIT 3
;
```

- 있었는데요 없었습니다

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)
# 입양 보냄 (ID, 종, 입양일, 이름, 성별/중성화)

# (조건) 보호 시작일보다 입양일이 더 빠른 동물의 => INNER JOIN
# (핵심) 아이디와 이름을 조회
# (부가) 보호 시작일이 빠른 순으로 조회

SELECT I.ANIMAL_ID, I.NAME
FROM ANIMAL_INS I
JOIN ANIMAL_OUTS O
ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.DATETIME > O.DATETIME
ORDER BY I.DATETIME
;
```

- 없어진 기록 찾기

```sql
# 보호소 동물 정보 (ID, 종, 보호 시작일, 보호 시작시 시 상태, 이름, 성별/중성화)
# 입양 보냄 (ID, 종, 입양일, 이름, 성별/중성화)

# (조건) 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 
# (핵심) ID와 이름을
# (부가) ID 순으로 조회

SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_INS I
RIGHT JOIN ANIMAL_OUTS O
ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.DATETIME IS NULL
ORDER BY O.ANIMAL_ID
;
```
