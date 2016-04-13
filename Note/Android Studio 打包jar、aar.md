Android Studio打包jar、aar及其引用
---------------------------
项目有module app，需要引用module b
1. 新建 lib module b
Studio中新建module，File--New Module--Android Library
2. 导出jar、aar
生成的lib包路径：
jar：build\intermediates\bundles\debug\classes.jar
aar：build\outputs\aar\b-release.aar
3. 引用
修改项目（不是module app）的build.gradle

	    allprojects {
	    repositories {
	        jcenter()
	        flatDir{
	            dirs 'libs'
	        }
	    }
libs用的是默认的相对路径，如果你的libs是与res目录平级的（eclipse项目），那么就是../app/src/main/libs,即相对于当前build.gradle的路径
修改module app的build.gradle

    dependencies {
    //    jar lib
    compile files('libs/classes.jar')
    //     aar lib
    compile(name: 'b', ext: 'aar')
    }
同样，libs是默认的相对路径，如果是eclipse项目则是src/main/libs/classes.jar
4. 参考
[如何在Android Studio添加本地aar包引用](http://jingyan.baidu.com/article/2a13832890d08f074a134ff0.html)
