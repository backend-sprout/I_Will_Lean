Entity vs VO vs DTO  
==================================   
   
자바 프로그래밍 공부를 접한 사람들은 누구나 접해봤을 구성 요소들이다.       
하지만, 흔히 `Entity == VO` 또는 `VO == DTO` 또는 `Entity == DTO`라는 잘못된 정보가 너무 많다.  
이러한 잘못된 정보를 최대한 교정하고자 내 자신도 개념을 확립하고자 이 게시글을 작성하게 되었다.   

# DTO(Data Transfer Object)  
> DTO = Domain Information + View Information      

`DTO`는 `데이터 전송 객체`로 흔히 **레이어간의 전송에 사용된다고 알려져있는데 반은 이는 `잘못된 개념`이다.❌**         
사실 `DTO 의 핵심 가치`는 **도메인 로직과 비즈니스 로직을 분리**하는 것이다.           
                  
이를 알기 위해서, 현재 웹 아키텍처에서 가장 많이 사용되고 있는 `MVC 패턴`에 대해서 알아보자      
`MVC 패턴`은 구성 요소를 `Model`과 `View` 그리고 `Controller`라는 **세 가지 역할로 구분하는 디자인 패턴**이다.   
여기서 재밌는 점은 비즈니스 처리 결과인 `Model`과 UI영역인 `View`는 서로를 모르고    
그저 Controller가 통해 통신하고 있다.   




   
DTO가 View Information을 포함하고 View와 맞닿아 의사소통하는 덕분에,        
Domain 객체는 View에 구애받지 않고, 순수하게 도메인 로직만을 담당하는 객체로 살아갈 수 있다.    
하지만 View Information이 별로 필요하지 않다면,        
굳이 DTO을 일부러 따로 만들 필요 없이 Domain 객체만 사용해도 괜찮다.        
어차피 View는 DTO인지 Domain 객체인지 알지도 못하고 알 필요도 없다. 그저 View를 그리는데 필요한 정보만 모두 담겨있다면 무엇이든 상관없다.       
   
   
Domain 객체만으로 모두 커버할 수 있다면 가장 단순하고 우아한 해법일 것이다. 하지만 View는 기대만큼 단순하지 않기 마련이다. 그래서 View Information이 필요하고 결국 DTO가 필요한 경우가 많다.

 
 
   
그렇기에 DTO에 대해서 다시 정의 내리자면 아래와 같다.    
`DTO`는 **클라이언트 요청에 포함된 데이터를 담아 서버 측에 전달**하고, 또 **서버 측의 결과 데이터를 담아 클라이언트에 전달하는 역할**을 한다.       




 

# En


# 참고 
[#](https://www.slipp.net/questions/93)
[#](https://xlffm3.github.io/spring%20&%20spring%20boot/DTOLayer/)
