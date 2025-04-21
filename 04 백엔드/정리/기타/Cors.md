## Cors 개념
CORS(Cross-Origin Resource Sharing) 문제는 프론트엔드와 백엔드가 다른 도메인 또는 포트에서 실행될 때 발생하는 일반적인 문제입니다.

## Http 요청
### `@CrossOrigin` 어노테이션 사용
컨트롤러 클래스나 메서드에 `@CrossOrigin` 어노테이션을 추가하여 **특정 경로에 대해 CORS를 설정**할 수 있습니다.

```java
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@CrossOrigin(origins = "http://example.com") // 허용할 출처를 지정
public class MyController {

    @GetMapping("/api/data")
    public String getData() {
        return "Hello, CORS!";
    }
}
```


### 글로벌 CORS 설정
**모든 컨트롤러 클래스의 Cors를 설정**할 수 있습니다.
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**") // 모든 경로에 대해 CORS 설정
                .allowedOrigins("http://example.com") // 허용할 출처
                .allowedMethods("GET", "POST", "PUT", "DELETE") // 허용할 HTTP 메서드
                .allowCredentials(true); // 자격 증명 허용
    }
}
```


### 스프링 시큐리티에서 설정
▶ `CorsConfig` 생성
`CorsConfig` 클래스에서 `CorsConfigurationSource`를 등록합니다.
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;

import java.util.Collections;

@Configuration
public class CorsConfig {

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(Collections.singletonList("http://example.com"));
        configuration.setAllowedMethods(Collections.singletonList("*"));
        configuration.setAllowCredentials(true);
        configuration.setAllowedHeaders(Collections.singletonList("*"));
        configuration.setMaxAge(3600L);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration); // 모든 경로에 대해 CORS 설정
        return source;
    }
}
```

▶ Security 설정 클래스
`SecurityConfig` 클래스에서 `cors()` 메서드를 사용하여 CORS를 활성화합니다. 그리고  `CorsConfigurationSource`를 자동으로 찾아서 사용합니다. 

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;

@Configuration
public class SecurityConfig {

    @Bean
    SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
        ...
        
        http.cors() // CORS 활성화
	    return http.build();
    }
}
```


## WebSocket
### WebSocket CORS 설정
```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

	// 웹 브
    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic");
        config.setApplicationDestinationPrefixes("/app");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws")
                .setAllowedOrigins("http://localhost:3000") // 허용할 출처
                .withSockJS();
    }
}
```