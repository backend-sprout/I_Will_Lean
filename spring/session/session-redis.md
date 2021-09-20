# Redis
> 다양한 참고 자료를 확인했는데 [에릭님의 블로그](https://deveric.tistory.com/76)가 가장 이해하기 쉽고 적용도 정리도 잘 되었다.  



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
