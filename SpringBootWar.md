
# Spring Boot 에서 war 파일 추출

  Remote 서버 톰캣에 올릴 war 파일을 Spring Boot 에서 추출하는 것을 기록할려고 한다. 기본적인 내용이지만 자주 안하다 보면 까먹게 되고 다시 찾아보게 되는 내용인 것 같다.
  
  1. 빌드 설정
  2. 빌드 실행
  3. WAR 파일 추출


  ## 1. 인텔리제이 에서 빌드 설정
  ![제목 없음](https://user-images.githubusercontent.com/79154652/149286347-7fde2738-54ac-4ce5-9c38-a439aa0ff30a.png)
  
  1) 상단 메뉴바 에서 파일 > Project Structure > Artifacts > + > Web Application : Archive 로 진행
  2) Web Application : Archive 를 선택하면 Empty 와 For 'Gradle ... 이 나온다.
  3) For 'Gradle ... war(exploded) 를 선택한다.
  4) 다른건 건드리지말고 OK를 누른다.
  5) 상단 메뉴바 `Build` -> Build Artifact 에서 방금 선택했던 war 파일 이름을 찾아서 Build를 한다.
  6) 최종적으로 war 파일이 생성 된 것을 볼 수 있다.
  
  
  
