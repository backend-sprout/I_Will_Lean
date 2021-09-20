# Redis
> 다양한 참고 자료를 확인했는데 [에릭님의 블로그](https://deveric.tistory.com/76)가 가장 이해하기 쉽고 적용도 정리도 잘 되었다.  
           
**HTTP는 stateless 한 프로토콜이다.**          
각각의 요청과 응답은 독립적으로 동작하며 서버는 신규 방문자와 재방문자를 구별할 수 없다.            
그러나 때로는 여러 요청에서 클라이언트의 활동을 추적해야 하는 경우가 있다.      
이는 세션 관리를 통해 수행되지만 세션 또한 애플리케이션

특정 사용자에 대한 세션 정보를 저장하기 위해 웹 컨테이너에서 사용하는 메커니즘입니다.


![image](https://user-images.githubusercontent.com/50267433/133992674-91925337-07b7-473a-a6c5-6de497c8ce3f.png)   
![image](https://user-images.githubusercontent.com/50267433/133992692-90c426cc-0f48-4034-b410-63289d4f6a1d.png)  


## 애플리케이션


## Redis 설치
```
docker pull redis
``` 
도커를 통해 레디스를 설치하자     
  
```
docker run --name redis -d -p 6379:6379 redis
```
설치한 Redis를 `백그라운드` + `포트 포워딩`을 통해 실행시키자.   
    
``` 
docker exec -it some-redis redis-cli\n
``` 
위와 같은 명령어로 컨테이너의 `redis-cli`에 바로 접근 가능하다.    

# 정리 
https://www.javainuse.com/spring/springboot_session_redis
