# Multi Thread


## 목차
1. 스프링의 멀티스레드
2. Thread safe

---

## 1. 스프링의 멀티스레드
- 스프링의 정의 : 자바 `엔터프라이즈` 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크
- 엔터프라이즈 개발 : 기업을 대상으로 하는 개발을 의미, 즉 **서비스를 이용하고자 하는 사용자가 많을 것**이라는 의미

  ∴ Spring의 Tomcat을 포함한 웹 서버는 **멀티스레드** 환경 제공
- 위 환경에서 싱글턴 패턴을 사용하지 않을 경우

  → 수많은 사용자들로 인해 발생하는 요청을 멀티 스레드로 생성된 수많은 스레드가 **처리하는 과정마다 필요한 객체를 생성**해야 함

  → 시스템 성능 저하 및 메모리 낭비 발생
- Spring에서 싱글턴 패턴을 제공하는 이유

  → 위 문제를 개선하기 위해 모든 Bean을 싱글턴 객체로 생성하여 모든 사용자들의 Thread가 공유할 수 있도록 함
  ![스프링 스레드와 싱글톤](https://velog.velcdn.com/images/wldns2577/post/6854727b-8264-463e-b279-c9d417b1de7c/image.png)


## 2. Thread safe
- 정의 : 멀티 스레드 프로그래밍에서, 어떤 공유 자원에 여러 스레드가 동시에 접근해도 프로그램 실행에 문제가 없는 상태
- 멀티스레드의 특징 : 지역 변수를 저장하는 Stack 영역은 각 Thread 별로 할당되지만, 전역 변수를 저장하는 Heap 영역과 함께 Code, Data 영역은 공유
- 스프링의 Thread safe? : 스프링 환경이 기본적으로 Thread safe를 만족하지는 않음, 제공되는 Spring Bean(@Controller, @Service, @Repository, @Component 어노테이션이 달린 객체 등) 또한 **불변 객체**로써 활용되어야 Thread safe를 만족하게 됨
- **불변 객체** : Java에서 class의 instance가 생성된 시점부터 내부 상태가 일정하게 유지되는 객체

```
class MutablePerson {
   public int age; // 전역 변수는 내부 상태가 변경될 수 있으므로 해당 객체는 가변 객체
   public int name; // 마찬가지
    
   public MutablePerson(int age, int name) {
    	this.age = age;
        this.name = name;
    }
}
```

- 불변 객체로 활용하려면? : 전역 변수를 두지 않거나, 전역 변수를 final로 선언

 ```
class ImmutablePerson {
    private final int age; // 외부에서 이 전역 변수의 상태를 변경할 수 없음
    private final int name;
    
    public ImmutablePerson(int age, int name) {
    	this.age = age;
        this.name = name;
    }
}
 ```
 
- 즉, 우리가 사용해왔던 Spring Bean이 **불변 객체**로 만들어져 있었기 때문에 자연스럽게 Thread safe한 멀티 스레드 환경을 만들 수 있었던 것
- 예를 들어, 아래 @Service 코드를 보면 전역변수가 선언되어 있지 않음을 확인 가능

```
package org.springframework.stereotype;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.core.annotation.AliasFor;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any (or empty String otherwise)
	 */
	@AliasFor(annotation = Component.class)
	String value() default ""; // 컴포넌트 스캔 시 개발자가 정한 빈 이름이 있을 경우 그 이름으로 빈 등록

}
```
```
@Service("userServiceOne") // value를 지정했기 때문에 빈 등록 시 userServiceOne으로 등록
public class UserService {
    // ...
}
```
```
@Service // value를 지정하지 않았기 때문에 빈 등록 시 규칙에 따라 userService로 등록
public class UserService {
    // ...
}
```
