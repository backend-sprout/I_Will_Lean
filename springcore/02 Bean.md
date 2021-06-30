Bean 🥜
=============

# 📕 JAVA Bean   
`JAVA Bean`이란? 'JAVA Bean 규약' 또는 'JAVA Bean 관례'에 따라 만들어진 클래스다.         
주로, `인스턴스 변수`들과 `getter/setter 메소드`가 존재하는 클래스를 의미한다.       
      
JAVA Bean 관련 설계 규약     
1. 클래스를 패키지화 하여야 한다.  
2. 멤버변수는 Property 라 부른다.    
3. 각 property마다 setter, getter가 존재해야 한다.   
4. Property의 접근제어자는 private이다.      
5. 만약 property가 boolean이라면 getter대신 is메소드를 사용해도 된다.    
    
# 📗 Spring Bean        
`Spring IoC Container`는 하나 이상의 빈을 관리한다.             
이러한 `Bean`은 `BeanDefinition(빈 설정 메타 정보)`로 인해 생성된다.             
(`Spring Container`, `BeanDefinition(빈 설정 메타 정보)`)   
   
## 📖 BeanDefinition(스프링 빈 설정 메타 정보)   
스프링은 어떻게 이런 다양한 설정 형식을 지원하는 것일까?     
그 중심에는 `BeanDefinition`이라는 추상화가 있다. (빈 정보 자체를 추상화)     

쉽게 이야기해서 **`역할`과 `구현`을 개념적으로 나눈 것이다.**        
* `XML`을 읽어서 `BeanDefinition`을 만들면 된다.   
* 자바코드를 읽어서 `BeanDefinition`을 만들면 된다.   
* 스프링 컨테이너는 자바 코드인지, XML인지 몰라도 오로지 `BeanDefinition`만 알면 된다.   
        
`BeanDefinition`을 빈 메타 정보라고 말한다.      
* `@Bean`, `<bean>`당 각각 하나씩 메타 정보가 생성된다.      
* 스프링 컨테이너는 이 메타 정보를 기반으로 스프링 빈을 생성한다.      
 
[사진]()    
[사진]()    
       
`BeanDefinition` 인터페이스를 구현한 클래스들은        
`BeanDefinitionReader`인터페이스를 구현한 구현체를 의존하고 있다.        
        
`BeanDefinitionReader` 인터페이스는 설정파일(자바,xml,..등등)에서 정보를 읽는 역할을 한다.            
그리고 설정파일을 기반으로 `BeanDefinition`을 생성하는 역할을 한다.         
정확히 말하면 `Bean`의 메타정보를 읽어오는 것이다.    
    
* `AnnotationConfigApplicationContext`는 `AnnotatedBeanDefinitionReader`를 사용해서     
  `AppConfig.class`를 읽고, `BeanDefinition`을 생성한다.      
* `GenericXmlConfigApplicationContext`는 `XmlBeanDefinitionReader`를 사용해서     
  `AppConfig.xml`을 읽고, `BeanDefinition`을 생성한다.       
* 새로운 형식의 설정 정보가 추가되면, `XxxBeanDefinitionReader`를 만들어서 `BeanDefinition`을 생성하면 된다.  
       
**`XxxBeanDefinitionReader` 만드는 방법은? 🤔**        
구글링을 해도 방법이 나오지 않는다!! (오히려 만들면 최초이기에 흥미가 돋는다.)         
지금은 집중해야할 것이 있으니 나중에 만드는 방법을 직접 찾아봐야겠다.       
`JSONBeanDefinition`이라는 주제로 만들어보자  

## BeanDefinition 살펴보기 
* **BeanClassName:** 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
* **factoryBeanName:** 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig
* **factoryMethodName:** 빈을 생성할 팩토리 메서드 지정, 예) memberService
* **Scope:** 싱글톤(기본값)
* **lazyInit:** 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연처리 하는지 여부  
* **InitMethodName:** 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명    
* **DestroyMethodName:** 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명    
* **Constructor arguments, Properties:** 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)     

**코드로 확인해보기**
```java
package hello.core.beandefinition;

import hello.core.AppConfig;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class BeanDefinitionTest {

    AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
    GenericXmlApplicationContext xmlApplicationContext = new GenericXmlApplicationContext("appConfig.xml");

    @Test
    @DisplayName("빈 설정 메타정보 확인")
    void findAnnotationApplicationBean() {
        String[] beanDefinitionNames = annotationConfigApplicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = annotationConfigApplicationContext.getBeanDefinition(beanDefinitionName);

            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                System.out.println("beanDefinitionName = " + beanDefinitionName +
                        " beanDefinition = " + beanDefinition);
            }
        }
    }

    @Test
    @DisplayName("빈 설정 메타정보 확인")
    void findXmlApplicationBean() {
        String[] beanDefinitionNames = xmlApplicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = xmlApplicationContext.getBeanDefinition(beanDefinitionName);

            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                System.out.println("beanDefinitionName = " + beanDefinitionName +
                        " beanDefinition = " + beanDefinition);
            }
        }
    }

}
```
   
**정리**     
* `BeanDefinition`을 직접 생성해서 스프링 컨테이너에 등록할 수 도 있다.         
  하지만 실무에서 `BeanDefinition`을 직접 정의하거나 사용할 일은 거의 없다.           
  그래도 나중에 직접 구현을 해보는 편이 개발자 다운 삶이지 않을까 싶다.        
* `BeanDefinition`에 대해서는 너무 깊이있게 이해하기 보다는,     
  스프링이 다양한 형태의 설정 정보를 `BeanDefinition`으로 추상화해서 사용하는 것 정도만 이해하자      
* 가끔 스프링 코드나 스프링 관련 오픈 소스의 코드를 볼 때,`BeanDefinition` 이라는 것이 보일 때가 있다.    
  이때 이러한 메커니즘을 떠올리면 된다.   
      
**필자가 생각하기론**      
인스턴스를 만들어 반환하는 것은 단순히 자바 코드를 수행하기 위한 작업일 뿐이고      
`BeanDefinitionReader` 구현체가 해당 클래스를 통해 정보를 읽어들어와          
`BeanDefinition`이라는 메타데이터를 만들고, 이를 통해 다시 객체를 새로 생성한다고 생각한다.      
       
**참고로 `ApplicationContext`로 의존하지 않은 이유는? 🤔**   
* `ApplicationContext`에서는 `getBeanDefinition()`이라는 메서드가 존재하지 않는다.    

**그리고**   
`GenericXmlApplicationContext` 인스턴스는 
**factoryBeanName**, **factoryMethodName** 요소가 null로 빠져있다.   
           
사실 이러한 이유에 대해서 아래 내용을 참고하자   
스프링에 빈을 등록하는 방법이 크게 2가지로 나뉜다.   
          
1. 직접 스프링빈을 등록하는 방법 (xml)     
2. 팩터리 메서드를 이용해서 등록하는 방법(class)  
      
예전, XML로 설정하는 경우 직접 스프링빈을 등록하는 방법이기에 빈에 대한 클래스가 밖에 드러났는데           
`JAVAConfig`로 바뀌면서 팩토리 메서드(AppConfig와 메서드)를 이용하는 방법을 주로 사용하게 되었고      
`BeanDefinition` 또한 팩터리 클래스와 메서드를 사용하는지를 출력하도록 설정이 변경되었다.      
그렇기에 `xml`의 직접 스프링빈을 등록하는 방법은 팩터리 클래스와 메서드에 대해 `null`값을 출력한다.    

