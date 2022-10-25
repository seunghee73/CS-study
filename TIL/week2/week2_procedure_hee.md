## 프로시저

- 프로시저
  - 특정 작업을 수행하는, 이름이 있는 PL/SQL BLOCK
  - 매개 변수를 받을 수 있고, 반복적으로 사용할 수 있는 BLOCK
  - 연속 실행 또는 구현이 복잡한 트랜잭션을 수행하는 PL/SQL BLOCK을 데이터베이스에 저장하기 위해 생성
- PL/SQL
  - Qracle's Procedural Language extension to SQL의 약자로 SQL 문장에서 변수정의, 조건처리, 반복처리 등을 지원하며 오라클 자체에 내장되어 있는 Procedure Language
  - DECLARE문을 이용하여 정의되며, 선언문의 사용은 선택사항
  - PL/SQL문은 블록 구조로 되어 있고 PL/SQL 자신이 컴파일 엔진을 가지고 있다.
  - PL/SQL은 선언부, 실행부, 예외처리부로 구성되어 있다.
    - DECLARE : 선언부로 변수, 상수, 커서, USER_DEFINE Exception을 선언하는 부분이다. 필수가 아니고 선택 사항이다.
    - BEGIN/END : 실행부로 SQL, 반복문, 조건문을 실행하는 부분이다. BEGIN으로 시작해서 END로 종료되고 실행문은 프로그램 내용이 들어가는 부분으로 필수적으로 사용되어야 한다.
    - EXCEPTION : 예외처리부로 일반적으로 오류를 정의하고 처리하는 부분으로 선택 사항이다.
- 저장 프로시저
  - SQL Server에서 제공되는 프로그래밍 기능. 쿼리문의 집합으로 어떤 동작을 일괄 처리할 때 사용
  - 저장 프로시저의 수정 : ALTER PROCEDURE
  - 저장 프로시저의 삭제 : DROP PROCEDURE
  - 매개변수의 사용 : `@입력_매개_변수_이름 데이터_형식 [ = 디폴트값]`
  - 저장 프로시저 실행 : `EXECUTE 프로시저_이름 [전달값]`

```
USE sqlDB;
-- 사용할 데이터베이스
GO

CREATE PROCEDURE usp_users
AS
    SELECT * FROM useTbl;
    -- 저장 프로시저 내용
GO

EXEC usp_users;
```

- 저장 프로시저의 특징
  - SQL Server의 성능을 향상시킬 수 있다.
    - 처음 실행하게 되면 최적화, 컴파일 등의 과정을 거쳐서 그 결과가 캐시(메모리)에 저장된다.
    - 저장 프로시저를 실행하면 캐시(메모리)에 있는 것을 가져다 사용하므로 다시 최적화 및 컴파일을 수행하지 않으므로 실행 속도가 빨라진다.
    - 동일한 저장 프로시저가 자주 사용될 경우에는 일반 쿼리를 반복해서 실행하는 것보다 SQL Server의 성능이 향상될 수 있다.
  - 유지관리가 간편하다.
    - 응용 프로그램에서 직접 SQL문을 작성하지 않고 저장 프로시저 이름만 호출하도록 설정함으로써, 데이터 베이스에 저장 프로시저의 내용을 일관되게 수정/유지보수 등의 작업을 할 수 있다.
  - 모듈식 프로그래밍이 가능하다.
    - 한 번 저장 프로시저를 생성해 놓으면 언제든지 실행이 가능하고 저장 프로시저로 저장해놓은 쿼리의 수정, 삭제 등의 관리가 수월해진다.
  - 보안을 강화할 수 있다.
    - 사용자별로 테이블에 접근 권한을 주지 않고, 저장 프로시저에만 접근 권한을 줌으로써 좀 더 보안을 강화할 수 있다.
  - 네트워크 전송량의 감소
    - 긴 코드의 쿼리를 서버에 저장 프로시저로 생성해 놓았다면 단지 저장 프로시저 이름 및 매개변수 등 몇 글자의 텍스트만 전송하면 되므로 네트워크의 부하를 크게 줄일 수 있다.
- 프로시저 예시 1 : if-else

```
CREATE PROC usp_ifElse
    @userName NVARCHAR(10)
    -- 매개변수, 프로시저 호출 시 입력
AS
    DECLARE @bYear INT
    -- 출생년도를 저장할 변수
    SELECT @bYear = birthYear FROM userTbl
        WHERE name = @userName;
    -- userTbl에서 userName과 일치하는 birthYear를 bYear 변수에 입력

    IF (@bYear >= 1980)
        BEGIN
            PRINT '아직 젊군요...';
        END
    ELSE
        BEGIN
            PRINT '나이가 지긋하네요..';
        END
    -- IF/ELSE를 이용하여 메시지 출력
```

```
EXEC usp_ifElse '조용필';
```

- 프로시저 예시 2 : case

```
CREATE PROC usp_case
    @userName NVARCHAR(10)
    -- 매개변수, 프로시저를 실행할 때 입력으로 받아야하는 값
AS
    DECLARE @bYear     INT
    DECLARE @tti     NCHAR(3) -- 띠
    -- 트랜잭션 내에서 사용하는 변수 선언

    SELECT @bYear = birthYear FROM userTbl
        WHERE name = @userName;
          -- userTbl에 있는 값중 입력으로 받은 유저 이름과 같은 유저의 'birthYear' 컬럼의 값을 bYear에 할당
            -- 테이블에 있는 값을 할당하는 경우에는 SELECT문 사용

    SET @tti =
        CASE
            WHEN ( @bYear%12 = 0 ) THEN '원숭이'
            WHEN ( @bYear%12 = 1 ) THEN '닭'
            WHEN ( @bYear%12 = 2 ) THEN '개'
            WHEN ( @bYear%12 = 3 ) THEN '돼지'
            WHEN ( @bYear%12 = 4 ) THEN '쥐'
            WHEN ( @bYear%12 = 5 ) THEN '소'
            WHEN ( @bYear%12 = 6 ) THEN '호랑이'
            WHEN ( @bYear%12 = 7 ) THEN '토끼'
            WHEN ( @bYear%12 = 8 ) THEN '용'
            WHEN ( @bYear%12 = 9 ) THEN '뱀'
            WHEN ( @bYear%12 = 10 ) THEN '말'
            ELSE '양'
        END;
            -- CASE 문을 이용해서 띠를 찾고 이를 tti의 값으로 설정
    -- 어떠한 결과값에 대해서는 SET 사용

    PRINT @userName + '의 띠 ==> ' + @tti;
    -- 메시지에 출력되는 내용
GO
```

```
EXEC usp_case '조관우';
-- 프로시저 실행
-- 매개변수가 있는 프로시저기 때문에 userName을 입력해주어야함
```

- 프로시저 예시 3 : table

```
CREATE PROC USP_BS_DEMO_2 (
    @IN_P_ID    NVARCHAR(5)
    -- 매개변수, 프로시저 사용 시 입력 필요
)
AS

BEGIN

    DECLARE @TEMP TABLE (
    -- 프로시저 내에서 임시로 테이블을 선언하여 사용할 수 있음
        P_ID    NVARCHAR(50),
        NAME    NVARCHAR(50),
        DEPART    NVARCHAR(50),
        GRADE    SMALLINT,
        S_ID    NVARCHAR(50),
        SUBJECT    NVARCHAR(50),
        SCORE    SMALLINT,
        CLASS    NVARCHAR(10)
    )

    INSERT INTO @TEMP
    -- 임시로 선언한 TEMP 테이블에 BS_DEMO_8_1 테이블과 BS_DEMO_8_2 테이블을 조인한 값을 입력
    SELECT A.P_ID,
      B.NAME,
      B.DEPART,
      B.GRADE,
      A.S_ID,
      C.SUBJECT,
      A.SCORE,
      A.CLASS
    FROM BS_DEMO_8_3 A
    INNER JOIN BS_DEMO_8_1 B ON A.P_ID = B.P_ID
    INNER JOIN BS_DEMO_8_2 C ON A.S_ID = C.S_ID
    WHERE A.P_ID = @IN_P_ID

SELECT * FROM @TEMP
-- TEMP 테이블 조회

END
```
