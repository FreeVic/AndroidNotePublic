# Java 反射

## 简介

在运行时检查 分析 修改代码

## 使用

### 类

```java
Class<? extends String> stringGetClass = stringer.getClass();
Class<String> stringClass = String.class;
Class.forName("java.lang.String");
```

### 接口

接口不能实例化,只能暴露方法,获取基本信息

### 枚举

枚举是一个不变的 java 类,有一些枚举专用的反射方法

```java
java.lang.Class.isEnum()： 如果元素是枚举返回true否则返回false。
java.lang.Class.getEnumConstants()： 获取给定枚举所有的常量值。
java.lang.reflect.Field.isEnumConstant()： 如果字段是一个枚举返回true，否则返回false
```

### 基本数据类型

基本数据类型可以获取其反射类,无法使用 newInstance() 创建实例

```java
Class<?> intClass = int.class;
intClass.isPrimitive();
```



## 提高效率

1. 尽量不要getMethods() 后遍历筛选,而直接用getMethod(methodName)来直接获取方法
2. Class.forName()太耗时,对 Class 进行缓存
3. 使用高效率的反射库