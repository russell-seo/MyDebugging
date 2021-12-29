
# Tomcat(톰캣) 설치 및 WAR 배포

  1.  Wget으로 Tomcat install

   먼저, 아래 있는 Tomcat 공식 홈페이지에 접속, 다운로드 링크를 복사하자.

   [톰캣 공식 홈페이지](https://tomcat.apache.org/download-90.cgi)
   
   Core : tar.gz 링크 주소를 복사한다.
   
   ![image](https://user-images.githubusercontent.com/79154652/147619917-5b874e89-0ba5-4bde-8139-df373016ea20.png)

   
  2. 터미널을 열고 `wget 주소` 를 입력해준다.
    ![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJWTf6%2FbtrcXjG4Q6l%2FpeF5TqQGUyQGHZ2vISaeSK%2Fimg.png)
  
  
  3. 설치가 끝났다면, /home/teler/tomcat 폴더를 생성하였고, 설치된 파일을 옮겨준다. 이후 압축까지 풀면 끝.
    
    ~~~
    1. sudo mkdir /home/teler/tomcat
    
    2. sudo mv apache-tomcat-9.0.52.tar.gz /home/teler/tomcat/
    
    3. cd /home/teler/tomcat
    
    4. sudo xvfz apache-tomcat-9.0.52.tar.gz // 압축 풀기
    
    5. sudo rm rf apache-tomcat-9.0.52.tar.gz // 제거해주기
    
    ~~~
    
   ![image](https://user-images.githubusercontent.com/79154652/147620151-34220c53-e420-411a-99a3-ab0bbab63610.png)


  4. 환경변수 설정

   ~~~
   1. vi /etc/profile 
   
   profile 파일에서 export CATALINA_HOME=/home/teler/tomcat 저장
   
   2. source /etc/profile
   
   3. echo $CATALINA_HOME
   ~~~
   
   ![image](https://user-images.githubusercontent.com/79154652/147620282-0a3b798f-72d5-4c36-9878-e6010a2495ed.png)
