# GET
# POST
# PUT
# DELETE
# PATCH

# GET VS POST
# PUT VS PATCH

# 정리해보자 
현시점에서 HTTP를 사용한다라는 의미는 보통은 TCP 기반의 HTTP를 사용한다는 것을 의미             
이는 곧, TCP로 연결을 주고 받다보니까 HTTP는 TCP에 영향을 받고 있다는 것을 말한다.     
      
**TCP는 신뢰성이 높다는 말을 많이 사용한다.**   
TCP는 이 신뢰성이 높게끔 만들기 위한 여러 장치들이 있다.    
      
* 흐름 제어
* 혼잡 제어
* 기타 등등
   
이들은 인터넷 구간에서 패킷을 유실하더라도 재전송할 수 있는 여러 가지 장치를 넣은 것  
서버에서 받을 수 있는 패킷의 사이즈는 어느 정도인지 측정하는 방식 중 하나로 슬로우 스타터가 있음 

![image](https://user-images.githubusercontent.com/50267433/138400390-ba648fb6-28ad-4f83-bc41-c08a5425ee6e.png)

슬로우 스타터는 패킷의 사이즈를 점차 늘리면서 보내다가      
로스(패킷 손실)가 일어나면, 네트워크 혼잡으로 판단하고 window size 방식을 진행        
즉, 패킷의 양을 점점 늘리면서 최대한 패킷을 많이 보내려 하다가 문제 생기면 적정 수치부터 다시 진행  
그리고 이러한 슬로우 스타터는 TCP 커넥션을 시작할 때 마다 새로 시작한다.    
           
# 📄 HTTP 1.0 

HTTP 1.0은 단순하게 open/operation/close의 방식을 취하고 있어서 단순하다.    
TCP connection당 하나의 URL만 fetch하며 매번의 request/response가 끝나면 연결이 끊긴다.       

![image](https://user-images.githubusercontent.com/50267433/138400458-93c5d283-7dbe-459c-81a5-71f777a2ec6c.png)

         
**Http1.0은 필요할 때 마다 커넥션을 해야 하므로 속도가 떨어지며 TCP Slow Starter와 상성은 정말 최악이다.**           
슬로우 스타터는 커넥션이 생성될때 시작되며 초기에는 매우 낮은량의 패킷을 보낸다.                        
Http 1.0 같은 경우 매 요청시 새로 커넥션을 생성하기에 **매번 낮은량의 패킷을 보내게 되면서 성능상 이슈가 있다.**                      
         
이외에도 여러 다른 이유들도 있겠지만 Http1.0의 문제를 해결하기 위해 HTTP 1.1이 등장한다.     
   
# HTTP 1.1

![image](https://user-images.githubusercontent.com/50267433/138401350-0673a9b2-eaec-422d-96f4-04ff46e86ac4.png)

HTTP 1.1에는 keep-alive라는 지속 커넥션을 이용하여 TCP Connection 비용을 대폭 줄이 수 있다.    
             
**keep-alive**           
1. timeout 시간이 지나면 확인 패킷을 보낸다.     
    1. 응답을 받으면 다시 카운트 한다.          
    2. 응답을 받지 못하면 인터벌 타임 이후 다시 요청을 보내본다.(요청 횟수는 서버에서 설정 가능하다.)         
        1. 만약 여기서도 응답이 없다면 소켓을 닫아버린다.      
2. 소켓은 양층의 포트를 열고 통신하는 파이프라인 같은 것              
   여기서 중요한 것은 **소켓 연결 == 포트를 여는 것이기에 리소스 고갈 == 포트 없음**             
   즉, 웹 애플리케이션에서 설정된 기간까지 최대한 연결을 유지하려고 한다.          
      
# HTTP 2.0
   
![HTTP2](https://user-images.githubusercontent.com/50267433/138404216-2ebecce2-f15c-4783-b0b3-cb0e885b2862.png)





# HTTP 3.0
# QBIC 
