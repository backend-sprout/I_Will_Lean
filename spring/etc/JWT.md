# JWT
JWT는 정보를 JSON 객체를 사용해서 토큰 자체의 정보들을 저장하고 있는 `Web Token`이다.      
JWT는 `Header`, `Payload`, `Signature` 3 개의 부분으로 구성되어져 있다.   
  
* Header: **시그니처를 해싱하기 위한 알고리즘 정보들이 담겨져있다.(변경 가능)**            
* Payload: 서버와 클라이언트가 주고 받는, **시스템에서 실제로 사용될 정보에 대한 내용들을 담고 있다.**        
* Signature: 토큰의 유효성 검증을 위한 문자열로 **서버에서는 이 문자열로 토큰이 유효한지 검증한다.**         
    
즉, Signature를 통해 인증을 하는데 이를 위한 보안 알고리즘(공개키/기본키 or 비밀키)이 Header에 들어있으며    
인증이 완료되면 `Payload`의 데이터를 통해 서로 정보를 교환한다.   
   
쿠키나 세션을 이용한 인증보다 안전하고 효율적이다.(이 부분은 상황마다 다르다.)     
 
## 장단점    
### 장점    
`JWT`의 근간이 되는 `JSON`은 `XML`보다 크기가 작고 간결하여 읽기도 쉽고 트래픽도 적게 발생한다.      
더불어 `Spring` 같은 경우 JSON을 자동으로 매핑하는 기능들이 많기에 이를 적극 활용하기도 편하다.          
    
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


## 동작 원리
**http_request_message header**
```http
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

### 토큰 인증 타입  
```http
Authorization: <type> <credentials> 
```  
`<type>` 부분에 들어갈 값으로     
엄격한 규칙이 있는건 아니고 일반적으로 많이 사용되는 형태라고 생각하면 된다.     
        
|토큰 인증 타입|설명|
|--|-----|
|Basic|사용자 아이디와 암호를 Base64 로 인코딩한 값을 토큰으로 사용|
|Bearer|JWT 또는 OAuth 에 대한 토큰을 사용|
|Digest|서버에서 난수 데이터 문자열을 클라이언트에 보냄<br>클라이언트는 사용자 정보와 nonce 를 포함하는 해시값을 사용하여 응답|
|HOBA|전자 서명 기반 인증|
|Mutual|암호를 이용한 클라이언트-서버 상호 인증|
|AWS4-HMAC-SHA256|AWS 전자 서명 기반 인증|
  
# 리프레시 토큰       
**JWT 역시 탈취되면 누구나 API를 호출할 수 있다는 단점이 있다.**     
세션은 탈취된 경우 세션 저장소에서 세션 ID를 삭제하면 되지만,      
**JWT는 서버에서 관리하지 않기 때문에 속수 무책으로 당할 수 밖에 없다.**        
         
그래서 탈취되어도 피해가 최소화 되도록 유효시간을 짧게 가져간다.        
하지만 만료 시간을 30분으로 설정하면 일반 사용자는 30 분마다 새로 로그인 하여 토큰을 발급받아야 한다.     
사용자가 매번 로그인 하는 과정을 생략하기 위해 필요한 게 `Refresh Token`이다       
           
`Refresh Token`은 `로그인 토큰 (Access Token)` 보다 긴 유효 시간을 가진다.   
만약, `Access Token`이 만료된 사용자가 **재발급을 원할 경우 `Refresh Token`을 함께 전달한다.**     
서버는 `Access Token` 에 담긴 사용자의 정보를 확인하고        
**`Refresh Token`이 아직 만료되지 않았다면 새로운 토큰을 발급해줍니다.**                  
이렇게 하면 사용자가 매번 로그인해야 하는 번거로움 없이 로그인을 지속적으로 유지할 수 있다.     
   
`Refresh Token`은 사용자가 **로그인 할 때 같이 발급되며,**       
**클라이언트가 안전한 곳에 보관**하고 있어야 합니다.      
      
`Access Toekn`과 달리 매 요청마다 주고 받지 않기 때문에 탈취 당할 위험이 적으며,        
요청 주기가 길기 때문에 별도의 저장소에 보관 한다. (정책마다 다르게 사용)    
      
## Refresh Token 저장소    
`Refresh Token`은 **서버에서 별도의 저장소에 보관하는 것이 좋다.**      
        
`Refresh Token`은 사용자 정보가 없기 때문에        
저장소에 값이 있으면 검증 시 어떤 사용자의 토큰인지 판단하기 용이하다.         
           
탈취당했을 때 저장소에서 `Refresh Token` 정보를 삭제하면        
`Access Token` 만료 후에 재발급이 안되게 **강제 로그아웃 처리가 가능하다**    
**일반적으로 Redis 많이 사용한다.**         
        
## Refresh Token 으로 Access Token 재발급 시나리오  
1. `access token`으로 요청을 마구 보내던 클라이언트는 유효기간이 얼마 남지 않았음을 확인    
2. `access token`이 만료되었거나 만료가 근접한 시점에 **재발급을 위해 `access token + refresh token` 을 함께 보냄**    
3. 서버는 `refresh token`의 만료 여부를 확인
4. `access token`으로 유저 정보 (username 또는 userid) 를 획득하고      
   저장소에 해당 유저 정보를 `key 값`으로 한 `value`가 `refresh token`과 일치하는지 확인   
   `{username : refreshtoken}`  
5. 3~4번의 검증이 끝나면 **새로운 토큰 세트 (access + refresh) 발급**     
   `refreshtoken`도 새롭게 발급하는 것이다.        
6. 서버는 `refresh token 저장소`의 `value` 업데이트    
