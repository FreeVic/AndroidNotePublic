# Android 代码混淆

[TOC]

-injars  androidtest.jar【jar包所在地址】 
-outjars  out【输出地址】
-libraryjars    'D:\Android-sdk-windows\platforms\android-9\android.jar' 【引用的库的jar，用于解析injars所指定的jar类】

-optimizationpasses 5
-dontusemixedcaseclassnames 【混淆时不会产生形形色色的类名 】
-dontskipnonpubliclibraryclasses 【指定不去忽略非公共的库类。 】
-dontpreverify 【不预校验】
-verbose
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/* 【优化】
-keep public class * extends android.app.Activity　　【不进行混淆保持原样】
-keep public class * extends android.app.Application
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep public class * extends android.content.ContentProvider
-keep public class * extends android.app.backup.BackupAgentHelper
-keep public class * extends android.preference.Preference
-keep public class com.android.vending.licensing.ILicensingService
-keep public abstract interface com.asqw.android.Listener{
public protected <methods>;  【所有方法不进行混淆】
}
-keep public class com.asqw.android{
public void Start(java.lang.String); 【对该方法不进行混淆】
}
-keepclasseswithmembernames class * { 【保护指定的类和类的成员的名称，如果所有指定的类成员出席（在压缩步骤之后）】
native <methods>;
}
-keepclasseswithmembers class * { 【保护指定的类和类的成员，但条件是所有指定的类和类成员是要存在。】
public <init>(android.content.Context, android.util.AttributeSet);
}
-keepclasseswithmembers class * {
public <init>(android.content.Context, android.util.AttributeSet, int);
}
-keepclassmembers class * extends android.app.Activity {【保护指定类的成员，如果此类受到保护他们会保护的更好 】
public void *(android.view.View);
}
-keepclassmembers enum * {
public static **[] values();
public static ** valueOf(java.lang.String);
}
-keep class * implements android.os.Parcelable {【保护指定的类文件和类的成员】
public static final android.os.Parcelable$Creator *;
}

## 注意

1. 保留注解,否则混淆把注解搞丢了,很多功能都将失效,例如JS/java互相调用的代码需要保留JavaScriptInterface注解

   ```
   -keepattributes Annotation
   ```

2. 内部类不能用.进行连接,需要使用$

3. 混淆与multiDex问题

   ```
   Error:Execution failed for task ':app:shrinkDebugMultiDexComponents'.
   > java.io.IOException: The output jar [C:\worksapce\Eclite5006\AndroidEcLite43\app\build\intermediates\multi-dex\debug\componentClasses.jar] must be specified after an input jar, or it will be empty.
   ```

   解决：

   1. 更新sdk相关tool

   2. 修改混淆配置忽略相关jar

      ```
      -dontwarn com.nhaarman.**
      ```

## 参考

[android 混淆文件proguard.cfg详解](http://blog.csdn.net/laoyao_moyan/article/details/7353768)