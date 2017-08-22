# Android实践

1. scaleType中只有fitXY会改变图片的比例,其他的均会保持图片比例

2. 判断View是否显示 `View.isShown()`

3. TextView提示用户输入有误 `TextView.setError()`

4. **SparseArray** 替换 **HashMap**
   1. 采用压缩的方式表示洗漱数组的数据，节约内存空间
   2. 存取数据采用的是二分查找法
   3. 主要有`SparseLongArray SparseIntArray SparseBooleanArray SparseArray`
   4. 避免了自动装箱过程

5. http://www.trinea.cn/android/performance/

6. Adb端口被占用

   1. 查看5037端口的进程 netstat -ano|findstr “5037”
   2. ![](img/04181.png)
   3. 打开任务管理器杀掉其他进程即可

7. Adb devices 设备offline

   1. 浪费大半天的时间，第二天正要重装系统时尝试着看adb版本是不是有问题，果然，还真是adb的，可能是那些刷机软件写入到系统环境下的adb属于自己修改的版本，我android sdk配置的环境变量并没有生效
      1. 设备未认证PC调试（没有保存adbkey.pub）
      2. Adb问题，尝试adb kill-server
      3. 环境问题 重启设备或电脑
      4. Adb版本问题（旧或其他修改的版本），尝试正确版本

8. 修改手机hosts文件

   1. 替换掉/system/etc/hosts即可
   2. 获取权限mount -o remount,rw /system
   3. 覆盖文件 adb push hosts /system/etc/hosts

9. 查看apk包信息

   1. 命令行输入 aapt dump badging *.apk即可

   2. 然后问题来了，TM在sdk里没看到有appt.exe啊。其实这个命令是在sdk的build_tools文件夹下的~再说了，这种问题度娘肯定知道。太多参数了不做解释，要用到自己再google去吧，这里仅贴一下

   3. ```
      aapt d[ump] [--values] WHAT file.{apk} [asset [asset ...]]
      badging          Print the label and icon for the app declared in APK.
      permissions      Print the permissions from the APK.
      resources        Print the resource table from the APK.
      configurations   Print the configurations in the APK.
      xmltree          Print the compiled xmls in the given assets.
      xmlstrings       Print the strings of the given compiled xml assets.
      ```

10. 截图

   1. 使用系统目录下自带的命令 /system/bin/screencap
   2. 截图 adb shell /system/bin/screencap -p /sdcard/screenshot.png

11. 清缓存

   1. 系统目录下 /system/bin/pm
   2. adb shell /system/bin/pm clear packageName