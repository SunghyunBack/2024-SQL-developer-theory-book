
# 관계형 데이터베이스(Relatioin DataBase)
 - 1970년 E.F. Codd박사의 논문에 처음 소개 되었다
 - 릴레이션(Relation)과 조인 연산을 통해 합집합, 교집합, 차집합 등을 만들수 있다.
TIP - 릴레이션(Relation)은 데이터들의 표(Table)의 형태로 표현한것으로 구조를 나타내는 릴레이션 스키ㅏ와 실제 값들인 릴레이션 인스턴스로 구성된다.
![image](https://github.com/user-attachments/assets/655281af-91ae-4d66-8b65-1d9517bfba82)


● 데이터베이스와 데이터베이스 관리 시스템의 차이
- 데이터베이스는 데이터를 어떠한 형태의 자료구조(Data Structrue)로 사용하느냐에 따라서 나누어진다.
- 데이터베이스의 종류로는 계층형, 네트워크형, 관계형 등이 있다.
![image](https://github.com/user-attachments/assets/726d4e40-81e0-4c28-9c49-80ca624dd439)

- 데이터베이스 관리 시스템(Database Management System)은 데이터베이스 자료구조등을 관리하기 위한 소프트웨어를 의미하면 DBMS라고 한다.
- DBMS의 종류는 오라클 등 잇으며 모두 관계형 데이터베이스를 지원한다. 

● 관계형 데이터베이스 집합 연산과 관계 연산
- 집합연산
  - 합집합(Union) : 2개의 릴레이션을 하나로 합하는것, 중복된 행(튜플)은 한번만 조회
  - 차집합(Difference) : 본래 릴레이션에는 존재하고 다른 릴레이션에는 존재하지 않는것을 조회
  - 교집합(Intersection) : 두 개의 릴레이션 간에 공토된 것을 조회한다.
  - 곱집합(Cartesian product) : 각 릴레이션에 존재하는 모든 데이터를 조합하여 연산한다.
 
- 관계연산
  - 선택 연산(Selection) : 릴레이션에서 조건에 맞는 행(튜플)만을 조회한다.
  - 투명 연산(Projection) : 릴레이션에서 조건에 맞는 속성만을 조회한다.
  - 결합 연산(Join) : 여러 릴레이션의 공통된 속성을 사용해서 새로운 릴레이션을 만들어 낸다.
  - 나누기 연산(Division) : 기준 릴레이션에서 나누는 릴레이션이 가지고 있는 속성과 동일한 값을 가지는 행(튜플)을 추출하고 나누는 릴레이션의 속성을 삭제한 후 중복된 행을 제거하는 연산
 

- 테이블
  ![image](https://github.com/user-attachments/assets/c98feabe-ad17-4890-8139-a7f9adf2126c)

 - 기본키(Primary key)는 하나의 테이블에서 유일성과 최소성, Not Null을 만족하면서 해당 테이블을 대표한다. EMP테이블에서는 사원번호가 기본키에 해당된다.
 - 테이블은 행과 칼럼으로 구성되어 있으며 행은 하나의 테이블에 저장되는 값으로 튜플(Tuple)이라고도 한다.
 - 칼럼(Column)은 어떤 데이터를 저장하기 위한 필드(Field)로 속성(Attribute)이라고도 한다.
 - 외래키(Foreign key)는 다른 테이블의 기본키를 참조(whdls)하는 칼럼이다. EMP 테이블에서는 부서코드는 DEPT테이블의 기본키인 부서코드를 참조한다.
 - 외래키는 관계 연산 중에서 결합 연산(join)을 하기 위해서 사용한다.



# SQL종류

● SQL(Structured Query Language)
 - 데이터의 구조를 정의, 데이터 조작, 데이터 제어 등을 할수 있는 절차형+비절차형 언어이다.
 - SQL문을 사용하여 데이터베이스를 누구나 쉽게 사용할수 있다.
 - ANSI/ISO 표준을 준수하기 때문에 관리 시스템이 변경되어도 그대로 사용할수 있다.

● SQL 종류

- 종류
 - DDL(Data Definition Language) :  관계형 데이터베이스의 구조를 정의하는 언어 / CREATE, ALTER, DROP, RENAME, TRUNCATE문이 있다.
 - DML(Data Manipulation Language) : 테이블에서 데이터를 입력,수정, 삭제, 조회한다. / INSERT, UPDATE< DELETE, SELECT문이 있다. 
 - DCL(Data COntrol Language) : 데이터베이스 사용자에게 권한을 부여하거나 회수한다. / GRANT, REVOKE문이 있다.
 - TCL(Transaction Control Language) : 트랜잭션을 제어하는 명령어이다. / COMMIT, ROLLBACK, SAVEPOINT문이 있다.

- 트랜잭션(Transaction)
 - 데이터베이스의 작업을 처리하는 단위이다.

 - 트랜잭션의 특성
    - 원자성(Atomicity) : 데이터베이스 연산의 전부가 실행되거나 전혀 실행되지 않아야 한다(ALL OR NOTHING)
    - 일관성(Consistency) : 실행 결과로 데이터베이스의 상태가 모순되지 않아야 한다. 실행 후에도 일관성이 유지되어야 한다.
    - 고립성(Isolation) : 실행 중 생성하는 연산의 ㅈ우간결과는 다른 트랜잭션이 접근할수 없다.
    - 영속성(Durability) : 트랜잭션이 그 실행을 성공적으로 완료하면 그 결과는 영구적 보장이 되어야 한다.
  

- SQL문의 실행 순서
   - 파싱(Parsing) : SQL문의 문법을 확인하고 구문 분석한다.
   - 실행(Execution) : 옵티마이저(Optimizer)가 수립한 실행 계획에 따라 SQL을 실행한다.
   - 인출(Fetch) : 데이터를 읽어서 전송한다.



- SELECT문
  
