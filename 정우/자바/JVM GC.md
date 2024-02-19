## 목차
### 1. JVM Garbage Collection 이란?
### 2. JVM Garbage Collection 동작 방법
### 3. JVM Garbage Collection 종류
</br>

## 🤔 JVM Garbage Collection 이란?

</br>

- 자바의 메모리 관리 방법 중의 하나로 JVM(자바 가상 머신)의 **`Heap`** 영역에서 **동적으로 할당했던 메모리** 중 필요 없게 된 메모리 객체를 모아 주기적으로 제거하는 프로세스를 말한다

C / C++ 언어는 가비지 컬렉션(Garbage Collection, 이하 GC)가 없어 프로그래머가 수동으로 메모리 할당과 해제를 일일히 해줘야 하지만 Java는 GC가 메모리 관리를 대행해주기 때문에 다음과 같은 장단점이 존재한다

### 장점

1. Java 프로세스가 한정된 **메모리를 효율적으로 사용**할 수 있음
2. 개발자 입장에서 메모리 관리, 메모리 누수 문제에 대해서 벗어나므로 **개발에만 집중**할 수 있음

### 단점

1. 메모리가 언제 해제되는지 정확하게 알 수 없어 제어하기 힘들다
2. GC가 동작하는 동안에는 다른 동작을 멈추기 때문에 오버헤드가 발생한다(Stop The World)

#### Stop The World(STW)
- STW는 GC를 수행하기 위해 JVM이 프로그램 실행을 멈추는 현상을 의미한다
- GC가 작동하는 동안 GC 관련 쓰레드를 제외한 모든 쓰레드가 멈추게 되어 서비스 이용에 차질이 생길 수 있다

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/0c9bb834-f88e-40e8-bb4b-6fec22b9282a)

- GC가 너무 자주 실행되면 성능 하락의 문제가 되기도 한다. (특히 실시간 성이 강조되는 프로그램)
- 따라서 어플리케이션의 사용성을 유지하되 효율적으로 GC를 실행해야 하고, 이러한 GC 최적화 작업을 **`GC튜닝`** 이라고 한다

## JVM Garbage Collection 동작 원리

### GC 대상
GC는 특정 객체가 garbage인지 아닌지 판단하기 위해서 도달성 이라는 개념을 적용한다.
객체의 레퍼런스(참조 값)가 있다면 **`Reachable`** 로 구분되고, 객체의 유효한 레퍼런스가 없다면 **`Unreachable`** 로 구분해버리고 수거해버린다. 

- Reachable : 객체가 참조되고 있는 상태
- Unreachable  : 객체가 참조되고 있지 않은 상태 (GC의 대상이 됨)

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/7f9b91a2-ee3c-40fc-836a-8f65b4e2117c)

- JVM 메모리에서 객체를 실질적으로 Heap 영역에서 생성되고, `Method Area` 나 `Stack` 에서는 객체의 주소를 참조하는 형식으로 구성된다
- 메서드가 끝나거나 특정한 이벤트들로 인하여 `Method Area` 나 `Stack` 에서 Heap에 있는 객체의 참조 값이 삭제되면 **어디서든 참조하고 있지 않는 객체**가 되고 GC를 통해 제거되는 것이다.

### Mark And Sweep (GC 동작 방법)
가비지 컬렉션이 될 대상 객체를 **`식별(Mark)`** 하고 **`제거(Sweep)`** 하며 객체가 제거되어 파편화된 메모리 영역을 앞에서부터 채워나가는 **`작업(Compaction)`** 을 수행하게 된다.

- Mark : GC Root 로 부터 참조 값을 쭉 따라가며 연결된 객체들을 찾아내어 각각 어떤 객체를 참조하고 있는 지 찾아서 마킹한다
- Sweep : 참조하고 있지 않은 객체 즉 `Unreachable` 객체들을 Heap에서 제거한다
- Compact : Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축한다. (메모리 단편화 문제를 해결)

<br>

#### heap 메모리의 구조

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/9880186f-ba0e-44c8-a374-a14016be486f)

Heap영역은 처음 설계될 때 다음의 2가지를 전제 (Weak Generational Hypothesis)로 설계되었다.

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

**즉, 객체는 대부분 일회성되며, 메모리에 오랫동안 남아있는 경우는 드물다는 것이다.**

이러한 특성을 이용해 JVM 개발자들은 보다 효율적인 메모리 관리를 위해, 객체의 생존 기간에 따라 물리적인 Heap 영역을 나누게 되었고 Young 과 Old 총 2가지 영역으로 설계하였다.

#### Minor GC

#### 1

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/3787fbb8-fbed-495a-b386-d2a5dade39f9)

#### 2

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/18975b9c-6004-43d2-9e09-d53d28dd2e8e)

#### 3

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/f577161c-3414-4031-95d1-8947b64f3b71)

#### 4

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/fa31c3e7-934c-4fc9-8446-2d9fc2dd15f5)

#### 5

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/7927ca39-8df4-4484-9bd9-c082779dde19)

#### 6

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/63228cfd-b00f-4e43-9ea0-5da079a1b82a)

#### 7

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/5309ae44-d087-40b4-bec3-20a8e20780ea)

#### 8

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/abf1b706-5b8b-4339-8690-d01094bdb3ec)

#### 9

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/97ca6c7c-822e-4731-9052-ac0264795686)

#### Major GC

#### 1

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/b314d335-ae99-47a9-b8a8-504f6f70d3b6)

#### 2

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/3f09eaa2-f4f3-413c-94ea-2e11688945aa)

#### 3

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/06ba1e6e-b9b1-45f8-9f91-9dd612715cff)

## JVM Garbage Collection 종류

### Serial GC
- 서버의 CPU 코어가 1개일 때 사용하기 위해 개발된 가장 단순한 GC
- GC를 처리하는 쓰레드가 1개 (싱글 쓰레드) 이어서 가장 stop-the-world 시간이 길다
- Minor GC 에는 Mark-Sweep을 사용하고, Major GC에는 Mark-Sweep-Compact를 사용한다.
- 보통 실무에서 사용하는 경우는 없다 (디바이스 성능이 안좋아서 CPU 코어가 1개인 경우에만 사용)

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/5d509961-5384-4b91-bbe7-6f999ea4d1b7)

### Parallel GC
- Java 8의 디폴트 GC
- Serial GC와 기본적인 알고리즘은 같지만, Young 영역의 Minor GC를 멀티 쓰레드로 수행 (Old 영역은 여전히 싱글 쓰레드)
- Serial GC에 비해 stop-the-world 시간 감소

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/ebe8221d-d1f6-4a7e-97fa-5bb67209e1d8)

### CMS GC (Concurrent Mark Sweep)
- 어플리케이션의 쓰레드와 GC 쓰레드가 동시에 실행되어 stop-the-world 시간을 최대한 줄이기 위해 고안된 GC
- 단, GC 과정이 매우 복잡해짐.
- GC 대상을 파악하는 과정이 복잡한 여러단계로 수행되기 때문에 다른 GC 대비 CPU 사용량이 높다
- 메모리 파편화 문제
- CMS GC는 Java9 버젼부터 deprecated 되었고 결국 Java14에서는 사용이 중지

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/966ad43d-1300-4411-83b6-5d312629c41a)

### G1 GC(Garbage First)
- CMS GC를 대체하기 위해 jdk 7 버전에서 최초로 release된 GC
- Java 9+ 버전의 디폴트 GC로 지정
- 4GB 이상의 힙 메모리, Stop the World 시간이 0.5초 정도 필요한 상황에 사용 (Heap이 너무작을경우 미사용 권장)
- 기존의 GC 알고리즘에서는 Heap 영역을 물리적으로 고정된 Young / Old 영역으로 나누어 사용하였지만, G1 gc는 아예 이러한 개념을 뒤엎는 Region이라는 개념을 새로 도입하여 사용.전체 Heap 영역을 Region이라는 영역으로 체스같이 분할하여 상황에 따라 Eden, Survivor, Old 등 역할을 고정이 아닌 동적으로 부여
- Garbage로 가득찬 영역을 빠르게 회수하여 빈 공간을 확보하므로, 결국 GC 빈도가 줄어드는 효과를 얻게 되는 원리

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/2b91d291-40dd-4d33-8b1f-9cb4319813af)

