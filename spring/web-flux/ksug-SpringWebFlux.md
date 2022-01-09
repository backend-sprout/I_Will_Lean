# Spring WebFlux  
* 구 Spring-Web-Reactive   
* 스프링5의 메타가 JDK9 적용에서 WebFiux로 바뀔만큼 중요한 모듈 탄생     
* 스프링 리액티브 스택의 웹 파트를 담당한다.     

# 용도 
* 비동기-논블록킹 리액티브 개발에 사용  
* 효율적으로 동작하는 고성능 웹 애플리케이션 개발  
* 서비스가 호출이 많은 마이크로 서비스 아키텍처에 적합    

# 2가지 개발 방식 지원 

* 기존의 `@MVC` 방식    
* 새로운 함수형 모델(RouterFunction, HandlerFunction)
    * 함수형 프로그래밍은 애노티에션 대신 명확한 함수를 사용을 권장함 

# 새로운 요청-응답 모델 
  
* 서블릿 스택과 API에서 탈피    
* 서블릿 API는 리액티브 함수형 스타일에 적합하지 않다.    
* ~~HttpServletRequest, HttpServletResponse~~  
* ServerRequest, ServerResponse 

# 지원 웹 서버/컨테이너
* Servlet 3.1(Tomcat, Jetty..) - 3.1 부터 비동기-논블록킹 요청 가능  
* Netty   
* Undertow   

# 함수형 스타일 WebFlux  
## 스프링이 웹 요청을 처리하는 방식  

* 요청 매핑 
    * 웹 요청을 어느 핸들러에게 보낼지 결정
    * URL, 헤더
    * @RequestMapping  
* 요청 바인딩 
    * 핸들러에게 전달할 웹 요청 준비 
    * 웹 URL, 헤더, 쿠키, 바디 
    * 어댑터 
* 핸들러 실행 
    * 전달 받은 요청 정보를 이용해 로직을 수행하고 결과를 리턴  
* 핸들러 결과 처리(응답 생성)
    * 핸들러의 리턴 값으로 웹 응답 생성  

## RouterFunction 
> 함수형 스타일의 요청 맾이 

* 함수형 스타일의 웹 핸들러(컨트롤러 메서드)   
* 웹 요청을 받아 웹 응답을 돌려주는 함수  

## 함수형 WebFlux가 웹 요청을 처리하는 방식 

* 요청 매핑 : RouterFunction
* 요청 바인딩 : HandlerFunction
* 핸들러 실행 : HandlerFunction
* 핸들러 결과 처리(응답 생성) : HandlerFunction 




 


