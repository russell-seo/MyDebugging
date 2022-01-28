
# Spring - Redis 적용 및 외부 서버 연동

  필자는 회사 프로젝트를 진행하면서 Spring 에 Redis를 적용시켜 캐시DB로 사용하기로 결정하였고, 필자의 회사같은 경우 택배 기사들이 쓰는 앱을 주로 만들고 있다. 그 중 앱을 열었을때 
  모든 택배기사들이 기초정보 데이터를 가져와서 업로드 해주어야 했다. 이과정에서 DB에 부하를 줄이기 위해 기초정보 데이터를 Redis에 담아서 사용할 계획이다.
  
  
  ## RedisConfig 및 의존성
  
  필자의 회사는 Spring Boot를 적용하고 있지 않아서 Spring과 Redis 호환 버전을 맞추는데 애를 먹었다. 저와 같이 Spring을 쓰는 분들은 아래 의존성을 추가하면
  호환이 될 것 이다.
  
  1. build.gradle 에 아래의 의존성을 추가해주면 된다.
  
  ![image](https://user-images.githubusercontent.com/79154652/151497634-226929bf-1434-4e10-9399-ca1a71abd14b.png)
  
  
  2. Spring boot 같은 경우는 자동으로 설정을 해주지만, Spring은 Config 파일을 작성해야한다.
  
   ~~~java
    @Configuration
    @PropertySource("classpath:db.properties")
    public class RedisConfig {

        @Value("${spring.redis.host}")
        private String redisHost;

        @Value("${spring.redis.port}")
        private int redisPort;


        @Bean //이 객체가 Connect와 관련된 객체이다.
        public RedisConnectionFactory redisConnectionFactory(){
            LettuceConnectionFactory lettuceConnectionFactory = new LettuceConnectionFactory(redisHost, redisPort);
            return lettuceConnectionFactory;
        }

        @Bean // 이 객체가 실제로 Template 역할 Key Serializer, Value Serializer를 통해서 실제 데이터를 
                 변환하는 과정이 필요하다
        public RedisTemplate<String,Object> redisTemplate(){
            RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
            redisTemplate.setConnectionFactory(redisConnectionFactory());
            redisTemplate.setKeySerializer(new StringRedisSerializer());
            redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<Object>(Object.class)); //이 친구덕에 객체가 Json 형태로 변환 된다.
            return redisTemplate;
          }
        }
   ~~~
 
 
3. Redis와 연동 Test
  
  - 위의 설정을 통해서 먼저 Redis와 연동됬는지 확인하는 간단한 Test API를 작성해보자
  
 __Controller__ 
 ~~~java

 
 @GetMapping("/redistest")
 public Object redis(@ModelAttribute String key){
        return redisService.Test(key);
    }
 
 ~~~
 
 __Service__
 ~~~java
 
@Service
public class RedisService {

    @Autowired
    RedisTemplate<String, Object> redisTemplate;

    public Object Test(String key){
        ValueOperations<String, Object> vop = redisTemplate.opsForValue();
        LoginRequestVO loginRequestVO = new LoginRequestVO();
        loginRequestVO.setId("kiaofk");
        loginRequestVO.setTel("01092967357");
        vop.set(key, loginRequestVO);
        System.out.println("vop.get(\"\") = " + vop.get(key));
        return vop.get(key);
    }
}
 
 ~~~
 
 
4. AWS EC2 인스턴스에 Redis Server 설치
- 필자의 EC2 인스턴스는 Ubuntu 입니다.

  1) 먼저 apt-get 업데이트 한다.
     ~~~java
     $ sudo apt-get update
     $ sudo apt-get upgrade
     ~~~
  2) Redis 서버를 설치한다
     ~~~java
     $ sudo apt-get install redis-server
     ~~~
  3) redis.conf 파일을 열어서 `bind 127.0.0.1` -> `bind 0.0.0.0` 으로 바꾸어 모든 ip접속을 열어준다.
  
      ![image](https://user-images.githubusercontent.com/79154652/151499764-86c2788f-1dec-4069-8058-19a6480f3614.png)
      
  4) 추가로 559번 라인의 maxmemory 를 500m로 바꾸어 준다(AWS 프리티어 기준 1G 이기 때문)
     
     vi 편집기에 번호를 나타내는 명령어는 `:set number` 라고 입력하면 된다.
     
     ![image](https://user-images.githubusercontent.com/79154652/151500053-6a712354-2bc4-4d60-b8ce-183a2bbb9baa.png)

  5) 이제 테스트를 진행해봤는데 Redis Connection 에러가 계속 떠서 삽질을 많이 했는데. AWS EC2 인바운드 규칙에 6379포트를 할당하지 않아서이다.
  ![image](https://user-images.githubusercontent.com/79154652/151500338-9565949b-556d-46b0-a729-8777e56ce831.png)

  6) 인바운드 규칙까지 적용했다면 Postman 으로 호출해보면 Response 값이 잘 나오는 것을 볼 수 있다.
  
    ![image](https://user-images.githubusercontent.com/79154652/151500540-c86dcaaf-4d76-40f7-9c23-44d2a04de735.png)

## 다음에는 이제 Cache를 적용해서 Redis를 실제 프로젝트에 적용해 보겠다.

