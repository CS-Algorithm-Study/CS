<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/5598768b-baa4-4067-b552-d55053e6e824" width="70%">

## 데이터베이스의 구성 방식

데이터베이스는 기본적으로 데이터베이스 서버와 데이터베이스가 저장되는 스토리지가 1:1로 구성된다.

하지만 서비스를 운영하다 보면 DB 서버에서 트랜잭션을 수용하지 못하거나, DB 스토리지에 저장된 데이터가 손상될 경우가 있다. 클러스터링과 리플리케이션은 이러한 문제점을 해결하기 위해 고안된 DB 서버 - DB 스토리지 구성 방식이다.

## 두 개 이상의 서버를 하나로 묶어서 운영하는 클러스터링

- 데이터베이스에서 클러스터링은 여러 개의 DB 서버가 하나의 스토리지를 나눠서 처리하는 방식
- 데이터베이스 클러스터링은 **Active - Active**와 **Active - Stand by** 방식으로 나뉨
<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/e2855d60-b30b-4faa-83d6-bb0dbd4b9597" width="70%">

- Active - Active 방식의 장점은 하나의 DB 서버가 중단되더라도 다른 DB 서버가 작동할 수 있음
- 서버가 두개이기 때문에 DB 서버의 CPU, 메모리 등이 두 배가 되어 가용성 측면에서도 두 배 이상의 성능을 발휘할 수 있음
- Active-Active 방식은 스토리지 하나를 공유하기 때문에 병목이 생길 수 있음
- 두 서버를 동시에 운영하기 때문에 비용 부담이 생길 수 있음

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/fd482f94-843c-4d43-a7c6-17a2a83337f6" width="70%">

- Active - Stand by는 하나의 DB 서버는 Active(작동) 상태로, 나머지 DB 서버는 Stand by(대기) 상태로 둠
- Active된 DB 서버에 문제가 생길 경우, Stand by 서버를 Active 서버로 작동하는 방식
- Active - Stand by의 장점은 운영 비용 절감
- Stand by 서버는 평소에 작동하지 않기 때문에 Active 상태의 DB 서버 비용만 지출하면 됨
- 반면, 실제로 Stand by 서버는 평소에 작동하지 않기 때문에 Active 서버로 전환하기까지 시간이 수십 초에서 수십 분까지 소요됨

---

## **데이터베이스 스토리지를 복제하는 리플리케이션**

- 리플리케이션은 다양한 이슈로 데이터 유실이 생길 경우를 대비해서 스토리지까지 복제함으로써 데이터의 유실을 최소화하는 것
- 리플리케이션은 Source(Master)와 Replica(Slave)라는 수직 구조로 구축

- 리플리케이션을 구축하는 목적 4가지
    1. 스케일 아웃: 갑자기 늘어나는 트래픽에 대해 부하를 줄이기 위해 서버를 늘려 성능을 개선
    2. 백업: 백업 과정에서 쿼리 손상이 입을 가능성이 있어 Source 데이터에 영향을 미치지 않고 Replica에서 데이터 백업을 진행할 수 있음
    3. 데이터 분석: Master 서버 성능에 영향을 주지 않고 Slave에서 데이터 분석을 수행할 수 있음
    4. 데이터의 지리적 분산: Source 서버와 물리적인 거리가 있더라도 Replica 서버를 통해 응답을 받을 수 있고, 속도도 높일 수 있음

- 단순 백업은 두 개 이상의 데이터베이스 서버와 스토리지를 Source(Master)와 Replica(Slave)로 나눠서 동일한 데이터를 저장하는 방식
- 분산 방식은 Slave DB를 백업용으로 활용하기 아까워 Slave db는 부하 분산을 하는 방식으로 사용함

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/23ffe6bf-4bdd-466a-8a3c-4e9240cbbb42" width="70%">

- Slave를 백업 용도로 활용하는 방식은 두 개 이상의 데이터베이스 서버와 스토리지를 Master와 Slave로 나눠 동일한 데이터를 동기화 및 저장하는 방식

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/bad25fef-b056-47c8-b151-3e9b99e06a97" width="70%">

- Slave를 읽기 용도로 활용하는 방식은 Master의 부하를 줄이기 위해 Select 작업을 Slave에서 하도록 구성하는 방식
- Select 작업에 시간이 걸리기 때문에 다른 작업을 하기 어렵기 때문에 Slave를 통해 분산처리 할 수 있어 성능 향상에 도움을 줄 수 있음
- 리플리케이션의 단점으로는 Master, Slave는 서로 다른 서버를 운영하기 때문에 별도의 버전 관리를 해야
    - 이때 Master와 Slave의 데이터베이스도 동일하게 맞춰줘야 함
- 데이터의 정확도를 보장할 수 없음
    - Slave가 Master의 데이터 처리 속도를 따라가지 못한다면 두 개의 데이터는 일치하지 않을 수 있음
