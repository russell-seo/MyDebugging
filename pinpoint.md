

# 네이버 클라우드 플랫폼 Pinpoint 적용


  __Pinpoint 란?__
  
  - Java로 작성된 대규모 분산 시스템용 APM 도구
    - APM : Application Performance Management 의 약자, 응용 소프트웨어의 성능과 서비스가용성을 모니터링 관리하는 도구
  - Transcation 추적을 제공(이 포인트가 도입한 가장 큰 이유)
  - 임계치를 설정하여 Event 발생 시 SMS 또는 Email을 통해 알림을 받을 수 있다.


  
  ## Pinpoint 사용하기
  
  1. 네이버 클라우드 플랫폼 콘솔로 들어가서 Product&Service -> Management -> Pinpoint Cloud 로 들어간다.
  
  ![image](https://user-images.githubusercontent.com/79154652/165008797-6c87bf18-d2bf-4e4e-8bbf-455545f0b583.png)

  
  2. 이동한 화면에서 Repository 생성 버튼을 클릭한다.


  3. Repository 생성 관련된 정보를 작성한다. 아래와 같이 관리자에 대한 정보는 Pinpoint 접속을 위해 필요한 정보들이다.
  
  ![image](https://user-images.githubusercontent.com/79154652/165008902-e71a4edd-bf09-4eb6-90c9-673099ddd25c.png)
  
  4. Repository 생성 후 URL -> 바로가기를 클릭 -> 위에서 입력한 정보로 로그인 한다.

  ![image](https://user-images.githubusercontent.com/79154652/165008988-5f8090f6-0c0b-4bf7-be40-bf13699287f4.png)


  5. Pinpoint Agent 를 설치해야 한다. 페이지 오른쪽 설정 -> Install 버튼을 클릭 -> 아래와 같은 페이지가 나올 것 이다. -> Download link를 클릭하여 Pinpoint Agent 를 다운 한다.
      -> 해당 Agent는 WAS에 붙어서 request, response 등 모든 Transcation을 추적한다.
  
  ![image](https://user-images.githubusercontent.com/79154652/165009057-dd6277e0-6834-439d-aad5-035706206733.png)
  
  
  6. 해당 파일의 압축을 풀어준다. -> Pinpoint-agent 파일들이 생겨나고 .jar 파일도 함께 볼 수 있을 것이다. -> 아래 이미지에서 보이는 `pinpoint.license`파일에 위의 웹페이지에서 보이는
     Agent Licence key를 복사해 저장한다.

  ![image](https://user-images.githubusercontent.com/79154652/165009253-470f0fed-4281-469e-93bd-239a725afb33.png)
  
  
  7. key 등록이 끝나면 pinpoint-agent 폴더 경로를 환경 변수로 설정한다.

  8. 마지막으로 Spring WAS 를 띄울 때 에이전트를 함께 실행시킨다. 

     윈도우 기반이 아니라면 `java -jar -javaagent:${pinpointPath}/pinpoint-bootstrap-2.2.3-NCP-RC1.jar -Dpinpoint.applicationName=원하는이름 -Dpinpoint.agentId=원하는아이디 빌드된서버파일.jar`
     해당 명령어로 실행
     
     윈도우 기반이라면 필자 같은 경우 IntelliJ 의 톰캣 `VM Options` 설정에 추가하여 실행하였다.
     
     ![image](https://user-images.githubusercontent.com/79154652/165009535-eef7b536-bb0b-4a25-aae0-a0b8c4469976.png)
     
     
  9. WAS가 정상적으로 올라가면 다시 Pinpoint 페이지에서 설정한 어플리케이션 목록을 볼 수 있다.
     
     


  

  
