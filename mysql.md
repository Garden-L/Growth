# MYSQL
```mysql
create table department(
	dep_id integer not null primary key,
    dep_name varchar(20)
);
drop table employee;
create table employee (
	emp_id integer not null primary key,
    emp_name varchar(20),
    dep_id integer,
    foreign key(dep_id) references department(dep_id)
);

insert into department values(1, "개그맨부서");
insert into department values(2, "PD부서");
insert into department values(3, "MC부서");

insert into employee values (1, "유재석", 3);
insert into employee values (2, "나PD", 2);
insert into employee values (3, "김구라", 3);
insert into employee values (4, "신동엽", 3);
insert into employee values (5, "김PD", 2);
insert into employee values (6, "신PD", 2);
insert into employee values (7, "허경환", 1);
insert into employee values (8, "오나미", 1);
```
## FORIEGN KEY(외부키)

### 문법
```mysql
[CONSTRAINT [symbol]] FOREIGN KEY
    [index_name] (col_name, ...)
    REFERENCES tbl_name (col_name,...)
    [ON DELETE reference_option]
    [ON UPDATE reference_option]

reference_option:
    RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT
```
#### CONSTRAINT \[SYMBOL\]
제약 조건의 이름이다. 따로 이름을 지정하지 않으면 DBMS에서 자동으로 이름을 부여한다.

#### \[INDEX NAME\]

#### \[ON DELETE reference_option\]
ON DELETE 옵션은 외부키 설정이 된 행이 참조하는 테이블의 행이 **삭제**될 때 참조하는 테이블의 행을 어떻게 처리할지에 대한 조건이다.

#### \[ON UPDATE reference_option\]
ON UPDATE 옵션은 외래키 설정이 된 속성이 참조하는 테이블의 속성 값이 **변경**될 때 속성을 어떻게 처리할지에 대한 조건이다.

#### reference_option
* SET NULL
  + ON DELETE, ON UPDATE 둘 다 사용 가능하며, 부모 테이블의 삭제 또는 변경되면 자식테이블의 속성 값이 NULL로 설정된다.
  + 단, 자식 테이블에서 해당 속성이 NOT NULL이 설정되어 외부키 조건으로 SET NULL을 지정할 수 없다.
* CASCADE
  + ON DELETE, ON UPDATE 둘 다 사용가능하며, 부모 테이블이 변경되거나 삭제되면 자식 테이블도 변경 또는 삭제된다.
* RESTRICT
  + ON DELETE, ON UPDATE 둘 다 사용가능하며, 자식 테이블에서 삭제하려는 부모테이블의 속성을 참조하고 있으면 부모 테이블에 대한 삭제, 변경 작업을 못 하도록 제한한다. 
  + ON DELETE, ON UPDATE를 따로 지정하지 않으면 **RESTRICT가 디폴트 옵션**이 된다.
* NO ACTION
  + MYSQL에서는 RESTRICT와 같은 기능이다
* SET DEFAULT
  + InnoDB와 NDB에서는 SET DEFAULT 구문을 사용할 수 없다.


### RANK()
#### 기본 RANK()문법
```mysql
SELECT [열],RANK() OVER (ORDER BY [정렬할 열, ...] {ASC : DEFUALT, DESC}) [as ...] FROM {테이블 이름} 
```
공동 순위가 있을 경우 다음 순위는 건너뛴다.  
예) 1위가 2명일 경우 2등이 없고 바로 3등

#### DENSE_RANK()
공동 순위가 있는 경우 그 다음 순위는 이어간다.  
예) 1등이 2명이면 그 다음 순위는 2등부터 시작한다.
```mysql
SELECT [열,...], RANK() OVER (ORDER BY [정렬할 열,...] {ASC : DEFUALT, DESC}) [AS ... ] FROM {테이블이름}
```

#### 그룹별 랭킹 매기기
RANK() 함수는 기본적으로 전체에 대해서 순서를 매긴다. 특정 컬럼의 그룹별로 등수를 매길려면 **PARTITION BY**를 써야된다.
```mysql
SELECT [열,...], RANK() OVER (PARTITION BY {그룹핑할 열} ORDER BY [정렬할 열,...] {ASC : DEFUALT, DESC}) [AS ... ] FROM {테이블이름}
```
