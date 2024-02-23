# JWT


## 목차
1. 개념
2. 구조
3. 장점
4. 단점
5. Refresh token

---

## 1. 개념
- `JWT (JSON Web Token)` : 두 개체에서 JSON 객체를 사용하여 가볍고 **자가수용적인** (self-contained) 방식으로 정보를 안정성 있게 전달
  - `자가수용적` : JWT 안에 인증에 필요한 모든 정보를 자체적으로 지니고 있음
    
  ![jwt](https://velog.velcdn.com/images/jiun2577/post/523a5ecc-88e7-431e-9692-a574dd53609d/image.png)

## 2. 구조
![구조](https://velog.velcdn.com/images/jiun2577/post/bd38e4d0-ebd5-440d-941e-a30bb27ef20c/image.png)

### 헤더 (Header)
- typ : 토큰의 타입 (JWT)
- alg : 해싱 알고리즘 (Signature 를 해싱하기 위한 알고리즘 지정)
### 내용 (Payload)
- `클레임 (claim)` : 토큰에 담을 정보의 한 조각을 의미
  - `등록된 클레임 (registered)`<br>
  : 서비스에서 필요한 정보들이 아닌, 토큰에 대한 정보들을 담기 위해 이름이 이미 정해진 클레임. 모두 선택적(optional)으로 사용

  > iss: 토큰 발급자 (issuer)<br>
  > sub: 토큰 제목 (subject)<br>
  > aud: 토큰 대상자 (audience)<br>
  > exp: 토큰의 만료시간 (expiraton), 시간은 NumericDate 형식(예: 1480849147370)이며 항상 현재 시간 이후로 설정<br>
  > nbf: 토큰의 활성시간 (not before), exp와 마찬가지로 NumericDate 형식이며, 이 날짜가 지나기 전까지는 토큰 처리 X<br>
  > iat: 토큰이 발급된 시간 (issued at), 이 값을 사용해 토큰의 age 판단 가능<br>
  > jti: JWT의 고유 식별자로서, 주로 중복적인 처리를 방지를 위해 사용. 일회용 토큰에 유용.<br>
  
  - `공개 클레임 (public)`
  : 사용자 정의 클레임으로 공개용 정보를 위해 사용, 충돌 방지를 위해 **URI 포맷**을 이용

  ```
  {
	"https://naver.com": true
  }
  ```
  
  - `비공개 클레임 (private)`
  : 사용자 정의 클레임으로 서버와 클라이언트 사이에 임의로 지정한 정보를 저장, 이름이 중복되어 충돌이 발생할 수 있으므로 사용 시 유의

  ```
  {
	"username": "wiz"
    "email": "wiz@gmail.com"
  }
  ```
### 서명 (Signature)
- 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 **고유한 암호화 코드**
> `서명 생성 과정`
> 1. 헤더와 페이로드 값을 각각 BASE64 로 인코딩
> 2. 위에서 인코딩한 값을 비밀 키를 이용해 헤더에서 정의한 알고리즘으로 해싱
> 3. 위에서 해싱한 값을 다시 BASE64 로 인코딩
## 3. 장점
- 인증에 필요한 정보가 모두 토큰에 들어있으므로 별도의 저장소 필요 X<br>
  단, 보안성을 높이기 위해 Refresh Token을 사용하는 경우 별도의 저장소에 저장하면서 사용하는 경우 존재
- Cookie & Session 사용 시 문제점이였던 stateful<br>
  → JWT 토큰 사용 시 **stateless** 특성 만족 (서버가 클라이언트의 상태를 저장할 필요 X)
- HTTP 헤더에 넣어 쉽게 전달 가능
- 확장성에 용이하므로 **MSA 환경 적용에 유리**
## 4. 단점
- 거의 모든 요청에 토큰이 포함되므로 트래픽 크기에 영향
- 토큰에 정보가 많아져 토큰의 크기가 커지면 네트워크에 부하 발생 가능
- 페이로드는 암호화된 것이 아니라 BASE64로 인코딩된 것<br>
  → 중간에 토큰을 탈취당하면 페이로드의 데이터를 제3자가 확인 가능 (보안 문제)
## 5. Refresh token
- Access Token은 로컬 스토리지 또는 세션 스토리지에 저장
- Refresh Token은 쿠키에 저장하며 보안 옵션들(HTTP Only, Secure Cookies)을 활성화, **서버에 저장**
  
![refresh](https://velog.velcdn.com/images/jiun2577/post/975e334a-ee2f-4a0a-a8ae-6ff5ee74b4dd/image.png)

> `HTTP Only Cookies`<br>
> 클라이언트에서 일반적으로 자바스크립트를 통해 쿠키 조회 가능<br>
> → 해당 옵션을 활성화 하면 브라우저에서 쿠키에 접근할 수 없으므로 XSS와 같은 공격으로부터 안전.

> `Secure Cookies`<br>
> HTTP 프로토콜은 언제든지 패킷을 중간에 가로챌 수 있는 문제 존재<br>
> → 보안개념을 추가한 HTTPS 프로토콜을 사용하여 데이터를 암호화해 통신<br>
> if. HTTPS 로 전송되어야 할 정보가 휴먼에러로 인해 HTTP 로 전송될 경우?<br>
> → HTTPS 프로토콜이 아닌 경우에는 쿠키를 전송하지 않도록 해당 옵션 활성화

- 여전히 공격자는 탈취한 Refresh Token 으로 계속 Access Token을 생성해서 정상적인 사용자처럼 서버에 요청 가능<br>
  → DB에 사용자와 Access Token, Refresh Token 들을 매핑하여 저장

  > `정상적인 유저의 Access Token이 만료된 경우`<br>
  > 1. Access Token, Refresh Token 쌍을 서버로 보내 새 Access Token을 요청<br>
  > 2. 서버에서는 DB에 저장된 Access Token, Refresh Token 쌍과 클라이언트에서 보낸 토큰 쌍을 비교 후, 일치하면 새 Access Token 토큰 발급<br>

  > `공격자가 Refresh Toekn을 탈취한 경우`<br>
  > 1. 공격자가 탈취한 Refresh Token으로 새 Access Token 생성 요청<br>
  > 2. Access Token 없이 요청하면 공격으로 간주, 서버에서 Access Token, Refresh Token 폐기<br>
