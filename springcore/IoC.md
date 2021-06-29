# IoC(Inversion of Control)  
`IoC(Inversion of Control)`란, 이름 그대로 **제어의 역전**이라는 의미를 가지고 있다.       
      
**기존 프로그램**
```java
public class Sample {
    public void run(String keyword) {
        NameSearch nameSearch = new NameSearch();
        Result result = nameSearch(keyword);
    }
}
```

기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고, 연결하고, 실행했다.   


한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종했다. 개발자 입장에서는 매우 자연스러운 흐름이었다.
AppConfig의 등장으로 클라이언트 구현 객체는 자신의 로직을 실행하는 역할만 담당하게 되었다.
프로그램에 대한 제어 흐름에 대한 권한은 AppConfig가 가지고 있다.
인터페이스를 구현한 객체를 다루므로 로직의 주도권을 잡고있다고 생각하면 된다.
이렇듯 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전이라고 한다.

일반적으로 프로그램을 구현할 때 사용하는 프로세스는 일체형으로    
`메인 클래스`에서 `서브 클래스`들을 생성하여 이를 부품과 도구 처럼 활용하여 사용했다.   
즉, 객체를 `new 키워드`를 통해 직접 생성하고 이를 활용했다.    

```java
public class Sample {
    public void run(SearchStrategy searchStrategy) {
        NameSearch nameSearch = new NameSearch();
        Result result = nameSearch(keyword);
    }
}
```

하지만, 이같은 

스프링 프로세스 : 분리/도킹형으로 컨테이너에서 서브 클래스들을 생성하여 메인 클레스에서 DI로 받는 방식

기존에 객체를 활용하는 프로세스는    
개발자가 `new 객체()`를 통해 객체를 만들고 사용하는 전략이었다.  


기존 프로세스와 달리 객체의 생성 흐름이 반대로 동작하는 것을 의미 (서브 먼저 생성 후 사용)

