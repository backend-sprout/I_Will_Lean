Bean 🥜
=============

# 📕 JAVA Bean   
`JAVA Bean`이란? 'JAVA Bean 규약' 또는 'JAVA Bean 관례'에 따라 만들어진 클래스다.         
주로, `인스턴스 변수`들과 `getter/setter 메소드`가 존재하는 클래스를 의미한다.       
      
JAVA Bean 관련 설계 규약     
1. 클래스를 패키지화 하여야 한다.  
2. 멤버변수는 Property 라 부른다.    
3. 각 property마다 setter, getter가 존재해야 한다.   
4. Property의 접근제어자는 private이다.      
5. 만약 property가 boolean이라면 getter대신 is메소드를 사용해도 된다.    


        
# 📗 Spring Bean      
`Spring IoC Container`는 하나 이상의 빈을 관리한다.           
이러한 `Bean`은 `Spring Container`에 제공하는 구성 메타 데이터로 인해 생성된다.         
