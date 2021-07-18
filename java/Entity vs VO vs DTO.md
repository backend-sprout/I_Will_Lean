Entity vs VO vs DTO  
==================================   
   
자바 프로그래밍 공부를 접한 사람들은 누구나 접해봤을 구성 요소들이다.       
하지만, 흔히 `Entity == VO` 또는 `VO == DTO` 또는 `Entity == DTO`라는 잘못된 정보가 너무 많다.  
이러한 잘못된 정보를 최대한 교정하고자 그리고 내 자신도 개념을 확립하고자 이 게시글을 작성하게 되었다.   

# 📗 Entity
> Entity = DB Table Mapping Object   
        
**Entity는 DB 테이블과 직접적으로 매핑된 객체이다.**                     
테이블과 매핑되었기에 **대부분의 필드들이 테이블의 컬럼과 매칭되는 경우가 많다.**          
단, 그렇다고 해서 `Entity 필드 == DB 테이블 컬럼`라고 생각하면 안 된다.             
**`@Transient`를 통해 `2차 비밀번호`와 같은 DB 테이블에 매핑되지 않는 필드도 충분히 올 수 있기 때문이다.**         
                       
`JPA`에서는 **Entity를 기준으로 테이블이 생성되고 스키마가 변경된다.**                           
그렇기 때문에 **Entity를 요청이나 응답값을 전달하는 클래스로 사용하는 것을 권장하지는 않는다.**                         
왜냐하면 값이 변동 될 수 있고 이로 인해 DB 테이블의 값을 변경할 수 있기 때문이다.(더티 체크)            
사실 이러한 이유 때문에 Entity 를 Dto로 변환시켜 데이터를 전달하는 경우가 대다수이다.        
   
**Entity의 특징은 아래와 같다.**           
1. Entity를 식별할 수 있는 ID(식별자)가 존재해야한다.                    
2. ID(식별자)를 통해 Entity를 지속해서 추적할 수 있다.                            
3. ID(식별자)를 변경하지 않는 한, 내부 값을 변경해도 식별자를 통해 찾을 수 있다.                 
4. Domain Logic(비즈니스 로직)을 포함하는 것이 좋고 Presentation Logic은 되도록 구현하면 안 된다.(getter)      
   
## JPA에서의 Entity   





# 📕 VO (Value Object)     
> VO = Value + Business Logic   
     
마틴 파울러가 언급한 `VO`의 개요를 보면 다음과 같다.  
             
* 👉 프로그래밍할 때, 사물을 복합물로 표현하는 것이 유용한 경우가 종종 있다.  
* 👉 x, y로 이루어진 2차원 좌표, 숫자와 통화로 이루어진 금액, 시작 날짜와 끝 날짜로 이루어진 날짜 기간 등이 있다.   
  
VO는 도메인에서 한 개 또는 그 이상의 속성들을 묶어서 **특정 값을 나타내는 객체**를 의미한다.           
물론, 자바에서는 메서드를 활용할 수 있기에 **값을 제공함에 있어서 더 많은 기능을 지원해줄 수 있다.**                  
       
VO는 도메인 객체의 일종이고 보통 기본 키로 식별 값을 갖는 Entity와 구별해서 사용한다.        
  
## equals & hash code 재정의하기   
VO는 객체 자체로 **하나의 값 단위**이다.      
그렇기에, **객체에 속한 속성들이 하나라도 다를 경우 이는 다른 값으로 취급을 한다.**         
**객체에 속한 속성들이 모두 같을 경우 이는 동일한 값으로 취급을 한다.**                
         
예를 들자면, **도시/지번/우편번호가 같더라도 동호수가 다르면 다른 집이다.**               
이처럼 **모든 속성들에 대한 하나의 표현이 `값`이고 이를 객체로 나타낸것이 `VO`다.**          

```java
public class Address {
    
    private String city;         // 도시 
    private String street;       // 지번/도로명주소 
    private String zipcode;      // 우편 번호
    private String roomNumber;   // 동호수 
    
    ... // 생략
}
```

    
자바에서는 `==` 비교는 '메모리 주소값'이 동일한지 비교를 한다.             
더불어, `equals()`도 기본 세팅은 '메모리 주소값'이 동일한지 비교를 한다.          
더 나아가, Hash 관련 컬렉션프레임워크를 이용한다면 `hashcode` 가 동일한지도 비교를 한다. ([해시충돌 방지](https://github.com/springframework-sprout/spring-expert/blob/main/java/Hash%20Conflict.md))        

하지만, **VO는 `하나의 값 단위`이기에 값을 기준으로 비교를 하도록 Override를 해줘야한다.**          
    
```java
public class Address {

    private String city;         // 도시 
    private String street;       // 지번/도로명주소 
    private String zipcode;      // 우편 번호
    private String roomNumber;   // 동호수 

    ... // 생성자 생략  
    ... // getter 생략       
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Address address = (Address) o;
        return Objects.equals(getCity(), address.getCity()) && 
               Objects.equals(getStreet(), address.getStreet()) && 
               Objects.equals(getZipcode(), address.getZipcode()) && 
               Objects.equals(getRoomNumber(), address.getRoomNumber());
    }

    @Override
    public int hashCode() {
        return Objects.hash(getCity(), getStreet(), getZipcode(), getRoomNumber());
    }
    
}
```

이로써 VO의 '메모리 주소값'이 다르더라도 모든 속성 값이 동일하면 같은 객체로 인식하게 만들었다.              
            
## 수정자(setter)가 없는 불변 객체여야 한다.     
VO는 객체 자체로 **하나의 값 단위**이다.              
    
**값은 불변하다**          
숫자 `1, 2, 3, 4, ...` 와 알파벳 `a, b, c, d, ...` 같은 **값들은 항상 고정(불변)되어 있다.**            
이는 VO도 마찬가지이며 되도록 **VO의 값은 변하지 않으며 값 하나당 하나의 메모리를 가지도록 하는 것이 좋은 전략이다.**          
         
```java
public class Address {

    private String city;         // 도시 
    private String street;       // 지번/도로명주소 
    private String zipcode;      // 우편 번호
    private String roomNumber;   // 동호수 
    
    ... // 생성자 생략 
    ... // getter 생략 
    
    public void setCity(String city) { 
        this.city = city; 
    }
    public void setStreet(String street) { 
        this.street = street; 
    }
    public void setZipcode(String zipcode) { 
        this.zipcode = zipcode;
    }
    public void setRoomNumber(String roomNumber) {   
        this.roomNumber = roomNumber;   
    }
    
    ... // hash & equals 생략 
}
```

**그런데 만약 내부에 존재하는 값을 외부에서 막 이리저리 수정할 수 있다면? 🤔**         
이는 `값`이라고는 볼 수 없으며 이로 인해 **추적이 불가하고, 복사될 때는 의도치 않은 객체들이 함께 변경되는 문제를 유발한다.**        
            
**그렇다면 어떻게 값을 변하지 않게 만들 수 있을까? 🤔**               
가장 쉬운 방법으로 처음 생성을 하고 값을 변하지 못하게 하도록 `setter` 를 제거하면 된다.        
    
```java
public class Address {

    private String city;         // 도시 
    private String street;       // 지번/도로명주소 
    private String zipcode;      // 우편 번호
    private String roomNumber;   // 동호수 
    
    public Address(String city, String street, String zipcode, String roomNumber) {
        this.city = city;
        this.street = street;
        this.zipcode = zipcode;
        this.roomNumber = roomNumber;
    }
    ... // getter 생략 
    ... // hash & equals 생략 
}
```   
인스턴스 필드들이 private 이기에 외부에서는 더이상 setter를 통해 값을 변경하지 못하게 되었다.         
그런데 아직까지도 **내부에서 로직을 잘못 작성하면 값을 변경할 수 있는 가능성이 남아있다.**      
   
```java
public class Address {

    private final String city;         // 도시 
    private final String street;       // 지번/도로명주소 
    private final String zipcode;      // 우편 번호
    private final String roomNumber;   // 동호수 
    
    public Address(String city, String street, String zipcode, String roomNumber) {
        this.city = city;
        this.street = street;
        this.zipcode = zipcode;
        this.roomNumber = roomNumber;
    } 
    ... // getter 생략 
    ... // hash & equals 생략 
}
```   
이로써 완전한 불변객체가 되었다고 말을 할 수 있을 것 같다.      
이전에 언급했던 `hash & equals 를 재정의`한다면 다른 '메모리 주소값'이라도 같은 객체로 인식을 할 것이다.         

### 캐시 적용 
```java
public final class LottoNumber implements Comparable<LottoNumber> {

    private static final Map<Integer, LottoNumber> CACHE;
    private static final int MINIMUM = 1;
    private static final int MAXIMUM = 45;

    private final int lottoNumber;

    static {
        CACHE = new TreeMap<>();
        for (int i = MINIMUM; i <= MAXIMUM; i++) {
            CACHE.put(i, new LottoNumber(i));
        }
    }

    private LottoNumber(int lottoNumber) {
        validateRange(lottoNumber);
        this.lottoNumber = lottoNumber;
    }

    public static final LottoNumber valueOf(String lottoNumber) {
        return valueOf(Integer.valueOf(lottoNumber));
    }

    public static final LottoNumber valueOf(int lottoNumber) {
        validateRange(lottoNumber);
        return CACHE.get(lottoNumber);
    }

    private static final void validateRange(int lottoNumber) {
        if (lottoNumber < MINIMUM || lottoNumber > MAXIMUM) {
            throw new LottoNumberOutOfRangeException();
        }
    }
    
    ... // 생략
}    
```
만약, 같은 메모리 주소를 반환하고자 한다면 캐시를 만들어 미리 저장했다가 반환하는 방식을 고려해도 좋을 것같다.    
     
### JPA에서의 VO   
JPA에서의 VO는 주로 Entity 클래스에 속한 인스턴스 필드로 사용된다.                   
즉, Entity와 매핑된 테이블의 하나의 **컬럼 역할**을 맡고 있다고 생각해도 좋다.              
그런데 **JPA에서의 VO는 문법상 파라미터가 없는 기본 생성자를 정의해야한다는 규칙때문에 final을 사용하지 못한다.**          
왜냐하면 **final은 생성자 호출시 무조건 초기화가 되어야하는데 기본 생성자로 인해 초기화가 되지 않기 때문이다.**           
(생성원리는 리플랙션을 이용한 강제 생성인데, 리플랙션에서 기본 생성자를 기반으로 강제 생성을 하기 때문이다.)        
   
```java  
@Embeddable  
public class Address {
                                 // @Column()을 입력해야하지만, 생략시 기본 전략으로 세팅된다.   
    private String city;         // 도시 
    private String street;       // 지번/도로명주소 
    private String zipcode;      // 우편 번호
    private String roomNumber;   // 동호수 
      
    private Address(){}    
    // protected Address(){} , 둘중 하나 선택  
    
    public Address(String city, String street, String zipcode, String roomNumber) {
        this.city = city;
        this.street = street;
        this.zipcode = zipcode;
        this.roomNumber = roomNumber;
    }
    
    ... // getter 생략 
    ... // hash & equals 생략 
}
```   
그렇기에 위와 같이 `기본 생성자` + `private 인스턴스 필드` 사용을 기본적으로 하되         
`final`을 사용하지 못하기에 값 변경에 대해서 주의해서 코드를 작성해야한다.           

## VO 의 장점   
VO의 장점은 많지만 필자가 생각하는 장점은 아래와 같다.   
                                     
1. 엔티티의 크기가 줄어들어 가독성이 높아진다.                            
2. 연관된 데이터들의 군집을 형성하기에 이들을 통합적으로 관리 및 사용하기 쉽다.(라이프사이클)
3. 객체가 생성될 때 해당 객체안에 제약사항을 추가하여 데이터를 검증할 수 있다. 
4. 생성될 인스턴스가 정해져 있는 경우 미리 인스턴스를 생성해놓고 캐싱하여 성능을 높일 수 있다.       
5. pubilc 한 메서드를 만들어 사용할 수 있기에 테스트 케이스가 많아진다.         
    
## VO 정리  
VO는 객체 자체로 하나의 값 단위이다.      
값 단위이기에 '메모리 주소값'이 아닌 모든 값들을 기준으로 동등하다 판단하고 불변해야한다는 특징을 가진다.            
갑들을 기준으로 동등 판단을 위해 equals와 hash 를 재정의하고             
불변을 위해 setter 를 지우고 되도록 final 을 통해 값을 변경하지 못하도록 한다.          

**참고**   
VO와 비슷한 클래스가 있는데 바로 `String`이다.             
`String`은 내부적으로 `equals()`를 `@Override`하여 값을 기준으로 비교를 한다.        
그리고 `불변 객체`로서 최대한 특정 값에 대하여 동일한 메모리 주소를 반환하려고 한다.(`new`사용만 안하면 된다.)       
   

# 📘 DTO(Data Transfer Object)  
> DTO = Domain Information + View Information      
                
DTO는 레이어간의 데이터 전송을 위해 사용되는 자료구조다.(데이터를 담는 바구니)         
여러 레이어 사이에서 DTO를 사용할 수 있지만, 주로 View와 Controller 사이에서 데이터를 주고 받을 때 활용한다.    
DTO는 getter/setter 메소드를 포함할 수 있으며 **이 외의 비즈니스 로직은 포함하지 않는다.**            

```java
public class MemberDto {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```
단순히 데이터를 넣고 빼는 역할밖에 하지 않기에 [객체라는 말 대신, 자료구조](https://github.com/kwj1270/TIL_CleanCode/blob/master/06%20%EA%B0%9D%EC%B2%B4%EC%99%80%20%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0.md)라는 말을 사용했다.         

```java
public class MemberDto {
    private final String name;
    private final int age;

    public MemberDto(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```
한편, `setter`가 아닌 생성자를 이용해서 초기화하는 경우 불변 객체로 활용할 수 있다.            
불변 객체로 만들면 데이터를 전달하는 과정에서 데이터가 변조되지 않음을 보장할 수 있다.            
          
## DTO 의 본질 
흔히 `DTO의 핵심 가치`를 **레이어간의 전송에 사용된다고 알려져있는데 반은 맞고✔ 반은 틀린 생각이다.❌**         
진짜 `DTO의 핵심 가치`는 **Domain 로직과 UI 로직의 의존성을 낮추는 역할**을 하는 것이다.               
   
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

## MVC Pattern과 Layered Architecture        
기능을 차츰 구현하거나 서비스가 점점 확장되면서 `Controller`에서 모든 작업을 처리하는 것은 힘들어졌다.          
즉, `Controller`내의 중복된 코드가 많아지고 가독성이 떨어지게되면서 유지 보수하기 힘들어지게 되었다.           
이를 해결하기 위해, **`유사한 관심사`들을 레이어로 나누고 추상화하여 수직적으로 배열하는 `Layered Architecture`가 등장했다.**      
    
![layer](https://user-images.githubusercontent.com/50267433/125921580-1fec4734-355a-470c-926f-5db481059759.png)   
     
`Layered Architecture`는 **유사한 관심사들을 레이어로 나누고 추상화하여 수직적으로 배열하는 아키텍처다.**         
하나의 레이어는 **자신에게 주어진 고유한 역할을 수행**하고, **인접한 다른 레이어와 상호작용**한다.         
이렇게 레이어로 나누면 **시스템 전체를 수정하지 않고도 특정 레이어를 수정 및 개선할 수 있어 재사용성과 유지보수에 유리하다.**           
   
## DTO 변환 위치           
`DTO`는 `View`와 `Controller`간의 데이터 통신에서 사용되고       
`Domain`은 도메인 로직을 수행하는 어느 곳에서 사용되어야 한다.       
**그렇다면 DTO 에서 Domain 으로 변환하는 과정은 어느 레이어에서 진행해야할지에 대한 의문은 남아있다.🤔**         
             
객체의 생성과 관리는 해당 객체가 필요한 시점에서 진행이 되는 것이 좋다.                 
즉, `DTO`는 `View`와 `Controller`간의 데이터 통신에서 사용되기에 `Controller`에 위치되는 것이 일반적이었다.         

```java
@RestController
public class QuestionController {
    
    private final QuestionService questionService;
    
    public QuestionController(QuestionService questionService) {
        this.questionService = questionService;
    }
    
    @PostMapping("/questions")
    public ResponseEntity<?> save(@RequestBody QuestionSaveRequestDto questionSaveRequestDto) {
        QuestionResponseDto questionResponseDto = new QuestionResponseDto(questionService.save(questionSaveRequestDto.entity()));
        return ResponseEntity.ok().body(questionResponseDto);
    }

}
```
```java
@Service
public class QuestionService {
    public Question save(Question newQuestion) {
        Set<Tag> tags = tagService.processTags(newQuestion.getPlainTags());
        Question savedQuestion = questionRepository.save(newQuestion);
        return savedQuestion;
    }
}
```    
   
그런데 만약, **`DTO`에서 `Entity`로 변환할 때 DB에서 데이터를 조회할 필요가 있다면 어떻게 될까?**           
이 때는 컨트롤러에서 처리할 수가 없기에 **서비스에서 `DTO`에서 `Entity` 객체 변환을 담당하게 해야할 것이다.**      

```java
@RestController
public class QuestionController {
    
    private final QuestionService questionService;
    
    public QuestionController(QuestionService questionService) {
        this.questionService = questionService;
    }
    
    @PostMapping("/questions")
    public ResponseEntity<?> save(@RequestBody QuestionSaveRequestDto questionSaveRequestDto) {
        QuestionResponseDto questionResponseDto = new QuestionResponseDto(questionService.save(questionSaveRequestDto));
        return ResponseEntity.ok().body(questionResponseDto);
    }
}
```
```java
@Service
public class QuestionService {
    public Question save(QuestionSaveRequestDto questionSaveRequestDto) {
        Set<Tag> tags = tagService.processTags(questionSaveRequestDto.getPlainTags());
        Question savedQuestion = questionRepository.save(questionSaveRequestDto.entity());
        return savedQuestion;
    }
}
``` 
하지만, Service layer에서 `DTO`를 `DTO`에서 `Entity` 객체 변환을 담당하게 될 경우           
해당 **Service를 사용하는 여러 Controller에서는 DTO 타입에 맞춰서 코드를 작성해야한다**는 문제가 발생한다.          
     
```java
@RestController
public class OtherController {
    
    ...// 생략
    
    @PostMapping("/others")
    public ResponseEntity<?> save(@RequestBody OtherRequestDto otherRequestDto) {
        QuestionSaveRequestDto = new QuestionSaveRequestDto(otherRequestDto.title(), otherRequestDto.content(), otherRequestDto.writer()); 
        QuestionResponseDto questionResponseDto = new QuestionResponseDto(questionService.save(questionSaveRequestDto));
        return ResponseEntity.ok().body(questionResponseDto);
    }
}
```
위 케이스 같은 경우는 `DTO`에서 `DTO`의 변환이 가능한 경우로 볼 수 있는데         
만약, 알맞는 값이 없어서 해당 서비스에 맞는 DTO로 변환하지 못하는 경우에는 아에 그 서비스를 이용하기 힘들 것이다.          
     
**그렇다면 어떻게 해야할까? 🤔**         
과거 개발자님들께서는 `transform tier`라는 별도의 **Converter 계층**을 두고 변환 로직을 모두 Converter에 담고,             
각 Converter를 서비스에 주입해서 서비스가 `DTO`에서 `Entity` 객체간 변환을 Converter에게 위임해서 처리하게 했다.                 
물론, 좋은 선택이었지만 한편으로 **불필요한 계층 생성 및 코드의 복잡성을 증가시키는건 아닐까?** 하는 의문점은 남아있다.     
       
이러한 의문에 [HomoEfficio 님](https://github.com/HomoEfficio)께서는 방향마다 다르게 보는 것이 낫다고 말씀해 주셨다.        
       
### View -> DTO -> Domain  
`View -> DTO -> Domain` 객체 방향의 흐름에서는 **View에서 전달받은 정보만으로 Domain 객체를 구성할 수가 없다.**       
예를 들면, **View에서는 ID만 전달하기도 하는데 ID만으로는 Domain 객체를 구성할 수 없다.**                     
즉, **Repository를 통해 조회한 후에나 Domain 객체를 구성할 수 있다는 상황이 발생한다.**                         
      
이 방향의 DTO 변환을 컨트롤러 계층에서 하는 건 사실 상 불가능하기도 하다.          
앞서 설명한 것처럼 서비스 계층에 DTO 가 아닌 도메인 객체를 넘겨줘야 하는데,             
컨트롤러는 스스로 Domain 객체를 구성할 능력이 없기 때문이다.              
그래서 `DTO -> Domain` 객체 변환 책임을 서비스에게 넘기는 것이 낫다는 의견이 있다.        
          
하지만 필자의 생각은 조금 다르다.           
**View에서 ID 하나만 전달한다면 굳이 DTO로 변환해 서사용할 필요가 없다고 생각이든다.**               
즉, ID 하나만 들어오면 해당 ID를 서비스로 넘겨주기만 하도록 하고        
**그외는 View로부터 받은 값들을 DTO를 만들었다가 Entity로 변환하는 작업을 진행하면 된다.**             

물론, 통일성을 위해 ID를 DTO로 변환해서 작업하는 것도 나쁘지는 않지만 이로인해 발생하는 리스크가 너무 많다.      
사실 **코드에는 정답은 없으니** 이정도 유연성을 가지고 코드를 구현하는 것은 **나쁘지 않다고 본다.**               
  
**정리**     
* ID값만 넘어왔을 경우 변환하지 않고 Service로 ID를 넘긴다.       
* 왠만하면 DTO로 만들어서 Service로 넘어가기전에 Controller에서 Entity로 변환한다.       
* Service는 Entity를 파라미터 타입으로 원할 경우 다양한 Controller에서 사용하기 편해진다.   
* 마찬가지로 ID만 넘겨주는 것도 다양한 Controller에서 사용하기 편해진다.    
  
### View <- DTO <- Domain
필요한 모든 Domain 객체의 데이터는 서비스의 Repository를 통해 얻어올 수 있다.          
또한 DTO에 전달할 데이터를 서비스단의 Domain 객체가 모두 가지고 있으므로                
**서비스에서 Domain -> DTO 변환은 적극적으로 찬성하는 바이다.**           
                 
아니면, 서비스가 Domain 객체를 DTO 를 생성할 때 주입하고            
컨트롤러가 DTO에서 필요한 데이터를 빼낼 때 변환 로직이 실행되는 방식도 좋아 보인다.         
그리고 이 방향의 DTO 변환을 서비스 계층에서 할 때 어떤 단점이 있는지는 잘 떠오르는 것은 없다.    
   
**그렇다면 만약, 이 방향에서의 DTO 변환을 Controller 계층에서 한다면 어떤 단점이 있을까?🤔**     
* 반환할 필요가 없는 데이터까지 Domain 객체에 포함되어 컨트롤러 계층에 까지 넘어온다.    
* 여러 Domain 객체로부터 조합되는 DTO의 경우 컨트롤러 계층에서 조합해야 하며 결국 응용 로직이 컨트롤러에 스며든다.       
* 여러 Domain 객체를 조회하는 서비스를 각각 호출해야 하므로 의존하는 서비스의 갯수가 늘어날 수 있다.

## DTO 정리   

# 참고 
[VO(Value Object)란 무엇일까?](https://woowacourse.github.io/tecoble/post/2020-06-11-value-object/)      
[#](https://www.slipp.net/questions/93)       
[#](https://xlffm3.github.io/spring%20&%20spring%20boot/DTOLayer/)   
[#](https://github.com/HomoEfficio/dev-tips/blob/master/DTO-DomainObject-Converter.md)       
[#](https://woowacourse.github.io/tecoble/post/2021-05-16-dto-vs-vo-vs-entity/)          
[#](https://dev-splin.github.io/spring/Spring-Entity-DTO-VO/)    
