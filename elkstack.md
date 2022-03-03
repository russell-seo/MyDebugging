  
  
 # ELK Stack 다른 서버에서 오는 Log 분리하여 Index 생성
 
 
  필자는 회사 프로젝트에서 ELK Stack 을 통해 Log를 수집해야 했다. 필자의 TIL Repo에서 관련하여 ELK Stack 을 Spring 프로젝트와 연동하는 글을 작성해 놓았다.
  
  이번 글은 각자 로그를 뿌리는 서버가 다르기 때문에 서버를 구분하여 Kibana에 Index를 생성하여 모니터링 할 예정이다.
  
  
  
  ## LogStash Output 수정
  
  먼저 Tomcat이 돌아가고 있는 서버에서 `filebeat`를 통해 `logStash` 가 구동되고 있는 서버로 log 파일을 보낸다.
  
  
   ![image](https://user-images.githubusercontent.com/79154652/156509962-ed82eff5-fed0-4a6a-8cd6-eeeac355c29a.png)


  logstash에서 받은 log 파일이 위와 같은 이미지 처럼 Json 형태로 나타난다. 아래 데이터를 기준으로 필자는 elasticSearch 에 보낼 log를 분리하였다.



  ### __logstash.conf__
  
  logstash.conf 파일을 아래와 같이 수정하였다.
  
  ~~~
  input {
  beats {
    port => 5044
	host => "0.0.0.0"
  }
}

  output {
  		if [agent][hostname] == "DESKTOP-5H98PGG"{ //위 이미지의 Json 형태 데이터에서 agent에 있는 hostname 데이터가 해당 데이터면 
			elasticsearch {
				hosts => ["http://localhost:9200"]
				index => "sangwon-%{+YYYY.MM.dd}" // 해당 인덱스를 생성.
			}
		}
		else {
			elasticsearch {                     // 아니면 해당 인덱스를 생성.
				hosts => ["http://localhost:9200"]
				index => "logfinder-%{+YYYY.MM.dd}"
				#user => "elastic"
				#password => "changeme"
			}
		}
  stdout{
	codec => rubydebug
  }
}
  ~~~
  
  1. 위와 같이 logstash.conf 작성 후 fileBeat->data->registry를 모두 삭제 한 후
  2. filebeat와 logstash를 재 구동 시켰다.
  3. kibana index pattern 에서 확인 해 보면 아래와 같이 인덱스가 2개 생성 된 것을 볼 수 있다.

<img width="734" alt="스크린샷 2022-03-03 오후 3 46 14" src="https://user-images.githubusercontent.com/79154652/156511572-b23f9735-2dc2-4200-9ec5-9a3329d68ec6.png">



마치며
---
생각보다 필자는 이 작업을 하는데 오래 걸렸다. 어떤 기준으로 데이터를 걸러야 하는지에 대해서 계속 오류가 발생하였고, 추가적으로 Logstash Filter에 대한 공부가 필요할 것 같다.
