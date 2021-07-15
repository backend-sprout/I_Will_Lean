Entity vs VO vs DTO  
==================================   
   
자바 프로그래밍 공부를 접한 사람들은 누구나 접해봤을 구성 요소들이다.       
하지만, 흔히 `Entity == VO` 또는 `VO == DTO` 또는 `Entity == DTO`라는 잘못된 정보가 너무 많다.  
이러한 잘못된 정보를 최대한 교정하고자 내 자신도 개념을 확립하고자 이 게시글을 작성하게 되었다.   

# DTO(Data Transfer Object)  
> DTO = Domain Information + View Information      
  
`DTO`는 `데이터 전송 객체`로 흔히 **레이어간의 전송에 사용된다고 알려져있는데 이는 `잘못된 개념`이다.❌**         
사실 `DTO 의 핵심 가치`는 **Domain 로직과 UI 로직의 의존성을 낮추는 역할**을 하는 것이다.             
  
![mvc](https://user-images.githubusercontent.com/50267433/125761569-8ac8b212-4a22-4fb2-8fe2-20bf05017525.png)

`DTO 의 핵심 가치`를 알기 위해서, 현재 웹 아키텍처에서 가장 많이 사용되고 있는 `MVC 패턴`에 대해서 알아보자      
MVC 패턴은 구성 요소를 `Model`과 `View` 그리고 `Controller`라는 **세 가지 역할로 구분하는 디자인 패턴**이다.      
       
       
`비즈니스 처리 결과인 Model`과 `UI 영역인 View`가 `Controller`와 연결되어 있으며               
`Controller`는 View로 부터 들어온 클라이언트 요청을 해석하여        
Model을 업데이트하거나 Model로부터 데이터를 받아 View로 전달하는 작업 등을 수행한다.            
즉, `MVC 패턴`은 `Model`과 `View`를 분리함으로써 **서로의 의존성을 낮추고 독립적인 개발을 가능하게 해준다.**       
이러한 MVC 패턴에서 `DTO`는 **View와 Controller 간의 데이터 통신시에 사용되는 데이터를 저장하는 역할을 맡고있다.**  
     
![dto](https://user-images.githubusercontent.com/50267433/125815067-c61ce567-dbe8-4bb3-b42d-22bb328cccb8.png)
    

**DTO의 역할** 
* 클라이언트 요청에 포함된 데이터를 담아 서버 측에 전달 
* 서버 측의 결과 데이터를 담아 클라이언트에 전달    
            
결과적으로, `DTO`가 `View`와 맞닿아 의사소통하고 화면을 구성하는 `View Information`을 포함하는 덕분에,                
**Domain 객체는 View와 관련된 로직을 신경쓰지 않고 순수하게 도메인 로직만을 담당할 수 있게 되었다.**                    
                  
물론, `View Information`이 별도로 필요하지 않다면 `Domain 객체`를 이용할 수도 있다.                
다만, 불필요한 도메인 데이터 및 로직을 노출 시키지는 않는지, 이로 인한 사이드 이펙트는 없는지는 고려해야할 사항이다.         
더불어, JPA는 `Entity`값이 바뀌면 DB에 반영시키는 `Dirty Checking`이 있기에 `Entity`를 노출시키는 것은 매우 위험하다.

**불필요한 데이터 로직을 포함한 경우**
```java
public class User {
    private Long id;
    private String name;
    private String email;
    private String password; //외부에 노출되서는 안 될 정보
    public DetailInformation detailInformation; //외부에 노출되서는 안 될 정보
}
```
```java
@GetMapping
public ResponseEntity<User> showArticle(@PathVariable long id) {
    User user = userService.findById(id);
    return ResponseEntity.ok().body(user);
}
```
* **도메인 모델의 모든 속성이 외부에 노출된다.**        
  * UI 화면마다 사용하는 Model의 정보는 상이하지만,         
  Model 객체는 UI에서 사용하지 않을 불필요한 데이터까지 보유하고 있다.     
  * **비즈니스 로직 등 User의 민감한 정보가 외부에 노출되는 보안 문제와도 직결된다.**    
* **UI 계층에서 Model의 메서드를 호출하거나 상태를 변경시킬 위험이 존재한다.**    
  * Model과 View가 강하게 결합되어, View의 요구사항 변화가 Model에 영향을 끼치기 쉽다.     
  * 또한 User의 속성이 변경되면, View가 전달받을 JSON 및 Js 코드에도 변경을 유발하기에 상호간 강하게 결합된다.     
        
## DTO 변환 위치           
`DTO`는 `View`와 `Controller`간의 데이터 통신에서 사용되고       
`Domain`은 도메인 로직을 수행하는 어느 곳에서 사용되어야 한다.       
**그렇다면 DTO 에서 Domain 으로 변환하는 과정은 어느 레이어에서 진행해야할지에 대한 의문은 남아있다.🤔**         
       
객체의 생성과 관리는 해당 객체가 필요한 지점에서 진행이 되는 것이 좋다.               
즉, `DTO`는 `View`와 `Controller`간의 데이터 통신에서 사용되기에 `Controller`에 위치되는 것이 좋다.         



### View -> Controller -> Service  

### View <- Controller <- Service

 







 



# 참고 
[#](https://www.slipp.net/questions/93)
[#](https://xlffm3.github.io/spring%20&%20spring%20boot/DTOLayer/)
