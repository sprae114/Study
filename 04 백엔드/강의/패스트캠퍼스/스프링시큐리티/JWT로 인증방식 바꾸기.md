1. **JWT 생성**
```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import java.util.Date;

public class JwtUtil {

    private String SECRET_KEY = "SECRET";

    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        return createToken(claims, userDetails.getUsername());
    }

    private String createToken(Map<String, Object> claims, String subject) {
        return Jwts.builder().setClaims(claims).setSubject(subject)
            .setIssuedAt(new Date(System.currentTimeMillis()))
            .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 10)) // 10시간 유효 토큰
            .signWith(SignatureAlgorithm.HS256, SECRET_KEY).compact();
    }
}
```
=> 이 코드는 `generateToken` 메소드를 통해 사용자의 상세 정보를 받아 JWT를 생성합니다. JWT는 `createToken` 메소드에서 실제로 생성되며, 이 메소드는 claims(토큰에 포함될 정보), subject(토큰의 주체, 여기서는 사용자 이름)를 인자로 받습니다. 생성된 토큰은 발행일로부터 10시간 동안 유효하며, HS256 알고리즘과 비밀 키를 이용해 서명됩니다.


2. **JWT 검증**
```java
public class JwtUtil {

    // ...

    public boolean validateToken(String token, UserDetails userDetails) {
        final String username = extractUsername(token);
        return (username.equals(userDetails.getUsername()) && !isTokenExpired(token));
    }

    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    private Boolean isTokenExpired(String token) {
        return extractExpiration(token).before(new Date());
    }

    public Date extractExpiration(String token) {
        return extractClaim(token, Claims::getExpiration);
    }

    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token);
        return claimsResolver.apply(claims);
    }

    private Claims extractAllClaims(String token) {
        return Jwts.parser().setSigningKey(SECRET_KEY).parseClaimsJws(token).getBody();
    }
}
```
=> `validateToken` 메소드는 받아온 토큰과 사용자 상세 정보를 비교하여 토큰의 유효성을 확인합니다. 먼저, 토큰에서 사용자 이름을 추출한 후, 추출된 사용자 이름과 사용자 상세 정보의 사용자 이름이 일치하고, 토큰이 만료되지 않았다면 토큰은 유효한 것으로 판단합니다.


3. **JWT를 이용한 인증 필터**
```java
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;

public class JwtRequestFilter extends OncePerRequestFilter {

    @Autowired
    private MyUserDetailsService userDetailsService;

    @Autowired
    private JwtUtil jwtUtil;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        final String authorizationHeader = request.getHeader("Authorization");

        String username = null;
        String jwt = null;

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
            jwt = authorizationHeader.substring(7);
            username = jwtUtil.extractUsername(jwt);
        }

        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);

            if (jwtUtil.validateToken(jwt, userDetails)) {
                UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken = 
                    new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                usernamePasswordAuthenticationToken
                    .setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);
            }
        }
        filterChain.doFilter(request, response);
    }
}
```
=> `JwtRequestFilter`는 각 요청에 대해 실행되며, 요청의 헤더에서 "Authorization" 필드를 찾아 JWT를 추출합니다. JWT에서 사용자 이름을 추출하고, 해당 사용자 이름에 대한 사용자 상세 정보를 로드한 후, JWT를 검증합니다. 검증이 성공하면, `UsernamePasswordAuthenticationToken` 객체를 생성하여 스프링 시큐리티 컨텍스트에 설정함으로써, 해당 요청이 인증된 것으로 처리합니다.


4. **스프링 시큐리티 설정**
```java
@EnableWebSecurity
public class SecurityConfigurer extends WebSecurityConfigurerAdapter {
    
    @Autowired
    private MyUserDetailsService myUserDetailsService;

    @Autowired
    private JwtRequestFilter jwtRequestFilter;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(myUserDetailsService);
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests().antMatchers("/authenticate").permitAll()
                .anyRequest().authenticated()
                .and().sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS);
        http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
    }

    @Bean
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
```
=> `SecurityConfigurer` 클래스는 스프링 시큐리티 설정을 담당합니다. `configure` 메소드는 HTTP 보안을 구성하는데, 이 예시에서는 CSRF 보호를 비활성화하고, "/authenticate" 경로로 들어오는 모든 요청은 인증 없이 허용하며, 그 외의 모든 요청은 인증을 필요로 합니다. 또한 세션 정책을 설정하고, `JwtRequestFilter`를 `UsernamePasswordAuthenticationFilter` 전에 추가함으로써, 각 요청에 대해 JWT 인증 필터가 먼저 실행되도록 합니다.