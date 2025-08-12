= 🛠️ build过程中技术问题记录

[TOC]

- 🚩 问题一： 私有的POM依赖不在Maven中央仓库中，Maven无法下载

> 日期：2025-06-16

------

### 📌 问题描述

在本地运行 Spring Boot 项目时，Maven报错，显示：

```
Could not find artifact com.xxx.spring.boot:xxx-spring-boot-parent:pom:1.4.17 in central (https://repo.maven.apache.org/maven2)
```

这表示：你的项目依赖了一个 私有的公司发布的 POM（`xxx-spring-boot-parent`），这个依赖不在 Maven 中央仓库中，因此 Maven 无法下载。

但是对于IDEA来说，我在本地下载了_apache-maven-3.6.3_，并且进行了这样的设置按理来说不应该无法下载的。

![image-20250616123521928](./images/image-20250616123521928.png)

### 🧪 排查过程

1. **打开浏览器测试访问** ✅

检查是否能访问对应的 `.pom 文件`

2. **Maven 未正确识别 `settings.xml`** ❌

尽管在 IDEA 中已经设置了`D:\apache-maven-3.6.3\conf\settings.xml`，仍然需要确认命令行构建是否也使用它。

```
mvn help:effective-settings
```


 经确认，实际构建时未正确使用正确的settings 文件。

### ✅ 解决方案

在命令行当中强制指定的`settings`文件，并强制刷新依赖缓存

```
mvn help:effective-settings -U -s D:\apache-maven-3.6.3\conf\settings.xml
```

清理缓存之后重新构建项目

```
mvn clean install -U -s D:\apache-maven-3.6.3\conf\settings.xml
```

其中`-U`表示强制更新依赖，`-s`指定`settings.xml`，让`maven`生效使用我配置的私服。

------

