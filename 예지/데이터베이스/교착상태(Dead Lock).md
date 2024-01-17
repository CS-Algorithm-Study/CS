# DB 교착 상태(Dead Lock)

> 데이터베이스에서 교착 상태는 여러 개의 트랜잭션들이 실행하지 못하고 서로 무한정 기다리는 상태를 말한다. 즉, 두 개 이상의 트랜잭션이 특정 자원의 lock을 획득한 채 다른 트랜잭션이 소유하고 있는 잠금을 요구하면 아무리 기다려도 상황이 바뀌지 않은 상태이다.
> 

➕ 운영체제에서의 교착 상태는 각각의 프로세스가 서로의 자원을 점유하기 위해 대기하면서 생기는 문제를 말한다.

<br>

### ▪️ 교착상태가 일어나는 상황

트랜잭션 1이 테이블 B에 insert를 하고, 트랜잭션 2가 테이블 A에 insert를 하고 나서 서로가 lock을 걸었던 행에 insert 작업을 시도한다면 두 개의 트랜잭션 모두 waiting이 발생하고 교착상태(Dead Lock)에 빠지게 된다. 

<br>

# 교착상태 해결방법

- 예방 기법
- 회피 기법
- 낙관적 병행 제어 기법
- 빈도 줄이기 기법

<br>

### ▪️ 예방기법

1. 각 트랜잭션이 실행되기 전에 필요한 모든 자원을 lock 한다.
    
    → 필요한 모든 데이터를 lock 해야하므로 병행성이 떨어진다.
    
2. SET LOCK_TIMEOUT문을 통해 일정 시간이 지나면 쿼리를 취소한다.
    - 기존의 교착상태인 데이터가 있다면, 그 데이터에 접근하는 쿼리만 취소한다,
    
    → 근본적인 해결책이 될 수 없다.
    

### ▪️ 회피 기법

자원을 할당할 때 시간 스탬프(time stamp)를 활용하여 교착상태가 일어나지 않도록 회피하는 기법이다. 

예방 기법의 단점 때문에 실제로는 회피 기법이 많이 사용된다.

1. Wait-Die 방식
    ![1541_Wait-die scheme -Deadlock prevention](https://github.com/CS-Algorithm-Study/CS/assets/48826098/337f0cdc-46c2-4d22-b484-764d6b097eff)

    
    - 트랜잭션 1이 트랜잭션 2에 의해 잠금된 데이터를 요청할 때 트랜잭션 1이 먼저 들어온 트랜잭션이라면 대기(wait)한다.
    - 트랜잭션 1이 나중에 들어온 트랜잭션이라면, 포기(die)하고 나중에 다시 요청한다.
2. Wound-Wait 방식
    ![2389_Wound-wait scheme-deadlock prevention](https://github.com/CS-Algorithm-Study/CS/assets/48826098/687cd0ac-32f8-44ce-a4d4-94cb753b0746)

    
    - 트랜잭션 1이 트랜잭션 2보다 먼저 들어온 트랜잭션이라면, 데이터를 선점(wound)한다.
    - 반면, 트랜잭션 1이 트랜잭션 2보다 나중에 들어온 트랜잭션이라면 대기(wait)한다.

<br>

### ▪️ 낙관적 병행 제어 기법

트랜잭션이 실행되는 동안에는 검사를 수행하지 않고, 트랜잭션이 커밋된 후에 데이터에 문제가 있다면 롤백하는 방법이다.

👉 판독 → 확인 → 기록의 단계를 따른다. 확인 단계를 성공적으로 거친 트랜잭션만 기록 단계를 수행할 수 있다.

<br>

### ▪️ 빈도 줄이기

- 트랜잭션을 자주 커밋한다.
- 정해진 순서로 테이블에 접근한다.
- 읽기 잠금 (SELECT ~ FOR UPDATE)의 사용을 피한다.
- 테이블 단위의 lock을 획득해 갱신을 직렬화 한다. (한 테이블의 복수 행을 복수의 연결에서 순서없이 갱신하면 교착 상태가 자주 발생한다. 그래서 테이블 단위의 잠금을 획득해 갱신을 직렬화 하면 동시성이 떨어지지만 교착 상태를 피할 수 있다.)


<br>

**reference**

[https://velog.io/@yrkim/Database-트랜잭션-deadlock](https://velog.io/@yrkim/Database-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-deadlock)

https://jaehoney.tistory.com/162 

https://www.expertsmind.com/questions/wait-die-scheme-deadlock-prevention-30141349.aspx 

https://www.expertsmind.com/questions/wound-wait-scheme-deadlock-prevention-30141353.aspx
