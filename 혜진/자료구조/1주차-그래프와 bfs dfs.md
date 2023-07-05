# ⭐그래프

### 1️⃣ 정의
연결되어 있는 원소간의 관계를 표현한 자료 구조

그래프는 (V,E)로 나타낸다.
V는 Vertices라고 불리는 노드의 집합이다.
E는 Edges라 불리는 Vertices 쌍의 모임이다.
Vetices와 Edges는 위치와 원소를 저장한다

그래프는 연결할 연결할 객체를 나타내는 정점과 객체를 연결하는 간선의 집합으로 구성된다

![image](https://mblogthumb-phinf.pstatic.net/MjAyMDExMjZfMjky/MDAxNjA2MzE3MTI3MjQz.xiYZF2LbmaKC1tjAoFEnaoeWVTok-h8QszcwbMwTIG0g.XihS2P7ztFzXekIAraSmUl6YzstmHVh_6QRKc9w6_9og.PNG.cyh197/image.png?type=w800)

 ### 2️⃣ Edge Types

 Directed edge(방향이 있는 엣지)
: 순서가 있고,(u , v)로 표기한다.
첫번째 vertex는 u로 origin이라 부른다.
second vertex v는 목적지이다.

Undirected dege(방향이 없는 엣지)
: 순서가 없고 (u, v)로 표기한다.
(u, v ) = (v, u)

Directed graph
: 모든 edges들은 직접적이다. (순서 상관 O)

Undirected graph
: 모든 deges들은 간접적이다.(순서 상관 X)

![image](https://mblogthumb-phinf.pstatic.net/MjAyMDExMjZfNTAg/MDAxNjA2MzE3NDMwMDMx.jkBssmBe-2M1OuolWj7pq7a-2rdVYMWz19fRDOZqHWcg.E8DN-BAE3cdpd2gI__kXCcDgd-PWGW2OZFgQqXdKTvUg.PNG.cyh197/image.png?type=w800)

### 3️⃣ 전문용어(Terminology)

: End vertices (or endpoints) of an edge
   U와 V는 edge a의 endpoints 이다.

: Edges incident on a vertex
   a,d,b는 V의 endpoint 중 하나이다.

: Adjacent vertices
   U와 V는 인접해 있다.

: Parallel deges
   h 와 i는 평행한 엣지이다.

: Self-loop
   j는 스스로 루프를 만든다.

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/49bd8780-4d00-4536-8ed6-bec68b0eb21f)


Path
- vertices와 edges가 교대로 나타나는 sequence이다.
- 정점 Vi 에서 Vj 까지 간선으로 연결된 정점을 순서대로 나열한 리스트
- vertex에서 시작하고 끝이난다.
- 각각의 edge들은 endpoints를 거친다.

Simple path
- 모두 다른 정점으로 구성된 경로
- path는 모두 그것의 vertices와 edges를 구분한다.

Examples
P1 = (V, b, X, h, Z) 는 simple path 이다.
P2 = (U, c, W ,e ,X, g ,Y , f, W, d, V) 는 simple path가 아니다.

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/2638ab7b-d15e-4ab7-8237-2faa2f284907)

Cycle
- 단순 경로 중에서 경로의 시작 정점과 마지막 정점이 같은 경
- vertice와 edge가 순환구조로 이루어져 있다.
- 각각의 과정은 enpoints를 지난다.

Simple Cycle
- cylcle은 모든 vertices와 edges들이 구분가능하다 (첫번째와 마지막을 제외하고)

Examples
- C1 = (V,X,Y,W,U,V) 는 simple
- C2 = (U,W,X,Y,W,V,U) 는 simple 이 아니다.

simple은 중복이 없어야 성립한다.
cycle을 표현할때 u는 표현하지 않고 vertex만 적어준다.
simple 그래프의 경우 u를 표기 하지 않고 vertex만 적어주어도 된다.

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/0dc32c02-017c-4824-a3a4-b5c9df9f74d8)

Dgree(차수) : 정점에 부속되어 있는 간선의 수
-in-degree(진입차수) : 정점을 머리로 하는 간선의 수
-out-degree(진출차수) : 정점을 꼬리로 하는 간선의 수


-여기서부턴 설명이...

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/1f3d0f5e-5a76-426b-8468-7ad2af044f25)

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/4a793537-cbff-43da-a8a9-d26d1d5f4815)


#### [Edge List structure]

vertex Object
- element
- reference to position in vertex sequence

Edge Object
- element
- origin vertex object
- destinantion vertex object
- reference to position in edge sequence

Vertex sequence
- sequence of vertex object
​
Edge sequence
- sequence of edge objects;

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/73a5c39c-24a7-45a1-aa3a-618961dc8724)


#### [Adjaceny List Structure]

Edge list structure
Incidence sequence for each vertex
- sequence of references to dege objects of incident edges

Augmented dege objects
- references to associated positions in incidence sequence of end vertices

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/3b65c79b-b377-44a4-87b4-748a58cb62dc)


#### [Adjacency Matrix Structure]

Edge list structure
Augmented vertex objects
- intger key (index) 는 vertex와 연관되어있다.

2d-Array adjaceny array
- reference to edge object of adjacent vertices
- Null for non nonadjacent vertices

The "old fashioned" version just has 0 for no edge and 1 for edge

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/f0348ccd-2b00-4290-8156-5e64a21d0873)

### 4️⃣ Performance (빅-oh 로 가정)

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/9c61d6b9-83fb-45b1-89cd-dfd1d41b6dbe)

출처 
https://m.blog.naver.com/cyh197/222154687389
https://leejinseop.tistory.com/43


# ⭐dfs

#### 1️⃣ Subgraphs
어떤 그래프 G의 서브그래프 S는 
S의 모든 정점들이 G의 정점의 부분(sub set)이다.
모든 S들은 G그래프 안에 있어야된다.

S를 구성하는 vertex와 edge들은 G에 속해있어야 된다.

Spanning subgraph가 되기위해서는 그래프 G의 모든 vertex들을 포함하고 있어야된다.
아래그림의 빨간색 정점과 간선들로 이루어진 부분이 스패닝 서브그래프이다.
스패닝서브 그래프는 모든 정점을 포함해야 한다. 그렇지 않으면 그냥 서브그래프 이다.

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/04ef0391-b14b-4422-b7ca-2bbfeeca9d58)


#### 2️⃣ Connectivity

모든 정점들은 최소 하나 이상의 path로 연결되어 있어야된다.
위 그림은 connected이고 아래그림은 Non connected이다.
​
![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/8133dd5d-2e57-46c4-8e0f-563c438a0b2b)


#### 3️⃣ Trees and Forests

그래프 T는 아래의 조건들을 만족해야한다.
1. T는 connect 되어야 한다.
2. cycle이 없어야 한다.
rooted tree와는 다른 구조이다.

forest는 여러개의 트리를 포함하고 있는 구조이다.
forest 또한 싸이클이 없어야 한다.

​![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/a350fd9a-23b8-4576-bec6-c92858271f6d)

3개의 tree로 이루어진 Forest 이다.

​
#### 4️⃣ Sapnning Trees and Forests

그래프가 트리가 아닌이상 unique할 필요는 없다.
spanning tree는 communiciation network를 디자인하는데 이용된다.
스패닝 포레스트는 여러개의 스패닝 트리들이 모인 것이다.

![image](https://github.com/CS-Algorithm-Study/CS/assets/70028148/d1f0d436-3cc9-480a-adbb-057c5e9cd148)

=>spanning tree이면서 spanning tree graph이다.

#### 5️⃣ Depth-First Serach (DFS)
DFS는 그래프를 traversing하는 알고리즘으로 대표적인 기술이다.
코딩테스트 단골문제이다.

DFS의 특징
1.그래프의 모든 정점들과 간선들을 방문한다.
2. G는 항상 connected 되어 있어야 한다.
3. sapnning froest인지 검사할때 DFS를 사용할 수 있다.
4. G의 connected를 계산할때 DFS를 사용 할 수 있다.

고급알고리즘을 해결하는데 자주 사용된다. 범용성이 상당히 크기때문에
반드시 알아두어야 되는 기술이다.

DFS는 n개의 vertices와 m개의 edges를 가질 때 
시간복잡도는 O(n+m) 소요된다.

두개의 정점 사이의 경로를 구할때 이용가능
그래프 내의 사이클을 찾는데 이용가능
