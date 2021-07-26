Exception 전략
===============
스프링은 `예외처리`에 대한 막강한 어노테이션들을 제공하고 있다.     
하지만, 해당 어노테이션을 언제 어떻게 사용할지 무엇을 위해 사용할지 그 감을 잡기 힘들다.        
필자 또한 처음 공부해보기에 나름대로 정리를 해서 문서화시키고자 한다.      
  
# 📙 @ExceptionHandler    
`@ExceptionHandler`는 하나의 클래스(컨트롤러 등)의 메서드에 추가되어      
해당 클래스의 `Exception Handler`로써 동작을 하는 어노테이션이다.       
즉, 해당 영역에서 에러가 발생하면 `@ExceptionHandler`가 정의된 메서드에서 나머지 작업을 처리한다.      

```java
pubilc class SampleCounter
@ExceptionHandler(MethodArgumentNotValidException.class)
protected ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {
    log.error("handleMethodArgumentNotValidException", e);
    final ErrorResponse response = ErrorResponse.of(ErrorCode.INVALID_INPUT_VALUE, e.getBindingResult());
    return new ResponseEntity<>(response, HttpStatus.BAD_REQUEST);
}

```




# Error Response 객체       
만약, **Error Response의 형태가 각양각색이면 어떨까? 🤔**              
클라이언트/서버 모두 **Error Response에 대한 별도의 처리 로직을 구현해야한다.**              
즉, **가독성을 해치는 것은 물론, 코드의 복잡성을 야기하여 에러의 가능성을 높인다.**      
이러한 문제를 해결하기 위해 **✔ 통일된 Error Reponse를 정의하여 사용해야 한다.**    
        
통일된 `Error Response`를 구현함에 있어 흔히들 객체를 유연하게 처리하기 위해 `Map<Key, Value>`형식을 사용한다.       
하지만, **`Map`은 런타입시에 정확한 형태를 갖추기 때문에 정확히 무슨 키에 무슨 데이터가 있는지 확인하기 어렵다.**             

```java

```

# 참고
https://velog.io/@hanblueblue/Spring-ExceptionHandler   
https://jeong-pro.tistory.com/195   
https://github.com/binghe819/TIL/blob/master/Spring/%EA%B8%B0%ED%83%80/%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC%20%EA%B0%9C%EB%85%90%20%EB%B0%8F%20%EC%A0%84%EB%9E%B5.md    
https://gaemi606.tistory.com/entry/Spring-Boot-ControllerAdvice%EB%A1%9C-Exception-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0     
https://cheese10yun.github.io/spring-guide-exception/     
