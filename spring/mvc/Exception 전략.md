Exception 전략
===============
스프링은 `예외처리`에 대한 막강한 어노테이션들을 제공하고 있다.     
하지만, 해당 어노테이션을 언제 어떻게 사용할지 무엇을 위해 사용할지 그 감을 잡기 힘들다.        
필자 또한 처음 공부해보기에 나름대로 정리를 해서 문서화시키고자 한다.      

# 📙 @ExceptionHandler
 

# Error Response 객체       
만약, **Error Response의 형태가 각양각색이면 어떨까? 🤔**              
클라이언트/서버 모두 **Error Response에 대한 별도의 처리 로직을 구현해야한다.**              
즉, **가독성을 해치는 것은 물론, 코드의 복잡성을 야기하여 에러의 가능성을 높인다.**      
이러한 문제를 해결하기 위해 **✔ 통일된 Error Reponse를 정의하여 사용해야 한다.**    
        
통일된 `Error Response`를 구현함에 있어 흔히들 객체를 유연하게 처리하기 위해 `Map<Key, Value>`형식을 사용한다.       
하지만, **`Map`은 런타입시에 정확한 형태를 갖추기 때문에 정확히 무슨 키에 무슨 데이터가 있는지 확인하기 어렵다.**             

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
protected ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {
    log.error("handleMethodArgumentNotValidException", e);
    final ErrorResponse response = ErrorResponse.of(ErrorCode.INVALID_INPUT_VALUE, e.getBindingResult());
    return new ResponseEntity<>(response, HttpStatus.BAD_REQUEST);
}
```

