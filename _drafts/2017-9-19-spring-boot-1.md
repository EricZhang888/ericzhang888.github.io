---
title: Spring boot(一) 认识启动
date: 2017-9-19 8:30:09
categories:
- Spring
comments: true
---

# Spring boot是什么？



# 为什么需要Spring boot？


# Spring boot の Hello world


## 官方示例代码

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Example.class, args);
    }

}
```


以上这段代码启动会看到如下日志

```text
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.5.7.RELEASE)

2017-09-19 09:21:50.284  INFO 930 --- [           main] c.c.r.activity.ActivityApplication       : Starting ActivityApplication on zaqwebs-MacBook-Air.local with PID 930 (/Users/zaqweb/crcWork/gitProjects/activity-springboot/target/classes started by zaqweb in /Users/zaqweb/crcWork/gitProjects/activity-springboot)
2017-09-19 09:21:50.289  INFO 930 --- [           main] c.c.r.activity.ActivityApplication       : No active profile set, falling back to default profiles: default
2017-09-19 09:21:50.435  INFO 930 --- [           main] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@530612ba: startup date [Tue Sep 19 09:21:50 CST 2017]; root of context hierarchy
2017-09-19 09:21:52.975  INFO 930 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port(s): 8080 (http)
2017-09-19 09:21:53.007  INFO 930 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2017-09-19 09:21:53.009  INFO 930 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.20
2017-09-19 09:21:53.238  INFO 930 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2017-09-19 09:21:53.238  INFO 930 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2809 ms
2017-09-19 09:21:53.480  INFO 930 --- [ost-startStop-1] o.s.b.w.servlet.ServletRegistrationBean  : Mapping servlet: 'dispatcherServlet' to [/]
2017-09-19 09:21:53.487  INFO 930 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'characterEncodingFilter' to: [/*]
2017-09-19 09:21:53.488  INFO 930 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
2017-09-19 09:21:53.489  INFO 930 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'httpPutFormContentFilter' to: [/*]
2017-09-19 09:21:53.491  INFO 930 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'requestContextFilter' to: [/*]
2017-09-19 09:21:54.088  INFO 930 --- [           main] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@530612ba: startup date [Tue Sep 19 09:21:50 CST 2017]; root of context hierarchy
2017-09-19 09:21:54.235  INFO 930 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2017-09-19 09:21:54.236  INFO 930 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2017-09-19 09:21:54.281  INFO 930 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2017-09-19 09:21:54.282  INFO 930 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2017-09-19 09:21:54.414  INFO 930 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2017-09-19 09:21:54.734  INFO 930 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2017-09-19 09:21:54.893  INFO 930 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)
2017-09-19 09:21:54.904  INFO 930 --- [           main] c.c.r.activity.ActivityApplication       : Started ActivityApplication in 5.951 seconds (JVM running for 7.254)
```


## @EnableAutoConfiguration
