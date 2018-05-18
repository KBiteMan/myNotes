---
title: Okhttp源码分析
grammar_cjkRuby: true
---

# 一、OkHttpClient
## 使用到的类
### 1）Protocol（协议）枚举
主要用于声明网络访问协议的。其中定义了协议名称，如http1.0，http2.0等。

### 2)ConnectionSpec（连接规范）
主要负责管理TLS版本和密码套件
使用构造者模式

常用密码套件：
```java
private static final CipherSuite[] APPROVED_CIPHER_SUITES = new CipherSuite[] {
//android4.4之后和java7之后才支持以下内容
      CipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
      CipherSuite.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,
      CipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
      CipherSuite.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,
      CipherSuite.TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256,
      CipherSuite.TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256,
//老版
      CipherSuite.TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,
      CipherSuite.TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,
      CipherSuite.TLS_RSA_WITH_AES_128_GCM_SHA256,
      CipherSuite.TLS_RSA_WITH_AES_256_GCM_SHA384,
      CipherSuite.TLS_RSA_WITH_AES_128_CBC_SHA,
      CipherSuite.TLS_RSA_WITH_AES_256_CBC_SHA,
      CipherSuite.TLS_RSA_WITH_3DES_EDE_CBC_SHA,
  };
```

