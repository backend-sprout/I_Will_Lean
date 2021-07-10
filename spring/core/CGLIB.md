CGLIB(Code Generator Library)
=================================

# CGLIB 소개 및 개요    

`CGLIB`는 코드 생성 라이브러리로서 **런타임에 동적으로 자바 클래스의 프록시를 생성해주는 기능을 제공한다.**         
CGLIB를 사용하면 매우 쉽게 **프록시 객체를 생성**할 수 있으며, 성능 또한 우수하다.        
더불어, 인터페이스가 아닌 **클래스에 대해서 동적 프록시를 생성**할 수 있기 때문에 다양한 프로젝트에서 널리 사용되고 있다.         
  
* Hibernate는 자바빈 객체에 대한 프록시(지연로딩)를 생성할 때 CGLIB를 사용     
* Spring은 프록시 기반의 AOP를 구현할 때 CGLIB를 사용   
* Mockito 는 모의 메서드에 cglib 를 사용하여 프록시 객체를 만든다.    
   
Java의 클래스는 런타임에 동적으로 로드된다.(Dynamic Loading - 참고)     
CGLIB는 이러한 특징을 활용하여 이미 실행 중인 Java 프로그램에 프록시 클래스를 추가한다.     
   
# CGLIB의 주요 구성 요소  
CGLIB 프록시 생성과 관련된 모듈은 아래 같다.    
  
* Enhancer 클래스    
* Callback 인터페이스   
* CallbackFilter 인터페이스    
     
이 세가지만 있으면 아주 손쉽게 원하는 프록시 객체를 생성할 수 있게 된다.     
 
# 의존성 주입 
```xml
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.2.4</version>
</dependency>
```

# 예시   




