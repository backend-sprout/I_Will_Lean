# LogBack
`버그가 발생`, `모니터링`과 같이 애플리케이션을 개발하는 과정에서 개발자는 로그를 기록하고 분석한다.    
`Logback`은 `Java`에서 가장 많이 사용되었던 로깅 라이브러리인 `log4j`의 후속 버전이다.       
`log4j`를 설계한 `Ceki Gülcü`에 의해 설계되었기에 더욱 좋아진 `log4j`라고 보아도 무방하다.

애플리케이션에 대한 로깅을 진행하고자 한다면 아래와 같은 3단계를 거치면 된다   
    
1. 로그백을 실행할 수 있는 환경을 구성한다.  
2. 로깅을 수행하려는 모든 클래스에서   
   `org.slf4j.LoggerFactory`클래스의 `getLogger()` 메소드를 호출하고    
   현재 클래스의 `FQCN` 또는 `클래스.class`를 매개 변수로 전달하여 `Logger` 인스턴스를 검색한다.   
3. `debug()`, `info()`, `warn()` 및 `error()` 메서드를 호출하여 Appender에 대한 로깅 출력을 생성한다.  

**간단 예시**
```java
package chapters.introduction;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld1 {

  public static void main(String[] args) {

    Logger logger = LoggerFactory.getLogger("chapters.introduction.HelloWorld1");
    logger.debug("Hello world.");

  }
}
```
```shell
20:49:07.962 [main] DEBUG chapters.introduction.HelloWorld1 - Hello world.
```


# LogBack 특징 
* 광범위한 지원 문서
* XML과 Groovy 설정을 지원
* 설정 변경시 설정 자동 reload(재로드)
* 로그 I/O (입출력) 오류 발생시 우아한 복구
* 필요없는 로그 파일 자동 제거
* 보관된 로그 파일의 자동 압축
* Prudent 모드
* 설정 파일에 조건문을 사용
* Filter
* SiftingAppender
* 패키징 데이터로 스택 트레이스 출력
* logback-classic은 SLF4J를 구현한다.     

By virtue of logback's default configuration policy, when no default configuration file is found, logback will add a ConsoleAppender to the root logger.
Appender는 출력 대상으로 볼 수있는 클래스입니다.
Appender는 콘솔, 파일, Syslog, TCP 소켓, JMS 등 다양한 대상에 존재합니다.  
또한 사용자는 특정 상황에 맞게 자신의 Appender를 쉽게 만들 수 있습니다
Appender 기본이 콘솔이기에 오류가 발생하면 logback이 콘솔에 내부 상태를 자동으로 인쇄합니다.

# LogBack 설명 

# LogBack 사용 

위의 예에서는 StatusPrinter.print () 메서드를 호출하여 내부 상태를 인쇄하도록 logback에 지시했습니다.    
Logback의 내부 상태 정보는 로그 백 관련 문제를 진단하는 데 매우 유용 할 수 있습니다.   



# 아키텍처 
현재 logback은 logback-core, logback-classic 및 logback-access의 세 가지 모듈로 나뉩니다.



# 참고 자료  
출처: https://dololak.tistory.com/632 [코끼리를 냉장고에 넣는 방법]
