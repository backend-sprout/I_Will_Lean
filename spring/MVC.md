# MVC  
`MVC`는 `Model-View-Controller`의 약자이다.    

![mvc](https://user-images.githubusercontent.com/50267433/123186655-92bd4e80-d4d3-11eb-8dbf-876cf076cc65.png)
   
* **Model :** 비지니스 로직에서의 알고리즘, 데이터 등의 기능을 처리합니다.
* **Controller :** 화면의 처리기능과 Model과 View를 연결시켜주는 역할을 합니다.
* **View :** 웹이라면 웹페이지, 모바일이라면 어플의 화면의 보여지는 부분입니다.     

`MVC`의 사용에 따라서 얻게되는 이점은   
Model, View, Controller 가 각자 독립적으로 실행되기에 교체가 쉬운등 유지보수가 편하며  



하지만 위와 같은 형태로만 코드를 작성한다면,      
클라이언트가 `요청하는 URL`마다 `Servlet`을 생성하고,            
그에 맞는 `Controller`에게 요청을 보내주는 코드를 전부 따로 작성을 해야 했다.       

```java
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
     
@WebServlet("/hello")  
public class HelloServlet extends HttpServlet {
	
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException { 
			// GET 방식에 대한 처리를 하는 곳 
	}
	
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException { 
			// POST 방식에 대한 처리를 하는 곳 
	} 
}
```
```java
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
     
@WebServlet("/goodbye")  
public class HelloServlet extends HttpServlet {
	
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException { 
			// GET 방식에 대한 처리를 하는 곳 
	}
	
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException { 
			// POST 방식에 대한 처리를 하는 곳 
	} 
}
```

![generic controller](https://user-images.githubusercontent.com/50267433/123186862-03fd0180-d4d4-11eb-8c25-b34b416d6b72.png)    
        
    
     
즉, `특정 url` 에 맞는 서블릿을 생성하고 이와 연결된 컨트롤러를 사용했어야 했다.    



[참고하면 좋은 사이트](https://m.blog.naver.com/zzang9ha/221846757385)

![mvc2](https://user-images.githubusercontent.com/50267433/123186597-74575300-d4d3-11eb-84a6-9f4e649412c5.png)

