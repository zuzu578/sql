# sql

# 데이터베이스 정규화
# 1차 정규화
각로우마다 컬럼의 값이 1개씩만 있어야한다 ( 컬럼이 원자값을 갖는다고 한다 ) 

# 2차 정규화
테이블의 모든 컬럼이 완전 함수적 종속을 만족해야한다.(기본키중 특정 컬럼에만 종속된 컬럼(부분적 종속)이 없어야한다.)


# 3차 정규화 
테이블을 분리함으로써 삽입 , 삭제 , 수정등의 문제를 해결한다. 

# 시퀄라이즈에서 paranoid 설정후 강제 삭제하는법
시퀄라이즈에서 paranoid 를 모델에 설정하면 소프트 삭제가 되는데 , 강제삭제를 원한다면 다음과같이한다. 

```typescript
export const deleteEmployeeScheduleByIdx = (option: string) => {
  return ScheduleEmployee.destroy({
    where: {
      scheduleIdx: option,
    },
    force: true,
  });
};

```
# Bulk insert

mysql 에서 대량으로 insert를 수행하게 해주는 sql문이다.
# 예제
```mysql
# 일반 insert

insert into table (a,b,c) values(1,2,3);
insert into table (a,b,c) values(4,5,6);
insert into table (a,b,c) values(7,8,9);

# bulk

insert into table(a,b,c) values (1,2,3),(4,5,6),(7,8,9);

이외에도 대용량 데이터파일을 가지고 db테이블을 만들어 insert 할수 있는 기능도 제공한다. 

```
# transction 의 개념

트랜잭션이란 데이터베이스의 상태를 변화시키기 위해서 수행하는 작업의 단위를 의미

*작업단위는 질의어 한문장이 아님을 명심한다.
*작업단위는 많은 질의어 명령문들을 사람이 정하는 기준에 따라 정하는 것을 의미한다.

# transaction의 특징
1) 원자성 : 트랜잭션이 데이터베이스에 모두 반영 , 아니면 전혀 반영되지 않아야한다는것 (예를들어서 유저등록시 , 유저는 등록되었지만 프로필 사진은 등록되지않은경우에는 유저와 프로필 사진이 반영되지 않아야한다.)
2) 일관성 : 트랜잭션 작업 처리 결과가 항상 일관성이 있어야한다. 즉 , 트랜잭션이 진행되는 동안에 데이터베이스가 변경 되더라도 업데이트된 데이터베이스로 트랜잭션이 진행되는것이 아니라 , 처음에 트랜잭션을 진행하기위해 참조한 데이터베이스로 진행한다.
3) 독립성 : 둘이상의 트랜잭션이 동시에 실행되고있을경우 어떤하나의 트랜잭션이라도 다른 트랜잭션의 연산에 끼어들수없다. 즉 하나의 특정 트랜잭션이 완료될때까지 다른 트랜잭션이 특정 트랜잭션의 결과를 참조할수 없다.
4) 지속성 : 트랜잭션이 성공적으로 완료되었을 경우 결과는 영구적으로 반영되어야한다.

# 트랜잭션의 commit , rollback 

# commit
하나의 트랜잭션이 성공적으로 끝났고 데이터베이스가 일관성있는 상태에 있을때 하나의 트랜잭션이 끝났다는것을 알려주기 위해 사용하는연산이다.

이 연산을 사용하면 수행했던 트랜잭션이 로그에 저장되며 , 후에 rollback 연산을 수행했었던 트랜잭션단위로 하는것을 도와준다.

# rollback
롤백이란 하나의 트랜잭션 처리가 비정상적으로 종료되어 트랜잭션의 원자성이 깨진경우, 트랜잭션을 처음부터 다시 시작하거나 , 트랜잭션의 부분적으로만 연산된 결과를 다시 취소한다.

# 1:1 관계
어느 엔티티 쪽에서 상대 엔티티와 반드시 단 하나의 관계를 가지는것

# 1:N 관계 
한쪽 엔티티가 관계를 맺은 엔티티 쪽의 여러 객체를 가질수 있는것을 의미
# N: M 관계 ( 다대 다 관계 )
관계를 가진 양쪽 엔티티 모두에서 1대 n관계를 가지는것을 말한다.
