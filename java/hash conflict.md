해시 충돌 
================

**hashCode()**     
`hashCode()` 메서드는 해싱 기법에 사용되는 해싱함수를 구현한 것이다.           

```
// 필자가 임의로 만든 문법이 없는 코드이다.
// 해싱을 이해하는데만 사용하자  

함수 hashing(주소값) {
    상자 번호 = 주소값 / 10;
    상자[상자 번호] = 값;
}
```
* 해싱은 데이터관리기법 중 하나로 다량의 데이터를 저장하고 검색하는데 유용하다.       
* 쉽게 설명하면, 마구잡이로 물건을 수납장에 넣는 것이 아닌, 특정 로직에 따라 분류해서 넣는다.   
* 데이터를 넣는 과정에서는 그냥 넣는 것에 비해 추가적인 로직이 들어가지만,              
* 데이터를 찾을 때는 모든 범위를 찾는 것이 아닌 특정 범위만 찾아도 되므로 검색에 빠르다.         

```
// 필자가 임의로 만든 문법이 없는 코드이다.
// 해싱을 이해하는 데만 사용하자  

함수 hashCode() {
    return 객체 주소값(디폴트) / 10; // 오버라이딩을 통해 원하는 피연산 값을 바꿀 수 있다.    
}
```
* 해시함수는 찾고자 하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시코드를 반환한다.   
     
그런데 `equals()`에서는 기존처럼 객체의 주소로 비교를 하거나          
오버라이딩을 통해 내부에 존재하는 값을 비교하여 같은지 여부를 판단하는데,       
`hashCode()`는 언제 사용되며 왜 `equals()`랑 같이 정의해야 한다고 말할까?        
이유는 `Hash 관련 클래스`에서 `hashCode()의 반환값`이 비교 대상이 되기 때문이다.     
    
일반적으로 해시코드가 같은 두 객체가 존재하는 것이 가능하지만,          
`Object` 클래스에 정의된 `hashCode()`는 객체의 주소값을 이용해서 해시코드를 만들어 반환한다.              
즉, 오버라이딩을 하지 않으면 서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없다.         
이 같은 특징은 디폴트로 설정된 만큼 모든 객체가 다르다는 장점으로 작용할 수 있지만     
`equals()`새롭게 정의해서 같은 객체이지만 반환되는 hashCode가 달라지는 **해시충돌**이 발생한다.      
       
**해시충돌**    
가정 1. `equals()`만 작성했을 경우          
```java
import java.util.HashMap;
import java.util.Objects;

public class Person {
    long id;

    public Person(long id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return id == person.id;
    }
 /*
    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
 */

}

class Main2 {
    public static void main(String[] args) {
        HashMap<Person, Integer> map = new HashMap<>();

        map.put(new Person(1), 1);
        map.put(new Person(2), 2);

        System.out.println(map.get(new Person(1)));
    }
}
````
```java
결과 : null
```  
* 내가 특정 비교 기준을 정하여 해당 기준으로 객체의 같은지 판단이 가능하다.  
* 단, `Hash 관련<key,value>` 컬렉션 프레임워크 사용시 문제가 발생한다.   
* `Hash` 컬렉션 프레임워크는 `key`값으로 해시코드를 저장한다.   
* `hashCode()`를 작성하지 않았을 경우 같은 객체로 지정했지만 해시코드는 서로 달라진다.   
* 기본 `hashCode()`는 주소값을 기준으로 해싱하기 때문이다.    
* 그렇기에 내부 값은 같은 새로운 객체를 생성하면 값을 찾을 수 없다는 null이 반환된다.   
       
가정 2. `hashCode()`만 작성했을 경우                
<img width="724" alt="스크린샷 2021-01-07 오후 10 17 03" src="https://user-images.githubusercontent.com/50267433/103897016-3ee1c780-5136-11eb-86ff-f257604fb7a1.png">  
* `HashTable`은 `<key,value>` 형태로 데이터를 저장한다. 
* 이 때 `hashCode()`을 이용하여 `key`값을 기준으로 고유한 식별값인 해시값을 만든다. 
* `hashCode()`를 통해 생성된 해시값을 `버킷(Bucket)`에 저장한다.
* `hashCode()`를 오버라이딩함으로써 해시값의 범위를 우리가 지정할 수 있다.  
* 하지만, `HashTable` 크기는 한정적이며 해시값이 겹칠 수도 있게 된다.   
* 해시값이 겹칠 경우 LinkedList 기반으로 앞 값에 뒤이어 붙는다.       
* 즉, 서로 다른 객체라 하더라도 같은 해시값을 갖게 될 수도 있다.
* 결과적으로 서로 다른 객체이지만 같은 해시코드를 같게 된다는 문제가 발생한다.    

클래스의 인스턴스변수 값으로 객체의 같고 다름을 판단해야 하는 경우라면         
`equals()` 뿐만 아니라 `hashCode()` 적절히 오버라이딩 해야한다.        
같은 객체라면 `hashCode()`를 호출했을 때의 결과값인 해시코드도 같아야 한다.        
그래야 Hash 관련 클래스를 사용할 때도 올바르게 동작을 하는 코드를 만들 수 있다.   

**String과 equals()와 hashCode()**     
```java
pubilc static void main(String[] args) {
    // Create three String objects.
    String strA = new String("APPLES");
    String strB = new String("APPLES");
    String strC = new String("ORANGES");

    // Create a String reference and assign an existing String's reference to it
    // so that both references point to the same String object in memory.
    String strD = strA;

    // Print out the results of various equality checks
    System.out.println(strA == strB); // false
    System.out.println(strA == strC); // false 
    System.out.println(strA == strD); // true
}
```
* String 클래스는 내부값이 한 번 할당되면 값을 바꿀 수 없는 불변 객체이다.     
* 그렇기 때문에 문자열을 바꾸고자 하면 다른 메모리를 참조하게 된다.            
* 만약, 새로운 메모리에 객체를 생성하고자 할 때는 `new String()`을 사용한다.           
* 하지만, 이 같은 경우 메모리 주소가 서로 다르므로 `false`를 리턴한다.     

```java
public class String {
    // 생략 
    
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
    
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
    
    // 생략 

}    
```
* **우리는 문자열의 비교를 메모리 주소가 아닌 문자열 리터럴로 비교하고 싶어한다.**     
* `String` 클래스는 `equals()`를 오버라이딩하여 문자열을 비교하도록 정의되어 있다.       
* 물론 이에 따른 `hashCode()`도 새롭게 오버라이딩 정의하고 있다.    
   
