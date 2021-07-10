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
## 초기 
```java
public class PersonService {
    public String sayHello(String name) {
        return "Hello " + name;
    }

    public Integer lengthOfName(String name) {
        return name.length();
    }
}
```
2가지 메서드를 가진 `PersonService`가 있다고 가정한다.      
   
## Enhancer 적용   
`sayHello()` 메소드에 대한 호출을 가로챌 간단한 프록시 클래스를 만들고자 한다.      
         
```java
Enhancer enhancer = new Enhancer();
enhancer.setSuperclass(PersonService.class);
enhancer.setCallback((FixedValue) () -> "Hello Tom!");
PersonService proxy = (PersonService) enhancer.create();

String res = proxy.sayHello(null);

assertEquals("Hello Tom!", res);
```
`Enhancer 클래스`는 동적으로 확장하여 프록시를 생성 할 수 있게 해준다.        
`setSuperclass()`메서드에 `PersonService`를 주입하여 상위 클래스로 설정을 하고       
`setCallback()`메서드에 새로운 값을 주입하여 실행 결과 값을 바꿀 수 있다.      
이후, `create()`를 통해 프록시 객체를 반환해서 사용할 수 있다.        
**이제 프록시에서 `sayHello()` 메서드를 실행하면 프록시 메서드에 지정된 값이 반환된다.**         

참고로 `FixedValue`는 단순히 프록시에서 새로 설정한 값으로 반환시켜주는 콜백 인터페이스다.   
사실, `setCallback`의 매개변수 타입으로 `CallBack`을 원하기에   
새로운 값에 `CallBack`를 인터페이스 상속한 `FixedValue`로 형변환 시켜 콜백 동작을 지원하도록 만든 것이다.      
    





