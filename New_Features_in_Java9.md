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
