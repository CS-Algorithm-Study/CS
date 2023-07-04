# ⭐그래프
==============================================================================================================

1️⃣개념
연결되어 있는 원소간의 관계를 표현한 자료 구조

그래프는 (V,E)로 나타낸다.
V는 Vertices라고 불리는 노드의 집합이다.
E는 Edges라 불리는 Vertices 쌍의 모임이다.
Vetices와 Edges는 위치와 원소를 저장한다

그래프는 연결할 연결할 객체를 나타내는 정점과 객체를 연결하는 간선의 집합으로 구성된다

![image](https://mblogthumb-phinf.pstatic.net/MjAyMDExMjZfMjky/MDAxNjA2MzE3MTI3MjQz.xiYZF2LbmaKC1tjAoFEnaoeWVTok-h8QszcwbMwTIG0g.XihS2P7ztFzXekIAraSmUl6YzstmHVh_6QRKc9w6_9og.PNG.cyh197/image.png?type=w800)

 ## Edge Types

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


