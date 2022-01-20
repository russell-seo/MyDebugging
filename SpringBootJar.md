
# Spring Boot Jar, 백그라운드 실행

필자는 처음 Spring Boot 를 EC2 에 배포하여 봤다. 항상 Tomcat webapps 에 war파일을 배포하여 실행하였다면, Spring Boot는 내장 톰캣을 가지고 있기 때문에 jar파일을 배포하고 실행하면 된다.

  ## Jar 파일 추출
  
  - Gradle 탭에서 - Task > Build > bootJar를 선택하여 실행한다.
  ![image](https://user-images.githubusercontent.com/79154652/150359457-f1bfe528-87b5-4bc0-a132-b7dab4e1ae0a.png)
  
  - Jar 파일이 생성된 위치를 확인한다.
  
    ![image](https://user-images.githubusercontent.com/79154652/150359579-e89a841e-746e-42b0-99c4-b5b06d770e86.png)


  
  
   - 위 이미지와 같이 FileZila를 통해서 `/home/teler` 경로에 jar파일을 배포하였다.
  
      ![image](https://user-images.githubusercontent.com/79154652/150357860-b6e30d6e-d1ac-441d-953f-fea23d7e4204.png)
      
   - `java -jar TellerServer.jar` 로 jar 파일을 실행시키면 로컬에서 띄어지는 Spring Boot와 똑같이 나온다.


  ## 백그라운드 실행
     
     `nohup java -jar TellerServer.jar &`
    


 
  
