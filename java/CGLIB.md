CGLIB(Code Generator Library)
=================================
# CGLIB 소개 및 개요      
`CGLIB`는 코드 생성 라이브러리로서 **런타임에 동적으로 자바 클래스의 프록시를 생성해주는 기능을 제공한다.**         
`CGLIB`를 사용하면 매우 쉽게 **프록시 객체를 생성할 수 있으며, 성능 또한 우수하다.**           
인터페이스가 아닌 **클래스의 상속을 이용하여 동적 프록시를 생성을 하기에 다양한 프로젝트에서 널리 사용되고 있다.**             
     
* `Hibernate`는 **엔티티 객체에 대한 프록시(지연로딩)를 생성할 때 CGLIB를 사용**            
* `Spring`은 프록시 기반의 **AOP를 구현할 때 CGLIB를 사용**        
* `Mockito`는 **모의 메서드에 cglib를 사용하여 프록시 객체를 만든다.**          
           
Java의 클래스는 런타임에 동적으로 로드된다.(Dynamic Loading)               
CGLIB는 이러한 특징을 활용하여 이미 실행 중인 Java 프로그램에 **프록시 클래스를 추가한다.**                      
                 
# JDK Proxy vs CGLIB  
   
![jaehun2841님의 프록시 이미지](https://user-images.githubusercontent.com/50267433/126331779-3967b050-0500-4fd9-903f-7b3f2a1d5a63.png)   

// 나중에 자료 찾아서 정리  
         
# CGLIB의 주요 구성 요소  
  
CGLIB 프록시 생성과 관련 모듈은 아래와 같다.             
  
* Enhancer 클래스    
* Callback 인터페이스     
* CallbackFilter 인터페이스    
         
이 3가지만 있으면 아주 손쉽게 프록시 객체를 생성할 수 있다.      
 
# 의존성 주입 
```xml
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.2.4</version>
</dependency>
```

# 예시    
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
            
## 일반적인 CGLIB 프록시 방법             
타겟 클래스의 메서드 호출을 가로챌 간단한 프록시 클래스를 만들어본다.                  

* **`net.sf.cglib.proxy.Enhancer` :** 클래스를 사용하여 원하는 프록시 객체 만든다.   
* **`net.sf.cglib.proxy.Callback` :** 프록시 객체 조작하게 해준다.   

```java
Enhancer enhancer = new Enhancer();
enhancer.setSuperclass(타겟_클래스.class);
enhancer.setCallback((FixedValue) () -> 변경할 값);
PersonService proxy = (타겟_클래스) enhancer.create();
proxy.타겟메서드();  
```                
* `Enhancer 클래스`는 동적으로 확장하여 프록시를 생성 할 수 있게 만들어주는 클래스다.                    
* `enhancer.setSuperclass()`에 **타겟 클래스 타입**을 입력하여 프록시 타켓을 설정한다.                    
* `enhancer.setCallback()`에 새로운 값을 주입하여 실행 결과 값을 바꿀 수 있다.       
* `enhancer.create()`를 통해 프록시 객체를 반환해서 사용할 수 있으며 `instanceOf` 관계이기에 타겟 타입으로 변환도 가능하다.              
* 프록시 객체에서 타겟 클래스의 메서드를 호출하면 그제서야 타켓 클래스를 불러와 원본 메서드를 호출한다.    

`FixedValue`는 프록시에서 새로 설정한 값으로 변환시켜주는 콜백 인터페이스다.   
`setCallback()`는 매개변수 타입으로 `CallBack`타입을 원하는데   
`CallBack`을 상속한 `FixedValue`로 형변환 시켜 콜백 동작을 지원하도록 만든 것이다.    
     
```java
Enhancer enhancer = new Enhancer();                         // 1. Enhancer 객체를 생성
enhancer.setSuperclass(MemberServiceImpl.class);            // 2. setSuperclass() 메소드에 프록시할 클래스 지정
enhancer.setCallback(NoOp.INSTANCE);                        // 3. 콜백 지정, 아무 작업도 수행하지 않고 곧바로 원본 객체를 호출한다.
Object obj = enhancer.create();                             // 4. enhancer.create()로 프록시 객체 생성
MemberServiceImpl memberService = (MemberServiceImpl) obj;  // 5. 프록시 형변환을 통해서 간접 접근 
memberService.regist(new Member());             
memberService.getMember("madvirus");                        
```    
앞선 예제는 단순히 원본 객체의 메소드를 직접적으로 호출하고 있다.                                      
하지만, **대부분의 프록시 객체는 원본 객체에 접근하기 전에 별도의 작업을 수행하며**                  
`CGLIB`는 `Callback`을 사용해서 별도 작업을 수행할 수 있도록 하고 있다.                             
        
```java  
Enhancer enhancer = new Enhancer();   
enhancer.setSuperclass(PersonService.class);     
enhancer.setCallback((FixedValue) () -> "Hello Tom!");    
PersonService proxy = (PersonService) enhancer.create();    
String res = proxy.sayHello(null);
assertEquals("Hello Tom!", res); // true  
```    
더불어, `FixedValue`를 통해 기존 로직을 원하는대로 변경하는 작업을 거쳤다.     
그러나 아직도, **특정 메서드**를 타겟으로 새로운 로직으로 변환하는 과정을 거치지 않는다.         
만약, 특정 메서드에 한 하여 로직을 바꾸고 싶다면 `MethodInterceptor`를 사용하면 된다.      
    
## 특정 메서드 로직 변환 - MethodInterceptor 콜백  
`MethodInterceptor`는 프록시와 원본 객체 사이에 위치하여 메소드 호출을 조작할 수 있도록 해 준다.         
**프록시 객체에 대한 모든 호출이 `MethodInterceptor`를 거친뒤에 원본 객체에 전달**된다.           
     
```java
public interface MethodInterceptor extends Callback {
    Object intercept(Object var1, Method var2, Object[] var3, MethodProxy var4) throws Throwable;
}
```      
* **object :** 원본 객체
* **method :** 원본 객체의 호출될 메소드를 나타내는 Method 객체
* **args :** 원본 객체에 전달될 파라미터
* **methodProxy :** CGLIB가 제공하는 원본 객체의 메소드 프록시
         
`MethodInterceptor`를 사용하면 아래와 같은 동작을 할 수 있다.   
    
* **원본 객체 대신 다른 객체의 메소드를 호출 할 수 있다.**                  
* **원본 객체에 전달될 인자의 값을 변경할 수도 있다.**                  
    
`MethodInterceptor 인터페이스`를 구현한 클래스는     
`intercept()`에서 원본 객체의 메소드를 호출해야 한다.         
원본 객체의 메소드를 호출하는 방법은 다음과 같은 2가지가 있다.         
     
```java
    // 방법1: 자바의 리플렉션 사용
    Object returnValue = method.invoke(object, args);
```
```java
    // 방법2: CGLIB의 MethodProxy 사용
    Object returnValue = methodProxy.invokeSuper(object, args);
```
   
**실제 예제**        
```java
enhancer.setCallback((MethodInterceptor) (obj, method, args, proxy) -> {
    if (method.getDeclaringClass() != Object.class && method.getReturnType() == String.class) {
        return "Hello Tom!";
    } else {
        return proxy.invokeSuper(obj, args);
    }
```
`MethodInterceptor`를 변환한 람다를 통해서 아래와 같은 작업을 수행했다.     
       
* **Object가 아니며 반환형이 String인 메서드 :** 기존 값을 `"Hello Tom!"` 변환한다.             
* **나머지 :** 기존 원본 클래스(상위 클래스)의 메서드 값을 그대로 사용한다.                      
                       
참고로 이 과정에서 `Object`에 속하는 메서드는 가로채는 대상이 되지 않는다.               
즉, `toString()` 또는 `hashCode()`와 같은 메서드는 가로채는 대상이 아니다.             
      
**전체 코드**
```java
Enhancer enhancer = new Enhancer();
enhancer.setSuperclass(PersonService.class);
enhancer.setCallback((MethodInterceptor) (obj, method, args, proxy) -> {
    if (method.getDeclaringClass() != Object.class && method.getReturnType() == String.class) {
        return "Hello Tom!";
    } else {
        return proxy.invokeSuper(obj, args);
    }
});
PersonService proxy = (PersonService) enhancer.create();
int lengthOfName = proxy.lengthOfName("Mary");
assertEquals("Hello Tom!", proxy.sayHello(null));
assertEquals(4, lengthOfName);
```
          
## 동적으로 객체 생성하기 - BeanGenerator          
기존에 존재하는 객체가 아닌 새로운 객체를 만들어서도 프록시를 진행할 수 있다.                 
`BeanGenerator`는 동적으로 빈을 생성하고 `setter` 및 `getter` 메서드와 함께 `필드`를 추가할 수 있다.             
코드 생성 도구에서 간단한 POJO 객체를 생성하는 데 사용할 수 있다.          
      
```java 
BeanGenerator beanGenerator = new BeanGenerator();                      // 빈생성기 생성
beanGenerator.addProperty("name", String.class);                        // 필드 생성(getter/setter 생성)   
Object myBean = beanGenerator.create();                                 // 자바빈 생성 

Method setter = myBean.getClass().getMethod("setName", String.class);   // 생성된 setName 메서드 리플랙션 반환 
setter.invoke(myBean, "some string value set by a cglib");              // 새로운 값 지정 

Method getter = myBean.getClass().getMethod("getName");                 // 생성된 getName 메서드 리플랙션 반환 
String actual = getter.invoke(myBean);                                  // 값을 반환 받는다.  
assertEquals("some string value set by a cglib", actual);               // 비교 
```
              
   
   
   
## MIXIN 만들기   
`mixin`은 하나로 여러 객체를 결합할 수 있는 구조를 가지고 있다.                   
몇 가지 클래스의 동작을 포함하고 해당 동작을 **단일 클래스 또는 인터페이스로 노출할 수 있다.**          
                          
`CGLIB 유지 mixin`은 하나의 객체로 여러 개체의 조합을 할 수 있다.             
그러나 그렇게 하려면 **믹스인에 포함된 모든 객체가 인터페이스로 뒷받침되어야 한다.**     
두 인터페이스의 믹스인을 만들고 싶다고 가정한다면 인터페이스와 해당 구현을 모두 정의해야 한다.      
   
```java
public interface Interface1 {
    String first();
}

public interface Interface2 {
    String second();
}

// 믹스인
public class Class1 implements Interface1 {
    @Override
    public String first() {
        return "first behaviour";
    }
}

// 믹스인 
public class Class2 implements Interface2 {
    @Override
    public String second() {
        return "second behaviour";
    }
}
```   
`Interface1` 및 `Interface2`의 구현을 구성하려면 둘 모두를 확장하는 인터페이스를 만들어야 한다.       
     
```java
public interface MixinInterface extends Interface1, Interface2 { }
```  
   
`MIXIN 클래스의create()` 메서드를 사용해서 우리의 행동을 포함 할 수 클래스 1 과 Class2의를 에 MixinInterface :
   
```java
Mixin mixin = Mixin.create(
  new Class[]{ Interface1.class, Interface2.class, MixinInterface.class },
  new Object[]{ new Class1(), new Class2() }
);
MixinInterface mixinDelegate = (MixinInterface) mixin;

assertEquals("first behaviour", mixinDelegate.first());
assertEquals("second behaviour", mixinDelegate.second());
```   
`mixinDelegate` 에서 메소드를 호출하면 `Class1` 및 `Class2` 에서 구현이 호출된다.     
   


 
