# Steps for Spring Security

## Step 1
Create a custom user service class, which implements UserDetailsService interface

```java
import org.springframework.security.core.userdetails.UserDetailsService;

public class CustomUserDetailsService implements UserDetailsService {
        @Override
        public UserDetails loadUserByUsername(String username){
            return null;
        }
}
```

## Step 2
#### Create a jwt service class and following methods:
```java
generateToken(String username);
getSignKey();
extractAllClaims(String token);
extractClaim(String token, Function<Claims, T> resolver);
extractUsername(String token);
isValid(String token, UserDetails user);
isTokenExpired(String token);
```

## Step 3
#### Create a jwt authentication filter class
- extend OncePerRequestFilter class
- implement ```doFilterInternal()```
- Get header from request.
- Check if header is null or not starts with ```Bearer```.
- Extract token.
- Extract ```username```  using JwtService class ```extractUsername()``` method.
- Check ```username``` is not null and ```SecurityContextHolder``` is null.
- Call ```loadUserByUsername()```.
- Check token is valid.
- Create an object ```UsernamePasswordAuthenticationToken()``` and store in a variable ```authentication```.
- Do ```setDetails()```.
- Update ```SecurityContextHolder```
- ```filterChain.doFilter(request, response)```

## Step 4
Create a class for spring security configuration
- Inject ```CustomUserDetailsService``` and  ```JwtAuthenticationFilter```.
- Create ```@Bean``` of ```SecurityFilterChain```.
- Disable ```CSRF```
- Configure HTTP requests (Which request should authenticate).
- Pass ```customUserDetailsService``` to ```userDetailsService```.
- Set ```SESSION STATELESS```
- Pass ```jwtAuthenticationFilter```, ```UsernamePasswordAuthenticationFilter.class``` to ```addFilterBefore()```.
- Create ```@Bean PasswordEncoder and AuthenticationManager```.