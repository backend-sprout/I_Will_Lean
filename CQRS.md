# Command Query Responsibility Segregation    

## Commnad Query(명령과 쿼리)   

* **명렁(Command) :** 
   * 시스템 데이터 변경 
   * 주문 취소, 배송 완료  
* **쿼리(Query) :**
   * 시스템 데이터 조회 
   * 주문 목록 조회  

## Responsibility Segregation(책임과 분리)

* 책임(Responsibility)
    * 구성 요소의 역할 
    * 구성 요소 모델
        * 클래스, 함수
        * 모듈, 패키지
        * 서버, DB, 컨테이너  
* 분리(Segregation) 
    * 역할에 따라 구성 요소 나누기 

## 정리 
   
명령 역할(시스템 데이터 변경)을 수행하는 구성요소와        
쿼리 역할(시스템 데이터 조회)을 수행하는 구성요소를 나누는 것이 CQRS         
    
구현 방식이나 시스템 규모에 따라서       
코드뿐만 아니라 DB나 프로세스를 나누기도 한다.    

## 예시    
  
![image](https://user-images.githubusercontent.com/50267433/137578315-5b9336f0-8fbb-416b-839c-ff154eea14c2.png)
    
상태를 변경하는 Command 에서는 CommandApi -> Service -> domain 식으로 진행   
상태를 읽는 Query에서는 QueryApi -> query -> dao(queryDSL용 클래스) -> data 식으로 진행   

## 그런데 뭐가 좋아?   

* 코드가 중복되는데?   
* 개발이 느려질텐데?      
* 그런데 왜?    

## 해답   
  
![image](https://user-images.githubusercontent.com/50267433/137578420-e126b42f-ed16-45c1-a0e2-a21c54226afb.png)

Member 클래스에서, 
  
1. 마지막 로그인 일시 추가해달라는 요구사항이 들어옴        
2. 최근 주문 일시 추가해달라는 요구사항이 들어옴        
3. 회원 이름 변경 기능 추가해달라는 요구사항이 들어옴     

그런데 이렇게 한 모델에 이것저것 넣다보니까 코드가 잡탕이 되어버린다.    
* 코드 역할/책임 모호   
* 의미/가독성 등 나빠짐   
* 유지보수성이 떨어짐      

즉, Member 클래스가 더이상 Member 테이블에 대응하는 클래스가 아니게 됨     
* 로그인 일시 추가 -> Login_History 테이블과 엮임 
* 최근 주문 일시 추가 -> Order 테이블과 엮임   
* 기능에 따라서 필드가 달라짐 
    * 이름/변경 기능 -> Member 테이블에 해당하는 데이터만 읽어서 객체를 만듬 
    * 주문목록조회 -> 3개의 테이블을 통해서 객체를 만듬 
    * 코드의 의미나 가독성이 나빠짐 -> 어디까지 패치 조인 했냐 모름 -> 유지보수 나쁘게함(N+1 유발)
      
![image](https://user-images.githubusercontent.com/50267433/137578783-7b3ccab8-14cb-4d33-b94b-1222daabcbae.png)  
       
JPA가 대표적인데..        
각각의 기능마다 EAGER 로딩, LAZY 로딩해야할지 다다름   
      
* 주문 취소 : OrderLine에서 Order 로딩 하면서, user를 EAGER할 필요 없음   
* 목록 조회 : OrderLine에서 Order 로딩 하면서, user를 EAGER할 필요함(누가 주문했는지)      
         
이렇게 기능에 따라서 연관한 로딩을 하는 방식이 달라져야하니까      
원하는 성능을 내려고 다양한 엔티티 그래프를 설정하기도 함       
그런데 만약? 이 같은 상황들이 100개 1000개 10000개라면? -> 관리 포인트 너무 많음       
즉, 이런식으로 단일 모델을 유지하려고 하면 복잡성이 늘어날 수 밖에 없음      

## 
![image](https://user-images.githubusercontent.com/50267433/137579069-19dbfb4a-6eef-416c-a8a4-95046683c1b7.png)

**그렇다면 왜 커맨드와 쿼리에 대해서 단일 모델을 고수하면 복잡성이 늘어날 수 밖에 없을까? 🤔**      
사실, 명령과 쿼리가 다루는 데이터는 다르기 때문이다.    

* 명령 -> 한 영역의 데이터   
* 쿼리 -> 여러 영역의 데이터   
