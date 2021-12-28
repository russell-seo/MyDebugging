
# cannot deserialize from object value (no delegate- or property-based creator)

  JPA를 사용하면서 위와 같은 에러가 발생하였다면?
  
  Controller
  ~~~java
@RestController
@RequiredArgsConstructor
public class JoinController {

    private final JoinService joinService;


    @PostMapping("/join")
    public UserResponseDto user_join(@RequestBody UserDto userDto){

        Long id = joinService.user_join(userDto);
        UserResponseDto userResponseDto = new UserResponseDto();
        userResponseDto.setId(id);

        return userResponseDto;
    }
}
  ~~~
  
  UserDto
  ~~~java
  @Data
public class UserDto {

    private String user_email;
    private String user_name;
    private String user_gender;
    private String user_birthday;
    private LocalDateTime joindate;
    private String user_device;
    private String user_is_proud;
    private SocialPath user_social_path;
    private InterestDto user_interest_list;
    
     public UserDto(String user_email, String user_name, String user_gender, String user_birthday, LocalDateTime joindate, String user_device, String user_is_proud, SocialPath user_social_path, InterestDto user_interest_list) {
        this.user_email = user_email;
        this.user_name = user_name;
        this.user_gender = user_gender;
        this.user_birthday = user_birthday;
        this.joindate = joindate;
        this.user_device = user_device;
        this.user_is_proud = user_is_proud;
        this.user_social_path = user_social_path;
        this.user_interest_list = user_interest_list;
    }
}
  ~~~
  
 - PostMan으로 POST요청을 보냈는데 

  cannot deserialize from object value 
  (no delegate- or property-based creator)
  
   - 위와 같은 에러가 발생한 이유는 `jackson library가 빈 생성자가 없는 모델을 생성하는 방법을 모르기 때문`

 ## 해결책
 - 빈 생성자를 추가 해주어야 한다.
 ~~~java
  @Data
public class UserDto {

    private String user_email;
    private String user_name;
    private String user_gender;
    private String user_birthday;
    private LocalDateTime joindate;
    private String user_device;
    private String user_is_proud;
    private SocialPath user_social_path;
    private InterestDto user_interest_list;
    
    public UserDto(){}
    
     public UserDto(String user_email, String user_name, String user_gender, String user_birthday, LocalDateTime joindate, String user_device, String user_is_proud, SocialPath user_social_path, InterestDto user_interest_list) {
        this.user_email = user_email;
        this.user_name = user_name;
        this.user_gender = user_gender;
        this.user_birthday = user_birthday;
        this.joindate = joindate;
        this.user_device = user_device;
        this.user_is_proud = user_is_proud;
        this.user_social_path = user_social_path;
        this.user_interest_list = user_interest_list;
    }
}
  ~~~
  
  > 참고 : JSON 형태로 데이터를 요청하는데 Object 즉 , UserDto 클래스에 매핑시켜서 출력 될 수 있는 이유는 `MessageConverter`가 JSON을 
          Object로 변환 해주기 때문이다.

