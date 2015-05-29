---
layout: post
title: Tomcat自签名HTTPS证书生成与配置
category: Java
tags: [Java, Tomcat, HTTPS, SSL]
keywords: iOS
description: 
---

> 说明如何生成SSL证书，并配置Tocmat的Https端口

##1. 自签名CA证书（根证书）
> CA证书的制作有两个步骤

1. 创建私钥文件

```
openssl genrsa -out myCA.key 2048
```

2.创建根证书

```
openssl req -x509 -new -key myCA.key -out myCA.cer -days 730 -subj /CN="My CA"
```

##2.自签名SSL证书（叶证书）

1.创建私钥文件

```
openssl genrsa -out server.key 2048
```

2.创建CSR

```
openssl req -new -out server.req -key server.key -subj /CN=127.0.0.1/CN=localhost
```
这里服务器地址为127.0.0.1，输出文件为server.req。

3.通过CSR创建SSL证书

```
openssl x509 -req -in server.req -out server.cer -CAkey myCA.key -CA myCA.cer -days 3650 -CAcreateserial -CAserial server.serial
```

4.导出p12证书

```
openssl pkcs12 -export -in server.cer -inkey server.key -out server.p12 -name "server"
```

5.导出Java KeyStore

```
keytool -importkeystore -v -srckeystore server.p12 -srcstoretype pkcs12 -srcstorepass 123123 -destkeystore server.keystore -deststoretype jks -deststorepass 123123
```

##3.使用SSL证书配置Tomcat的Https端口

修改tomcat的server.xml文件，添加Https配置
```
<Connector SSLEnabled="true" clientAuth="false" keystoreFile="server.keystore" keystorePass="123123" maxThreads="150" port="39080" protocol="HTTP/1.1" scheme="https" secure="true" sslProtocol="TLS"/>
```







