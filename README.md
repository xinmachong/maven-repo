# 使用说明
#### 一、引入依赖

###### 1、添加个人仓库

```xml
<repositories>
    <repository>
        <id>xinmachong</id>
        <url>https://raw.github.com/xinmachong/maven-repo/master</url>
    </repository>
</repositories>
```

###### 2、引入依赖

```xml
<dependencies>
	<dependency>
        <groupId>com.xinmachong</groupId>
        <artifactId>xmctools</artifactId>
        <version>1.0-RELEASES</version>
    </dependency>
</dependencies>
```

#### 二、配置 application.yml（前提是需要使用 JWT）

```properties
xinmachong:
  jwt:
    # 有效期
    expire: 86400
    # 私密签名
    signature: Xinmachong
    # JWT 放行路由数组
    excludePaths:
      - /v2/test1/getToken1
      - /v1/test/getToken
```

#### 三、使用方法

###### 1、关于 response

可以直接实例化对象使用，例如：new ApiResponse(200,"success",1);

###### 2、关于 JWT

由于其他接口都已经封装，所以需要使用的接口就是生成 token，实现方式如下：

```java
@Resource
private JWTUtils jwtUtils;

@GetMapping("/getToken")
public ApiResponse getToken() {
    Map<String,String> payload = new HashMap<>();
    payload.put("username", "张三");
    String token = jwtUtils.getToken(payload);
    return new ApiResponse(200,"success",token);
}
```

需要解释一下的是，JWTUtils 需要注入，而不是new JWTUtils()；原因在于 JWTUtils 工具类中，需要在对象实例化之前注入 jwt 的有效期和私密签名，所以只能通过注入的方式。
