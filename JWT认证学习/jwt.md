# JWT长什么样？
由三段信息构成， 这三段信息广西用.链接一起就构成了JWT字符串。就像这样
```token
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

# JWT构成

- 第一部分我们称之为头部（header）
- 第二部分我们称其为载荷（payloa,类似于飞机上的承载的物品）
- 第三部分是签证（signature）

## header 
jwt的头部承载两部分信息：
- 声明类型， 这里是jwt 
- 声明加密码的算法，通常直接使用 HMAC SHA256

完整的头部就像下面这样的JSON：
```js
{
    'typ':'jwt',
    'alg':'HS256'
}
```
然后将头部进行base64加密（该加密是可以对称解密的）构成了第一部分。
```js
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

## payload
载荷就是存放有效信息的地方。 这个名字像是特指飞机上承载的货品， 这些有效的信息包含三个部分：
- 标准中注册的声明
- 公共的声明
- 私有的声明

### 标准中注册的声明（建议但不强制使用）：
- jss : jwt签发者
- sub： jwt所面向的用户
- aud：接收 jwt的一方
- exp：jwt的过期时间， 这个过期时间必须要大于签发时间
- nbf：定义在什么时间之前， 该jwt都是不可用的； 
- iat: jwt的签发时间
- jti：jwt的唯一身份标识， 主要用来作为一次性token，从而回避重放攻击；

### 公共的声明

公共的声明可以添加任何的信息， 一般添加用户的相关信息或其他业务需要的必要信息， 但不建议添加第三信息， 因为该部分在客户端可以解密；

### 私有的声明

私有声明是提供和消费者所共同定义的声明， 一般不建议存放敏感信息， 因为base64是对称解密的， 意味着该部分信息可以归类为明文信息。

定义一个payload

```js
{
    "sub":"1234567890",
    "name":"John Doe",
    "admin":true
}
```
然后将其进行base64加密， 得到jwt的第二部分。
```js
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
```

## signature

jwt的第三部分是一个签证信息， 这个签证信息由三部分组成：

- header(base64后的)
- payload（base64后的）
- secret 

这个部分需要base64加密后的header和base64加密后的payload使用.连接组成的字符串， 然后通过header中声明的加密码方式进行加密secret组成加密，然后就构成了Jwt的第三部分。

```js
var encodedString = base64UrlEncode(header) + '.' + base64UrlEncode(payload);

var signature = HMACSHA256(encodedString, 'secret'); // TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ

```
将这三部分用.连接成一个完整的字符串，构成了最终的jwt:
```js
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```
**注意：secret是保存在服务器端的，jwt的签发生成也是在服务器端的，secret就是用来进行jwt的签发和jwt的验证，所以，它就是你服务端的私钥，在任何场景都不应该流露出去。一旦客户端得知这个secret, 那就意味着客户端是可以自我签发jwt了。**

## 如何使用

一般在请求头里面加入Authorization， 并加上Bearer标注：
```js
fetch('api/user/1', {
  headers: {
    'Authorization': 'Bearer ' + token
  }
})
```
服务端会验证token, 如果验证通过就会返回相应的资源。 整个流程就这样的：


