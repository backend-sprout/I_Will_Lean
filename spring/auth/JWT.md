# JWT
JWT는 정보를 JSON 객체를 사용해서 토큰 자체의 정보들을 저장하고 있는 `Web Token`이다.      
JWT는 `Header`, `Payload`, `Signature` 3 개의 부분으로 구성되어져 있다.   
  
* Header: **시그니처를 해싱하기 위한 알고리즘 정보들이 담겨져있다.(변경 가능)**            
* Payload: 서버와 클라이언트가 주고 받는, **시스템에서 실제로 사용될 정보에 대한 내용들을 담고 있다.**        
* Signature: 토큰의 유효성 검증을 위한 문자열로 **서버에서는 이 문자열로 토큰이 유효한지 검증한다.**         
    
즉, Signature를 통해 인증을 하는데 이를 위한 보안 알고리즘(공개키/기본키 or 비밀키)이 Header에 들어있으며    
인증이 완료되면 `Payload`의 데이터를 통해 서로 정보를 교환한다.   
   
쿠키나 세션을 이용한 인증보다 안전하고 효율적이다.(이 부분은 상황마다 다르다.)     

## 동작 원리
    
```http
# http_request_message header
Authorization: <type> <credentials> 
```  
  
일반적으로 [Bearer스키마](https://gist.github.com/egoing/cac3d6c8481062a7e7de327d3709505f)를 사용하여 **Authorization 헤더에 JWT를 보낸다.**    
`Request Header`에 담겨져 오기 때문에 `Header 값(Authorization)`을 확인해서 가져올 수 있다.       
          
서버는 `Authorization 헤더`에 유효한 JWT가 있는지 확인하고        
JWT가 유요할 경우 사용자는 보호된 리소스에 액세스할 수 있도록 인증/인가해준다.        
         
참고로, 토큰이 `Authorization 헤더` 로 전송되면      
`CORS(Cross-Origin Resource Sharing)`는 쿠키를 사용하지 않으므로 문제가 되지 않는다.     

1. 애플리케이션 또는 클라이언트가 `권한 부여 서버`에 권한 부여를 요청한다.          
   `권한 부여`에는 내부적으로 다양한 권한 부여 프로세스가 있는데 `권한 부여 서버`는 이들 중 하나를 수행한다.      
   예를 들어, 일반적인 OpenID Connect 호환 웹 애플리케이션은 `/oauth/authorize` 사용하여 엔드포인트를 통과한다.   
2. 권한이 부여되면 `권한 서버`는 **애플리케이션에 액세스 토큰을 반환한다.**      
3. 애플리케이션은 액세스 토큰을 사용하여 보호된 리소스(예: API)에 액세스합니다.    
  
         
## 장단점    
### 장점    
* 중앙 인증 서버, 데이터 스토어(저장소)에 대한 의존성이 없어서 수평 확장에 유리        
  **설명 :** 세션을 동기화할 필요 없이 토큰의 유무만으로 인증/인가가 가능        
* [Base64 URL Safe Encoding](https://babyprogram.tistory.com/50) 이라 URL, Cookie, Header 어떤 형태로도 사용 가능  
* [Stateless 한 서버 구현](https://junshock5.tistory.com/83) 가능     
* 웹이 아닌 모바일에서도 사용 가능
* 인증 정보를 다른 곳에서도 사용 가능 (OAuth)     
    
### 단점  
* `Payload` 에 저장하는 정보가 많아지면 네트워크 사용량 증가(트래픽이 증가), 데이터 설계 고려 필수    
* 토큰이 클라리언트에 저장, 서버에서 클라이언트의 토큰을 조작 불가능 
* 다른 사람이 토큰을 decode 하여 데이터 확인 가능 
* 토큰을 탈취당한 경우 대처하기 어려움
    * 기본적으로는 서버에서 관리하는게 아니다보니 탈취당한 경우 강제 로그아웃 처리가 불가능   
    * 토큰 유효시간이 만료되기 전까지 탈취자는 자유롭게 인증 가능  
    * 그래서 유효시간을 짧게 가져가고 refresh token 을 발급하는 방식으로 많이 사용   

## 구성요소 
### Header
헤더는 일반적으로 두 부분으로 구성된다.   
  
1. **alg:** Signature 를 해싱하기 위한 알고리즘 정보를 갖고 있다.(HMAC/HS256/RSA)       
2. **typ:** 토큰의 타입을 나타내는데 없어도 된다. 보통 JWT 를 사용한다.         
   
```json    
{
  "alg": "HS256",
  "typ": "JWT"
}
```
  
### Payload  
페이로드는 클레임(claims)을 포한한다.           
클레임은 엔터티(일반적으로 사용자) 및 추가 데이터에 대한 설명이다.                
클레임에는 `등록(register)`, `공개(public)`, `비공개(private)`라는 세 가지 유형이 있다.                  
 
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

#### Register claims 
필수는 아니지만 유용한 클레임을 제공하기 위해 권장되는 미리 정의된 클레임 집합        

* **iss:** 토큰 발급자
* **sub:** 토큰 제목
* **aud:** 토큰 대상
* **exp:** 토큰의 만료시간
* **nbf:** Not Before
* **iat:** 토큰이 발급된 시간
* **jti:** JWT의 고유 식별자
* [기타 등등](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1)    
   
#### Public claims    
JWT 사용자들이 마음대로 정의할 수 있는 클레임이다.       
그러나 다른 클레임과의 충돌 방지를 위해 `IANA JSON 웹 토큰 레지스트리`에 정의하거나     
`충돌 방지 네임스페이스`를 포함하는 URI로 정의해야 한다.       

#### Private claims   
사용에 동의하고 등록된 클레임이나 공개 클레임이 아닌      
단순히 당사자 간에 정보를 공유하기 위해 생성된 커스텀 클레임이다.      
    
### Signature      
서버에서 토큰이 유효한지 검증하기 위한 문자열이다.      
`Header + Payload + Secret Key`로 값을 생성하기에 **데이터 변조 여부를 판단할 수 있다.**          
`Secret Key`는 노출되지 않도록 서버에서 잘 관리할 필요가 있다.     
   
```json
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload), secret
)
```

**개인 키**로 서명된 토큰의 경우 JWT의 보낸 사람이 누구인지 확인할 수도 있다.    
  

### 실제 동작 확인 
[jwt 디버깅](https://jwt.io/#debugger-io) 사이트를 이용해서 실제 어떻게 변환되는지 확인할 수 있다.   

