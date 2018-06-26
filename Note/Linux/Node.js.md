# Node.js

[TOC]

### NPM 使用

- node js 已经集成了 npm, 直接使用,**查看版本号** `npm -v`

- **升级** `sudo npm install npm -g`

- **安装模块** `npm install <module-name>` ,加 `-g` 参数进行全局安装

  - 本地安装会在当前目录下 `node_modules`文件夹下
  - 全局安装会在 usr/local 下

- **查看安装信息** `npm list -g`

- **查看模块版本号** `npm list grunt`

- **使用package.json**

  - ```
    name 包名
    version 包版本号
    description 包描述
    homepage 包官网 url
    author 包作者
    contributors 包其他贡献者
    dependencies 依赖包列表
    repository 包代码存放地方的类型,可以是 git 或 svn
    main 指定了程序的主入口文件,默认值是根目录下的 index.js
    keywords 关键字
    ```

- **卸载模块** `npm uninstall express`

- **更新模块** `npm update express`

- **搜索模块** `npm search express`

- **创建模块** `npm init`

- **配置淘宝 npm 镜像**

  - `npm install -g npm --registry=https://registry.npm.taobao.org`
  - 接下来可以使用 `cnpm install [module-name]` 来安装模块

