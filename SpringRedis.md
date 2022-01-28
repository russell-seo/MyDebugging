
# Spring - Redis 적용

  필자는 회사 프로젝트를 진행하면서 Spring 에 Redis를 적용시켜 캐시DB로 사용하기로 결정하였고, 필자의 회사같은 경우 택배 기사들이 쓰는 앱을 주로 만들고 있다. 그 중 앱을 열었을때 
  모든 택배기사들이 기초정보 데이터를 가져와서 업로드 해주어야 했다. 이과정에서 DB에 부하를 줄이기 위해 기초정보 데이터를 Redis에 담아서 사용할 계획이다.
  
  
  ## RedisConfig 및 의존성
  
  필자의 회사는 Spring Boot를 적용하고 있지 않아서 Spring과 Redis 호환 버전을 맞추는데 애를 먹었다. 저와 같이 Spring을 쓰는 분들은 아래 의존성을 추가하면
  호환이 될 것 이다.
  
  1. build.gradle 에 아래의 의존성을 추가해주면 된다.
  
  ![image](https://user-images.githubusercontent.com/79154652/151497634-226929bf-1434-4e10-9399-ca1a71abd14b.png)
  
  
  2. Spring boot 같은 경우는 자동으로 설정을 해주지만, Spring은 Config 파일을 작성해야한다.
  
  
   ~~java
        @Configuration
        @PropertySource("classpath:db.properties")
        public class RedisConfig {

        @Value("${spring.redis.host}")
        private String redisHost;

        @Value("${spring.redis.port}")
        private int redisPort;


        @Bean
        public RedisConnectionFactory redisConnectionFactory(){
            LettuceConnectionFactory lettuceConnectionFactory = new LettuceConnectionFactory(redisHost, redisPort);
            return lettuceConnectionFactory;
        }

        @Bean
        public RedisTemplate<String,Object> redisTemplate(){
            RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
            redisTemplate.setConnectionFactory(redisConnectionFactory());
            redisTemplate.setKeySerializer(new StringRedisSerializer());
            redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<Object>(Object.class));
            return redisTemplate;
          }
        }
    ~~
  
