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

## 코드 
[예시 코드](https://github.com/my-sprout-code/jenkins-springboot)    
      
## 환경설정      
> 필자는 Homebrew 를 기반으로 다운을 진행할 예정이다.    
### Docker 다운
[docker 공식 사이트](https://www.docker.com/get-started)    

### 젠킨스 다운 및 실행
```sh
docker pull jenkins/jenkins:lts 
```
```
sudo docker run -d -p 8080:8080 -v /jenkins:/var/jenkins_home --name jenkins -u root jenkins/jenkins:lts
```

만약 도커를 실행하는 과정에서 아래와 같은 에러가 발생한다면    
```sh 
docker: Error response from daemon: Mounts denied:
The path /jenkins is not shared from the host and is not known to Docker.
You can configure shared paths from Docker -> Preferences... -> Resources -> File Sharing.
See https://docs.docker.com/docker-for-mac for more info.
```
   
마운트 설정을 통해 해결해주면 된다.    
* `Docker` -> `Preferences...` -> `Resources` -> `File Sharing.` 으로 접속   
* `/jenkins`폴더를 마운팅 허용하도록한다.      

## 젠킨스 실행하기 
브라우저에 `localhost:8080`를 입력한다.    

[]()

위와 같이 로그인을 위한 패스워드를 입력하라고 나온다.      

```sh
docker logs jenkins
```
다시 터미널로 돌아와서 위와 같은 명령어를 입력해준다.     
   
```sh
*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

????????????????????????????????

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************
```
그렇다면 `????????????????????????????????`부분에 영문과 숫자로 이루어진 패스워드가 있다.      
이를 입력해주면 젠킨스에 로그인이 가능하다.      





# 출처   
[기억하기 위한 개발노트](http://jmlim.github.io/docker/2019/02/25/docker-jenkins-setup/)      
[개발자의 기록습관](https://ict-nroo.tistory.com/31)   
