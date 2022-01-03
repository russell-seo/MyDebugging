
# JPA 연관관계 메소드, DTO -> Entity로 변환


  ## ManyToOne, OneToMany 양방향 연관관계
  
   - JPA를 사용하다 보면 양방향인 N:1 , 1:N 관계를 많이 만나게 된다. 필자도 역시 JPA를 활용하여 프로젝트를 개발하면서 마주하게 되었다.
     
     실제로 양방향 연관관계에서 가장 중요한 것은 N(다) 쪽이 외래키(FK)를 가지게 된다. 즉 외래키(FK)가 있는 쪽에 값을 셋팅해주지 않는다면
     
     FK값이 null로 DB에 저장되게 되어있다. 그러므로 항상 양방향 모두 값을 셋팅해주는 것이 중요하다.
     
     
     
  - 필자는 약 5일간의 삽질을 통해 계속 null 값만 넣어왔고 연관관계 메소드를 포함한 여러 방법을 적용시켜 보았다. 삽질 끝에 아래 방법으로 `N:1`, `1:N` 양방향 모두 값을 셋팅 하게 되었다.


  
  
 ## 회원가입(join)
 
 DTO -> Entity
 
 필자는 Controller 에서 DTO로 받아 User 객체 생성하면서 파라미터로 DTO를 받아 -> Entity로 변환하는 것이 가장 난관이었다. 제일 중요한 것은 userInterest_list 가 양방향 매핑이었고 아래 코드를 보면 User 도메인에 DTO 를 User Entity로 변환할 때 
 ~~~java
 this.userInterest_list = userDto.getUser_interest_list().stream()
                .map(o -> {
                    UserInterest userInterest = new UserInterest(o);
                    userInterest.setUser(this); // FK 값 있는 다(N) 방향도 값 셋팅
                    return userInterest;
                })
                .collect(Collectors.toList());
 ~~~                
 
위 코드를 통해 UserInterest에 값을 셋팅, 연관관계 메소드를 적용 해 보았지만 모두 실패... 이게 문제없는 코드인지는 모르겠지만 삽질 끝에 FK값을 양방향에 셋팅 할 수 있게 되었다.
 
 
 Controller
 ~~~java
 @Transactional
    public Long user_join(UserDto userDto){
        User user = new User(userDto);
        Long id = userRepository.save(user);
        return id;
    }
 ~~~
 
 
 User(도메인)
 ~~~java
 
  @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
    private List<UserInterest> userInterest_list = new ArrayList<>();
 
 
  public User(UserDto userDto) {
        this.user_email = userDto.getUser_email();
        this.user_name = userDto.getUser_name();
        this.user_gender = userDto.getUser_gender();
        this.user_birthday = userDto.getUser_birthday();
        this.user_joindate = userDto.getJoindate();
        this.user_device = userDto.getUser_device();
        this.socialPath = userDto.getUser_social_path();
        this.userInterest_list = userDto.getUser_interest_list().stream()
                .map(o -> {
                    UserInterest userInterest = new UserInterest(o);
                    userInterest.setUser(this); // FK 값 있는 다(N) 방향도 값 셋팅
                    return userInterest;
                })
                .collect(Collectors.toList());
    }
 ~~~
 
 
 UserInterest(도메인)
 ~~~java
 
 @ManyToOne(fetch = FetchType.Lazy, cascade = CascadeType.ALL)
 @Joincolumn(name = "user_id")
 private User user;
 
 ~~~

  
  
