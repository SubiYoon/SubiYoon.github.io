# Spring Boot 외부경로 파일로 static 활용

**외부 이미지 경로 사용**

- java 파일을 생성해 경로값을 설정해준다.
- **반드시 ‘ / ‘ 닫아줘야 함**
    
    ```java
    import org.springframework.context.annotation.Configuration;
    import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
    import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
    
    @Configuration
    public class WebMvcConfig implements WebMvcConfigurer {
    
        @Override
        public void addResourceHandlers(ResourceHandlerRegistry registry) {
            registry.addResourceHandler("/images/**") //src에 작성할 값
                    .addResourceLocations("file:/절대 경로 값/"); //리눅스 root에서 시작하는 폴더 경로
        }
    }
    
    //윈도우일 경우 아래처럼 사용하면 된다.
    addResourceLocations("file:///D:/DATA/video/");
    ```