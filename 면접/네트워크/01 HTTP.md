# GET
# POST
# PUT
# DELETE
# PATCH

# GET VS POST
# PUT VS PATCH

# 정리해보자 
현시점에서 HTTP를 사용한다라는 의미는 보통은 TCP 기반의 HTTP를 사용한다는 것을 의미   
즉, 
이는 곧, TCP로 연결을 주고 받다보니까 HTTP는 TCP에 영향을 받고 있다는 의미이기도 합닏 



   


# TCP Slow Start   




# HTTP 1.0

## 📄 HTTP 1.0 과 문제점
* HTTP 1.0은 단순하게 open/operation/close의 방식을 취하고 있어서 단순하다.
* TCP connection당 하나의 URL만 fetch하며 매번의 request/response가 끝나면 연결이 끊긴다.
* 그러므로 매 번 필요할 때 마다 다시 연결을 해야 하므로 속도가 떨어진다.
* 또한, 한번에 얻어서 가져올 수 있는 데이터의 양이 제한되어 있다. 나아가 URL의 크기도 작다.
* 즉, 매번 Conncetion/Close 작업을 해야하며 전송할 데이터량이 작다, URL 크기도 작다.

반복되는 connect/disconnect현상으로 인해 한 서버에 계속해서 접속을 시도하게 되면 과부하가 걸리고 성능이 떨어지게 되는 문제가 발생한다.
이런 문제를 해결하기 위해 HTTP 1.1이 등장한 것이다.
 
# HTTP 1.1


# HTTP 2.0
# HTTP 3.0
# QBIC 
