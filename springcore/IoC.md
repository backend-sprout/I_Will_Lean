# IoC
`IoC(inversion of control)`란, 이름 그대로 `제어의 역전`을 의미한다.    
             
**객체를 생성하고 관리하는 `제어`를 역전시키는 것**으로              
하나의 클래스에서 객체를 `생성`하고 `사용`하면서 관리하는 것이 아니라   
`객체의 생성`은 외부에 맡기고 `객체의 사용`만을 관리하는 것이다.      
그리고 외부에 맡긴다는 말은 **프레임워크에 빈을 생성하고 관리하는 것을 맡긴다는 것이다.**  
             
**`IoC`라는 개념이 도입되기 사용되기 전에는**           
* 애플리케이션 수행에 필요한 **객체의 생성이나 객체와 객체 사이의 의존 관계를 개발자가 직접 처리했다.**       
* 이런 상황에서 **의존 관계에 있는 객체를 변경할 때, 반드시 코드를 수정해야 한다는 문제가 발생했다.**           
* 결과적으로 `SOLID 원칙`을 위배한 결합도 높은 코드를 작성하고 있었다.             
    * `생성`과 `사용`이라는 2가지 책임이 있으므로 `SRP(단일 책임 원칙)` 위배     
    * 기능의 확장이나 변경을 꾀한다면 코드를 수정해야하기에 `OCP(개방폐쇄원칙)` 위배

**`IoC`가 적용되면서**
* **객체 생성을 컨테이너(빈 컨테이너)가 대신 처리한다.**    
* **객체와 객체 사이의 의존관계 역시 컨테이너가 처리한다.**        
* 결과적으로 **소스에 `의존 관계`가 명시되지 않으므로 결합도가 떨어져서 유지보수가 편리해진다.**      
   
   
`IoC(Inversion of Control)`란, 이름 그대로 **제어의 역전**이라는 의미를 가지고 있다.       
           
```
IoC 란 제어의 역전으로   
객체를 생성하고 관리하는 '제어'를 역전시키는 것이다.   
```
       
보다 쉽게 설명하면, **내가 객체를 생성하고 사용하면서 관리하는 것이 아니라**          
`객체의 생성`은 외부에 맡기고 나는 **객체의 사용만을 관리**하는 것이다.             
외부에 맡긴다는 말은 주로, **프레임워크에 빈을 생성하고 관리하는 것을 맡긴다**는 것이다.                
   
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
기존 방식대로 코드를 짠다면 객체를 **생성**하고 **사용**하는 `2가지 책임`이 존재한다.      
       
만약, 객체를 생성하는 과정에 변경사항이 생긴다고 가정을 한다면     
**생성에 관한 코드를 수정**해야하는 것은 물론이고          
경우에 따라서 **객체를 사용하는 코드 또한 수정하는 경우가 존재한다.**         
                    
결국, 프로그래머는 `객체의 생성` 및 `객체의 사용` 또한 지속적으로 신경쓰면서 코딩해야하고                     
만약 이러한 내용을 잊어버리고 **어느 하나라도 누락하게 된다면 잠재적으로 버그를 가지게 된다.**         

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
위 코드를 보면 우리는 단순히 객체를 주입받아서 사용하는 것이기 때문에,                   
로직이 객체의 생성에 영향을 받는다하더라도,       
이제는 **생성을 제외한 사용하는 부분만 수정을 해주면 된다.**                 
            
            
            
            
            
            
            
            
            
            
즉, `IoC`를 이용하면 `SOLID`의 원칙 중 하나인 `SRP(단일 책임의 원칙)`를 지켜주며             
개발자는 객체의 생성과 유지 및 관리에 대한 신경을 쓰지 않도록 도와준다.     
       
보다 자세한 내용은 필자가 정리한 [클린코드 11장 시스템_제작과 사용을 분리하라](https://github.com/kwj1270/TIL_CleanCode/blob/master/11%20%EC%8B%9C%EC%8A%A4%ED%85%9C.md#%EC%A0%9C%EC%9E%91%EA%B3%BC-%EC%82%AC%EC%9A%A9%EC%9D%84-%EB%B6%84%EB%A6%AC%ED%95%98%EB%9D%BC)
을 참고하자.    
             
`IoC`와 관련되어서 `DI(의존성 주입)`, `DIP(의존관계역전)`라는 개념도 존재하지만,      
이 부분에 대해서는 따로 공부해보는 것을 추천한다.        
      
