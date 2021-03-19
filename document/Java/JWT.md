# JWT

## 1. JWT 简介

!> `JSON Web Token` （缩写 `JWT` ）是目前最流行的**跨域认证解决方案**。

### 1.1. 官方简介

`JWT` 是一种用于双方之间传递安全信息的简洁的、URL 安全的表述性声明规范。JWT 作为一个开放的标准（[RFC 7519](https://tools.ietf.org/html/rfc7519)），定义了一种简洁的，自包含的方法用于通信双方之间以 Json 对象的形式安全的传递信息。因为数字签名的存在，这些信息是可信的，JWT 可以使用 HMAC 算法或者是 RSA 的公私秘钥对进行签名。

> [!tip]
>
> JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the **HMAC** algorithm) or a public/private key pair using **RSA** or **ECDSA**. ——[来着官网](https://jwt.io/introduction)

### 1.2. 通俗解释

`JWT` 简称 `JSON Web Token` ，也就是通过 `JSON` 形式作为 `Web` 应用中的令牌，用于在各方之间安全地将信息作为 `JSON` 对象传输。在数据传输过程中还可以完成数据加密、签名等相关处理。

## 2. 为什么使用 JWT

### 2.1. 传统的 Session 认证

`http ` 协议本身是一种无状态的协议，而这就意味着如果用户向我们的应用提供了用户名和密码来进行用户认证，那么下一次请求时，用户还要再一次进行用户认证才行，因为根据 `http ` 协议，我们并不能知道是哪个用户发出的请求，所以为了让我们的应用能识别是哪个用户发出的请求，我们只能在服务器存储一份用户登录的信息，这份登录信息会在响应时传递给浏览器，告诉其保存为 `cookie ` , 以便下次请求时发送给我们的应用，这样我们的应用就能识别请求来自哪个用户了, 这就是传统的基于 `session ` 认证。

![image-20210318093926255](media/JWT.assets/image-20210318093926255.png)

**问题**

1. 每个用户经过我们的应用认证之后，我们的应用都要在服务端做一次记录，以方便用户下次请求的鉴别

   通常而言 _session_ 都是保存在内存中，而随着认证用户的增多，服务端的开销会明显增大

2. 用户认证之后，服务端做认证记录，如果认证的记录被保存在内存中的话，这意味着用户下次请求还必须要请求在这台服务器上（或是进行 session 共享），这样才能拿到授权的资源，这样在分布式的应用上，相应的限制了负载均衡器的能力。这也意味着限制了应用的扩展能力。

## 3. JWT 结构

### 3.1. 令牌（token）组成

- 标头( Header)
- 有效载荷(Payload)
- 签名(Signature)

> `JWT`通常如下所示：_xxxxx.yyyyy.zzzzz_；`Header.Payload.Signature`

### 3.2. Header

标头通常由两部分组成 ∶ 令牌的类型(即 JWT）和所使用的签名算法，例如 HNAC SHA256 或 RSA。它会使用 Base64 编码组成 JWT 结构的第一部分。

> `Base64`是一种编码，也就是说，它是可以被翻译回原来的样子来的。它并不是一种加密过程。

```json
{
    "alg":"HS256",
    "typ"："JWT"
}
```

### 3.3. Payload

令牌的第二部分是有效负载，其中包含声明。声明是有关实体(通常是用户）和其他数据的声明。同样的，它会使用`Base64 `编码组成`JWT`结构的第二部分。

```json
{
  "id": "123456",
  "name": "bzm",
  "admin": true
}
```

### 3.4. Signature

前面两部分都是使用 `Base64`进行编码的，即前端可以解开知道里面的信息。Signature 需要使用编码后的 header 和 payload 以及我们提供的一个密钥，然后使用 header 中指定的签名算法（HS256）进行签名。签名的作用是保证 JWT 没有被篡改过

> 如：`HMACSHA256(base64UrlEncode(header) + "." + + base64UrlEncode(payload),secret);`

### 3.5. 签名目的

最后一步签名的过程，实际上是对头部以及负载内容进行签名，防止内容被窜改。如果有人对头部以及负载的内容解码之后进行修改，再进行编码，最后加上之前的签名组合形成新的 JWT 的话，那么服务器端会判断出新的头部和负载形成的签名和 JWT 附带上的签名是不一样的。如果要对新的头部和负载进行签名，在不知道服务器加密时用的密钥的话，得出来的签名也是不一样的。

### 3.6. 信息安全问题

- Base64 是一种编码，是可逆的，那么我的信息不就被暴露了吗？

- 所以，在 JWT 中，不应该在负载里面加入任何敏感的数据。在上面的例子中，我们传输的是用户的 User ID。这个值实际上不是什么敏感内容，一般情况下被知道也是安全的。但是像密码这样的内容就不能被放在 JWT 中了。如果将用户的密码放在了 JWT 中，那么怀有恶意的第 三方通过 Base64 解码就能很快地知道你的密码了。因此 JWT 适合用于向 Web 应用传递一些非敏感信息。JWT 还经常用于设计用户认证和授权系统，甚至实现 Web 应用的单点登录。

![20200812134003526](media/JWT.assets/20200812134003526.png)

通过 Base64 编码之后

![20200812134056514](media/JWT.assets/20200812134056514.png)

## 4. 使用 JWT

### 4.1. 引入依赖

```xml
<!--引入jwt-->
<dependency>
  <groupId>com.auth0</groupId>
  <artifactId>java-jwt</artifactId>
  <version>3.4.0</version>
</dependency>
```

### 4.2. 生成 token

```java
	@Test
	public void createToken() {
		Calendar instance = Calendar.getInstance();
		instance.add(Calendar.SECOND, 900);

		Map<String, Object> map = new HashMap<>();
		// 生成令牌
		String token = JWT.create()
				.withHeader(map) // 设置Header 可以不写
				.withClaim("userId", 21) // 设置payload
				.withClaim("username", "张三") // 自定义用户名
				.withExpiresAt(instance.getTime()) // 过期时间
				.sign(Algorithm.HMAC256("!##$d34jd*)#"));// 设置签名

		System.out.println(token);
	}
```

### 4.3. 根据令牌和签名解析数据

```java
	@Test
	public void checkToken() {
		// 创建验证Token的对象
		JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("!##$d34jd*)#")).build();
		DecodedJWT verify = jwtVerifier.verify("eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2MTYxMjgwNjksInVzZXJJZCI6MjEsInVzZXJuYW1lIjoi5byg5LiJIn0.Bj6E6yeFND_PlYsySKyA-uEkHng4hMlJ01wTGN4zpsU");
		System.out.println("用户Id: " + verify.getClaim("userId").asInt());
		System.out.println("用户名: " + verify.getClaim("username").asString());
	}
```

> 签名和 token 要对应

### 4.4. 常见异常信息

- SignatureVerificationException：签名不一致异常
- TokenExpiredException：令牌过期异常
- AlgorithmMismatchException：算法不匹配异常
- InvalidClaimException：失效的 payload 异常

## 5. SpringBoot 整合 JWT

