  
  
  # Jenkins Context Path 충돌 문제
  
  
  - 필자는 행복한 주말을 보내고 월요일에 출근 하였는데, 개발하는 프로젝트에 문제가 생겨 수정 후 Jenkins 를 통해 배포를 진행했는데 갑자기 배포가 실패가 되었고 해당 관련 오류에 대해
    
    기록해 놓기 위해 이 글을 적는다.
    
    
    ![image](https://user-images.githubusercontent.com/79154652/163742194-1df24430-3848-41d2-a0b5-4c5839f0acf2.png)
  
  
  - Console에 위와 같은 로그가 출력되었고 해당 오류를 잡기 위해 구글링을 하던 중 Tomcat 설정에 server.xml 을 수정하면 된다는 블로그를 찾았다.

    [링크](https://www.lesstif.com/java/tomcat-root-context-webapp-14745616.html)
    
    [링크](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=goddlaek&logNo=220937395931)
    
    
   위에 관련된 블로그 링크를 걸어 놓았다.
   
   
   필자는 아래와 같은 방법으로 오류를 해결 하였다.
   
   
   ## Server.xml 변경
   
   1. tomcat의 context root 설정을 다음과 같이 변경한다

      ~~~
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="false">
        <Context path="/logen" docBase="logen"(여기는 내 앱을 넣으면된다)  reloadable="false" > </Context>    
      </Host>
     
      ~~~
      
      - side effect
           - Root 와 logen 두개의 Context 가 생기는 문제이다. 

      - 현재 톰캣의 webapps 디렉토리를 보면 root 디렉토리가 존재하고 우리가 context의 path를 root로 설정해준 logen 디렉토리도
        존재하게 될 것 이다. 이럴경우 톰캣에서 root url로 접근가능한 context가 두개가 되므로 문제가 발생할 수 있다.
        
        
