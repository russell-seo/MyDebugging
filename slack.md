
# Spring Webhook 으로 Slack 알림 보내기

  필자는 회사 프로젝트를 진행하면서 ERROR 로그를 즉각 알림 받아 대응해야했다. 그래서 Log ERROR 레벨을 모두 Slack으로 전송하는 봇을 구현하였다.
  
  
  1. 먼저 Slack을 가입하자
  2. 채널을 생성하고 아래 이미지와 같이 앱을 선택한 후 Incomming WebHooks를 해당 채널에 추가한다.
  ![image](https://user-images.githubusercontent.com/79154652/159825541-aae71029-0ecb-479e-ae73-34ad7987c219.png)

  3. WebHook을 열고 Slack에 추가 한다.
    
   ![image](https://user-images.githubusercontent.com/79154652/159825754-2a6e5726-fe8b-4c3e-b23e-39417baded06.png)

  
  4. 내가 추가하고 싶은 채널을 선택한다.
    
   ![image](https://user-images.githubusercontent.com/79154652/159825848-b60a433e-db05-4171-b7f1-cfda34756727.png)
   
  5. 추가하면 아래 이미지와 같이 페이지가 출력되며, 웹후크 URL을 복사하거나 기억하고 있자.
  
  ![image](https://user-images.githubusercontent.com/79154652/159825968-7b5f9e36-d011-4501-b892-44d33f5edf76.png)

  6. 이제 Webhook, Slack과 나의 프로젝트랑 연동할 수 있는 설정은 끝났다. 이제 Spring 프로젝트에서 로그 레벨을 기준으로 Slack으로 메시지를 보내 보자


  __build.gradle__
  
  bulid.gradle에 slack appender 라이브러리를 추가해 주어야 한다.
  ~~~java
  implementation 'com.github.maricn:logback-slack-appender:1.4.0'
  ~~~
  
  __logback.groovy__
  ~~~java
  appender('SLACK', SlackAppender) {
    channel = "#로젠서버-error"
    username = "LOGEN-WAS"
    colorCoding = "true"
    webhookUri = "webhookUri 입력"
    layout(PatternLayout) {
        pattern = "%d{yyyy-MM-dd HH:mm:ss.SSS} %msg %n"
    }
    filter(EvaluatorFilter) {
        evaluator(OnMarkerEvaluator) {
            marker = "SLACK_MARKER"
        }
    }
}

appender('ASYNC_SLACK', AsyncAppender) {
    appenderRef("SLACK")
    filter(ThresholdFilter){
        level = ERROR
    }
}


logger("com.jhc.logen.controller", ERROR, ["ASYNC_SLACK"], false)
logger("com.jhc.logen.annotation", ERROR, ["ASYNC_SLACK"], false)
logger("com.jhc.logen.log", ERROR, ["ASYNC_SLACK"], false)
  
  ~~~
  
  - 필자는 logback.groovy 형식으로 구현하였다. (XML 방식은 소스코드가 많이 올라와있다.)
  - appender로 slack에 등록한 `channel`, `username`, `webhookuri`를 입력해 준다.
      - username은 slack에 알림이 올때 보여지는 이름을 설정하는 것이다.
  
  - layout(patternLayout) 으로 알림올 메시지의 패턴을 설정해준다.
  - `ASYNC_SLACK` 비동기 방식으로 위의 SLACK을 레퍼런스로 한다.


 7. 테스트용으로 log 를 찍어볼 Controller를 만든다.

 ~~~java
 @GetMapping("/slackTest")
    public String slackTest(){
        log.info("제바알");
        log.error("됫으면좋겟다");
        return "성공";
    }
 ~~~
 
 
 8. 이제 PostMan으로 해당 메소드를 호출하면 Slack으로 알림이 온다. 예외가 터지면 알람이 오도록 설정해 놓았다.

![image](https://user-images.githubusercontent.com/79154652/159827229-fe230b0e-9c89-4964-ac68-49c6d8c94306.png)




참고
----
[https://kangwoojin.github.io/programing/logback-async-appender/](https://kangwoojin.github.io/programing/logback-async-appender/)

[https://ckddn9496.tistory.com/81?category=428336](https://ckddn9496.tistory.com/81?category=428336)
