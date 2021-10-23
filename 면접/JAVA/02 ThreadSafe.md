Thread Safe
===========
   
1. synchronyzed : 고유락을 통해 객체 단위로 락을거는 방식(락내의 다른 메서드를 호출하더라도 다른 스레드 접근 불가)   
2. volatile : 공유 인스턴스의 변수가 CPU CACHE로 인해 쓰레드마다 값이 다른걸 방지하기 위해 메인 메모리 직접 접근 
3. Atomic : 예상값을 비교하여 맞으면 새로 입력한 값으로 변경, 다르면 변경하지 않는 동작.  
4. Conccurrent : 위 3가지 기능을 적절히 사용하는 클래스들   

# 📕 synchronyzed  
메서드의 로직중 일부 영역을 임계영역으로 묶어서 관리 

```java
    public static Singleton getInstance() {
        if (singletonObject == null) {
            synchronized (Singleton.class) { // 자기 자신도 가능(this 가능) 
                if(singletonObject == null) {
                    singletonObject = new Singleton();
                }
            }
        }
        return singletonObject;
    }
```

# 📗 volatile         
공유되는 객체의 인스턴스 변수가   
각각의 Thread마다 CPU Cache에 저장된 값이 다르기에 값 불일치 문제가 발생하게 된다.                
이 같은 문제를 해결하기 위해 바로 메모리에 접근하도록 하는 기술 
       
**volatile**    
* volatile는 Java 변수를 Main Memory에 저장하겠다라는 것을 명시하는 것이다.
* 매번 변수의 값을 Read할 때마다 CPU cache에 저장된 값이 아닌 Main Memory에서 읽는 것이다.
* 또한 변수의 값을 Write할 때마다 Main Memory에 까지 작성하는 것이다.
      
```java
public class SharedObject {
    public volatile int counter = 0; 
}
```
   
`volatile`는 변수의 값을 `Read & Write`할 때마다 CPU Cache가 Main Memory에 저장된 값을 읽어 오도록 하는 키워드다.       
CPU Cache 가 아닌 Main Memory에 저장된 값을 바로 읽으므로 Multi Thread 환경에서의 불일치 문제를 해결할 수 있다.    
즉, 여러 쓰레드에서 공용으로 사용되는 변수의 값과 변동 사항에 대해서 일관성을 제공해준다.        
           
## 🤔 Long 과 Double 에서의 Volatile      
Java 는 long과 double을 제외한 primitive 타입의 변수를 읽고 쓰는 동작에 원자성을 지원해준다.            
그렇다면 왜 long과 double 변수에 대해서 원자성을 지원해주지 못하는걸까?    
             
이는 사실 JVM 비트수와 관련이 있다.           
자바 메모리 모델에 의하면 32비트 메모리에 값을 할당하는 연산은 중단이 불가능하다.(원자적이다)   
그렇지만 long과 double의 경우에는 64비트의 메모리 공간을 갖고있기 때문에 원자성 지원이 안 되었던 것이다.
       
* JVM 연산 비트수는 32비트이기에 long, doulbe에 대한 쓰기는 두번에 이루어진다 
 * 먼저 첫번째 32비트를 쓰고 다음 쓰기에서 두번째 32비트를 쓴다.
 * 첫번째 쓰기와 두번째 쓰기에서  다른 쓰레드가 끼어들어   
   두 32비트 값중 하나를 변경할 수 있기 때문에 long, double은 원자적 연산이 될 수 없다.
* 대신 volatile이 설정된 long, double이라면 항상 원자적이다.    
    * volatile을 사용을 한다는 것은 여러 쓰레드에서 하나의 변수가 같은 값을 읽도록 보장하는 것이기 때문에,   
      메모리를 2번 접근 하더라도 같은 값을 읽도록하는 변수에 접근하는 연산을 원자적으로 수행하게 보장한다는 것이다.     
* 이와 마찬가지로 shared 되는 64bit 값은 volatile이나 synchronize 로 선언하는게 좋다.    
   
   
이처럼, shared 되는 64bit 값은 volatile이나 synchronize 로 선언하는게 좋으며       
long, double 변수를 원자적으로 사용하고 싶다면 volatile로 선언하는게 좋다.     


# 📘 Atomic    
앞서 언급했듯이 Multi Thread 환경에서는 Thread가 값을 읽어올 때 각각의 CPU Cache에 저장된 값이 다르기에 값 불일치 문제가 발생한다.          
volatile은 Cpu Cache 가 아닌 Main Memory에서 바로 값을 가져오는 형태로 문제를 해결했다.            

**Atomic은 CAS를 기반으로 Cpu Cache 값이 Main Memory 값과 동일한 경우에만 동작하는 방식으로 문제를 해결한다.**          
즉, 현재 쓰레드 값과 메인 메모리 값을 비교하여 일치하는 경우 새 값으로 교체하고, 일치하지 않는다면 실패하고 재시도 한다.         
참고로 Main Memory에 존재하는 값을 확인하기 위해서 volatile 키워드도 사용하는 것을 알 수 있다.          
      
## 📖 CAS(CompareAndSwap)        
CAS란, 기존에 가지고 있던 값이 **예상하던 값과 같을 경우에만 새로운 값으로 할당하는 알고리즘이다.**              
   
```java
// CAS 예시
public class AtomicExample {
    int val;
    
    public boolean compareAndSwap(int oldVal, int newVal) {
        if(val == oldVal) {
            val = newVal;
            return true;
        } else {
            return false;
        }
    }
}
```
`이전 예상 값`과 `변경할 값`을 동시에 입력해서 일치하는지 검증한 후      
`이전 예상 값`이 일치한다면 값을 변경하는 작업을 수행하는 것이다.     
  
Java에서 제공하는 Atomic Type은 이러한 CAS를 하드웨어(CPU)의 도움을 받아               
**한 번에, 하나의 쓰레드만 접근 가능하고 메서드도 단 한번만 호출할 수 있다.**                    
이러한 Atomic 타입의 가장 큰 장점은 **NonBlocking 하면서 동기화 문제를 해결할 수 있다**는 점이다.            
     
# 📙 Concurrent 클래스    
Thread Safe 하다고 알려진 **Concurrent 클래스는 synchronized와 CAS를 이용한 클래스다.**                 
   
   
   
