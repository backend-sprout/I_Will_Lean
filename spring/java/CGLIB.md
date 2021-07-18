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
      
## 일반적인 CGLIB 프록시 적용   
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

프록시의 첫 번째 버전에는 프록시가 가로채야 할 메서드와 수퍼클래스에서 호출해야 하는 메서드를 결정할 수 없기 때문에 몇 가지 단점이 있습니다.   
   
## MethodInterceptor 콜백 적용  
`MethodInterceptor`인터페이스를 이용하면       
프록시의 모든 호출을 가로채고 특정 호출을 수행할지 또는 상위클래스의 메서드를 실행할지 결정할 수 있다.     

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

assertEquals("Hello Tom!", proxy.sayHello(null));
int lengthOfName = proxy.lengthOfName("Mary");
 
assertEquals(4, lengthOfName);
```
```java
enhancer.setCallback((MethodInterceptor) (obj, method, args, proxy) -> {
    if (method.getDeclaringClass() != Object.class && method.getReturnType() == String.class) {
        return "Hello Tom!";
    } else {
        return proxy.invokeSuper(obj, args);
    }
```
`Object 클래스`가 아니며 반환형이 `String 클래스`에 대해서 새로운 값을 할당하게끔 했다.      
나머지의 경우에는 기존 원본 클래스(상위 클래스)의 메서드 값을 그대로 사용하도록 했다.        
         
이 과정에서 `toString()` 또는 `hashCode()` 메서드는 `Object`에 속하기에 가로채지 않고       
`lengthOfName()` 메서드 또한 반환 유형이 정수이므로 가로채지 않는다.       
   
## BeanGenerator    
기존에 존재하는 객체가 아닌 완전 새로운 객체를 만들어서도 프록시를 진행할 수 있다.     
`BeanGenerator`는 동적으로 빈을 생성하고 `setter` 및 `getter` 메서드와 함께 `필드`를 추가할 수 있다.     
코드 생성 도구에서 간단한 POJO 개체를 생성하는 데 사용할 수 있다.     

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

# 깊게 공부하기 
## Enhancer를 사용한 프록시 객체 생성 및 MethodInterceptor 사용하기 - 개요       
CGLIB를 사용하여 프록시를 생성할 때에는 크게 크게 두가지 작업을 필요로 한다.
   
* **`net.sf.cglib.proxy.Enhancer` :** 클래스를 사용하여 원하는 프록시 객체 만든다.   
* **`net.sf.cglib.proxy.Callback` :** 프록시 객체 조작하게 해준다.   

```java
public class MemberServiceImpl implements MemberService {
    public MemberServiceImpl() {
        System.out.println("create MemberServiceImpl");
    }

    public void regist(Member member) {
        System.out.println("MemberServiceImpl.regist");
    }

    public Member getMember(String id) {
        System.out.println("MemberServiceImpl.getMember:" + id);
        return new Member();
    }
}
```
   
## Enhancer 클래스를 사용한 프록시 객체 생성 하기  
이제 `Enhancer 클래스`를 사용하여 `MemberServiceImpl 클래스`의 프록시 객체를 생성하면 아래와 같다.   

```java
        Enhancer enhancer = new Enhancer();                         // 1. Enhancer 객체를 생성
        enhancer.setSuperclass(MemberServiceImpl.class);            // 2. setSuperclass() 메소드에 프록시할 클래스 지정
        enhancer.setCallback(NoOp.INSTANCE);                        // 3. 콜백 지정, 아무 작업도 수행하지 않고 곧바로 원본 객체를 호출한다.
        Object obj = enhancer.create();                             // 4. enhancer.create()로 프록시 객체 생성
        MemberServiceImpl memberService = (MemberServiceImpl) obj;  // 5. 프록시 형변환을 통해서 간접 접근 
        memberService.regist(new Member());             
        memberService.getMember("madvirus");                        
```

## MethodInterceptor 사용하여 프록시 객체 다루기        
앞선 예제는 단순히 원본 객체의 메소드를 직접적으로 호출하고 있다.(NoOp.INSTANCE)             
하지만, **대부분의 프록시 객체는 원본 객체에 접근하기 전에 별도의 작업을 수행**하며,          
`CGLIB`는 `Callback`을 사용해서 별도 작업을 수행할 수 있도록 하고 있다.               
`Callback`을 사용해서 별도 작업을 수행하고자한다면 `MethodInterceptor`를 사용하면된다.       

[#](#) 
  
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
     
`MethodInterceptor`를 사용하면 **원본 객체 대신 다른 객체의 메소드를 호출 할 수 있다.**                  
더불어, **원본 객체에 전달될 인자의 값을 변경할 수도 있다.**                  
  
`MethodInterceptor 인터페이스`를 구현한 클래스는 `intercept()` 메소드에서 원본 객체의 메소드를 알맞게 호출해야 한다.        
원본 객체의 메소드를 호출하는 방법은 다음과 같은 두가지가 있다.        
   
```java
    // 방법1: 자바의 리플렉션 사용
    Object returnValue = method.invoke(object, args);
    
    // 방법2: CGLIB의 MethodProxy 사용
    Object returnValue = methodProxy.invokeSuper(object, args);
```
   


 