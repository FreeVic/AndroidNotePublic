# Android Studio文件分目录管理

[TOC]

## jar文件依赖分目录管理

- 在libs目录下新建子文件夹sub1,将相关jar包放在该文件夹内

- 在build.gradle中`dependencies` 节点内配置解析sub1下的依赖

  - ```
    compile fileTree(include: '*.jar', dir: 'libs/sub1')
    或者
    compile fileTree(include: '*/*.jar', dir: 'libs')
    ```

## .so文件分目录管理

- gradle默认会将jniLibs文件夹作为so库的目录，不需要进行任何配置

- 可以新建一个ocrLibs目录，将对应的`arm64-v7a` `x86` 等子文件夹放在该目录下

- 在build.gradle中android节点内配置ocrLibs目录

  - ```
    sourceSets{
            main{
                jniLibs.srcDirs = ['src/main/ocrLibs','src/main/ocrLibs2']
            }
        }
    ```

  ​