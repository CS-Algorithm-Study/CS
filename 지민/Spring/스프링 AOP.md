# 스프링 AOP(Aspect Oriented Programming)

AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것을 의미한다. 여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다.

예를 들어 핵심적인 관점은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직이 된다. 또한 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있다.

AOP에서 각 관점을 기준으로 로직을 모듈화한다는 것은 코드들을 부분적으로 나누어서 모듈화하겠다는 의미다. 이때, 소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는 데 이것을흩어진 관심사 (Crosscutting Concerns)라 부른다.
<img width="451" alt="image" src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/cdebe3b7-5f81-4612-9b84-f89a81e97d7c"></br>

위와 같이 흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지다.

## AOP 주요 개념
- Aspect : 위에서 설명한 흩어진 관심사를 모듈화 한 것. 주로 부가기능을 모듈화함.
- Target : Aspect를 적용하는 곳 (클래스, 메서드 .. )
- Advice : 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체
- JointPoint : Advice가 적용될 위치, 끼어들 수 있는 지점. 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용 가능
- PointCut : JointPoint의 상세한 스펙을 정의한 것. 'A란 메서드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음

## AOP 적용 방법
1. 컴파일 타임 적용
    - 컴파일 시점에 바이트 코드를 조작하여 AOP가 적용된 바이트 코드를 생성하는 방법
2. 로드 타임 적용
    - 순수하게 컴파일한 뒤, 클래스를 로딩하는 시점에 클래스 정보를 변경하는 방법
3. 런타임 적용
    - 스프링 AOP가 주로 사용하는 방법
    - A라는 클래스 타입의 Bean을 만들 때 A타입의 Proxy Bean을 만들어 Proxy Bean이 Aspect 코드를 추가하여 동작하는 방법

## 스프링 AOP 특징
- 스프링에서 제공하는 스프링 AOP는 프록시 기반의 AOP 구현체
- 프록시 객체를 사용하는 것은 접근 제어 및 부가 기능을 추가하기 위해서
- 스프링 AOP는 스프링 Bean에만 적용할 수 있음
- 모든 AOP 기능을 제공하는 것이 목적이 아닌 중복 코드, 프록시 클래스 작성의 번거로움 등 흔한 문제를 해결하기 위한 솔루션을 제공하는 것이 목적임
- 스프링 AOP는 순수 자바로 구현되었기 때문에 특별한 컴파일 과정이 필요하지 않음

### 프록시 패턴
<img width="419" alt="image" src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/8871c6a4-7e25-4f82-bf7a-d612e9f1b706">

- 프록시 패턴에서는 interface가 존재하고 client는 이 interface 타입으로 proxy 객체를 사용함
- proxy 객체는 기존의 타겟 객체(Real Subject)를 참조함
- proxy 객체와 기존의 타겟 객체의 타입은 같고, proxy는 원래 할 일을 가지고 있는 Real Subject를 감싸서 client의 요청을 처리함

## 스프링 AOP 구현
gradle

```java
implementation 'org.springframework.boot:spring-boot-starter-aop'
```

maven

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
- 스프링 AOP를 사용하기 위해서는 위와 같이 의존성을 추가해줘야 함

- 예시) **타켓 메서드의 실행 시간을 측정해주는 로직**

```java
@Component
@Aspect
public class PerfAspect {

    @Around("execution(* com.example..*.EventService.*(..))")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
        long begin = System.currentTimeMillis();
        Object reVal = pjp.proceed();
        System.out.println(System.currentTimeMillis() - begin);
        return reVal;
    }
}
```

- 스프링 AOP는 Bean에서만 동작한다고 했기 때문에 `@Component` 어노테이션 등을 통해 스프링 Bean으로 등록해준 뒤 사용해야 함
- `@Aspect` 어노테이션을 붙이면 해당 클래스가 Aspect라는 것을 명시해줌
    - Aspect: 흩어진 관심사를 모듈화 한 것. 주로 부가기능을 모듈화함
- logPerf() 메서드는 `@Around` 어노테이션의 execution 속성을 통해 Advice를 적용한 범위를 지정해줄 수 있음
    - Advice: 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체
    - 위의 코드를 예시로 들면 com.example 아래 모든 클래스에 서용하고, EventService 아래 모든 메서드에 적용하라는 뜻

- **경로 지정 방식이 아닌 특정 어노테이션이 붙은 포인트에 해당 Aspect를 실행할 수도 있음**

```java
@Component
@Aspect
public class PerfAspect {

  @Around("@annotation(PerfLogging)")
  public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
    long begin = System.currentTimeMillis();
    Object retVal = pjp.proceed();
    System.out.println(System.currentTimeMillis() - begin);
    return retVal;
  }
}
```

- `@Around` 어노테이션에 `@annotation(PerfLogging)` 처럼 적용될 어노테이션을 명시할 수 있음
- 해당 메서드를 적용시킬 특정 메서드에 `@PerfLogging` 어노테이션을 붙여주기만 하면 logPerf() 기능이 동작함

- **특정 Bean 전체에 해당 기능을 적용시킬 수도 있음**

```java
// Aspect 정의
@Component
@Aspect
public class PerfAspect {

  // 빈이 가지고있는 모든 퍼블릭 메서드
  @Around("bean(simpleServiceEvent)")
  public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
    long begin = System.currentTimeMillis();
    Object retVal = pjp.proceed();
    System.out.println(System.currentTimeMillis() - begin);
    return retVal;
  }
}
```
- `@Around` 어노테이션에 bean(simpleServiceEvent)처럼 적용될 Bean을 명시할 수 있음
- 해당 빈이 가지고 있는 모든 public 메서드에 해당 기능이 적용됨

- **@Around 외에 타겟 메서드의 Aspect 실행 시점을 지정할 수 있는 어노테이션**
1. @Before: Advice 타겟 메서드가 호출되기 전에 Advice 기능 수행
2. @After: 타겟 메서드의 결과에 관계없이 타겟 메서드가 완료되면 Advice 기능 수행
3. @AfterRunning: 타겟 메서드가 성공적으로 결과값을 반환한 후에 Advice 기능 수행
4. @AfterThrowing: 타겟 메서드가 수행 중 예외를 던지면 Advice 기능 수행
5. @Around: Advice가 타겟 메서드를 감싸 타겟 메서드 호출 전, 후에 Advice 기능 수행

[출처](https://code-lab1.tistory.com/193)
