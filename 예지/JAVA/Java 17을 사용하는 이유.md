<img width="466" alt="스크린샷 2024-01-04 오후 4 52 06" src="https://github.com/DAHLIACHOI/medicine/assets/48826098/2e793218-f5b1-47bf-9e8f-c44fb03d614b">


2021년까지 자바8의 점유율이 가장 많았다. 따라서 자바 8이 자바 11보다 더 긴 지원 기간을 갖고 있다.

| Java 8 | ~2030.12 |
| --- | --- |
| Java 11 | ~2026.09 |
| Java 17 | ~2029.09 |

> 여기서 의문인건 자바 8이 자바 17보다 지원기간이 더 긴데 자바 17을 굳이 사용하는 이유는?
> 

# ☕️ Java 17을 사용하는 이유

1. **Java 11보다 지원기간이 길다.** 
2. **Java 8이나 Java 11을 사용했을 때 보다 신규 버전으로 마이그레이션하기에 리스크가 적어진다.**
3. **SpringBoot 3.0 부터 Java 17 이상을 지원한다.**
    
    > 현재 SpringBoot 2.x.x는 23년 11월에 지원이 종료되었다.
    > 
4. **Java 17에서 가비지 컬렉션 알고리즘(ZGC, Shenandoah GC)이 개선되어 메모리 관리 효율이 향상되었으며, 컴파일러 최적화 기술이 업그레이드되어 애플리케이션의 실행 속도와 응답 시간이 개선되었다.**
5. **Java 17에서 암호화 및 인증 알고리즘의 최신 표준을 지원해서 웹 서비스 및 애플리케이션의 보안을 강화한다.**
6. **새로운 메서드 추가**

# 🔎 Java 17  메서드

### 1. recode

recode는 데이터 클래스를 의미하며 DTO같은 클래스를 쉽게 정의할 수 있다.

- getter가 자동적으로 생성된다.
- equals, hashcode, toString이 자동적으로 생성된다.
- 멤버변수가 private final로 선언된다.
- 기본생성자는 지원하지 않으므로 필요한 경우 직접 생성해야 된다.
- final 클래스이므로 다른 클래스를 상속하거나 상속시킬 수 없다.
- private final fields 이외의 인스턴스 필드를 선언할 수 없다.

**before**

```java
public class Response {
	
	private final String name;
	private final int age;

	public Response(final string name, final int age){
		this.name = name;
		this.age = age;
	}

	public String getName(){
		return name;
	}

	public int getAge(){
		return age;
	}
}
```

**after**

```java
public recode Response(
	String name;
	int age;
){}
```
<br>

### 2. Stream.toList()

Collectors.toList()를 .toList()로 표현할 수 있다.

**before**

```java
.stream()
.collect(Collector.toList())
```

**after**

```java
.stream()
.toList();
```
<br>

### 3. Switch Expression

기존의 switch ~ case: break 보다 간결하게 사용할 수 있다.

- 실행문은 `->` 을 이용해서 나타낸다.
- 조건들은 `,` 를 이용해 나열한다.
- break를 사용하지 않는다.

before

```java
switch(a){
	case "aa":
	case "bb":
		break;
}
```

after

```java
switch(a){
	case "aa", "bb"
		-> System.out.println("~~~");
}
```
<br>

### 4. 테스트 블록

기존에 문장을 합치려면 +를 사용했어야 해서 가독성이 안좋았는데 Java 17에서는 테스트 블록을 사용해서 표현할 수 있다.

**before**

```java
String jsonString = "{\n" +
    "  \"name\": \"Jinny\",\n" +
    "  \"age\": 20\n" +
    "}";
```

```java
@Query("select m from Member m"
				+ "where m.id = :id")
```

**after**

```java
String jsonString = """
        {
          "name": "Jinny",
          "age": 20
        }
        """;
```

```java
@Query("""
			select m from Member m
			where m.id = :id
			""")
```
