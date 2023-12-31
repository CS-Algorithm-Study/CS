## RESTful API vs GraphQL

</br>

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/752e97f0-d75f-4d20-9860-862866241a87" width="80%">

</br>

- RESTful API와 GraphQL은 모두 API 구조를 설계하고 데이터를 처리하기 위한 방식

1. RESTful API
- HTTP 메서드(GET, POST 등)을 사용해 API를 구축하기 위한 아키텍처 스타일
- 일반적으로 URL을 통해 사용자가 요청한 주소를 식별하고 JSON의 형태로 데이터를 반환함
- 클라이언트에서 서버로 각 요청을 보낼 때 필요한 모든 정보 포함

2. GraphQL
- 페이스북이 개발한 API용 쿼리 언어
- 클라이언트가 정확히 필요한 데이터를 명시해 요청하고, 서버는 요청된 특정 데이터만을 응답하는 방식
- 보다 효율적인 데이터 송수신이 가능하게 만든다는 장점

---

### RESTful API 장점

1. 개발자 친화적이며 단순함
2. 캐싱 지원
    - 캐싱을 지원하므로 서버에 대한 요청 수를 줄임으로써 API의 성능을 향상시킬 수 있다.
3. 풍부한 라이브러리
4. 호환성

### RESTful API 단점

1. 불필요한 데이터 전송
    - RESTful API는 고정된 응답 구조를 가지기 때문에 클라이언트가 필요로 하지 않는 데이터도 함께 전송될 수 있다.
    - 이는 대역폭 낭비와 불필요한 데이터 전송으로 이어질 수 있다.
2. Over-fetching 및 Under-fetching
    - 클라이언트가 필요로 하는 데이터 양과 서버에서 반환되는 데이터 양이 일치하지 않을 수 있다.
    - Over-fetching은 불필요한 데이터를 받는 경우를, Under-fetching은 필요한 데이터를 모두 받지 못하는 경우를 의미한다.
3. 버전 관리의 어려움
    - API가 발전함에 따라 새로운 기능을 추가하거나 수정할 때 기존 버전과의 호환성을 유지하기가 어려울 수 있다.
    - 새로운 엔드포인트를 생성하거나 새로운 버전을 배포해야 할 수도 있다.
4. 클라이언트와 서버 간의 강력한 결합
    - 클라이언트와 서버 간의 인터페이스가 강하게 결합되어 있어, 클라이언트 측에서 서버에 의존하는 상황에서 변경이 어려울 수 있다.

---

### GraphQL 장점

1. 효율성 향상
    - 클라이언트가 필요한 데이터를 정확히 지정할 수 있으므로 네트워크를 통해 전송되는 데이터의 양이 줄고 API 효율성을 높일 수 있다.
2. 유연한 쿼리
    - 클라이언트 측에서 여러 리소스와의 관계를 포함하는 복잡한 쿼리를 만들 수도 있으며, 쿼리 중첩을 통해 계층적 방식으로 데이터를 검색할 수도 있다.
3. 버전 관리
    - 예를 들면 버전이 1.0에서 2.0으로 올라간 경우 기존 1.0 서버를 중단하지 않고 간단하게 스키마만 변경하여 이를 구현할 수 있다.
    - 이전 버전과의 호환성을 유지하면서도 API를 변경할 수 있다.
4. 개발자 경험
    - 클라이언트 측의 개발자가 편하게 일할 수 있다.
    - 백엔드 측과 URL을 맞춰서 작업을 진행해야 하는 번거로움을 없앨 수 있다.

### GraphQL 단점

1. 초기 학습의 어려움
    - GraphQL은 RESTful API보다 학습 곡선이 높을 수 있다.
    - 특히 처음에는 GraphQL 스키마 및 쿼리 작성에 대한 이해가 필요하므로 초기 학습이 어려울 수 있다.
2. 보안 고려 사항
    - 클라이언트가 원하는 데이터를 자유롭게 요청할 수 있기 때문에, 적절한 보안 설정이 이루어지지 않으면 민감한 정보에 대한 노출이 발생할 수 있다.
3. 서버 부담
    - 복잡한 쿼리나 대용량 요청이 들어올 경우 서버에 부담을 줄 수 있다.
    - 이에 대비하여 쿼리의 복잡성에 대한 제한을 설정하는 것이 필요하다.
4. 파일 업로드 지원의 한계
    - GraphQL은 기본적으로 Multipart 방식의 파일 업로드를 지원하지 않는다.
    - 이로 인해 이미지, 영상 등의 파일 업로드에 어려움을 겪을 수 있다.
5. 캐싱의 어려움
    - RESTful API보다 캐싱이 어렵다는 주장도 있다.
    - 일부 캐싱 전략은 각각의 쿼리에 대한 캐싱을 구현하기 어려울 수 있다.

---

</br>

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/ce7c0531-1080-4e43-9750-b032e9a5dade" width="80%">

</br>

### 어떤 경우에 사용해야하는가?
- 두 가지 모두 장단점이 명확하다. RESTful API의 경우 모든 엔드포인트를 개발하고 여러 경우의 수를 대응해야하기에 개발 속도가 상대적으로 느리고, GraphQL의 경우 원칙적으로 Multipart 방식의 전송이 허용되지 않기에 이미지, 영상 등의 처리에 굉장히 취약하다.
- RESTful API
    - HTTP 와 HTTPs 에 의한 **Caching**을 잘 사용하고 싶을 경우
    - **Multipart** 방식의 전송이 필요한 경우 (이미지, 영상, 파일 등)
    - 요청의 **구조가 정해진** 경우
- GraphQL
    - **다양한 형태**의 요청 및 응답이 필요한 경우
    - 대부분의 요청이 **CRUD**(Create, Read, Update, Delete) 기반인 경우
    - **개발자 경험**을 중시하는 프로젝트에서 사용하는 경우

---

### 실습
<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/c22cdd9f-b291-4366-9f5b-f2c0b9235261" width="70%" align="center">

</br>

- JPA 사용해서 간단한 컨트롤러 생성
 
```
@Controller
public class PersonController {

    private final PersonRepository personRepository;

    public PersonController(PersonRepository personRepository) {
        this.personRepository = personRepository;
    }

    @SchemaMapping(typeName = "Query", value = "findAll")
    public List<Person> findAll() {
        return personRepository.findAll();
    }

    @SchemaMapping(typeName = "Query", value = "findById")
    public Optional<Person> findById(@Argument Long id) {
        return personRepository.findById(id);
    }
}
```

- @SchemaMapping 어노테이션을 통해서 GraphQL 요청에서 사용되는 value를 지정할 수 있다.
- 스프링부트 프로젝트를 실행하고 http://localhost:8080/graphiql 접속
 
</br>

#### findAll 테스트
<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/9f047caf-04f7-4edd-8576-2c75d31386b6">

</br>

#### findById 테스트
<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/76967124-1c17-4b52-8bb6-251b34377293">

- 참고
    - https://americanopeople.tistory.com/330
    - https://velog.io/@sjy0917/API-RESTful-vs-GraphQL
