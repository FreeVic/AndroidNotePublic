Android Studio 重复依赖编译不通过
---------------------
Android项目中经常会出现两个依赖中使用到了相同的jar包导致编译不通过,比如:

    java.util.zip.ZipException: duplicate entry: com/nineoldandroids/animation/Animator$AnimatorListener.class
而在Studio中有五种依赖类型，可以在module setting中看到:

- Compile 总会被编译
- Provided	只在编译时用到，不会打到APK包中
- APK 编译时不会用到，只会打到APK包当中
- Test Compile 
- Debug Compile
- Release Compile

重复依赖则存在以下几种情况：

1. 两个依赖引用了同一个jar
2. 两个依赖引用了同一个class
3. 一个引用的jar，一个导入的.jar文件，但是都用到了同一个jar
