# Java9 新特性

翻译自 : [https://docs.oracle.com/javase/9/whatsnew/toc.htm#JSNEW-GUID-5B808B2F-E891-43CD-BF6E-78787E547071](https://docs.oracle.com/javase/9/whatsnew/toc.htm#JSNEW-GUID-5B808B2F-E891-43CD-BF6E-78787E547071)

## Key Changes in JDK9

### 1. Java平台模块化系统

引入了一种新的Java编程组建 -- 一个被命名且自描述的代码和数据的集合 -- 模块.
在新的模块系统中:

1. 引入了一个新的可选的阶段, link-time. link-time是介于compile-time和run-time之间的,
在link-time 期间, 一组module可以被打包和优化进一个自定义的运行时镜像(小型容器?).

详情见 [jlink](https://docs.oracle.com/javase/9/tools/jlink.htm#JSWOR-GUID-CECAC52B-CFEE-46CB-8166-F17A8E9280E9) tool in Java Platform, Standard Edition Tools Reference

2. 在java的工具javac, jlink 和java中添加了新的option, 用来指定特定的module paths. Module path 定位了 Module 的定义.

3. 引入了模块化的JAR文件(在根目录中有module-info.class)

4. 引入了JMOD格式. 这个格式是一种和JAR相似的打包格式, 但是他可以包含native code和配置文件. 详情见 [jmod](https://docs.oracle.com/javase/9/tools/jmod.htm#JSWOR-GUID-0A0BDFF6-BE34-461B-86EF-AAC9A555E2AE) tool


JDK模块本身已经被分成了一组模块. 变化如下:

1. 允许你把JDK的 module 和各种配置文件结合起来, 包括:

> 有关JRE和JDK的配置
> 类似于Java8中的Compact Profiles的配置
> 自定义的配置,这些配置中包含了一组特定的Module以及它们所依赖的module

2. 重构了JDK和JRE的运行时镜像从而容纳module并且提升了性能, 安全以及可靠性.

3. 定义了一种新的URI模式用来命名存储在运行时镜像中的模块,类和资源, 从而不用暴露运行时镜像的内部结构和格式

4. 去掉了endorsed-standards override 机制(覆盖系统api) 和 扩展机制(添加系统默认api).

5. 从Java运行时镜像中去掉了rt.jar和tools.jar

6. 大多数的JDK内部API都默认不可见了(inaccessible), 但是留下来的一些极其重要 并 广泛被使用的内部API. 可以运行jdeps -jdkinternals 来决定代码是否使用JDK内部API

### 2. 新的版本格式

提供了一个简化的版本字符串格式从而更加清楚的区分major, minor, security 和 patch 的更新新版本. 新的版本格式如下:

> $MAJOR.$MINOR.$SECURITY.$PATCH

1. $MAJOR是主版本号, 例如JDK9

2. #MINOR是每次小型更新的版本号,例如bug fix, 标准api的修订 或者是一些平台说明文档之外的特性的实现

3. $SECURITY是安全更新版本的版本号.

4. $PATCH是包含了安全和高优先级的修补的版本的版本号

## What’s New for the JDK 9 Installer

略

## What’s New for Tools in JDK 9

### JShell

 在Java平台中添加了REPL功能. JShell提供了一个交互的命令行接口用来在Java编程平台上运行并计算declarations, statements, and expressions. 这个功能可以立即获得代码的运算结果或者反馈, 从而促进代码的开发以及为Java学习提供帮助.

 详情见 [jshell](https://docs.oracle.com/javase/9/tools/jshell.htm#JSWOR-GUID-C337353B-074A-431C-993F-60C226163F00) in Java Platform, Standard Edition Tools Reference 以及 Introduction to JShell in Java Platform, Standard Edition Java Shell User’s Guide.

### Add More Diagnostic Commands

 添加了更多的诊断命令从而提高诊断JDK和HotSpot的能力

 详情见 [jcmd](https://docs.oracle.com/javase/9/tools/jcmd.htm#JSWOR743) in Java Platform, Standard Edition Tools Reference.

### Remove Launch-Time JRE Version Selection

 去除了请求一个和加载时jre版本不同的jre版本的功能.

 现代的应用大多数通过Java Web Start, 本地OS 打包系统或者是安装包进行部署. 这些技术有他们自己的方法来管理所需要的JRE版本, 例如查找, 下载或者更新等. 因此 Launch-Time JRE Version Selection 这个功能已经过时了.

### Multi-Release JAR Files

 扩展了JAR文件格式，使得针对不同Java发行版的class文件可以在一个JAR中共存。

 一个多版本(multirelease JAR -- MRJAR)JAR文件包含了额外的版本化的目录结构，用来存放针对不同Java平台版本的类文件和资源文件。 可以通过[jar](https://docs.oracle.com/javase/9/tools/jar.htm#JSWOR614)工具的 --release选项来指定版本目录。

### Remove the JVM TI hprof Agent

 移除了JDK中的hprof agent代理。 Hprof agent 是作为JVM工具接口的实例代码的， 本来并没有被设计成一个产品级的工具。

 Hprof agent中有用的功能已经被其他替代产品所替代了。

 NOTE：
 > 虽然hprof被替代了， 但是用jmap或者其他诊断工具依旧可以创建一份hprof格式的heap dump

### Remove the jhat Tool

 从JDK中移除了jhat命令。

 jhat命令是在JDK6中加入的一个实验性的并且unsupported的工具。 它已经过时了;更高级的堆可视化工具和分析工具已经有很多年历史了。

### Validate JVM Command-Line Flag Arguments

 为了避免发生错误， 将会检查JVM中所有数值化的命令行参数。 如果参数不合法， 会输出一段对应的错误信息。

 范围以及选项限制检查已经被应用到所有需要用户指定的数值化的命令行参数上了。

 详情见[java](https://docs.oracle.com/javase/9/tools/java.htm#JSWOR624)和 [Validate Java Virtual Machine Flag Arguments](https://docs.oracle.com/javase/9/tools/java.htm#JSWOR-GUID-9569449C-525F-4474-972C-4C1F63D5C357)

### Compile for Older Platform Versions

 改进了javac命令， 从而使得它可以编译出跑在9之前版本的Java平台上的程序。

 当使用 -source 或者 -target命令的时候， 编译出的程序可能偶尔会调用先前版本不支持的API. 可以使用 --release选型来防止出现这个问题。

 详情见 [javac](https://docs.oracle.com/javase/9/whatsnew/toc.htm#JSNEW-GUID-71A09701-7412-4499-A88D-53FA8BFBD3D0)

### jlink: The Java Linker

 收集和优化一组模块以及它们的依赖， 并形成一个自定义的运行时镜像 （defined in [JEP 220](http://openjdk.java.net/jeps/220)）。

 jlink工具为装配阶段的转化和优化以及替代镜像格式的生成定义了一个插件机制。 它可以为一个特定的程序生成并优化出一个自定义的运行时。 [JEP 261](http://openjdk.java.net/jeps/261)定义了一个 link-time(链接时？) 的概念, 作为一个介于编译时和运行时这两个阶段之间的一个可选的阶段。 Link-Time需要一个Linking工具来装配并优化一组模块以及他们相关的依赖， 从而生成一个运行时镜像或是可执行文件。

 详情见[jlink](https://docs.oracle.com/javase/9/tools/jlink.htm#JSWOR-GUID-CECAC52B-CFEE-46CB-8166-F17A8E9280E9)

##  What’s New for Security in JDK 9

### Datagram Transport Layer Security (DTLS)

 添加了Java Secure Socket Extension(JSSE) API 和 SunJSSE 的安全实现， 从而支持了DTLS Version 1.0 和 DTLS Version 1.2 这两个协议。

 详情见[ Datagram Transport Layer Security (DTLS) in Java Platform, Standard Edition Security Developer's Guide.](https://docs.oracle.com/javase/9/security/java-secure-socket-extension-jsse-reference-guide.htm#JSSEC-GUID-D0F8B9C3-721B-43A4-B2CE-7512B175F76D)

### TLS Application-Layer Protocol Negotiation Extension

 允许客户端和服务器端在一个TLS连接中协商所要用的应用层协议。 在应用层协议协商汇中(Application-Layer Protocol Negotiation, ALPN), 客户端在 TLS ClientHello中发送一个支持的应用协议的列表。 服务器端选择一个协议并把这个选择作为TLS ServerHello的一部分内容发送回去。 ALPN可以在TLS的握手阶段完成， 不会对网络造成负担。

 详情见[TLS Handshake and Application Layer Protocol Negotiation in Java Platform, Standard Edition Security Developer's Guide.](http://www.oracle.com/pls/topic/lookup?ctx=javase9&id=JSSEC-GUID-C6C70051-04C6-45C0-A5D4-9FF08772ED66)

### OCSP Stapling for TLS

  (OCSP : Online Certificate Status Protocol 在线证书状态协议， 向一个CA颁发机构询问证书的合法性。 一般是由Client发起的。
  OCSP Stapling ： 由Server向OCSP URL询问证书的合法性并发给客户端。 主要是为了提速)

  允许TLS链接中的Server检查一个X.509证书是否已经废弃。 在TLS的握手阶段中， Server会联系(contact)一个OSCP的响应方来查询一个正在协商中的证书。 之后Server会把有关该证书的废弃信息附加进返回给客户端的证书中， 这样客户端就可以进行相应的操作。

  允许客户端向一个TLS Server发起 OCSP Stapling的请求。客户端会检查从支持这个功能的Server所返回的信息。

  详情见[See OCSP Stapling in Java Platform, Standard Edition Security Developer's Guide.](https://docs.oracle.com/javase/9/security/java-secure-socket-extension-jsse-reference-guide.htm#JSSEC-GUID-489366D5-635A-4204-8980-3FB126047C45)

### Leverage CPU Instructions for GHASH and RSA

  使用HotSpot的 GHASH 的原语把 AES/GSM/NoPadding算法的效率提升了34倍到150倍。 在Intel x64 CPU 上的PCLMULQDQ指令和 SPARC上的xmull/xmulhi指令都提高了GHASH原语的速度。

  使用HotSpot的RSA原语把 BigInteger.squareToLen 和 BigInteger.mulAdd 方法的性能提升了50%. RSA原语适用于Intel X64上的java.math.BigInteger类。

  引入了一个新的安全属性jdk.security.provider.preferred来配置针对特定的算法进行显著的性能提升的provider

  详情见[See Configuring the Preferred Provider for Specific Algorithms in Java Platform, Standard Edition Security Developer's Guide.](https://docs.oracle.com/javase/9/security/java-secure-socket-extension-jsse-reference-guide.htm#JSSEC-GUID-C3441400-9D82-4545-ADAE-0C332EA4AC58)

### DRBG-Based SecureRandom Implementations

  提供了在[ SecureRandom API](https://docs.oracle.com/javase/9/docs/api/java/security/SecureRandom.html)中提到的[NIST SP 800-90Ar1](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf)所说的伪随机数生成器机制的函数功能

  DRGB机制使用了与SHA-512和AES-256一样可靠的现代算法。 这些机制可以被配置成不同的安全强度和特性， 从而满足用户的需求。

### Disable SHA-1 Certificates

  通过提供了一种更加灵活的机制来禁用了基于SHA-1签名的X.509证书链， 从而提升了JDK的安全配置。

  禁用了JDK中默认包含的TLS Server中利用根证书定位的证书链中的SHA-1算法。 本地或者企业的证书认证不受影响。

  在jdk.certpath.disabledAlgorithms中添加了一些新的安全限制从而提升其安全属性。 这些限制扩大了对可能被禁用的证书的控制。

### Create PKCS12 Keystores by Default

  默认的keystore类型从JKS修改成了PKCS12. PKCS12是一个可扩展的标准的并且被广泛支持的存储加密密钥的存储格式。PKCS12 keystore通过存储私钥可信公钥证书以及密钥来提高保密性。这个特性还提供了与例如Mozilla, IE, OpenSSL以及其他支持PKCS12的系统交互的机会。

  SunJEES 提供了一个PKCS12的完整实现。 java.security.KeyStore可以用来读写PKCS12文件。

  详情见[Key Management in Java Platform, Standard Edition Security Developer's Guide.](https://docs.oracle.com/javase/9/security/java-cryptography-architecture-jca-reference-guide.htm#JSSEC-GUID-C730728A-DB4B-488F-8171-34FC04BD0FF1)

  可以用密钥和证书管理组件keytool来创建 PKCS12 keystores.

  详情见[Creating a Keystore in Java Platform, Standard Edition Security Developer's Guide ](https://docs.oracle.com/javase/9/security/java-secure-socket-extension-jsse-reference-guide.htm#JSSEC-GUID-3D26386B-BC7A-41BB-AC70-80E6CD147D6F) 和 [keytool in Java Platform, Standard Edition Tools Reference.](https://docs.oracle.com/javase/9/tools/keytool.htm#JSWOR-GUID-5990A2E4-78E3-47B7-AE75-6D1826259549)

###  SHA-3 Hash Algorithms

  支持了SHA-3加密哈希算法。
  java.security.MessageDigest 还支持了这些额外的标准算法：SHA3-224, SHA3-256, SHA3-384 和 SHA3-512

  下面的提供商支持SHA-3算法：

    1. SUN provider: SHA3-224, SHA3-256, SHA3-384, and SHA3-512

    2. OracleUcrypto provider: SHA-3 digests supported by Solaris 12.0


## What’s New for Deployment in JDK 9

### Deprecate the Java Plug-in

  在Oracle JDK9中反对Java Plug-in 和相关的applet技术的应用。 虽然这些技术在 JDK9中依旧是可以使用的， 但是这些技术在未来的版本中会被考虑删除。

  网页中嵌入的Applet和JavaFX应用需要Java Plug-in来运行， 所以请考虑用 Java Web Start或者其他技术重写这些应用。

  详情见 [Migrating Java Applets to Java Web Start and JNLP](https://docs.oracle.com/javase/9/deploy/migrating-java-applets-jnlp.htm#JSDPG-GUID-1F95EBB3-D5CB-434A-B069-2261900738F5) 和 [Self-Contained Application Packaging in Java Platform, Standard Edition Deployment Guide.](https://docs.oracle.com/javase/9/deploy/self-contained-application-packaging.htm#JSDPG583)

### Enhanced Java Control Panel

  提升了Java Control Panel中options的分类和展示。信息可以被更方便的定位， 提供了搜索框， 并且 Modal Dialog Boxes 被废弃掉了。 注意一些option的位置和先前版本的 Java Control Panel中不一致。

  详情见[ Java Control Panel in Java Platform, Standard Edition Deployment Guide.](https://docs.oracle.com/javase/9/deploy/java-control-panel.htm#JSDPG748)

### Modular Java Application Packaging

  把Jigsaw项目中的特性整合进了Java Packager, 包括模块化的意识和自定义运行时的创建。

  可以利用jlink工具来创建更小的 package

  只能创建只使用了JDK9运行时的应用。 不用用来打包使用早期版本的JRE的应用。

  详情见[Customization of the JRE](https://docs.oracle.com/javase/9/deploy/self-contained-application-packaging.htm#JSDPG-GUID-27A46A14-3499-4198-82E9-DA4E23AF5F32)和[Packaging for Modular Applications in Java Platform, Standard Edition Deployment Guide](https://docs.oracle.com/javase/9/deploy/self-contained-application-packaging.htm#JSDPG-GUID-E2DE420D-EF33-452A-9691-2B12FA93DD82)

### Deprecate the Applet API

  移除了Applet API. 这玩意已经过时了。

## What’s New for the Java Language in JDK 9

  Java9中语法的改变很小。只有以下几点改变：

  1. 允许在私有实例方法上使用@SafeVargs注解

  2. 允许事实上的final变量在try-with-resource语句中作为resource来使用, 这种情况下不再需要指定新的资源

  3. 允许在菱形括号内使用匿名类， 如果有关的参数类型可以被标识出来的话

  4. 单独的下划线不再可以作为一个标识符

  5. 添加了接口私有方法的支持

  详情见 (Java Language Changes for Java SE 9 )[https://docs.oracle.com/javase/9/language/toc.htm#JSLAN-GUID-16A5183A-DC0D-4A96-B9D8-AAC9671222DD]

## What’s New for Javadoc in JDK 9

### Simplified Doclet API

  用一种新的简化的API替代了旧的Doclet API. 标准doclet已经用新的Doclet API重写了.

### HTML5 Javadoc

  支持HTML5格式的输出。 为了确保和HTML5兼容， 请确保文档注释中的内容和H5兼容.

### Javadoc Search

  提供了一个搜索框来搜索API文档。 可以使用这个搜索框去查找程序元素， 关键词以及文档中的短语。

### Module System

  支持模块声明中的文档注释。 添加了新的命令行选项，用于配置哪些模块需要被纳入文档以及这些模块的总结。

## What’s New for the JVM in JDK 9

### Compiler Control

  通过编译指令选项提供了一种控制JVM编译的方式。 控制的级别有 runtime-managable 和 method-specific. Compiler Control 替代了并且向后兼容CompileCommand.(CompileCommand是之前的选项， 这个选项向后兼容Compiler Control)

  详情见[Compiler Control in Java Platform, Standard Edition Java Virtual Machine Guide.](Compiler Control in Java Platform, Standard Edition Java Virtual Machine Guide.)

### Segment Code Cache

  将代码缓存划分到不同的独立的段中。 每个段都包含了一个特定类型的已编译好的代码， 从而提升了性能， 为未来的扩展提供可能。

  详情见[java in Java Platform, Standard Edition Tools Reference.](https://docs.oracle.com/javase/9/tools/java.htm#JSWOR624)

### Dynamic Linking of Language-Defined Object Models

  
