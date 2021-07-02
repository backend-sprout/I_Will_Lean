# Jenkins         
## Jenkins 개요   
![Jenkins](./images/Jenkins.png)

젠킨스는 소프트웨어 개발 시 **지속적 통합(CI) 서비스를 제공하는 툴이다.**        
다수의 개발자들이 하나의 프로그램을 개발할 때 버전 충돌을 방지하기 위해    
각자 작업한 내용을 공유 영역에 있는 `Git`등의 저장소에 빈번히 업로드함으로써 지속적 통합이 가능하도록 해 준다.    

![nightly build](./images/nightly%20build.png)

`CI툴`이 등장하기 전에는 일정시간마다 빌드를 실행하는 방식이 일반적이었다.    
특히 심야 시간대에 이러한 빌드가 타이머에 의해 집중적으로 진행되었는데, 이를 `nightly-build`라 한다.    
하지만, 젠킨스는 정기적인 빌드에서 한발 나아가 `서브버전`, `Git` 과 같은 버전 관리시스템과 연동하여      
**소스의 커밋을 감지하면 자동적으로 자동화 테스트가 포함된 빌드가 작동되도록 설정할 수 있다.**   

## 젠킨스가 주는 이점   
코드의 변경과 함께 이뤄지는 자동화된 빌드와 테스트 작업들은 다음과 같은 이점들을 가져다 준다.

![aws ci](./images/aws_ci.png)
   
* 프로젝트 표준 컴파일 환경에서의 컴파일 오류 검출
* 자동화 테스트 수행
* 정적 코드 분석에 의한 코딩 규약 준수여부 체크
* 프로파일링 툴을 이용한 소스 변경에 따른 성능 변화 감시
* 결합 테스트 환경에 대한 배포작업
* [아마존에서 말하는 장점](https://aws.amazon.com/ko/devops/continuous-delivery/)

# 젠킨스와 Spring Boot + GitHub 연동하기 
> [참조 사이트](https://velog.io/@hind_sight/Docker-Jenkins-%EB%8F%84%EC%BB%A4%EC%99%80-%EC%A0%A0%ED%82%A8%EC%8A%A4%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-Spring-Boot-CICD)

## mac docker 다운로드  
> 필자는 Homebrew 를 기반으로 다운을 진행할 예정이다.   

**Homebrew 다운**
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```   
  
**docker 다운**
```sh
brew install docker
```

## Jenkins 이미지 다운 및 실행
```sh
docker run -d -p 9090:8080 --name jenkins jenkins/jenkins
```

# 출처   
[개발자의 기록습관](https://ict-nroo.tistory.com/31 )
[hind_sight.log](https://velog.io/@hind_sight/Docker-Jenkins-%EB%8F%84%EC%BB%A4%EC%99%80-%EC%A0%A0%ED%82%A8%EC%8A%A4%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-Spring-Boot-CICD)
