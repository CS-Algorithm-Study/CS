# DBMS


## 목차
1. 개념
2. 분류
3. NoSQL

---

## 1. 개념
- DBMS(Database Management System)
  : DB를 관리하고 운영하는 소프트웨어
- 상용되는 DBMS 종류 및 특징
  ![DBMS 종류 및 특징](https://github.com/CS-Algorithm-Study/CS/assets/90232934/4a4fabbb-bb9d-43e2-9fe6-106baa4c7805)
  
## 2. 분류
- 계층형
  - 1960년대 처음 등장한 DBMS 개념
  - 각 계층을 트리 구조로 연결
  - 하위 계층에서 다른 하위 계층으로 탐색하는 과정이 비효율적이라는 한계 존재 → 현재 사용 X
  ![계층형](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EA%B3%84%EC%B8%B5%ED%98%95DBMS.png)
  
- 망형
  - 1970년대 계층형의 문제를 개선한 개념
  - 트리 구조에서, 하위 계층끼리도 연결 가능한 비교적 유연한 구조
  - 프로그래머가 복잡하게 연결된 모든 구조를 이해해야 프로그램 작성 가능하다는 한계 존재 → 현재 사용 X
  ![망형](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EB%A7%9D%ED%98%95DBMS.png)
  
- **관계형 (RDBMS)**
  - 위의 한계를 극복하기 위한 개념
    - 초기 모델들의 수직적 구조에서 벗어나 "수평적, 2차원 구조"
      - 2차원 구조 예시) 엑셀 시트
    - 초기 모델은 특정 정보 검색 시, 해당 정보 탐색 과정 및 방법을 각각 개별 어플리케이션으로 구현 필요 → 질의어 문법을 활용해 질의 조건만 나열하면 원하는 정보를 손쉽게 탐색 가능
  - 1980년대 이래로 재무기록, 물류 정보, 인사 데이터 등의 정보 저장 목적으로 가장 널리 사용되는 스토리지 형태
  - 테이블(행, 열)이라는 최소 단위로 구성, key 값을 바탕으로 테이블 간의 관계 형성
  ![관계형](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/sql_table.png)

- 객체지향형
  - 1990년대 이후로 사용자 정의 데이터, 멀티미디어 데이터 등의 저장 및 처리 필요성 대두
  - DB에 프로그래밍의 객체지향 기술을 접목
  - 사용자 정의 타입 지원 및 비정형 복합 정보 모델링 가능
  - 객체 간 reference 구조를 통한 접근 가능

- 객체관계형
  - 기존의 관계형 + 객체지향형 장점 융합
  - 관계형에서 개선된 점
    - 사용자 정의 타입 지원
    - 참조(reference) 타입 지원
    - 중첩 테이블 지원 (테이블을 구성하는 Row 자체가 또 다른 테이블로 구성될 수 있음)
    - 대단위 객체(LOB = Large Object) 타입을 기본으로 지원하여 저장 및 추출 가능
    - 객체 간 상속 관계 지원
      - 예) 오라클은 OBJECT 타입 지원으로 상속 기능 구현

## 3. NoSQL
- NoSQL의 등장
  - 2000년대 후반 이래로 인터넷의 활성화, SNS 등장 → 관계형(정형) 데이터가 아닌 데이터 = **비정형데이터**의 저장 및 처리 필요성 대두
  - RDBMS에서 보장하는 ACID 특성을 제공하지 않으나 뛰어난 확장성 및 성능을 갖는 비관계형, 분산 DBMS의 총칭

- RDBMS 대비 NoSQL의 특징
  - 관계형 모델 X, 테이블 간 JOIN 기능 X
  - 직접 프로그래밍 등 비SQL 인터페이스를 통한 데이터 접근
  - **클러스터링**하여 하나의 DB 구성
    - 클러스터링 : 여러 대의 DB 서버를 묶음
  - Transaction ACID 미보장
  - **데이터의 스키마, 속성을 다양하게 수용 및 동적 정의 (Schema-less)** → 확장성
  - DB의 중단 없는 서비스, 자동 복구 기능 → 가용성
  - 단순 검색 및 추가 작업에 매우 최적화 → 응답속도 및 처리 효율 고성능

- NoSQL의 분류
  - Key Value DB
    - Key - Value 쌍으로 데이터가 저장되는 가장 단순한 형태
    - Riak, Vodemort, Tokyo, etc.
  - Wide Columnar Store (= Big Table DB)
    - Key Value에서 발전된 형태의 Column Family 데이터 모델 사용
    - HBase, Cassandra, etc.
  - Document DB
    - JSON, XML 등 Collection 데이터 모델 구조
    - MongoDB, CoughDB, etc.
  - Graph DB
    - Nodes, Relationship, Key-Value 데이터 모델 구조
    - Neo4J, OreientDB, etc.
