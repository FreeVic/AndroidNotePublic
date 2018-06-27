# Android实践

- scaleType中只有fitXY会改变图片的比例,其他的均会保持图片比例

- 判断View是否显示 `View.isShown()`

- TextView提示用户输入有误 `TextView.setError()`

  - **SparseArray** 替换 **HashMap**
  - 采用压缩的方式表示洗漱数组的数据，节约内存空间
  - 存取数据采用的是二分查找法
  - 主要有`SparseLongArray SparseIntArray SparseBooleanArray SparseArray`
  - 避免了自动装箱过程
  - http://www.trinea.cn/android/performance/

- Adb端口被占用

  - 查看5037端口的进程 netstat -ano|findstr “5037”
  - ![](http://7xqdxj.com1.z0.glb.clouddn.com/04181.png)
  - 打开任务管理器杀掉其他进程即可

- Adb devices 设备offline

  - 浪费大半天的时间，第二天正要重装系统时尝试着看adb版本是不是有问题，果然，还真是adb的，可能是那些刷机软件写入到系统环境下的adb属于自己修改的版本，我android sdk配置的环境变量并没有生效
  - 设备未认证PC调试（没有保存adbkey.pub）
  - Adb问题，尝试adb kill-server
  - 环境问题 重启设备或电脑
  - Adb版本问题（旧或其他修改的版本），尝试正确版本

- 修改手机hosts文件

  - 替换掉/system/etc/hosts即可
  - 获取权限mount -o remount,rw /system
  - 覆盖文件 adb push hosts /system/etc/hosts

- 查看apk包信息

  - 命令行输入 aapt dump badging *.apk即可

  - 然后问题来了，TM在sdk里没看到有appt.exe啊。其实这个命令是在sdk的build_tools文件夹下的~再说了，这种问题度娘肯定知道。太多参数了不做解释，要用到自己再google去吧，这里仅贴一下

  - ```
    aapt dump WHAT file.{apk} [asset [asset ...]]

    badging          Print the label and icon for the app declared in APK.

    permissions      Print the permissions from the APK.

    resources        Print the resource table from the APK.

    configurations   Print the configurations in the APK.

    xmltree          Print the compiled xmls in the given assets.

    xmlstrings       Print the strings of the given compiled xml assets.
    ```

- 截图

  - 使用系统目录下自带的命令 /system/bin/screencap
  - 截图 adb shell /system/bin/screencap -p /sdcard/screenshot.png

- 清缓存

  - 系统目录下 /system/bin/pm
  - adb shell /system/bin/pm clear packageName

- 高德地图获得当前缩放等级

  - mAMap.getCameraPosition().zoom < 19

- plugin is too old  set ANDROID_DAILY_OVERRIDE environment

  - 根目录gradle版本太新

- gradle.properties配置

  - ```
    org.gradle.daemon=true
    org.gradle.parallel=true
    org.gradle.jvmargs=-Xmx4000M
    ```

- Android studio 书签描述乱码

  - 设置字体Microsoft yahei等

- Kotlin配置findViewById say goodbye

  - 添加插件`apply plugin: 'kotlin-android-extensions'`
  - 直接在代码中使用id进行逻辑处理（ide会自动导入layout，例如`kotlinx.android.synthetic.main.fragment_mine`
  - ps：fragment需要在view初始化之后进行调用，即onViewCreated方法之后处理

- TextView设置首行缩进 添加转义字符`\u3000`

- 设置app默认字体颜色和大小

  - ```
    定义主题，并在AndroidManifest文件中引用
    <style name="AppTheme" parent="@android:style/Theme.Light.NoTitleBar">
        <item name="android:textSize">42dp</item>
        <item name="android:textColor">#FF0000</item>
    </style>
    ```



- 设置TextView点击时变换背景和字体颜色

  - 创建selector设置不同的背景对`state_-press` 属性区分，将selector设置给TextView作为背景
  - 创建selector设置不同的颜色，`state_-press` 属性区分，将selector设置给TextView作为字体颜色


- 当有2个子布局时点击事件由各子布局处理，当有1个子布局时，点击事件交给父布局处理

  - 分情况设置子布局的clickable属性（设置点击事件或者enable并不管用╮(╯▽╰)╭）

- margin 设置负值的作用
  - 控件的margin属性用来控制间距，如果是负值的话，就会进行重叠
  - 例如WebView界面，如果不想让用户看到底部的界面，那么就可以设置WebView的margin为负值

- 复写返回键，将应用切换到后台，而不是退出

  - ```
    //方式一：将此任务转向后台,部分手机不生效
    moveTaskToBack(false);
    ```

  - ```
    //方式二：返回手机的主屏幕
    Intent intent = new Intent(Intent.ACTION_MAIN);
    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    intent.addCategory(Intent.CATEGORY_HOME);
    startActivity(intent);
    ```



- Kotlin重写属性的set方法，注意field的赋值

  - ```
    var name:String = ""
            set(value) {
                field = value
                SPMain.setString(ConstValue.KEY_NAME,value)
            }
    ```



- BuildConfig 添加属性值


  - 在 build.gradle 中 buildTypes 节点下的某个编译类型内添加

  - String 类型的值需要加上转义的双引号 `\"`，或者用单引号括起来

  - ```
    //会在BuildConfig这个类中生成一个变量，变量名为LOG_DEBUG，值为false
       buildConfigField("boolean", "LOG_DEBUG", "false")  
       buildConfigField 'String','API_SERVER_URL','"http://wuxiaolong.me/"'
    ```

### setResult 调用时机

B 回退到 A 时,调用顺序

```
B---onPause
A---onActivityResult
A---onRestart
A---onStart
A---onResume
B---onStop
B---onDestroy
```

但不保证B 的 onPause 一定在 A 的 onActivityResult 之后,所以最好不要放在生命周期里调用setResult




