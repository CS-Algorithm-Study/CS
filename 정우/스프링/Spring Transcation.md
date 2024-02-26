# 스프링 트랜잭션

## 목차 
### 1. 스프링 트랜잭션
### 1-1. 트랜잭션 동기화
### 1-2. 트랜잭션 추상화
### 1-3. 선언적 트랜잭션
</br>

## 🤔 스프링 트랜잭션

- 스프링은 **트랜잭션 동기화**, **트랜잭션 추상화**, **선언적 트랜잭션**을 지원한다
- 우리가 자주 사용하는 **@Transactional** 어노테이션이 바로 선언적 트랙잭션이다

### 트랜잭션 동기화

먼저 기존에는 트랜잭션 동기화가 어떻게 이루어졌고 어떤 문제점이 있었으며, 스프링은 어떻게 해결했는 지 살펴보자.

</br>

스프링 없이 트랜잭션을 동기화 하는 코드(jdbc 사용)
```java
public interface MemberRepository {

    Long save(Connection conn, Member member);

    Member findById(Connection conn, Long id);
}

public class MemberService {
    ...
    public void updateMember(Long id, UpdateRequest updateRequest) throws SQLException {
        Connection conn = getConnection();
        conn.setAutoCommit(false);

        try {
            Member member = memberRepository.findById(conn, id);
            member.update(updateRequest);
            memberRepository.save(conn, new Member(joinRequest));
            conn.commit();
        } catch (Exception e) {
            conn.rollback();
        } finally {
            conn.setAutoCommit(true);
            conn.close();
        }
    }
}
```
**문제점**
- 매번 Repository 로직을 호출할 때마다 Connection 객체를 전달해야 한다 
- 사용하고자 하는 Data Access 기술이 바뀌면 고수준 명세인 인터페이스가 변경된다

</br>

스프링에서는 트랜잭션 동기화를 지원하기 위해 **트랜잭션 동기화 매니저**를 이용한다

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/47f3fb28-8e8e-4f61-90e6-f96fa420e9ad)

</br>

스프링을 통해 트랜잭션을 동기화 하는 코드(jdbc)
```java
public interface MemberRepository {

    Long save(Member member);

    Member findById(Long id);
}

public class MemberService {
    ...
    public void updateMember(Long id, UpdateRequest updateRequest) throws SQLException {
        TranscationSynchronizationManager.initSynchronization();
        Connection conn = DataSourceUtils.getConnection(dataSource);
        conn.setAutoCommit(false);

        try {
            Member member = memberRepository.findById(id);
            member.update(updateRequest);
            memberRepository.save(new Member(joinRequest));
            conn.commit();
        } catch (Exception e) {
            conn.rollback();
        } finally {
            DataSourceUtils.releaseConnection(conn, dataSource);
            TranscationSynchronizationManager.unbindResource(dataSource);
            TranscationSynchronizationManager.clearSynchronization();
        }
    }
}
```

- 이제는 Repository를 호출할 때마다 Connection 객체를 전달해주지 않아도 된다
- 하지만 내부적으로 위의 서비스 코드를 보면 spring jdbc 기술인 DataSourceUtils에 의존하고 있고 결과적으로 Data Access 기술에 의존하고 있는 문제는 해결되지 않는다
<br>

### 트랜잭션 추상화
앞서 Data Access에서 특정 기술에 의존하게 된 이유는 데이터베이스 접근 기술마다 데이터베이스의 연결 방법이 다르기 때문이다

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/4fc5c1dc-fdaf-4bdc-b541-0161f8481bf4)

<br>

**하지만 실제로 하는 일을 보면**
- 트랜잭션을 가져온다 / 생성한다
- 해당 트랜잭션을 commit 한다
- 해당 트랜잭션을 rollback 한다

**가져오는 Transcation 객체만 다를 뿐 하는 일은 같다**

따라서 추상화가 가능하고 스프링은 다음과 같이 트랜잭션 추상화를 제공한다.

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/fc1b3231-d2b1-45c2-899f-77d6caa2614d)

```java
public interface PlatformTransactionManager extends TransactionManager {
    
      TransactionStatus getTransaction(@Nullable TransactionDefinition definition)
              throws TransactionException;
              
      void commit(TransactionStatus status) throws TransactionException;
      void rollback(TransactionStatus status) throws TransactionException;
}
```
<br>

이제 추상화가 적용된 코드를 살펴보면
```java

public class MemberService {
    private final PlatformTransactionManager transactionManager;

    public MemberService(PlatformTransactionManager transactionManager) {
        transactionManager = transactionManager;
    }

    public void updateMember(Long id, UpdateRequest updateRequest) {
        TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition());

        try {
            Member member = memberRepository.findById(id);
            member.update(updateRequest);
            memberRepository.save(new Member(joinRequest));
            transactionManager.commit(status);
        } catch (Exception e) {
            transactionManager.rollback();
        } 
    }
}
```
- 트랜잭션 매니저를 초기화하거나 하는 코드도 제거되었고, 트랜잭션 경계를 설정하는 코드가 더 간결해졌으며, Data Access 기술에 의존하지 않게 되었다.
- 하지만 비즈니스 로직이 아닌 다른 관심사(트랜잭션)를 위한 코드가 Service 코드에 존재하고 중복되는 것이 신경쓰인다

### 선언적 트랜잭션

선언적 트랜잭션은 우리가 사용하는 @Transactional 어노테이션이다

선언적 트랜잭션을 적용한 코드를 살펴보면,
```java
public class MemberService {
    ...
    @Transcational
    public void updateMember(Long id, UpdateRequest updateRequest) {
        Member member = memberRepository.findById(id);
        member.update(updateRequest);
        memberRepository.save(new Member(joinRequest));
    } 
```
- 이제는 트랜잭션을 위한 코드마저 제거되면서 모든 문제를 해결할 수 있게 되었다

<br>

아래와 같이 핵심적인 비즈니스 로직의 앞 뒤로 실행되는 부가 기능을 처리하기 위해서 이전에 Spring AOP 를 통해 해결했었다

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/5226bdce-27b1-48a9-bac7-8285e2116a5f)

마찬가지로 트랜잭션 또한 부가 기능을 처리하기 위한 것이기 때문에 Spring AOP를 이용해 구현되었다
