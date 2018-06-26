# Mac 外接高清显示器

解决Mac 外接2K，4K 显示器字体很小的问题

- 开启 HiDPI

  - ```
    打开终端，输入
    sudo defaults write /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled -bool true
    然后输入管理员密码，回车确认
    ```

- 获取显示的2个 ID

  - ```
    终端输入
    ioreg -l | grep "DisplayVendorID"
    ioreg -l | grep "DisplayProductID"
    如果 ID 有2个话，第二个就是你的外接显示器的·
    ```

  - 将两个数字转换成 十六进制 [转换](http://blog.zhouding.cn/staticfiles/jinzhi.swf)

  - ```
    "DisplayVendorID" = 1507      5E3
    "DisplayProductID" = 9360     2490
    ```

- 任意位置生成文件

  - 新建文件夹命名  `DisplayVendorID-XXX`  XXX是DisplayVendorID 十六进制小写

  - 在文件夹内建一个空文件 命名 `DisplayProductID-XXX` XXX是DisplayProductID的十六进制小写

  - ```
    所以最终得到
    DisplayVendorID-5e3
    DisplayProductID-2490
    ```

  - 在线生成配置文件,将生成的内容粘贴到上面的空文件中 [地址](https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/)

- 把DisplayVendorID-XXXX文件夹拷贝到

  - ```
    /System/Library/Displays/Contents/Resources/Overrides/
    (10.10及以下是 /System/Library/Displays/Overrides/ )
    完成后重启电脑(注销不可以)
    ```

- 关于 sip 问题

  - 重启系统 command+R 进入 recoverary 模式，终端输入`csrutil disable/enable`
  - 安全起见，用完以后再关闭

- 下载 RDM 方便切换分辨率 [地址](http://avi.alkalay.net/software/RDM/)

## 参考

[Mac 开启 HiDPI](https://bbs.feng.com/forum.php?mod=viewthread&tid=11104214)