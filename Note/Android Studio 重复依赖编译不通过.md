Android Studio 重复依赖编译不通过
---------------------
Android项目中经常会出现两个依赖中使用到了相同的jar包导致编译不通过,比如:

    java.util.zip.ZipException: duplicate entry: com/nineoldandroids/animation/Animator$AnimatorListener.class

    com.android.ide.common.process.ProcessException: org.gradle.process.internal.ExecException: Process 'command 'H:\jdk1.8\bin\java.exe'' finished with non-zero exit value 2

在Studio中有六种依赖类型，可以在module setting中看到:

- Compile 是对所有的build type以及favlors都会参与编译并且打包到最终的apk文件中
- Provided 是对所有的build type以及favlors只在编译时使用，类似eclipse中的external-libs,只参与编译，不打包到最终apk
- APK 只会打包到apk文件中，而不参与编译，所以不能再代码中直接调用jar中的类或方法，否则在编译时会报错
- Test Compile 仅仅是针对单元测试代码的编译编译以及最终打包测试apk时有效，而对正常的debug或者release apk包不起作用
- Debug Compile 仅仅针对debug模式的编译和最终的debug apk打包
- Release Compile 仅仅针对Release 模式的编译和最终的Release apk打包

而重复依赖有下面两种情况:

###1. maven库依赖与本地jar冲突

去掉本地jar

使用Provided类型引用本地jar

maven库依赖exclude相应的jar

     compile ('com.facebook.fresco:fresco:0.9.0+'){
        exclude module: 'library'
    }
    compile ('com.baoyz.swipemenulistview:library:1.3.0'){
        exclude module: 'support-v4'
    }
  
 网上有很多类似这样的代码教你去掉重复引用的jar,但怎么知道本地用的jar名字是什么呢?
 

    查看项目用到的依赖:
    gradlew -q dependencies moduleName:dependencies --configuration compile
    结果:
    compile - Classpath for compiling the main sources.
	+--- com.android.support:appcompat-v7:23.0.0
	|    \--- com.android.support:support-v4:23.0.0 -> 23.0.1
	|         \--- com.android.support:support-annotations:23.0.1
	+--- com.squareup.okhttp:okhttp:2.5.0
	|    \--- com.squareup.okio:okio:1.6.0
	+--- com.squareup.okio:okio:1.6.0
	+--- in.srain.cube:ultra-ptr:1.0.11
	+--- com.android.support:recyclerview-v7:23.0.1
	|    +--- com.android.support:support-v4:23.0.1 (*)
	|    \--- com.android.support:support-annotations:23.0.1
	+--- jp.wasabeef:recyclerview-animators:1.3.0
	+--- org.jbundle.util.osgi.wrapped:org.jbundle.util.osgi.wrapped.org.apache.http.client:4.1.2
	+--- me.drakeet.labelview:labelview:0.7
	+--- org.simple:androideventbus:latest
	+--- io.reactivex:rxandroid:1.0.1	
	|    \--- io.reactivex:rxjava:1.0.13 -> 1.0.15
	+--- io.reactivex:rxjava:1.0.15
	+--- com.jakewharton.rxbinding:rxbinding:0.2.0
	|    +--- com.android.support:support-annotations:23.0.0 -> 23.0.1
	|    \--- io.reactivex:rxjava:1.0.14 -> 1.0.15
	+--- com.squareup.retrofit:retrofit:1.9.0
	|    \--- com.google.code.gson:gson:2.3.1
	+--- com.squareup.leakcanary:leakcanary-android:1.3
	|    \--- com.squareup.leakcanary:leakcanary-analyzer:1.3
	|         +--- com.squareup.haha:haha:1.1
	|         \--- com.squareup.leakcanary:leakcanary-watcher:1.3
	可以看到,每个依赖都由三段组成,groupId:artifactId:version,即 分组:名字:版本,所以名字就直接拿中间那段就对了
###2. 本地jar包A和B含有相同的class(包名一样的才会有冲突)

通过代码混淆,让类的名字变掉(比较简单,就不贴了),暂时还没找到其他更好的替代方案
