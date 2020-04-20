---
title: Spring Boot Cors Ajax Config Code Sample
category: Java
---

# spring boot cors ajax 配置代码示例

### ajax 请求

```
$(document).ready(function() {
    var param={'password': 'password'};
    $.ajax({
        url: "http://localhost:8888/user/name/xiaozhang",
        type: "POST",
        async:false, // 因为是登录操作
        xhrFields: {
            withCredentials: true // 跨域请求要想带cookie必须加。
        },
        contentType:"application/json",
        dataType:"json",
        data:JSON.stringify(param),
        success: function(result){
            console.log("success");
            console.log(result);
        },
        error: function (request, status, error) {
            console.log(request.responseText); // returned json data
            console.log("error");
            console.log(error);
        }
    });
});
```

其中`withCredentials: true`，是因为要携带cookie，而服务器端也要设置：

### spring boot cors 设置

add this before request method：
`@CrossOrigin({"http://localhost:9000", "http://example.com"}, allowCredentials = "true")`

or add global config：
```
@Configuration
public class CorsConfig extends WebMvcConfigurerAdapter {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurerAdapter() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedHeaders("*")
                        .allowedMethods("GET", "POST", "PUT", "DELETE", "HEAD")
                        .allowedOrigins("http://localhost:63342", "http://localhost:8081")
                        .allowCredentials(true).maxAge(3600);
            }
        };
    }
}
```

### spring boot return http status code along with json data

`return new ResponseEntity<Object>(map, HttpStatus.OK);`