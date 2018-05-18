---
title: Nexus基本使用
tags: 
grammar_cjkRuby: true
---

## Gradle提交脚本
在根目录下的build.gradle下：
```gradle
allprojects {
    repositories {
        jcenter()
        google()
        mavenLocal()
    }
}
```

在app/build.gradle下的android代码块下：
```gradle
uploadArchives {
        repositories.mavenDeployer {
            repository(url:"http://192.168.192.100:8081/repository/demo/") {
                authentication(userName:"admin", password:"chenchen.199")
            }
            pom.version="1.0"
            pom.artifactId="demo"
            pom.groupId="date.bitman"
        }
    }
```