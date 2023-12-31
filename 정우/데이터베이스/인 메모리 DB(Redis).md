# 📲 인 메모리 DB
## 목차
### 1. 인 메모리 DB란?
### 2. Redis란?
### 3. Redis 사용 시 주의사항

</br>

## 🤔 인 메모리 DB란?

- **`인 메모리` 란 컴퓨터의 메인 메모리에 데이터를 올려서 사용하는 방법을 말한다**
- **즉, `인 메모리 DB` 는 컴퓨터의 메인 메모리를 이용하여 DB를 사용하는 것을 말한다**
- 메인 메모리를 DB로 사용하게 되면 **`속도`** 가 수백배 이상 빨라진다
  
  ![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/ac33fd5c-4db7-4790-af75-bb13196cd7db)
- **`속도`** 가 매우 빠른 반면, 용량이 작고 용량을 늘리려면 높은 비용이 발생한다
- 따라서 적합한 인 메모리 DB의 사용 방법은 캐시 DB로 사용하는 것이 괜찮다고 개인적으로 생각한다 

</br>

## 🤔 Redis란?

- **`Redis` 는 대표적인 인메모리 DB이다**

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/c6107e19-fc89-4af5-8292-f689f404e9aa)


- 단어의 의미를 보면 외부에서 key-value를 저장하는 서버를 말한다
- [redis.io](https://redis.io/) 에선 Redis를 인메모리 데이터 구조 저장소로, 데이터베이스, 캐시, 메시지 브로커로 사용한다고 말한다
- Redis는 주로 캐시로 이용된다
- **`Local Cache`** 보다는 **`Global Cache`** 에 적합하다
### Cache 
- **Local Cache**
  - Local 장비 내에서만 사용 되는 캐시로 Local 장비의 Resource(메모리, 디스크)를 이용한다
  - Local에서만 사용 되기 때문에 속도가 빠른 반면, 다른 서버와의 데이터 공유는 어렵다
  
- **Global Cache**
  - 여러 서버에서 Cache Server에 접근하여 사용하는 캐시이다
  - 데이터를 분산하여 저장할 수 있다
  - Local Cache에 비해 상대적으로 느린반면(네트워크 트래픽), 별도의 Cache Server를 이용하는 것이기 때문에 서버 간 데이터 공유가 쉽다

### Redis 데이터 구조
- redis는 기본적으로 key-value 형태의 데이터 저장소이지만, 다음과 같은 다양한 종류의 데이터 구조를 지원한다
  - 문자열(String)
  - 해시
  - 리스트(List)
  - 집합(Set)
  - 정렬된 집합(Sorted Set)
  - 비트맵, 하이퍼로그 로그, 지리공간 인덱스, 스트림 등
- 다양한 형태의 데이터 구조를 사용할 수 있어 큰 이점이 있다
- 예를 들어, 상위 랭킹 100위의 정보를 저장한다고 했을 때 redis가 제공하는 **`sorted set`** 을 이용하면 쉽게 랭킹데이터를 관리할 수 있다

</br>

## 🧐 Redis 사용 시 주의사항

- **Redis는 `메모리` 에 데이터를 저장하여 사용하기 때문에 `메모리 관리` 가 가장 중요하다!**

### 메모리 SWAP

- **메인 메모리가 부족할 경우 메모리에 올린 프로세스들이 부족한 공간을 해결하기 위해 `하드디스크` 에 SWAP 공간을 만들어 임시 저장하게 된다**
  
- 메모리 부족을 해결할 순 있겠지만, 이렇게 되면 Redis의 본래 사용 목적(캐시)에 어긋나게 된다

- Redis가 느려졌다면 메모리가 부족하기 때문에 메모리 SWAP이 발생한 것으로 프로세스를 재실행해야 한다

<br>

### 메모리 단편화
- 저번에 설명했던 내용으로 자세한 설명은 생략하고, **다양한 크기의 데이터 사용을 줄이고 유사한 크기의 데이터를 사용하면 메모리 단편화를 어느정도 해결할 수 있다**

</br>

### 싱글 스레드
- **Redis는 `싱글 스레드`로 동작한다. 즉, 동시에 처리할 수 있는 명령어의 개수는 1개이다**
- **'싱글 스레드'** 의 단점을 생각해보면 Redis를 어떤 식으로 사용해야 할 지 감이 올 것이다!
- 간단하게 요약하면, 무거운 명령을 지양하고, 적절하게 나눠서 명령해야 한다 
