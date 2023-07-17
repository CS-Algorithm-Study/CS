# 👨‍💻 DNS

## 목차 
### 1. DNS(Domain Name System)란?
### 2. DNS 동작 순서
### 3. DNS 서버 종류  
</br>

## 🤔 DNS란?

- **도메인 네임 시스템은 호스트의 도메인 네임(www.naver.com) 을 IP주소(223.130.200.104)로 변환하거나, 그 반대의 역할을 수행하는 시스템이다.** <br>

- 우리가 사용하는 `naver.com`, `google.com`을 **`Domain Name`** 이라고 할 수 있다.

- 즉, 우리가 기억하기 어려운 숫자의 나열인 **IP주소**를 기억하기 쉬운 **Domain Name** 으로 바꿔주는 시스템이다.

</br>

## 🧐 DNS 동작 과정
크게 보면 

- 유저가 도메인 명(naver.com) 을 입력
- 도메인 정보가 저장된 네임 서버(DNS 서버)로 가서 도메인과 일치하는 IP주소를 받음
- 해당 IP주소로 접속 

이런식이지만 사실은 더 복잡하다. *(자세한 동작 과정은 바로 아래에서 설명)* <br><br>

👉 **전세계 도메인 수가 너무 많아 DNS 서버 종류를 계층화 해서 단계적으로 처리하기 때문이다**

### 동작 과정 (브라우저에 naver.com 입력하면 일어나는 일)
![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/d76e3c63-f45a-4015-8b5f-43f4a5c7c001)

1. *웹 브라우저에 `www.naver.com` 을 입력하면 먼저 PC에 저장된 Local DNS(기지국 DNS 서버)에게 `www.naver.com` 이라는 `Domain Name`에 대한 IP 주소를 요청한다.* <br>
```
Local DNS(기지국 DNS 서버)란?

인터넷을 사용하기 위해 IP를 할당해주는 통신사(KT, SKT, LG)에 등록하게 되고,
컴퓨터가 LAN선을 통해 인터넷에 연결되면, 가입했던 각 통신사의 기지국 DNS 서버가 자동으로 등록되게 된다.
```
Local DNS에는 `www.naver.com` 의 IP주소가 있을 수도, 없을 수도 있으며, 만약 해당 Local DNS 서버를 이용하는 사용자가 이전에 네이버에 접속했던 전적이 있다면 접속 정보가 캐싱되어 있어 바로 PC에 IP주소를 주고 끝난다. <br>
아래 과정은 Local DNS에 `www.naver.com` IP주소가 없을 때이다. <br><br>

2. *Local DNS에 `www.naver.com` IP주소를 찾아내기 위해 다른 DNS 서버들과 통신(DNS 쿼리)을 시작한다.*
먼저 `Root DNS 서버`에게 `www.naver.com` IP주소를 요청한다.
```
Root DNS(루트 네임서버)란?

Root DNS는 인터넷의 도메인 네임 시스템의 루트 존이다.
ICANN이 직접 관리하는 절대 존엄 서버로, TLD DNS 서버 IP들을 저장해두고 안내하는 역할을 한다.
현재 전세계의 약 1000개가 넘는 루트 DNS가 운영되고 있다.
```
<br>

3. *Root DNS 서버는 "`www.naver.com`의 IP 주소는 찾을 수 없는데 그걸 알만한 놈을 알아" 라고 하며 Local DNS 서버에게 com 도메인을 관리하는 TLD DNS 서버(최상위 도메인 서버)의 주소를 알려준다.*
 <br>
 
4. *이제 com 도메인을 관리하는 TLD DNS 서버에게 `www.naver.com`에 대한 IP주소를 요청한다.*

```
TLD DNS(Top-Level-Domain) 서버란?

TLD는 도메인 등록 기관(Registry)이 관리하는 서버로, 도메인 네임의 가장 마지막 부분인 `.com`이나 `co.kr', `.net` 등을 말한다.
```
<br>

5. *com 도메인을 관리하는 DNS 서버에도 해당 정보가 없으면, TLD DNS는 naver.com DNS 서버(Authoritative DNS 서버)의 주소를 알려준다.*

```
Authoritative DNS Server 란?

실제 개인 도메인과 IP 주소의 관계가 기록/저장/변경되는 서버.
그래서 권한의 의미인 Authoritative가 붙는다.
일반적으로 도메인/호스팅 업체의 ‘네임서버’를 말하지만, 개인이나 회사 DNS 서버 구축을 한 경우에도 여기에 해당하게 된다.
```
<br> 

6. *Local DNS 서버는 naver.com DNS 서버(Authoritative DNS 서버)에게 다시 `www.naver.com` 의 IP 주소를 요청한다.* <br>

7. *최종적으로 naver.com DNS 서버에는 `www.naver.com`의 IP 주소가 있다.* 
그래서 Local DNS 서버에게 `www.naver.com`에 대한 IP 주소는 `223.130.200.104`라는 응답을 한다.
<br>

8. *이를 수신한 Local DNS 서버는 `www.naver.com`의 IP주소를 캐싱하고, 이후 다른 요청이 있을 시 바로 응답한다.*
<br>

이렇게 Local DNS 서버가 여러 DNS 서버에 차례대로 (Root DNS 서버 → TLD DNS 서버(.com) → Authoritative DNS 서버(naver.com) 요청하여 그 답을 찾는 과정을 재귀적 쿼리 `Recursive Query` 라고 부른다.

<br><br>

## 🧐 DNS 서버 종류
- 앞에서 간단하게 설명을 했고, 추가적인 설명이 필요한 서버만 따로 설명하겠음

<br>

### Root DNS 서버
Root DNS는 최상위 DNS서버로 해당 DNS부터 시작해서 아래 딸린 node DNS 서버에게로 차례차례 물어보게 되는 구조로 짜여져 있다. <br>
![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/a7420e64-5f68-48aa-b7a4-88f6342aeeac)

- 즉, 모든 DNS 서버들은 이 Root DNS Server의 주소를 기본적으로 갖고 있다는 말이다.
- 그래서 모르는 Domain name이 온다면 가장 먼저 Root DNS에게 물어보게 되는 것이다.
- 하지만 Root DNS Server의 목록에도 해당 Domain Name의 IP 정보가 없다면 다음 DNS 서버로 리턴을 해주는데, 그것이 바로 TLD(최상위 도메인) 서버 이다.
- 도메인이 google.com 이라면 뒤의 문자를 보고 .com을 관리하는 TLD 서버에게 물어보라고 정보를 주는 것이다.

Root DNS Server : *"나한탠 해당 도메인 주소가 없다. 대신 google.com의 주소중 .com의 주소를 알고 있으니, com DNS주소에게 물어봐라."*

</br>

### TLD 서버
- 루트도메인 바로 아래단계에 있는 것을 1단계도메인이라고 하며 이를 TLD(최상위 도메인)이라고 한다.
- TLD(최상위 도메인은 국가명을 나타내는 국가최상위도메인과 일반적으로 사용되는 일반최상위도메인으로 구분된다.
- 도메인을 구입할 경우 1단계의 도메인중에 하나를 선택하고 원하는 도메인명을 지정하여 등록한다.
![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/b91d3723-89b3-43db-8b4c-86b5a6d74f61)

### Second-level DNS 서버 (2차 도메인)
- 위에서 TLD 서버에 요청하여 naver DNS 서버를 return 했었다.
- return해준 서버로 다시 요청하면 Second DNS 서버는 자체적으로 sub 도메인 서버로 또 넘기게 된다.

<br>

### Sub DNS 서버 (최하위 서버)
- 서브 도메인 서버는 www. dev. mail. cafe. 등등 을 구분하는 최하위 서버를 말한다.
- naver 서버라도 그 안에서 네이버 홈, 메일, 블로그, 카페 등 여러 서비스가 있다. 이 서비스들을 구분하는 도메인 네임이라고 보면 된다. <br>

ex) **www**.naver.com, **mail**.naver.com, **blog**.naver.com, **cafe**.naver.com
