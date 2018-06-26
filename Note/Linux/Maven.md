# Maven

- 下载 https://maven.apache.org/index.html

- 设置环境变量 测试 `mvn -v`

- 设置镜像 /Users/zhangshengli/Documents/work/maven-3.5.3/conf

  - ```xml
    在 mirrors 节点添加以下配置
    <mirror>
          <!--This sends everything else to /public -->
          <id>nexus-aliyun</id>
          <mirrorOf>*</mirrorOf>
          <name>Nexus aliyun</name>
          <url>http://maven.aliyun.com/nexus/content/groups/public</url>
         </mirror>
    ```

  - ​