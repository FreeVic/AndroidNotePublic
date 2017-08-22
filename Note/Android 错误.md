

# Android 错误

[TOC]

## 混淆打包

```
Error:Execution failed for task ':app:shrinkDebugMultiDexComponents'.
> java.io.IOException: The output jar [C:\worksapce\Eclite5006\AndroidEcLite43\app\build\intermediates\multi-dex\debug\componentClasses.jar] must be specified after an input jar, or it will be empty.
```

解决：

1. 更新sdk相关tool

2. 修改progard文件忽略相关jar

   ```
   -dontwarn com.**
   ```

3. 修改progard中-dontpreverify为-dontoptimize


## 找不到资源

```
C:\worksapce\Eclite5006\AndroidEcLite43\app\build\intermediates\exploded-aar\com.android.support\appcompat-v7\24.2.0\res\values-ldltr-v21\values-ldltr-v21.xml
Error:(2) Error retrieving parent for item: No resource found that matches the given name 'android:TextAppearance.Material.Widget.Button.Inverse'.
```

这个错误一般都是values-21文件夹下大量资源找不到

解决：

1. 更新sdk相关support包

2. 修改build.gradle把编译版本和目标版本设置一致，然后以这样的方式引入support v7包

   ```
   正确
   compile 'com.android.support:appcompat-v7:20.+'
   错误
   compile 'com.android.support:appcompat-v7:24.0.2'
   ```

## Transform API

```
Error:Access to the dex task is now impossible, starting with 1.4.0
1.4.0 introduces a new Transform API allowing manipulation of the .class files.
See more information: http://tools.android.com/tech-docs/new-build-system/transform-api
```

旧项目升级gradle插件版本去使用AS 2.0之后新特性Instant Run 出现的错误

解决：

## Gradle版本

```
Error:Gradle version 2.2 is required. Current version is 2.10. If using the gradle wrapper, try editing the distributionUrl in C:\worksapce\tt\android\gradle\wrapper\gradle-wrapper.properties to gradle-2.2-all.zip.
Please fix the project's Gradle settings.
```

Gradle版本不对

解决：

1. 修改wrapper下gradle.properties里gradle版本

## Instant Run

```
 Unsupported method: AndroidProject.getPluginGeneration().
          The version of Gradle you connect to does not support that method.
          To resolve the problem you can change/upgrade the target version of Gradle you connect to.
          Alternatively, you can ignore this exception and read other information from the
```

Gradle版本不支持Instant Run 设置里勾选掉Instant Run

## 找不到类

```
Process: com.eclite.activity, PID: 11973
                                                   java.lang.NoClassDefFoundError: com.eclite.activity.BaseActivity
                                                       at com.eclite.app.EcLiteApp.onCreate(EcLiteApp.java:357)
                                                       at android.app.Instrumentation.callApplicationOnCreate(Instrumentation.java:1007)
```

解决:

可能是multidex在app启动时没有成功加载,导致类找不到.查看application代码,是否存在耗时操作

## instant run 无效

```java
instant run在multidex为true时无效
```

解决:

将minsdk修改为21之后/使用API版本在21之后的测试机调试应用

## UNsupport class file version 52.0

```
52 版本是 java 8 编译出来的 class 文件版本号。

怀疑是你使用了某些库， 这些库是使用 java 8 编译的，而你机器上是 java 7 或以下版本。

你可以升级你机器上的 java 版本， 或者找下这个库的老版本.

另外可以这样改下：
将 buildToolsVersion "24.0.0" 改为 buildToolsVersion "23.0.3"
```