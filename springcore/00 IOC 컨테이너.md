IoC 컨테이너 
==================
   
# 📗 IoC
`IoC(inversion of control)`란, 이름 그대로 `제어의 역전`을 의미한다.    
               
**객체를 생성하고 관리하는 제어를 역전시키는 것**으로                
하나의 클래스에서 객체를 생성하고 사용하면서 관리하는 것이 아니라    
`객체의 생성`은 외부에 맡기고 `객체의 사용`만을 관리하는 것이다.        
그리고 외부에 맡긴다는 말은 **프레임워크에 빈을 생성하고 관리하는 제어권을 맡긴다는 것이다.**    
즉, **개발자는 프레임워크로부터 빈을 주입받아 사용하기만 하면 된다는 의미기도 하다.**      
  
* 프레임워크에 빈을 생성하고 관리하는 제어권을 맡긴다.     
* 개발자는 프레임워크로부터 빈을 주입받아 사용하기만 하면 된다.  

## 📖 IoC라는 개념이 도입되기 전
`IoC`라는 개념이 도입되기 전에는       
애플리케이션 수행에 필요한 **객체의 생성이나 객체와 객체 사이의 의존 관계를 개발자가 직접 처리했다.**       
그리고 이런 상황에서 **의존 관계에 있는 객체를 변경할 때, 반드시 코드를 수정해야 한다는 문제가 발생했다.**           
         
결과적으로 `SOLID 원칙`을 위배한 결합도 높은 코드를 작성하고 있었다.               
* `생성`과 `사용`이라는 2가지 책임이 있으므로 `SRP(단일 책임 원칙)` 위배       
* 기능의 확장이나 변경을 꾀한다면 코드를 수정해야하기에 `OCP(개방폐쇄원칙)` 위배   
   
## 📖 IoC가 적용되면서
  
개발자는 `생성`과 `사용`이라는 2가지 측면을 고려하지 않아도 되므로     
더 가독성이 좋고, 유지보수하기 편한 코드를 작성할 수 있게 되었다.   
   
* **객체 생성을 컨테이너(빈 컨테이너)가 대신 처리한다.**    
* **객체와 객체 사이의 의존관계 역시 컨테이너가 처리한다.**        
* **소스에 `의존 관계`가 명시되지 않으므로 결합도가 떨어져서 유지보수가 편리해진다.**      
     
# 📘 DI(Dependency injection)    
> 의존성 주입  
  
`DI(Dependency injection)`란, 이름 그대로 **의존 관계 주입(의존성 주입)** 을 의미한다.        
         
## 📖 의존 관계란 무엇일까?   
```
자동차는 내부적으로 타이어를 사용한다.   
즉, 타이어가 있어야 자동차가 존재한다.    
   
자동차 -> 타이어
```
```java
public class Car {
    Tier tier;
} 
```
`의존 관계`란, 일반적으로 `코드에서의 두 모듈 간의 연결`을 의미하지만,           
'객체 지향 언어'에서는 `두 클래스 간의 관계`라고도 말을 한다.            
쉽게 표현하면 **두 개의 연관된 클래스 중 하나가 다른 하나를 어떤 용도를 위해 사용하는 것을 말한다.**       
  
* `Car` 는 `Tier`를 가지고 있다.    
* `Tier`가 있어야 `Car`이다.         
* `Car` 는 `Tier`에 의존적이다.       
* 의존 관계 그래프 : `Car -> Tier`

## 📖 `의존성 주입`이란?              
의존 관계의 객체를 직접 생성하고 사용하는 것이 아니라,        
**이미 만들어진 의존 관계의 객체를 외부로부터 주입받아 사용하는 것을 말한다.**      
      
**IoC VS DI**   
* IoC : 객체의 제어권을 프레임워크에 맡긴다.       
* ID : 외부로부터, 생성된 객체를 주입받아 사용한다.     
             
**기존 코드**
```java
import me.kwj1270.javaapi.test.domain.SampleInstance;

public class StudyTest {

    SampleInstance sampleInstance = new SampleInstance();       // 객체의 생성 방법이 바뀌면 코드 수정해야함

    public void doLogic() {
        ...                                                     // 객체를 사용하는 코드 또한 영향을 받을 가능성이 있다.    
    }
}
```
일반적인 방식은, **객체를 `생성`하고 `사용`하는 `2가지 책임`이 존재한다.**        
                   
만약, 객체를 생성하는 과정에 변경사항이 생긴다고 가정을 한다면,     
객체를 생성하는 코드를 수정해야하는 것은 물론이고        
객체를 사용하는 코드 또한 수정하는 상황이 발생한다.             
                 
결국, 개발자는 객체의 `생성` 및 `사용`을 지속적으로 신경쓰면서 코딩해야하고                         
만약, 이를 잊어버리고 코드 수정을 누락하게 된다면, **잠재적으로 버그를 가지게 된다.**           
          
**생성과 관련된 코드**
```java
public class TestApplication {

    public static void main(String[] args) {
        SampleInstance sampleInstance = new SampleInstance();
        StudyTest studyTest = new StudyTest(sampleInstance);
    }
    
}
```
**사용과 관련된 코드**
```java
import me.kwj1270.javaapi.test.domain.SampleInstance;

public class StudyTest {

    private SampleInstance sampleInstance;                      // 객체의 생성 방법이 바뀌어도 영향이 없다.

    public StudyTest(SampleInstance sampleInstance) {
        this.sampleInstance = sampleInstance;
    }

    public void doLogic() {
        ...                                                     // 객체를 사용하는 코드는 영향을 받을 가능성이 있다.
    }
}
```
위와 같이 객체의 `생성`과 `사용`을 분리를 하게 된다면        
로직이 객체의 생성에 영향을 받는다하더라도,            
이제는 **`생성`을 제외한 `사용` 부분만 수정을 해주면 된다.**                    
              
보다 자세한 내용은 필자가 정리한 [클린코드 11장 시스템_제작과 사용을 분리하라](https://github.com/kwj1270/TIL_CleanCode/blob/master/11%20%EC%8B%9C%EC%8A%A4%ED%85%9C.md#%EC%A0%9C%EC%9E%91%EA%B3%BC-%EC%82%AC%EC%9A%A9%EC%9D%84-%EB%B6%84%EB%A6%AC%ED%95%98%EB%9D%BC)
을 참고하자.   
   
     
# 📕 스프링 IoC 컨테이너     
Spring에서 말하는 `Spring IoC`는 `DI`와 동일하다고 말한다.(Spring 레퍼런스에서 직접 언급)            
즉, **어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라, 주입 받아 사용하는 방법을 의미한다.**   
                           
## 📖 스프링 IoC(DI)는 어디에서 빈(Bean)을 가져와 주입해주는 것일까?          
Spring은 `스프링 설정` 및 `애플리케이션 구현`과 관련된 `빈(Bean)`들을 `Spring Container`에 저장한다.           
(`Spring Container` 는 `Servlet 컨테이너`와 비슷하다.)     
        
그리고 이러한 `Spring Container`의 종류는 크게 2가지가 있다.    
     
1. BeanFactory    
2. ApplicationContext   

### 📄 BeanFactory     
스프링 설정 파일에 등록된 `Bean`을 생성하고 관리하는 가장 기본적인 컨테이너 기능만 제공한다.        
처음부터 객체를 생성하지 않고, 클라이언트의 요청(Lookup)에 의해서만 `Bean`이 생성되는 **지연로딩 방식을 사용한다.**        
일반적인 스프링 프로젝트에서 `BeanFactroy`를 사용할 일은 거의 없다.          
            
### 📄 ApplicationContext     
`BeanFactory`를 상속받고 있다. (인터페이스 상속, `HierarchicalBeanFactory`)       
컨테이너식으로 동작하며 `트랜잭션 관리`나 `리소스 로딩 기능` 그리고 `메시지 기반의 다국어 처리` 등 다양한 기능을 지원한다.    
**대부분의 스프링 프로젝트는 `ApplicationContext` 유형의 `Spring Container`를 이용한다.**               
      
* `BeanFactory`, `ApplicationEventPublisher`, `EnvironmentCapable`,   `HierarchicalBeanFactory`, `ListableBeanFactory`,        
  `MessageSource`, `ResourceLoader`, `ResourcePatternResolver`등의 인터페이스를 구현하고 있다.     
              
`BeanFactory` 같은 경우, 빈을 관리하는 기본적인 역할만 수행하기에             
대부분의 스프링 프로젝트에서 사용하는 컨테이너는 대부분 `ApplicationContext`를 상속받은 구현체들이다.         

## 📖 스프링 IoC(DI) 컨테이너 직접 사용
       
* ClassPathXmlApplicationContext (XML)
* AnnotationConfigApplicationContext (Java)

### 📄 ClassPathXmlApplicationContext (XML)    
  
**설정 xml파일**    
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="memberService" class="hello.core.member.MemberServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository"/>
    </bean>

    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository"/>

    <bean id="orderService" class="hello.core.order.OrderServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository"/>
        <constructor-arg name="discountPolicy" ref="discountPolicy"/>
    </bean>

    <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy"/>
</beans>
```
```java
        ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");
        MemberService memberService = ac.getBean("memberService", MemberService.class);
```

### 📄 ApplicationContext   

```java
@Configuration
public class DiscountPolicyConfig {
 
    @Bean
    public DiscountPolicy rateDiscountPolicy() {
        return new RateDiscountPolicy();
    }
    @Bean
    public DiscountPolicy fixDiscountPolicy() {
        return new FixDiscountPolicy();
    }
}
```
```java
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(DiscountPolicyConfig.class);    
    DiscountPolicy rateDiscountPolicy = ac.getBean(DiscountPolicy.class);
```
