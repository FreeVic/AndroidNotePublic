# 使用Mockito进行单元测试

[TOC]

## 配置

### 添加依赖

- app/build.gradle dependencies

  ```
  testCompile 'org.mockito:mockito-core:1.9.5'
  ```

### 常见错误

- 缺少google dexmaker包

  ```
  java.lang.VerifyError: org/mockito/cglib/core/ReflectUtils
  at org.mockito.cglib.core.KeyFactory$Generator.generateClass(KeyFactory.java:167)
  at org.mockito.cglib.core.DefaultGeneratorStrategy.generate(DefaultGeneratorStrategy.java:25)
  at org.mockito.cglib.core.AbstractClassGenerator.create(AbstractClassGenerator.java:217)
  at org.mockito.cglib.core.KeyFactory$Generator.create(KeyFactory.java:145)
  at org.mockito.cglib.core.KeyFactory.create(KeyFactory.java:117)
  at org.mockito.cglib.core.KeyFactory.create(KeyFactory.java:109)
  at org.mockito.cglib.core.KeyFactory.create(KeyFactory.java:105)
  at org.mockito.cglib.proxy.Enhancer.<clinit>(Enhancer.java:70)
  at org.mockito.internal.creation.jmock.ClassImposterizer.createProxyClass(ClassImposterizer.java:77)
  解决:
  添加依赖 androidTestCompile 'com.google.dexmaker:dexmaker-mockito:1.1'
  ```

- jdk版本不支持

  ```
  Error:Error converting bytecode to dex:
  Cause: Dex cannot parse version 52 byte code.
  This is caused by library dependencies that have been compiled using Java 8 or above.
  If you are using the 'java' gradle plugin in a library submodule add 
  targetCompatibility = '1.7'
  sourceCompatibility = '1.7'
  to that submodule's build.gradle file.
  解决:
  project:build.gradle设置jdk版本
  allprojects {
      tasks.withType(JavaCompile) {
          sourceCompatibility = 1.7
          targetCompatibility = 1.7
      }
  }
  ```

## Show me the code

- 创建测试类
  - 在project/app/src/test目录下创建测试类
- 使用mockito
  - 初始化mockito,三种方式都可以
    - 在 Junit 的类上面使用 @RunWith(MockitoJUnitRunner.class) 注解
    - 在测试方法被调用之前，使用 MockitoAnnotations.initMocks(this);
    - public MockitoRule rule = MockitoJUnit.rule(); 这个方法会调用 validateMockitoUsage 方法
  - 注解
    - ​
- 运行测试

## Espresso+mockito进行测试

## 参考

- [Mockito](http://site.mockito.org/)
- ​