Bean ๐ฅ
=============

# ๐ JAVA Bean   
`JAVA Bean`์ด๋? 'JAVA Bean ๊ท์ฝ' ๋๋ 'JAVA Bean ๊ด๋ก'์ ๋ฐ๋ผ ๋ง๋ค์ด์ง ํด๋์ค๋ค.         
์ฃผ๋ก, `์ธ์คํด์ค ๋ณ์`๋ค๊ณผ `getter/setter ๋ฉ์๋`๊ฐ ์กด์ฌํ๋ ํด๋์ค๋ฅผ ์๋ฏธํ๋ค.       
      
JAVA Bean ๊ด๋ จ ์ค๊ณ ๊ท์ฝ     
1. ํด๋์ค๋ฅผ ํจํค์งํ ํ์ฌ์ผ ํ๋ค.  
2. ๋ฉค๋ฒ๋ณ์๋ Property ๋ผ ๋ถ๋ฅธ๋ค.    
3. ๊ฐ property๋ง๋ค setter, getter๊ฐ ์กด์ฌํด์ผ ํ๋ค.   
4. Property์ ์ ๊ทผ์ ์ด์๋ private์ด๋ค.      
5. ๋ง์ฝ property๊ฐ boolean์ด๋ผ๋ฉด getter๋์  is๋ฉ์๋๋ฅผ ์ฌ์ฉํด๋ ๋๋ค.    
    
# ๐ Spring Bean        
`Spring IoC Container`๋ ํ๋ ์ด์์ ๋น์ ๊ด๋ฆฌํ๋ค.             
์ด๋ฌํ `Bean`์ `BeanDefinition(๋น ์ค์  ๋ฉํ ์ ๋ณด)`๋ก ์ธํด ์์ฑ๋๋ค.             
(`Spring Container`, `BeanDefinition(๋น ์ค์  ๋ฉํ ์ ๋ณด)`)   
   
## ๐ BeanDefinition(์คํ๋ง ๋น ์ค์  ๋ฉํ ์ ๋ณด)   
์คํ๋ง์ `Java`, `XML`, `Gradle` ๋ฑ์ ํตํด ์คํ๋ง ์ปจํ์ด๋๋ฅผ ์์ฑํ  ์ ์๋ค.       
   
**๊ทธ๋ ๋ค๋ฉด ์ด๋ป๊ฒ ์ด๋ฐ ๋ค์ํ ์ค์  ํ์์ ์ง์ํ๋ ๊ฒ์ผ๊น? ๐ค**            
๊ทธ ์ค์ฌ์๋ `BeanDefinition`์ด๋ผ๋ ์ถ์ํ๊ฐ ์๋ค. (๋น ์ ๋ณด ์์ฒด๋ฅผ ์ถ์ํ)         
              
์ฝ๊ฒ ์ด์ผ๊ธฐํด์ **`์ญํ `๊ณผ `๊ตฌํ`์ ๊ฐ๋์ ์ผ๋ก ๋๋ ๊ฒ์ด๋ค.**             
์ฆ, ์คํ๋ง ์ปจํ์ด๋๋ `Java`์ธ์ง, `XML`์ธ์ง ๋ชฐ๋ผ๋ ์ค๋ก์ง `BeanDefinition`๋ง ์๋ฉด ๋๋ค.       
     
* `XML`์ ์ฝ์ด์ `BeanDefinition`์ ๋ง๋ค๋ฉด ๋๋ค.   
* `Java`๋ฅผ ์ฝ์ด์ `BeanDefinition`์ ๋ง๋ค๋ฉด ๋๋ค.   
* `Gradle`๋ฅผ ์ฝ์ด์ `BeanDefinition`์ ๋ง๋ค๋ฉด ๋๋ค.       
                 
`BeanDefinition`์ `๋น ๋ฉํ ์ ๋ณด`๋ผ๊ณ  ๋งํ๋ค.         
* `@Bean`, `<bean></bean>`๋น ํ๋์ฉ ๋ฉํ ์ ๋ณด๊ฐ ์์ฑ๋๋ค.           
* ์คํ๋ง ์ปจํ์ด๋๋ ์ด ๋ฉํ ์ ๋ณด๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ์คํ๋ง ๋น์ ์์ฑํ๋ค.           
    
`BeanDefinition` ์ธํฐํ์ด์ค๋ฅผ ๊ตฌํํ ํด๋์ค๋ค์        
`BeanDefinitionReader`์ธํฐํ์ด์ค๋ฅผ ๊ตฌํํ ๊ตฌํ์ฒด๋ฅผ ์์กดํ๊ณ  ์๋ค.        
        
`BeanDefinitionReader` ์ธํฐํ์ด์ค๋ ์ค์ ํ์ผ(์๋ฐ,xml,..๋ฑ๋ฑ)์์ ์ ๋ณด๋ฅผ ์ฝ๋ ์ญํ ์ ํ๋ค.            
๊ทธ๋ฆฌ๊ณ  ์ค์ ํ์ผ์ ๊ธฐ๋ฐ์ผ๋ก `BeanDefinition`์ ์์ฑํ๋ ์ญํ ์ ํ๋ค.         
์ ํํ ๋งํ๋ฉด `Bean`์ ๋ฉํ์ ๋ณด๋ฅผ ์ฝ์ด์ค๋ ๊ฒ์ด๋ค.    
    
* `AnnotationConfigApplicationContext`๋ `AnnotatedBeanDefinitionReader`๋ฅผ ์ฌ์ฉํด์     
  `AppConfig.class`๋ฅผ ์ฝ๊ณ , `BeanDefinition`์ ์์ฑํ๋ค.      
* `GenericXmlConfigApplicationContext`๋ `XmlBeanDefinitionReader`๋ฅผ ์ฌ์ฉํด์     
  `AppConfig.xml`์ ์ฝ๊ณ , `BeanDefinition`์ ์์ฑํ๋ค.       
* ์๋ก์ด ํ์์ ์ค์  ์ ๋ณด๊ฐ ์ถ๊ฐ๋๋ฉด, `XxxBeanDefinitionReader`๋ฅผ ๋ง๋ค์ด์ `BeanDefinition`์ ์์ฑํ๋ฉด ๋๋ค.  
       
**`XxxBeanDefinitionReader` ๋ง๋๋ ๋ฐฉ๋ฒ์? ๐ค**        
๊ตฌ๊ธ๋ง์ ํด๋ ๋ฐฉ๋ฒ์ด ๋์ค์ง ์๋๋ค!! (์คํ๋ ค ๋ง๋ค๋ฉด ์ต์ด์ด๊ธฐ์ ํฅ๋ฏธ๊ฐ ๋๋๋ค.)         
์ง๊ธ์ ์ง์คํด์ผํ  ๊ฒ์ด ์์ผ๋ ๋์ค์ ๋ง๋๋ ๋ฐฉ๋ฒ์ ์ง์  ์ฐพ์๋ด์ผ๊ฒ ๋ค.       
`JSONBeanDefinition`์ด๋ผ๋ ์ฃผ์ ๋ก ๋ง๋ค์ด๋ณด์  

## BeanDefinition ์ดํด๋ณด๊ธฐ 
* **BeanClassName:** ์์ฑํ  ๋น์ ํด๋์ค ๋ช(์๋ฐ ์ค์  ์ฒ๋ผ ํฉํ ๋ฆฌ ์ญํ ์ ๋น์ ์ฌ์ฉํ๋ฉด ์์)
* **factoryBeanName:** ํฉํ ๋ฆฌ ์ญํ ์ ๋น์ ์ฌ์ฉํ  ๊ฒฝ์ฐ ์ด๋ฆ, ์) appConfig
* **factoryMethodName:** ๋น์ ์์ฑํ  ํฉํ ๋ฆฌ ๋ฉ์๋ ์ง์ , ์) memberService
* **Scope:** ์ฑ๊ธํค(๊ธฐ๋ณธ๊ฐ)
* **lazyInit:** ์คํ๋ง ์ปจํ์ด๋๋ฅผ ์์ฑํ  ๋ ๋น์ ์์ฑํ๋ ๊ฒ์ด ์๋๋ผ, ์ค์  ๋น์ ์ฌ์ฉํ  ๋ ๊น์ง ์ต๋ํ ์์ฑ์ ์ง์ฐ์ฒ๋ฆฌ ํ๋์ง ์ฌ๋ถ  
* **InitMethodName:** ๋น์ ์์ฑํ๊ณ , ์์กด๊ด๊ณ๋ฅผ ์ ์ฉํ ๋ค์ ํธ์ถ๋๋ ์ด๊ธฐํ ๋ฉ์๋ ๋ช    
* **DestroyMethodName:** ๋น์ ์๋ช์ฃผ๊ธฐ๊ฐ ๋๋์ ์ ๊ฑฐํ๊ธฐ ์ง์ ์ ํธ์ถ๋๋ ๋ฉ์๋ ๋ช    
* **Constructor arguments, Properties:** ์์กด๊ด๊ณ ์ฃผ์์์ ์ฌ์ฉํ๋ค. (์๋ฐ ์ค์  ์ฒ๋ผ ํฉํ ๋ฆฌ ์ญํ ์ ๋น์ ์ฌ์ฉํ๋ฉด ์์)     

**์ฝ๋๋ก ํ์ธํด๋ณด๊ธฐ**
```java
package hello.core.beandefinition;

import hello.core.AppConfig;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class BeanDefinitionTest {

    AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
    GenericXmlApplicationContext xmlApplicationContext = new GenericXmlApplicationContext("appConfig.xml");

    @Test
    @DisplayName("๋น ์ค์  ๋ฉํ์ ๋ณด ํ์ธ")
    void findAnnotationApplicationBean() {
        String[] beanDefinitionNames = annotationConfigApplicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = annotationConfigApplicationContext.getBeanDefinition(beanDefinitionName);

            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                System.out.println("beanDefinitionName = " + beanDefinitionName +
                        " beanDefinition = " + beanDefinition);
            }
        }
    }

    @Test
    @DisplayName("๋น ์ค์  ๋ฉํ์ ๋ณด ํ์ธ")
    void findXmlApplicationBean() {
        String[] beanDefinitionNames = xmlApplicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = xmlApplicationContext.getBeanDefinition(beanDefinitionName);

            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                System.out.println("beanDefinitionName = " + beanDefinitionName +
                        " beanDefinition = " + beanDefinition);
            }
        }
    }

}
```
   
**์ ๋ฆฌ**     
* `BeanDefinition`์ ์ง์  ์์ฑํด์ ์คํ๋ง ์ปจํ์ด๋์ ๋ฑ๋กํ  ์ ๋ ์๋ค.         
  ํ์ง๋ง ์ค๋ฌด์์ `BeanDefinition`์ ์ง์  ์ ์ํ๊ฑฐ๋ ์ฌ์ฉํ  ์ผ์ ๊ฑฐ์ ์๋ค.           
  ๊ทธ๋๋ ๋์ค์ ์ง์  ๊ตฌํ์ ํด๋ณด๋ ํธ์ด ๊ฐ๋ฐ์ ๋ค์ด ์ถ์ด์ง ์์๊น ์ถ๋ค.        
* `BeanDefinition`์ ๋ํด์๋ ๋๋ฌด ๊น์ด์๊ฒ ์ดํดํ๊ธฐ ๋ณด๋ค๋,     
  ์คํ๋ง์ด ๋ค์ํ ํํ์ ์ค์  ์ ๋ณด๋ฅผ `BeanDefinition`์ผ๋ก ์ถ์ํํด์ ์ฌ์ฉํ๋ ๊ฒ ์ ๋๋ง ์ดํดํ์      
* ๊ฐ๋ ์คํ๋ง ์ฝ๋๋ ์คํ๋ง ๊ด๋ จ ์คํ ์์ค์ ์ฝ๋๋ฅผ ๋ณผ ๋,`BeanDefinition` ์ด๋ผ๋ ๊ฒ์ด ๋ณด์ผ ๋๊ฐ ์๋ค.    
  ์ด๋ ์ด๋ฌํ ๋ฉ์ปค๋์ฆ์ ๋ ์ฌ๋ฆฌ๋ฉด ๋๋ค.   
      
**ํ์๊ฐ ์๊ฐํ๊ธฐ๋ก **      
์ธ์คํด์ค๋ฅผ ๋ง๋ค์ด ๋ฐํํ๋ ๊ฒ์ ๋จ์ํ ์๋ฐ ์ฝ๋๋ฅผ ์ํํ๊ธฐ ์ํ ์์์ผ ๋ฟ์ด๊ณ       
`BeanDefinitionReader` ๊ตฌํ์ฒด๊ฐ ํด๋น ํด๋์ค๋ฅผ ํตํด ์ ๋ณด๋ฅผ ์ฝ์ด๋ค์ด์          
`BeanDefinition`์ด๋ผ๋ ๋ฉํ๋ฐ์ดํฐ๋ฅผ ๋ง๋ค๊ณ , ์ด๋ฅผ ํตํด ๋ค์ ๊ฐ์ฒด๋ฅผ ์๋ก ์์ฑํ๋ค๊ณ  ์๊ฐํ๋ค.      
       
**์ฐธ๊ณ ๋ก `ApplicationContext`๋ก ์์กดํ์ง ์์ ์ด์ ๋? ๐ค**   
* `ApplicationContext`์์๋ `getBeanDefinition()`์ด๋ผ๋ ๋ฉ์๋๊ฐ ์กด์ฌํ์ง ์๋๋ค.    

**๊ทธ๋ฆฌ๊ณ **   
`GenericXmlApplicationContext` ์ธ์คํด์ค๋ 
**factoryBeanName**, **factoryMethodName** ์์๊ฐ null๋ก ๋น ์ ธ์๋ค.   
           
์ฌ์ค ์ด๋ฌํ ์ด์ ์ ๋ํด์ ์๋ ๋ด์ฉ์ ์ฐธ๊ณ ํ์   
์คํ๋ง์ ๋น์ ๋ฑ๋กํ๋ ๋ฐฉ๋ฒ์ด ํฌ๊ฒ 2๊ฐ์ง๋ก ๋๋๋ค.   
          
1. ์ง์  ์คํ๋ง๋น์ ๋ฑ๋กํ๋ ๋ฐฉ๋ฒ (xml)     
2. ํฉํฐ๋ฆฌ ๋ฉ์๋๋ฅผ ์ด์ฉํด์ ๋ฑ๋กํ๋ ๋ฐฉ๋ฒ(class)  
      
์์ , XML๋ก ์ค์ ํ๋ ๊ฒฝ์ฐ ์ง์  ์คํ๋ง๋น์ ๋ฑ๋กํ๋ ๋ฐฉ๋ฒ์ด๊ธฐ์ ๋น์ ๋ํ ํด๋์ค๊ฐ ๋ฐ์ ๋๋ฌ๋ฌ๋๋ฐ           
`JAVAConfig`๋ก ๋ฐ๋๋ฉด์ ํฉํ ๋ฆฌ ๋ฉ์๋(AppConfig์ ๋ฉ์๋)๋ฅผ ์ด์ฉํ๋ ๋ฐฉ๋ฒ์ ์ฃผ๋ก ์ฌ์ฉํ๊ฒ ๋์๊ณ       
`BeanDefinition` ๋ํ ํฉํฐ๋ฆฌ ํด๋์ค์ ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๋์ง๋ฅผ ์ถ๋ ฅํ๋๋ก ์ค์ ์ด ๋ณ๊ฒฝ๋์๋ค.      
๊ทธ๋ ๊ธฐ์ `xml`์ ์ง์  ์คํ๋ง๋น์ ๋ฑ๋กํ๋ ๋ฐฉ๋ฒ์ ํฉํฐ๋ฆฌ ํด๋์ค์ ๋ฉ์๋์ ๋ํด `null`๊ฐ์ ์ถ๋ ฅํ๋ค.    

