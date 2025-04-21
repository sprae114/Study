# 왜 jwt는 필터에서 처리하는걸까? 인터셉터에서 하면 처리하면 안돼?
JWT(Json Web Token)를 스프링 애플리케이션에서 **필터(Filter)**에서 처리하는 이유와, **인터셉터(Interceptor)**에서 처리할 수 없는지에 대한 질문에 대해 자세히 설명하겠습니다. 결론부터 말하면, JWT는 필터에서 처리하는 것이 일반적이지만, 인터셉터에서 처리하는 것도 가능합니다. 다만, 두 방식의 차이와 설계 목적 때문에 필터가 더 적합한 경우가 많습니다. 아래에서 이유와 차이를 분석하겠습니다.

---

### 1. **필터와 인터셉터의 기본 차이**
#### **필터 (Filter)**
- **위치**: 서블릿 컨테이너 레벨에서 동작하며, `DispatcherServlet` 이전에 요청을 가로챕니다.
- **역할**: HTTP 요청/응답을 전처리하거나 후처리 (예: 인증, 로깅, 인코딩).
- **특징**:
  - 스프링 컨테이너 밖에서 실행되므로, 스프링의 IoC나 빈에 직접 접근하려면 추가 설정이 필요.
  - 모든 요청(스프링 외부 요청 포함)에 대해 적용 가능.
- **주요 메서드**: `doFilter(ServletRequest, ServletResponse, FilterChain)`.

#### **인터셉터 (Interceptor)**
- **위치**: 스프링 컨테이너 레벨에서 동작하며, `DispatcherServlet`이 컨트롤러로 요청을 전달하기 전/후에 개입합니다.
- **역할**: 컨트롤러 호출 전/후에 로직을 추가 (예: 인증, 로깅, 요청 검증).
- **특징**:
  - 스프링 컨텍스트 내에서 동작하므로 빈에 쉽게 접근 가능.
  - 스프링 MVC 요청(`DispatcherServlet`이 처리하는 요청)에만 적용.
- **주요 메서드**: `preHandle`, `postHandle`, `afterCompletion`.

---

### 2. **JWT를 필터에서 처리하는 이유**
JWT는 주로 **인증(Authentication)** 과 **인가(Authorization)** 를 위해 사용되며, HTTP 요청의 헤더(예: `Authorization: Bearer <token>`)에 포함됩니다. 이를 필터에서 처리하는 이유는 다음과 같습니다:

#### (1) **요청 처리의 초기 단계**
- JWT는 요청이 컨트롤러에 도달하기 전에 **인증 여부를 확인**해야 하는 경우가 많습니다. 필터는 `DispatcherServlet` 이전에 동작하므로, 인증 실패 시 불필요한 스프링 MVC 처리(컨트롤러 호출 등)를 피할 수 있습니다.
- 예: 유효하지 않은 토큰이면 바로 `401 Unauthorized` 응답을 반환.

#### (2) **스프링 외부 요청 포함**
- 필터는 스프링 컨테이너 밖에서 동작하므로, 스프링이 처리하지 않는 요청(예: 정적 리소스, 다른 서블릿 요청)에도 적용 가능합니다.
- 반면, 인터셉터는 `DispatcherServlet`이 관여하는 요청에만 적용되므로 범위가 제한적입니다.

#### (3) **보안의 계층화**
- 인증은 애플리케이션의 가장 바깥 계층에서 처리하는 것이 보안적으로 적합합니다. 필터는 서블릿 컨테이너 레벨에서 동작하므로, 보안 로직을 더 낮은 레벨에서 적용할 수 있습니다.
- 인터셉터는 스프링 MVC 내부에서 동작하므로, 보안 처리가 스프링에 의존적이 됩니다.

#### (4) **표준화된 방식**
- JWT 인증은 서블릿 기반의 표준 보안 필터(예: `OncePerRequestFilter`)와 잘 맞습니다. 스프링 시큐리티(Spring Security)도 필터 체인을 사용해 JWT를 처리하며, `JwtAuthenticationFilter` 같은 필터를 추가하는 방식이 널리 사용됩니다.

#### **예시: JWT 필터**
```java
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.filter.OncePerRequestFilter;
import java.io.IOException;

public class JwtFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException {
        String header = request.getHeader("Authorization");
        if (header != null && header.startsWith("Bearer ")) {
            String token = header.substring(7);
            // JWT 검증 로직 (유효성, 만료 체크 등)
            if (isValidToken(token)) {
                // 인증 정보 설정 (예: SecurityContext에 저장)
                chain.doFilter(request, response); // 다음 단계로 진행
                return;
            }
        }
        response.setStatus(HttpServletResponse.SC_UNAUTHORIZED); // 401 반환
    }

    private boolean isValidToken(String token) {
        // JWT 검증 로직 구현
        return true;
    }
}
```
- **등록**:
```java
@Configuration
public class WebConfig {
    @Bean
    public FilterRegistrationBean<JwtFilter> jwtFilter() {
        FilterRegistrationBean<JwtFilter> registrationBean = new FilterRegistrationBean<>();
        registrationBean.setFilter(new JwtFilter());
        registrationBean.addUrlPatterns("/api/*");
        return registrationBean;
    }
}
```

---

### 3. **인터셉터에서 JWT를 처리하면 안 되는 이유는?**
인터셉터에서 JWT를 처리하는 것이 불가능한 것은 아니지만, 몇 가지 단점과 설계적 부적합성 때문에 덜 선호됩니다:

#### (1) **처리 시점의 차이**
- 인터셉터는 `DispatcherServlet`이 요청을 컨트롤러로 라우팅하기 직전에 동작합니다. 즉, 스프링 MVC의 일부 처리가 이미 시작된 후에야 JWT를 검증하게 됩니다.
- 인증 실패 시 불필요한 초기화 작업(핸들러 매핑 등)이 발생할 수 있어 비효율적입니다.

#### (2) **적용 범위의 제한**
- 인터셉터는 스프링 MVC 요청에만 적용됩니다. 정적 리소스(예: `/static/css/style.css`)나 다른 서블릿 요청에는 작동하지 않습니다.
- 반면, JWT 인증은 모든 요청에 대해 일관되게 적용되어야 하는 경우가 많습니다.

#### (3) **스프링 시큐리티와의 통합성**
- 스프링 시큐리티를 사용할 경우, 인증은 필터 체인(예: `UsernamePasswordAuthenticationFilter`)에서 처리됩니다. 인터셉터는 이 체인과 별개로 동작하므로, 보안 설정과 통합하기 어렵습니다.

#### **예시: 인터셉터로 JWT 처리**
```java
import org.springframework.web.servlet.HandlerInterceptor;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class JwtInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        String header = request.getHeader("Authorization");
        if (header != null && header.startsWith("Bearer ")) {
            String token = header.substring(7);
            if (isValidToken(token)) {
                return true; // 컨트롤러로 진행
            }
        }
        response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
        return false; // 중단
    }

    private boolean isValidToken(String token) {
        return true;
    }
}
```
- **등록**:
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new JwtInterceptor()).addPathPatterns("/api/**");
    }
}
```
- **단점**: 정적 리소스 요청에는 적용되지 않고, `DispatcherServlet` 이후에만 동작.

---

### 4. **필터 vs 인터셉터: JWT에 적합한 선택**
| **구분**            | **필터**                          | **인터셉터**                       |
|---------------------|----------------------------------|-----------------------------------|
| **동작 레벨**       | 서블릿 컨테이너                  | 스프링 MVC                        |
| **적용 범위**       | 모든 요청 (스프링 외부 포함)      | 스프링 MVC 요청만                 |
| **처리 시점**       | `DispatcherServlet` 이전         | `DispatcherServlet` 이후          |
| **보안 통합**       | 스프링 시큐리티와 잘 맞음         | 별도 관리 필요                    |
| **유연성**          | 초기 단계에서 차단 가능           | 컨트롤러 호출 직전에만 개입        |

#### **왜 필터가 더 적합한가?**
- **초기 차단**: 인증은 요청 처리의 첫 단계에서 끝내버리는 것이 효율적이고 안전합니다.
- **범용성**: 스프링 MVC 외의 요청에도 적용 가능.
- **스프링 시큐리티**: JWT를 스프링 시큐리티와 통합할 때 필터 체인을 활용하는 것이 표준.

#### **인터셉터를 사용할 수 있는 경우**
- JWT 검증이 특정 스프링 MVC 컨트롤러에만 필요한 경우.
- 스프링 빈(예: `UserService`)에 접근해야 하는 인증 로직이 필요할 때 (필터는 추가 설정 필요).

---

### 5. **실제 사용 사례**
- **스프링 시큐리티 + JWT**:
  - `JwtAuthenticationFilter`를 필터로 추가해 토큰 검증 후 `SecurityContext`에 인증 정보 저장.
  - 이후 컨트롤러에서 `@PreAuthorize`로 인가 체크.
- **인터셉터 사용 예외**:
  - 간단한 프로젝트에서 정적 리소스 처리가 없고, 스프링 빈과 밀접한 로직이 필요할 때.

---

### 결론
JWT는 **필터에서 처리**하는 것이 일반적인 이유는:
1. 요청 처리의 초기 단계에서 인증을 끝낼 수 있어 효율적.
2. 모든 요청에 적용 가능하며, 스프링 시큐리티와 통합성이 좋음.

**인터셉터에서 처리하는 것도 가능**하지만, 적용 범위와 처리 시점의 한계 때문에 덜 적합합니다. 따라서 JWT 인증은 필터에서 처리하는 것이 설계적으로 더 자연스럽고 널리 사용되는 방식입니다.

추가로 궁금한 점이 있으면 말씀해주세요!