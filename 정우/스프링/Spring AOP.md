# 🔖 Spring AOP

## 목차 
### 1. AOP란?
### 2. AOP 용어
### 3. Spring AOP 동작 과정
### 4. AOP 적용 예시 코드
</br>

## 🤔 AOP란?

- AOP란 Aspect Oriented Programming의 약자로 **관점 지향 프로그래밍**을 의미한다
  
- AOP는 핵심적인 비즈니스 로직으로부터 **`횡단 관심사`** 를 분리하는 것에 목적을 둔다
  - 횡단 관심사: 애플리케이션에서 코드가 중복되고, 강력하게 결합되어 있어 **다른 로직과 분리할 수 없는 애플리케이션 로직**
  - ex) 로깅, 트랜잭션, 예외 처리, 보안 등

</br>

- 횡단 관심사 코드 예시

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/e90352f9-c80e-4a68-9c39-280f8e5ccfa9)

- 카트에 물건을 담는 메서드에 **`실행 시간을 측정하는 부가 기능`** 추가

- 만약 이 메서드 뿐 만 아니라 **모든 메서드에 실행 시간을 측정하는 기능을 추가해야 된다면** 코드가 중복되고, 강력하게 결합되는 문제가 발생

- **AOP를 이용하여 부가기능을 따로 관리함으로써 핵심 로직에 집중할 수 있고, 위와 같은 문제점을 해결할 수 있음**

</br>

## 🧐 AOP 용어

- **Target**
  - 부가기능을 부여할 대상 (ex: Service, Controller 등)

- **Advice**
  - `Target` 에게 제공할 부가기능을 담은 모듈 **(어떤 기능을 언제 실행할건지)**
    - `Before` : 메서드가 호출되기 이전에 호출
    - `After` : 메서드가 반환되거나 예외 상황이 발생한 이후에 호출
    - `AfterReturing` : 메소드가 반환된 이후에 호출
    - `AfterThrowing` : 메소드가 예외 상황을 발생시킨 이후에 호출
    - `Around` : 메소드의 호출 전후로 호출

- **Join point**
  - `Advice` 가 적용될 수 있는 위치
  - AOP 를 구현한 자바 구현체인 `AssertJ` 에서는 `메서드`, `필드`, `객체`, `생성자` 등에서 Advice를 적용할 수 있지만, **Spring AOP에서는 `메서드`에서만 Advice가 적용될 수 있음** 

- **Point cut**
  - 실제 `Advice`가 적용될 지점을 결정하는 일종의 필터 역할 (정규표현식으로 선별) 
  - Spring AOP에서는 Advice가 적용될 메서드를 말함 (ex: CartService의 putItemIntoCart 메서드)

- **Advisor**
  - `Point cut`과 `Advice`를 하나씩 가지고 있는 오브젝트

- **Weaving**
  - Join point에 Advice를 적용하는 방법
  - **AspectJ**
    - `컴파일 시점`, `클래스 로드 시점`
  - **Spring AOP**
    - `런타임 시점`
</br>

## 🧐 Spring AOP 동작 과정

</br>

### BeanPostProcessor

</br>

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/3f9576a3-da9f-4b08-8f52-9aeda88d4887)

</br>

- 생성된 빈 객체를 **스프링 컨테이너에 등록하기 전에 조작**하는 객체
  - 생성된 빈 전달
  - 빈 교체 대상 확인 
  - 빈 교체 대상이 맞다면 이를 조작 또는 교체한다
  - 교체되었다면 교체된 빈을, 교체되지 않았다면 받은 빈을 그대로 반환한다
  - 반환된 빈 객체를 컨텍스트에 등록

</br>
</br>

### 프록시

</br>

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/deb7f9c8-73e9-4e8e-abba-156dd2847c73)

</br>

- Spring AOP에서 프록시는 원본 객체를 감싼 새로운 객체로, **`부가기능`을 추가할 때 사용하는 패턴**

</br>
</br>

### 동작 과정

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/e250efab-a4ec-4547-b2ed-0952c2e87689)

</br>

- 빈 객체를 생성한 뒤 빈 후처리기에게 전달한다
- 어드바이저 내의 Point cut을 이용하여 **전달받은 빈이 프록시 적용 대상인지 확인**한다
- 프록시 적용 대상인 경우 프록시 생성기에 해당 빈을 전달하여 **프록시 객체를 생성**한다
- 프록시를 생성한 빈이라면 프록시 객체를 반환하고, 프록시를 생성하지 않은 빈이라면 기존 빈을 반환한다
- 빈 후처리기에게 전달받은 객체를 컨테이너 빈으로 등록한다

</br>

## AOP 적용 예시 코드

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/fc172bed-c0a4-438b-98fe-866f350ff9e5)

</br>

### Point cut 예시

```java
@Pointcut("execution(public * *(..))")
private void anyPublicOperation() {} // (1)

@Pointcut("within(com.xyz.myapp.trading..*)")
private void inTrading() {} // (2)

@Pointcut("anyPublicOperation() && inTrading()")
private void tradingOperation() {} // (3)
```

- anyPublicOperation은 메서드 실행 조인 포인트가 공용 메서드의 실행을 나타내는 경우 일치한다
- inTrading 메서드 실행이 거래 모듈에 있는 경우에 일치한다
- tradingOperation은 메서드 실행이 거래 모듈의 공개 메서드를 나타내는 경우 일치한다
