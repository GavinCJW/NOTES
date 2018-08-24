# NOTES
---

## 跨域session一致性
```javascript
  每一次跨域请求，都会重新生成一个session，因此需要解决session不一致的问题
  首先需要在前端ajax跨域请求接口时设置withCredentials: true，发送Ajax时，Request header中便会带上 Cookie 信息。
  然后需要在后台设置跨域允许，并设置header("Access-Control-Allow-Credentials: true");，来运行客户端携带证书式访问。
  最后需要注意后台 Access-Control-Allow-Credentials = true时，参数Access-Control-Allow-Origin 的值不能为 '*' 。
  
  JS
  $.ajaxSetup({
      crossDomain: true,
      xhrFields: {
          withCredentials: true
      },
  });
  $.get(url,function(result){console.log(result);},'json');
  
  JAVA
  public CorsFilter corsFilter() {
      CorsConfiguration corsConfiguration = new CorsConfiguration();
      for (String key:origin)
          corsConfiguration.addAllowedOrigin(key);
      for (String key:header)
          corsConfiguration.addAllowedHeader(key);
      for (String key:method)
          corsConfiguration.addAllowedMethod(key);

      corsConfiguration.setAllowCredentials(true);

      UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
      source.registerCorsConfiguration("/**", corsConfiguration); 
      return new CorsFilter(source);
  }
```
