# Spring_Expert
스프링에서 깊게 공부하고 싶은 내용들을 정리하는 레포지토리

[[#1] BeanDefinition](#)     
[[#2] ThreadLocal](#)     
[[#3] Immutable Object vs stateless Object](#)         
[[#4] Singleton Registry](#)          
[[#5] CGLIB](#)           
[[#6] 각각의 의존관계 호출 시점](#)         
[[#7] final 클래스와 스프링 빈 등록 -> 프록시가 가능한가?](#)   
[[#8] LogBack 로그백](#https://goddaehee.tistory.com/206)               
[[#9] 스프링 컨테이너에 빈 등록을 안하면 주입받을 수 있는가](#)        
이 부분에 대해서는 이 [사이트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/lecture/55380?tab=community&q=131530)를 보고 이해했는데, DI 대상 빈을 우리가 만들고 사용한다면 그에 따른 객체 또한 우리가 직접 주입해주어야 한다.    
그렇기에 DI의 대상이 되는 클래스들 또한 빈으로 등록되어야하고 이 과정에서 의존관계를 맺어야 한다.  


