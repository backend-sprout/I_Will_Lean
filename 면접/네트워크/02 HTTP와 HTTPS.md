# HTTP 의 문제점
* HTTP 는 평문 통신이기 때문에 도청이 가능하다.     
* 통신 상대를 확인하지 않기 때문에 위장이 가능하다.      
* 완전성을 증명할 수 없기 때문에 변조가 가능하다.      
     
# TCP/IP 는 도청 가능한 네트워크이다.
TCP/IP 구조의 통신은 전부 통신 경로 상에서 엿볼 수 있으며 패킷을 수집하는 것만으로 도청할 수 있다.   
평문으로 통신을 할 경우 메시지의 의미를 파악할 수 있기 때문에 암호화하여 통신해야 한다.  
           
암호화 프로토콜인 SSL(Secure Socket Layer) 또는 TLS(Transport Layer Security)를 조합함으로써 HTTP 의 통신 내용을 암호화할 수 있다   
SSL 을 조합한 HTTP 를 HTTPS(HTTP Secure) or HTTP over SSL이라고 부른다     
    
콘텐츠를 암호화 말 그대로 HTTP 를 사용해서 운반하는 내용인, HTTP 메시지에 포함되는 콘텐츠만 암호화하는 것이다.       
암호화해서 전송하면 받은 측에서는 그 암호를 해독하여 출력하는 처리가 필요하다.        

# HTTPS 

![http-1-14](https://user-images.githubusercontent.com/50267433/138423993-cea44886-cb35-4fc1-ae56-64320b6ee7b8.png)



# SSL


# TLS 




https://12bme.tistory.com/80
