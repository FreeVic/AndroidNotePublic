# React Native

[TOC]

## 环境搭建

- 安装 [python 2](https://www.python.org/downloads/)

- 安装 [node.js](http://nodejs.cn/) 安装是否成功查看 `npm -v`

- 安装 [yarn](https://yarnpkg.com/zh-Hans/) (需要梯子)

  - 设置 npm 镜像服务器

  - ```
    yarn config set registry https://registry.npm.taobao.org --global
    yarn config set disturl https://npm.taobao.org/dist --global
    ```

- 安装React Native命令行工具（react-native-cli）

  - ```
    npm install -g yarn react-native-cli
    ```

- 添加 Android sdk 环境变量 `ANDROID_HOME`

  - build tool 需要版本 `23.0.1`

- 手动启动包管理器(异常时) `npm start`

- 初始化第一个项目`react-native init projectName`

  - 是在当前的文件夹创建，可以先切换到指定的目录再执行

- 运行 APP `react-native run-android`

## 执行官方 examples

## 问题

- **unable to load script from assets ‘index.android.js’**

  - 新版本将index.android.js和index.ios.js合并为App.js了

  - 创建assets目录，并执行下面的代码（ps：每次修改都需要执行，很无语）

  - ```
    react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res
    ```

- com.android.ddmlib.InstallException: Failed to install all 

  - 部分5.0系统问题，如果是小米，开发者选项--关闭 MIUI 优化