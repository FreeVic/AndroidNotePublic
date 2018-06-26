# WebServer

- **连接服务器** `ssh username@hostname` 根据提示输入yes

  - ```
    ssh -p 端口号 username@hostname
    默认端口号是22，如果没有修改，可以不用加-p参数
    ```

- **断开连接**

  - ```
    control+D
    或者 logout
    ```

- **传输文件**

  - ```
    scp username@hostname:/usr/tmp/1.json /Users/zhangshengli/Desktop/1.json 下载文件
    scp /Users/zhangshengli/Desktop/1.json username@hostname:/usr/tmp/1.json 上传文件
    scp -r 用于传输整个目录
    ```

- **压缩解压文件**

  - **tar**
    - **解压**文件 `tar -zxvf fileName`
    - **压缩**文件 `tar -czvf srcFile dstFile`
  - **gz**
    - **解压**文件 `gzip -d fileName`
    - **压缩**文件 `gzip srcFile dstFile`
  - **zip**
    - **解压**文件 `unzip fileName`
    - **压缩**文件 `zip srcFile dstFile`
  - **tar**
    - 解压文件 `tar -xf fileName`
    - 压缩文件 `tar -cvf *.jpg`

- 防火墙(centos 7.0以上版本)

  - **查看防火墙状态** `firewall-cmd --state`
  - **开启** `systemctl start firewalld.service`
  - **关闭** `systemctl stop firewalld.service`
    - **禁止开机启动** `systemctl disable firewalld.service`
  - **重启** `firewall-cmd —reload`
  - **查看已经开启的端口**
    - `firewall-cmd --list-ports`
  - ​
  - **开放指定端口** 
    - `firewall-cmd --zone=public --add-port=8080/tcp --permanent`
  - **端口重定向**
    - 80端口重定向到8080
      - 修改tomcat bin 目录权限
        - chmod -R 777 bin
        - 注意安全组规则也需要添加80端口
      - 修改端口
        - `iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080`

- **tomcat 入口修改**

  - tomcat/conf/server.xml 文件中 Host 节点添加
    - `<Context path="" doBase="demo" debug="0" reloadable="true" />`
    - 其中 demo 是在 webapps 目录下

- **查看端口占用**

  - `netstat -tlnp`

- **查看进程**

  - `ps -ef | grep tomcat`

- **杀掉进程**

  - `kill -s 9 pid`

- 安装 **tar** 命令 `yum -y install tar`

- ​