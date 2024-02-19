# 🔎 Spring Cache란?

자주 사용되는 데이터를 매번 요청할 때마다 생성하여 응답하는 것보다는 생성된 데이터를 저장해놓고 똑같은 요청이 왔을 때 로직을 거치지 않고 데이터를 반환해주어 빠른 검색을 가능하게 하는 기능을 제공한다.

- 로컬 캐시이기 때문에 application간 공유가 되지 않고 서버를 끌 때 데이터가 날라가므로 계속 유지하고 싶다면 redis같은 외부 저장소를 사용하는 것이 좋다.

<br>

### 사용 해야 하는 곳

- 클라이언트에게 전달되는 값이 동일할 때
- 빈번하게 호출될 때
- 한 번 처리할 때 많은 서버 리소스를 요구할 때

<br>

### 사용하지 말아야할 곳

- 실시간으로 정확성을 요구하는 경우
- 빈번하게 데이터 변경이 일어나는 경우

<br><br>

## 스프링 설정

```java
implementation 'org.springframework.boot:spring-boot-starter-cache
```

<br>

## 어노테이션

| 어노테이션 | 설명 |
| --- | --- |
| @EnableCache | Spring Boot Cache를 사용하기 위해 ‘캐시 활성화’를 위한 어노테이션 |
| @CacheConfig | 캐시 정보를 ‘클래스 단위’로 사용하고 관리하기 위한 어노테이션 |
| @Cacheable | 캐시 정보를 메모리 상에 ‘저장’하거나 ‘조회’해오는 기능을 수행하는 어노테이션 |
| @CachePut | 캐시 정보를 메모리상에 ‘저장’하며 존재 시 갱신을 수행하는 어노테이션 |
| @CacheEvict | 캐시 정보를 메모리상에 ‘삭제’하는 기능을 수행하는 어노테이션 |
| @Caching | 여러 개의 ‘캐시 어노테이션’을 함께 사용할 때 사용하는 어노테이션 |

<br>

## @Cacheable, @CachePut, @CacheEvict

```java
@Cacheable(cacheNames = "memberCache", key = "#key")
public Member getCacheData(final String key) {
	log.info("해당 key에 대한 캐시가 없는 경우 로그가 찍힌다.");
	return Member;
}

@CachePut(cacheNames = "memberCache", key = "#key")
public Member updateCacheData(final String key, final String value) {
	log.info("해당 key에 대한 캐시가 없데이트 되는 경우 로그가 찍힌다.");
	Member member = new Member();
	member.setValue(value);
	member.setExpirationDate(LocalDateTime.now().plusDays(1));
	return member;
}

@CacheEvict(cacheNames = "memberCache", key = "#key")
public boolean expireCacheData(final String key) {
	log.info("해당 key에 대한 캐시를 지울 경우 로그가 찍힌다.");
	return true;
}
```

<br>

```java
@Cacheable(cacheNames = "memberCache", key = "#key")
```

> 해당 key에 대한 데이터가 존재한다면 getCacheData가 실행되지 않고 기존 데이터가 return 된다.
메서드가 실행되지 않기 때문에 로그도 찍히지 않는다.
> 

<br>
<br>

```java
  @Cacheable("memberCacheStore")
  public Member cacheable(String date) {
	log.info("cache 저장");
	return member;
  }

  @Cacheable(value = "memberCacheStore", key = "#member.name")
  public Member cacheableByKey(Member member) {
	log.info("cache 저장(key 지정)");
	return member;
  }

  @Cacheable(value = "memberCacheStore", key = "#member.name", condition = "#member.name.length() > 5")
  public Member cacheableWithCondition(Member member) {
	log.info("조건부 cache 저장");
	return member;
  }
```

- spEL(Spring Expression Language)문법을 사용하여 String , Integer, Long같은 값은 **#변수명** 형태로 사용되고, 객체 안에 멤버 변수를 비교하는 경우에는 **#객체명.멤버명** 형태로 사용한다.


<br><br>

## @CachePut vs @Cacheable

> @CachePut과 @Cacheable은 유사하게 실행 결과를 캐시에 저장하지만, @Cacheable은 **캐시 정보를 조회**하는 데 주로 사용되고 @CachePut은 **캐시 정보를 저장**하는데 주로 사용된다.
> 
- @CachePut은 무조건 메서드를 실행하게 되고, @Cacheable은 캐시 데이터가 없는 경우에만 실행하게 된다.
