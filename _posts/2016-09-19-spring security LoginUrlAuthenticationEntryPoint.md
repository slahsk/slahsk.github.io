---
layout: post
title:  LoginUrlAuthenticationEntryPoint
date:   2016-09-19
categories: spring
tags: java spring security
excerpt: LoginUrlAuthenticationEntryPoint
---

session 끊긴후 ajax 를 호출하면 상태값이 200 떨어지면서 정상처리 된다.

이를 처리 하기 위해서 `LoginUrlAuthenticationEntryPoint` 에서 `commence` 메소드를 오버라딩 해한다.

`AuthenticationException or AccessDeniedException`

발생시 ExceptionTranslationFilter 에서 처리 하는듯 하다.

`AccessDeniedException` 발생시 url 검사를 하여 `ajax` 포함 하고 있으면 `response header` 상태값을 `403` 으로 변화시킨다

```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
    
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.LoginUrlAuthenticationEntryPoint;
   
public class AjaxAwareAuthenticationEntryPoint extends LoginUrlAuthenticationEntryPoint{
  public AjaxAwareAuthenticationEntryPoint(String loginUrl) {
    super(loginUrl);
  }
    
  @Override
  public void commence(
    HttpServletRequest request, 
    HttpServletResponse response, 
    AuthenticationException authException) 
    throws IOException, ServletException {

    boolean isAjax = request.getRequestURI().contains("ajax");

    if (isAjax) {
      response.sendError(403, "Forbidden");
    } else {
      super.commence(request, response, authException);
    }
  }
}

```

`ExceptionTranslationFilter` 에 등록해서 사용 하지만 간단하게 사용시
```xml
    <bean id="exceptionTranslationFilter" class="org.springframework.security.web.access.ExceptionTranslationFilter">
     <property name="authenticationEntryPoint" ref="authenticationEntryPoint"/>
     <property name="accessDeniedHandler" ref="accessDeniedHandler"/>
    </bean>
    
    <bean id="authenticationEntryPoint"
         class="org.springframework.security.web.authentication.LoginUrlAuthenticationEntryPoint">
     <property name="loginFormUrl" value="/login.jsp"/>
    </bean>
    
    <bean id="accessDeniedHandler" class="org.springframework.security.web.access.AccessDeniedHandlerImpl">
      <property name="errorPage" value="/accessDenied.htm"/>
    </bean>
```


```xml
<http auto-config="true" entry-point-ref="ajaxAwareAuthenticationEntryPoint">
....
</http>
```
