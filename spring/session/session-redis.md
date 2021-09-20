# Redis
> 다양한 자료를 확인했는데 [에릭님의 블로그](https://deveric.tistory.com/76)가 가장 이해하기 쉽게 정리가 잘 되어있다.   
      
**HTTP는 stateless 한 프로토콜이다.**                         
각각의 `요청과 응답`은 독립적으로 동작하며 서버는 이전에 있던 `요청/응답` 정보에 대해서 알지 못한다.        
그러나 때로는 여러 `요청/응답`에 대해서 클라이언트의 활동을 추적해야 하는 경우가 있다.        
주로 이 같은 경우 `세션`을 사용해 관리하지만 **세션 또한 애플리케이션 서버에 종속되어 있다.**       

![image](https://user-images.githubusercontent.com/50267433/133992674-91925337-07b7-473a-a6c5-6de497c8ce3f.png)      
   
예시로 `로그인한 유저의 정보를 세션을 통해서 관리한다.`라고 가정해보겠다.           
**만약, 어떠한 이유로 인해 기존 서버에서 다른 서버로 요청 흐름을 바꿔야한다면?🤔**                
앞서 언급했듯이 세션은 서버에 종속적이기 때문에 기존 정보는 더 이상 사용하지 못하게 되는 문제가 발생한다.                
                    
특히 클라우드 환경에서의 `오토 스케일 아웃`, 로드 밸런서와 같은 `부하 분산 시스템`에서는 이 같은 문제는 더욱 부각된다.           
        
![image](https://user-images.githubusercontent.com/50267433/133992692-90c426cc-0f48-4034-b410-63289d4f6a1d.png)      
              
스프링 세션은 이 같은 문제를 해결하기 위해 등장한 라이브러리로            
기존 `HttpSession`에 저장된 데이터를 더 나아가 **DB와 같은 데이터 저장소에 저장하는 매컴니즘으로 동작한다.**               
별도의 데이터 저장소에 저장함으로써 각각의 서버는 해당 저장소를 접근해서 관리하기만 하면 장점이 있다.             
단, 기존 메모리 접근 방식이 아닌 별도의 데이터 저장소에 접근하면서 비교적 시간이 느리다는 단점도 있다.          
             
최근에는 `Stateless`한 JWT를 많이 사용하는 추세이긴 하지만 이 기능도 주의해야 할 포인트가 많다.   
  
**Spring Session은 다음 모듈들로 구성된다.**    
* **Spring Session Core :** 핵심 Spring Session 기능 및 API 제공   
* **Spring Session Data Redis :** Redis 및 구성 지원이 지원하는 SessionRepository 및 ReactiveSessionRepository 구현을 제공     
* **Spring Session JDBC :** 관계형 데이터베이스 및 구성 지원에 의해 지원되는 SessionRepository 구현을 제공 
* **Spring Session Hazelcast :** Hazelcast 및 구성 지원이 지원하는 SessionRepository 구현을 제공    


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
