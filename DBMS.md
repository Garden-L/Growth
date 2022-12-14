# 기본은 MYSQL을 바탕으로 합니다.

<br></br>
## Numeric Data Type
##### https://dev.mysql.com/doc/refman/8.0/en/numeric-type-syntax.html


### ■ BIGINT\[(M)] [UNSIGNED] [ZEROFILL]
최대 64비트(8바이트)크기의 정수를 저장할 수 있는 숫자 데이터 타입이다. BIGINT는 데이터 타입 중에서 가장 중요하다. 왜냐하면 모든 산술 계산이 BIGINT 타입으로 계산되기 때문이다.
* 알고 있어야 하는 사실
 + 모든 산술 계산은 부호 없는 BIGINT(signed bigint)형으로 계산되기 때문에 최대 2^63-1까지 표현되기 때문에 이 수치를 넘어가지 않도록 주의해야한다.
   -  +, -, * 연산은 모두 BIGINT형으로 계산되므로 최대 표현값을 넘기면 잘못된 결과값을 반환한다.
 + 문자열 형태로 BIGINT인 열에 값을 삽입할 수 있다. MYSQL에서는 문자열을 BIGINT형으로 변환하여 저장한다.

<br></br>
## INSERT
### ■ INSERT란?
INSERT는 이미 존재하고 있는 테이블에 새로운 열(데이터)를 삽입한다. DBMS에서 insert를 사용하기 위해서는 insert 권한이 있어야 한다. 대부분 DBMS 계정에 기본적으로 insert 권한이 부여되어 있다.

### ■ INSERT 기본 명세서
#### 기본 명세
* 테이블 이름 뒤에 콤마로 분리된 열 이름들이 괄호 안에 제공한다. 열에 알맞는 데이터는 VALUES 뒤에 list 형태로 반드시 제공해야한다. 
```mysql
INSERT INTO tbl_name(col1, col2, ...) VALUE (val1, val2)
```
* 만약 테이블 이름 뒤에 열 이름들을 제공하지 않는다면 반드시 삽입할 값들이 모든 컬럼과 일치해야한다. 삽입하려는 테이블의 이름을 잊어버렸다면 'DESCRIBE tbl_name'을 사용하여 테이블 컬럼들의 정보를 확인할 수 있다.
```mysql
// 테이블의 열을 기입하지 않음.
INSERT INTO tbl_name VALUE (val1, val2) // 모든 컬럼의 개수, 데이터 타입이 일치해야한다.
```
#### 값을 삽입하는 여러 방법
* strictSQL 모드가 활성화 되지 않았으면 테이블의 컬럼을 지정하지 않아도 자동으로 디폴트 값이 삽입되게 된다.
* strictSQL 모드가 활성화 되어 있으면 테이블의 컬럼을 기입하지 않은 채로 데이터의 개수가 일치하지 않으면 에러를 발생시킨다.
* 컬럼과 VALUE list가 비어있으면 디폴트 값을 생성하여 삽입한다. 만약 디폴트 값을 생성하지 않도록 지정했다면 에러를 발생한다.
```mysql
INSERT INTO tbl_name(col1, col2, ...) VALUE (val1, val2)
```

#### 다중 행을 삽입
* 여러 행의 값을 한번에 삽입하는 문법을 지원한다. 반드시 괄호 각 열 값을 묶고 컴마로 분리 시켜야한다.
```mysql
INSERT INTO tbl_name (a,b,c) VALUES(1,2,3), (4,5,6), (7,8,9);
```
* values list가 열의 개수의 배수라면 한번에 값을 입력하는 것이 가능하다. 아래에 예제에서 테이블이 3개의 열을 가지고 있으므로 밸류 값이 3의 배수와 정확히 일치하면 총 3의 배수 만큼 행이 각각 값에 맞추어 삽입된다.
```mysql
INSERT INTO tbl_name (a,b,c) VALUES(1,2,3,4,5,6,7,8,9);
```

## 윈도우 함수(WINDOW FUNCTION)
### ■ 윈도우 함수란?
SUM, AVG 함수들은 모두 행을 고려하지 않은 열을 이용한 집계함수이다. 그래서 SUM함수를 사용한다고하면 단순하게 결과 값이 하나만 출력될 것이다. 
```sql
mysql> SELECT * FROM sales ORDER BY country, year, product;
+------+---------+------------+--------+
| year | country | product    | profit |
+------+---------+------------+--------+
| 2000 | Finland | Computer   |   1500 |
| 2000 | Finland | Phone      |    100 |
| 2001 | Finland | Phone      |     10 |
| 2000 | India   | Calculator |     75 |
| 2000 | India   | Calculator |     75 |
| 2000 | India   | Computer   |   1200 |
| 2000 | USA     | Calculator |     75 |
| 2000 | USA     | Computer   |   1500 |
| 2001 | USA     | Calculator |     50 |
| 2001 | USA     | Computer   |   1500 |
| 2001 | USA     | Computer   |   1200 |
| 2001 | USA     | TV         |    150 |
| 2001 | USA     | TV         |    100 |
+------+---------+------------+--------+
```

위의 예제를 보면 만약 나라별로 profit을 구하고하면 group by로 나라를 그룹핑하고 각 합을 출력할 것이다. 그러면 총 3나라 이므로 3개의 행이 출력 될 것이다. 그룹 하나당 하나의 행만 출력하는 것이다. 하지만 윈도우 함수는 모든 행에 결과를 반영한다. 총 행의 수가 14개 이므로 14 모두에 각 나라별의 profit값을 반영한다. SUM은 집계함수인데 왜 윈도우에 쓰이는가?? 윈도우 함수는 RANK 같이 따로 정해져 있지만 집계함수를 지원한다. 아래의 차이를 보면 확연한 차이가 보인다.

#### 단순한 집계함수 결과
```sql
mysql> SELECT SUM(profit) AS total_profit
       FROM sales;
+--------------+
| total_profit |
+--------------+
|         7535 |
+--------------+
mysql> SELECT country, SUM(profit) AS country_profit
       FROM sales
       GROUP BY country
       ORDER BY country;
+---------+----------------+
| country | country_profit |
+---------+----------------+
| Finland |           1610 |
| India   |           1350 |
| USA     |           4575 |
+---------+----------------+
```
#### 윈도우 함수 결과
```sql
 SELECT
         year, country, product, profit,
         SUM(profit) OVER() AS total_profit,
         SUM(profit) OVER(PARTITION BY country) AS country_profit
       FROM sales
       ORDER BY country, year, product, profit;
+------+---------+------------+--------+--------------+----------------+
| year | country | product    | profit | total_profit | country_profit |
+------+---------+------------+--------+--------------+----------------+
| 2000 | Finland | Computer   |   1500 |         7535 |           1610 |
| 2000 | Finland | Phone      |    100 |         7535 |           1610 |
| 2001 | Finland | Phone      |     10 |         7535 |           1610 |
| 2000 | India   | Calculator |     75 |         7535 |           1350 |
| 2000 | India   | Calculator |     75 |         7535 |           1350 |
| 2000 | India   | Computer   |   1200 |         7535 |           1350 |
| 2000 | USA     | Calculator |     75 |         7535 |           4575 |
| 2000 | USA     | Computer   |   1500 |         7535 |           4575 |
| 2001 | USA     | Calculator |     50 |         7535 |           4575 |
| 2001 | USA     | Computer   |   1200 |         7535 |           4575 |
| 2001 | USA     | Computer   |   1500 |         7535 |           4575 |
| 2001 | USA     | TV         |    100 |         7535 |           4575 |
| 2001 | USA     | TV         |    150 |         7535 |           4575 |
+------+---------+------------+--------+--------------+----------------+
```
윈도우 연산은 OVER 키워드를 포함해야한다.
* 첫 번째 윈도우 연산은 OVER가 비어 있으므로 전체 테이블을 한 파티션으로 사용한다. 그리고 global sum으로 처리하지만 각 행마다 처리한다.
* 두 번째 윈도우 연산은 OVER에 나라별로 나누라는 partion by 키워드가 있어 나라별로 테이블을 조각낸다. 그러면 총 3개의 파티션으로 분할 되고 각 파티션의 행마다 합을 처리한다.

### ■ 윈도우 함수의 사용 조건
* 윈도우 함수는 SELECT 절 또는 ORDER BY 절에서만 사용가능하다.
* 결과 값은 FROM으로 부터 결정 되고, WHARE, HAVING, GROUP BY 연산 후에 처리되고 즉 LIMIT, ORDER BY, SELECT DISTICT 전에 처리 된다.
* 윈도우 함수뿐만 아니라 집계 함수도 사용 가능하다. 윈도우 동작의 필요조건은 집계 또는 윈도우 함수 이후 OVER의 유무이다.

### ■ 윈도우 함수 포멧
윈도우 함수를 형태는 다음과 같다.
```sql
(function) OVER {(window_spec) | (window_name)}
```
#### function
집계함수 또는 윈도우 함수이다. RANK(), SUM(), AVG() 등 ...

#### window_spec
윈도우 스펙의 형태는 다음과 같다.
```sql
[window_name] [partition_clause] [order_clause] [frame_clause]
```
* window_name : C언어의 define같은 것이다. 윈도우를 따로 정의하여 사용할 수 있다.
* partition_caluse : PARTITION BY col,\[col,...]...의 표현으로 행을 파티션할 기준이다.
* order_clause : ORDER BY col, \[col,...]...의 표현으로 각 파티션 마다 정렬의 기준이다.
* frame_clause : 프레임 절은 해당 행을 연산할 때 다양한 범위로 연산을 할 수 있도록 지원한다. 만약 2022년 1월 2일인 행을 윈도우 연산을 할 때 이 행을 기준으로 이전 행 10개 이후 행 10개의 데이터만 처리하도록 할 수 있다. frame_clause 여러가지 범위를 지정할 수 있는 키워드가 있다.

#### frame_clause

## 인덱스(INDEX)
### ■ 다중 컬럼 인덱스 (Multiple-column index)
##### 다중 열을 인덱스로 지정 했을 경우 최적화 
다중 열을 인덱스로 지정했을 경우 가장 좌측 열부터 일치 여부를 판단한다.
```sql
CREATE TABLE test (
    id         INT NOT NULL,
    last_name  CHAR(30) NOT NULL,
    first_name CHAR(30) NOT NULL,
    PRIMARY KEY (id),
    INDEX name (last_name,first_name)
);
```
위의 테스트 테이블은 last_name, first_name 순으로 다중 열 인덱스를 지정했다. WHERE 구문으로 데이터를 조회하였을 시 and 구문으로 last_name 과 first_namd을 연결할텐데 이때 열의 순서는 무조건 last_name, first_name 순으로 조회하여야한다. 만약 first_name을 WHERE절에 먼저 사용한다면 FULL SCAN으로 인덱스를 사용할 수 없다. 또한 순서대로 사용했다고 해도 OR연산자로 인덱스 열을 묶은 경우도 불가능하다.
```sql
SELECT * FROM test WHERE first_name='John'; # first_name은 두번째 순서기 때문에 가장 좌측부터 일치한다는 인덱스 원리에 부합

SELECT * FROM test
  WHERE last_name='Jones' OR first_name='John'; # OR 연산자로 다중 열을 지정할 경우 최적화 불가능
```
#### 인덱스가 3개라면?
(col1, col2, col3) 순서대로 인덱스를 지정했을 경우 가능한 경우는 총 4개가지 이다.
* 인덱스로 검색 가능한 경우
  + col1=val1 and col2=val2 and col3=val3
  + col1=val1 and col2=val2
  + col1
col1=val1 and col3=val3는 중간에 col2가 없기 때문에 가장 좌측부터 순서대로 지정해야한다는 인덱스 원칙에 부합하기 때문에 불가능하다.
