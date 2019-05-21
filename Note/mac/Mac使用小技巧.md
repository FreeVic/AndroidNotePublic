# Mac使用小技巧

[TOC]

## 软件

### 系统支持

- Mounty：NTFS硬盘格式支持
- The Unarchiver：压缩解压
- PaintBrush：画图板
- MplayerX：视频加速播放／多格式支持
- Dr.Cleaner：清理
- Office：文档处理
- Sublime 2：文本编辑
- Typora:md文档书写
- shiftIt 多窗口管理
- LICEcap 录制Gif
- N多手机助手 连接管理Android手机
- go2shell 命令行支持
- ————
- Near Lock：蓝牙解锁
- 坚果云：资料多端同步
- Iterm2：命令行终端
- QQ         
- 微信
- 网易云音乐
- Cheatsheet：查看页面的快捷键
- PdfNut：pdf阅读器

### 开发

- Android Studio
- Eclipse
- Xcode
- SourceTree：版本管理
- SmartSvn
- Sqlite Professional：db文件查看
- DiffMerge：文件对比
- Dash：API查看
- Charles：抓包
- Axure rp：流程图／交互图
- chrome 插件
  - Postman 调试接口
  - Gliffy Diagrams 流程图
  - Vysor 连接手机
  - Json Editor
  - TimeDoser
  - LastPass 密码管理
  - 猫抓
  - crx Mouse 鼠标手势
  - 空格之神
  - 为知笔记
  - Octotree github目录结构

## 技巧

### Finder

1. whitespace 即时预览

2. enter 重命名

3. cmd+z 撤销(几乎可以撤销一切)

4. 在Finder中添加快捷图标：在应用程序的文件夹下按住cmd拖拽图标到Finder的窗口，移除同样是按住cmd键拖拽

5. 选中要整理的文件cmd+control+n 新建文件夹,或者右键选择归类文件

6. 保存文件时,切到桌面 cmd+d

7. 终端中用Finder打开当前目录 open .

8. 快捷推出U盘 cmd+e

9. spotlight中打开文件位置 cmd+enter

10. 显示简介 cmd+i

11. 打开文件 cmd+o

12. cmd + shift + A/U/D/H/C，分别进入Finder的应用程序、实用工具、桌面、个人和电脑文件夹

13. 移动文件 复制文件后cmd+option+v

14. 显示隐藏文件
```
defaults write com.apple.finder AppleShowAllFiles -bool true;killall Finder
```
15. finder标题栏显示完整路径
```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
```
### 系统

1. 全屏 cmd＋control+F

2. 回到桌面 F11

3. 强制退出 cmd+option+esc

4. 息屏 shift＋control＋电源键

5. 修改mac hosts: finder->/private/etc/hosts

6. 关闭Dashboard：defaults write com.apple.dashboard mcx-disabled -boolean YESkillall Dock（在系统设置里mission control里也可以修改关闭掉）

7. 删除设置中的插件图标 option+右键 就会弹出删除选项

8. 显示隐藏Dock cmd+option+d

9. 切换不同APP cmd+tab/~(上/下),切换相同APP不同tab cmd+~,可以cmd+q退出程序

10. 开机启动项 设置-用户与群组-登录项

11. 系统中英文输入法切换 caps lock

12. 移动窗口到另外一个屏幕 (shiftIt) control+alt+cmd+N

13. 打开go2shell偏好

   ```
   open -a Go2Shell --args config
   ```

14. 创建可执行脚本
```
#! /bin/bash
echo abc
终端中修改权限 chmod +x 文件.sh
文件拖拽到命令行进行执行/或双击执行
可以在脚本中切换到当前目录，防止找不到文件目录
如果要查看报错信息，可以使用 sleep time 来延时一下

```
15. 查看端口占用及杀进程

    1. `sudo lsof -i :port`
    2. `sudo kill -9 pid`

16. 环境变量

    1. 修改  **/etc/profile**  全局所有用户生效  修改 **.bash_profile** 当前用户生效

       1. ```
          文件末尾添加剂
          export JAVA_HOME=/usr/share/jdk1.6.0_14 
          export PATH=$JAVA_HOME/bin:$PATH 
          zsh的话需要配置一下每次都要 source 当前的环境变量 source ./.bash_profile
          ```

17. 修改Launchpad图标行列
```
列 defaults write com.apple.dock springboard-columns -int 8;killall Dock;
行 defaults write com.apple.dock springboard-rows -int 8;killall Dock;
恢复默认 defaults write com.apple.dock springboard-columns -int Default;killall Dock;
```

### 手势

1. 打开launchpad 五指合并
2. 切换桌面 三指左右滑动
3. 切换程序 三指上滑／cmd＋tab
4. 翻译,三指轻点
   ![](http://7xqdxj.com1.z0.glb.clouddn.com/2016%E5%B9%B408%E6%9C%8816%E6%97%A5_0.jpg)

## Sublime Text

- 安装 [Package Control](https://packagecontrol.io/installation) ，进行插件管理
- GBK Support GBK 编码支持
- SideBarEnhancements 侧边栏文件夹支持
- [Spacegray](https://github.com/kkga/spacegray) 主题
- 全局搜索
  - CMD+Shift+F,默认搜索当前打开的文件和文件夹
- 文件搜索
  - CMD+F
  - 下一个 CMD+G
  - 上一个 CMD+Shift+G
- 查找文件
  - CMD+P
- 查找方法
  - CMD+R

## Terminal

- 查看当前路径 `pwd` (print working directory)

- cd

  - `cd 不加参数` 或 `cd ~` 进入 root
  - `cd -` 返回到上一个访问的目录
  - `cd ..` 上级目录

- ls

  - `ls -alt`  t 按修改时间排序 a 显示隐藏的文件和文件夹 l 格式化显示

- mkdir

  - 创建多个文件夹 `mkdir dir1 dir2`
  - 创建多级文件夹 `mkdir -p dir1/dir2/dir3`
  - 创建一个目录,然后直接进入这个目录?
    - `mkdir -p dir1/dir2/dir3 && $_` 
      - `&&` 用于执行多个命令 `$_` 前面命令的最后一个参数
  - 如何操作当前路径 `-` 开头的目录或文件
    - 可以 `cd ./-目录`

- cp

  - `cp sourceFile targetFile 或者 targetDir` 拷贝文件到指定位置
  - `cp sourceFile1 souceFile2 targetDir` 拷贝多个文件到指定位置
  - `cp * targetDir` 通配符拷贝文件到指定位置
  - `cp -R sourceDir targetDir` 递归拷贝文件夹到指定位置

- mv

  - 与 cp 类似

- rm

  - 删除文件 `rm file1`

    - `rm dir1/*` 通配符删除所有文件

  - 删除文件夹 `rm -r dir1`

  - 正则删除指定文件

    ```
    删除带 齐齐哈尔 的文件
    目录结构如下
    ├── 北京->齐齐哈尔.png
    └── 齐齐哈尔->北京.png
    ```

    

    - 方式1 

      ```
      rm -r `ls | grep "\S*齐齐\S*"`
      ```

    - 方式2

      ```
      rm -r 正则如何写?
      ```

      

- 添加 Dock 分隔符

  - ```
    defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'; killall Dock
    ```

- `>` 重定向 redirection

  - `echo 'hello' > hello.txt` 把左边的输出重定向到右边
  - `echo 'hello >> hello.txt'` 把左边输出追加到右边

- `|` 管道 pipe

  - `cat lakes.txt | sort > sorted_lakes.txt`  cat输出排序然后写到文件

- sort 排序

  - 把标准输入根据字母排序

- uniq 去重

  - 把标准输入相邻行进行去重

- grep 字符串查找(大小写敏感)

  - `grep -i Mount mountains.txt` `-i` 大小写不敏感
  - `grep -R Arctic /home/ccuser/workspace/geography` 递归搜索目录下的文件,输出匹配的文件名和结果行
  - `grep -Rl Arctic /home/ccuser/workspace/geography` l 只输出文件名

- wc (wordcount) 计数

  - 输出 c字节数 w字数 l行数

- sed 替换

  - `sed 's/snow/rain/g' forests.txt` 
    - s 表示替换
    - snow 要查找的字符串
    - rain 要替换的字符串(后面需要加一个/,否则报错)
    - g 全局,去掉g则只会替换每行第一个匹配的结果

- head/tail 显示头部/尾部多少行(默认10行)

  - `ls -l | head -15` 显示前15行

- tree

  - 使用 brew 安装 `brew install tree`

  - `tree -d ` 只显示文件夹

  - 解决中文目录乱码 `tree -N`

  - `-I` 过滤匹配的文件

    - 通配符只允许 * ? [] ^ |

    - ```
      Valid wildcard operators are`*' (any zero or more characters), `?' (any  single  character),`[...]'  (any single character listed between brackets (optional- (dash) for character  range  may  be  used:  ex:  [A-Z]),  and`[^...]'  (any  single character not listed in brackets) and `|'separates alternate patterns.
      ```

      

  - `-P` 只显示匹配的文件

  - `-L` 层级

- alias 别名

- history 命令历史

- ll

- export 设置环境变量(environment variables )

  - 可以在终端或编程中使用的变量 例如 `export USER='FreeVic'`
  - 使用 `echo $USER`
  - 默认变量 
    - PS1="$"  终端光标前默认显示的字符
    - HOME 根目录 
    - PATH 用冒号隔开的一系列目录(包含终端可执行的脚本)

- env 输出当前的环境变量

- date 当前时间

- less

  - 类似于 cat,更适合打开大文件,q 退出
  - 环境变量中 `LESS` 控制 less 的修饰符,比如 `LESS="-N"` 给 less 增加行号

## shell 脚本

- 文件开头 `#!bin/bash`

- 声明变量 `phrase="Hello"` =左右不能有空格

- 修改脚本可执行权限 `chmod +x fileName`

- 条件控制

  ```shell
  if [ $index -lt 5 ]
  then
    echo $index
  else
    echo 5
  fi
  ```

- 运算符

  - -lt 表示 less than
  - -eq 表示 equal
  - -ne 表示 not equal
  - -le 表示 less than or equal
  - -ge 表示 greater than or equal
  - -gt 表示 greater than
  - -z 表示 is null
  - 字符串比较最好把变量放在" " 中,防止null 或者空格
    - == equal
    - != not equal

- 循环 for while until

  ```shell
  for word in $paragraph
  do
    echo $word
  done
  
  while [ $index -lt 5 ]
  do
    echo $index
    index=$((index + 1))
  done
  
  until [ $index -eq 5 ]
  do
    echo $index
    index=$((index + 1))
  done
  
  
  自增语法??
  greeting_occasion=$((greeting_occasion + 1))
  ```

- 输入输出

  - 读取用户输入

    ```shell
    echo "Guess a number"
    read number
    echo "You guessed $number"
    ```

  - 读取执行时所带参数

    ```shell
    # $1 $2 $3.. 读取参数,角标从1开始
    # 如果个数模糊不清,也可以使用 $@ 遍历参数
    for param in $@
    do
    	echo param
    done
    ```

  - 读取路径下的文件

    ```shell
    # 声明一个变量,使用* 读取路径下的所有文件
    files=/some/directory/*
    ```

- 将脚本做成别名

  ```shell
  alias greet3="./script.sh"
  ```

- 其他代码

  ```shell
  #! /bin/bash
  if [ $@ -z ]
  then
  	echo "没有输入参数"
  else
  	echo "输入参数为："
  	for param in $@
  	do
  		echo "$param"
  	done
  fi
  
  # files= ./*
  # if [ $files -z ]
  # then
  # 	echo "目录为空"
  # else
  # 	for f in $files
  # 	do
  # 		echo "$f"
  # 	done
  # fi
  
  # 接收用户输入
  echo "请输入一个单词"
  read number
  echo $number
  
  # 当前目录，需要./ 
  firstLine=$(head -1 ./t.txt)
  # 切割字符串
  read -a splitefirstline <<< $firstLine
  version=${splitefirstline[1]}
  echo "version $version"
  ```

  





## 问题

不能更改“somefile”中的一个或多个项目,因为它们正在使用中 (外接移动硬盘时,无法拷贝文件到 Mac)

```
xattr -d com.apple.FinderInfo filePath
```

**破解 Vysor**

打开文件 `/Users/zhangshengli/Library/Application Support/Google/Chrome/Profile 1/Extensions/gidgenkbbabolejbgbpnhbimgjbffefm/2.0.9_0/uglify.js` 

把`_il:Te.a()` 替换为 `_il:true`

#### “Too many open files” exception (MacOS)

```
ulimit -S -n 2048
```

