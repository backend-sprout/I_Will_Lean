# JWT
JWT는 정보를 JSON 객체를 사용해서 토큰 자체의 정보들을 저장하고 있는 `Web Token`이다.      
`Header`, `Payload`, `Signature` 3 개의 부분으로 구성되어 있으며          
쿠키나 세션을 이용한 인증보다 안전하고 효율적이다.(이 부분은 상황마다 다르다.)     
    
```
#header
Authorization: <type> <credentials> 
```

일반적으로 `Request Header`에 담겨져 오기 때문에 `Header 값`을 확인해서 가져올 수 있다.     

## 장단점  
### 장점 
* 중앙 인증 서버, 저장소에 대한 의존성이 없어서 수평 확장에 유리하다.      
  **설명 :** 세션을 동기화할 필요 없이 토큰의 유무만으로 인증/인가가 가능하다.      
* [Base64 URL Safe Encoding](https://babyprogram.tistory.com/50) 이라 URL, Cookie, Header 어떤 형태로도 사용 가능하다.    
* [Stateless 한 서버 구현](https://junshock5.tistory.com/83) 가능하다.    
* 웹이 아닌 모바일에서도 사용 가능허다.   
* 인증 정보를 다른 곳에서도 사용 가능하다 (OAuth)   

### 단점  
* 
*
*
*


  
  
