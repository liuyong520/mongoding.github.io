https-http2
===



## springboot+http到https

1、http转https需要安全协议ssl证书，协议证书基本上都需要花钱买的，也有免费试用期限的，不过可以自己生成ssl证书（又为自签证书），自己生成证书的方法有多种，在这里就简单的介绍一种由jdk自带的keytool工具生成自签证书，生成证书的前提条件是已经安装了jdk，其次找到安装jdk的bin目录下，可以看到有个keytool

2、使用keytool命令生成证书


```
keytool

-genkey

-alias undertow(别名)

-keypass 123456(别名密码)

-storetype PKCS12(密钥类型)

-keyalg RSA(算法)

-keysize 1024(密钥长度)

-validity 365(有效期，天单位)

-keystore D:/keys/ldkeystore.p12(指定生成证书的位置和证书名称)

-storepass 123456(获取keystore信息的密码)

```

方便复制版：


```
keytool -genkey -alias springboot.spring.org -storetype PKCS12 -keyalg RSA  -keypass 123456 -keysize 1024 -validity 365 -keystore /data/keytool/ldkeystore.p12 -storepass 123456
```


如图：
![](http://topweshare.qiniudn.com/1526206415.png?imageMogr2/thumbnail/!70p)

3、上图选择y即可在/data/keytool文件夹内生成名为ldkeystore.p12的自签证书。

成功后无提示信息

注意：

①/data/keytool目录需要提前手动创建好，否则会生成失败

4、springboot配置自签证书

在application.properties中配置自签证书


```
#https
#证书
server.ssl.key-store=/data/keytool/ldkeystore.p12
#别名密码
server.ssl.key-store-password=123456
#证书类型
server.ssl.key-store-type=PKCS12
#别名
server.ssl.key-alias=springboot.spring.org
server.ssl.protocol=TLSv1.2
```


5、重启服务，通过https://...访问链接即可
![](http://topweshare.qiniudn.com/1526206782.png?imageMogr2/thumbnail/!70p)


注意：在第一次浏览时会提示，此网址存在安全问题，不要紧继续浏览此网页即可，这样就从http转换到https,有不懂http、https的小伙伴也可以先了解一下http和https之间的区别

三、springboot+https+http2

springboot 已经对HTTP2 有支持

```
server.http2.enabled=true
#https
#证书
server.ssl.key-store=/data/keytool/ldkeystore.p12
#别名密码
server.ssl.key-store-password=123456
#证书类型
server.ssl.key-store-type=PKCS12
#别名
server.ssl.key-alias=springboot.spring.org
```


2、重启服务，通过https://...访问链接即可 ；

3、验证http2:使用Chrome的网络工具，在地址栏中输入chrome://net-internals/#http2


笔者未测试出效果，启动的时候存在一行error 的日志。
没搞清楚要干些什么

```
o.a.coyote.http11.Http11NioProtocol      : The upgrade handler [org.apache.coyote.http2.Http2Protocol] for [h2] only supports upgrade via ALPN but has been configured for the ["https-jsse-nio-8080"] connector that does not support ALPN.

```

