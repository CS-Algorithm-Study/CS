# 1. DNS 서버에서 검색

www.google.com에서 도메인 이름에 해당되는 google.com을 DNS서버에서 검색한다.

웹 브라우저는 DNS 서버에 요청을 하기 전에 캐싱된 DNS 기록들을 먼저 확인하는데 해당 도메인 이름에 맞는 ip주소가 존재하면 DNS서버에 요청하지 않고 캐싱된 ip주소를 바로 반환한다.

<br><br>


# 2. DNS서버에서 해당 도메인 이름에 해당하는 ip주소를 찾고 사용자가 입력한 URL 정보와 함께 전달

ISP를 통해 DNS서버가 호스팅하고 있는 서버의 ip주소를 찾기 위해 DNS 쿼리를 전달한다.
<br>

🤔 ISP란?

> 인터넷 서비스 제공자를 의미하는데 우리나라의 sk 브로드밴드, kt같은 곳을 말한다.
> 
<br>

🤔 DNS Query란?

> 현재 DNS서버에 원하는 ip 주소가 존재하지 않으면 다른 DNS 서버를 방문한 과정을 원하는 ip주소를 찾을 때까지 반복한다.
> 
<br>

도메인 이름에 맞는 ip주소로 변환하는것은 점(.)을 기준으로 계층적으로 이루어지는데 뒤에서부터 해당 도메인 이름에 맞는 지역 DNS를 탐색한다.

> . → com → google.com

![image (2)](https://github.com/CS-Algorithm-Study/CS/assets/48826098/b917feae-c7a6-4dc1-b273-879078f5f43a)
<br>
<br>



# 3. 전달받은 ip주소를 이용해 웹 브라우저는 웹 서버에게 해당 웹 사이트에 맞는 html 문서 요청

http 요청 메시지는 tcp/ip 프로토콜을 사용하여 서버로 전송한다.

여기에서 tcp는 3-way-handshake를 통해 연결 및 데이터를 수신받고, 4-way-handshake를 통해 연결을 종료한다.
<br>
<br>

# 4. was에서 작업 처리 결과를 웹 서버로 전송하고, 웹 서버는 웹 브라우저에게 html 문서 결과 전달

이때 전달 과정에서는 status code를 통해서 서버 요청에 따른 결과 및 상태를 전송한다.

> 1xx, 2xx, 3xx, 4xx, 5xx
> 
<br>
<br>

# 5. 전달 받은 html 문서를 브라우저에 넣어준다.

- 브라우저에서 html 문서를 파싱하여 DOM 트리를 구축하고 css 파일 링크를 찾아 CSSOM 트리를 구축한다.
- 둘을 이용해 렌더트리를 구축한다.
- 렌더트리를 이용해 각 노드들의 위치를 지정하는 레이아웃 과정을 거친다.
- 화면에 paint한다.


<br><br>
reference<br>
https://velog.io/@tnehd1998/%EC%A3%BC%EC%86%8C%EC%B0%BD%EC%97%90-www.google.com%EC%9D%84-%EC%9E%85%EB%A0%A5%ED%96%88%EC%9D%84-%EB%95%8C-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EA%B3%BC%EC%A0%95
