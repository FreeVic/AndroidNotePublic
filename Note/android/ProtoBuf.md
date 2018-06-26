# ProtoBuf

[TOC]

## 什么是PB

protobuf，全称：Google Protocol Buffer，是 Google 08年左右开源的是一种以有效并可扩展的格式编码结构化数据的方式，可以用于结构化数据的串行化，也称作序列化，主要用于数据存储或是 RPC 数据交换，支持多语言，可拓展

优点:

1. 性能好/效率高
2. 无偿向后兼容
3. 多语言/可拓展

不足:

1. 二进制格式导致可读性差

## 环境配置

1. 从Protobuf主页下载源码包编译,或是直接下载打包好的文件 [源码地址](https://github.com/google/protobuf) [maven库地址](http://repo1.maven.org/maven2/com/google/protobuf/)
2. 需要文件
   1. 生成代码的可执行文件
   2. 对应语言所需要的jar包

## 原理

## 用法

1. 定义消息模型

   ```
   option java_package = "com.one.myapplication.proto";
   option java_outer_classname = "PersonA";
   option optimize_for = LITE_RUNTIME;

   message Person{
   	required string name = 1;
   	optional int32 age = 2[default = 10];
   }
   ```

2. 生成代码

   ```
   protoc --proto_path=IMPORT_PATH --java_out=DST_DIR *.proto
   ```


3. 序列化和反序列化

   ```
   PersonA.Person.Builder builder = PersonA.Person.newBuilder();
   builder.setAge(11);
   builder.setName("Vic");
   PersonA.Person person = builder.build();
   byte[] personBytes = person.toByteArray();

   try {
       PersonA.Person newPerson = PersonA.Person.parseFrom(personBytes);
       System.out.println(newPerson.getName());
       System.out.println(newPerson.getAge());
   } catch (InvalidProtocolBufferException e) {
   	e.printStackTrace();
   }
   ```

## Demo

## 语法

1. 标量值类型

   | .proto 类型 | Java 类型    | C++ 类型 | 备注                                       |
   | --------- | ---------- | ------ | ---------------------------------------- |
   | double    | double     | double |                                          |
   | float     | float      | float  |                                          |
   | int32     | int        | int32  | 使用可变长编码方式。编码负数时不够高效——如果你的字段可能含有负数，那么请使用 sint32。 |
   | int64     | long       | int64  | 使用可变长编码方式。编码负数时不够高效——如果你的字段可能含有负数，那么请使用 sint64。 |
   | uint32    | int[1]     | uint32 | Uses variable-length encoding.           |
   | uint64    | long[1]    | uint64 | Uses variable-length encoding.           |
   | sint32    | int        | int32  | 使用可变长编码方式。有符号的整型值。编码时比通常的 int32 高效。      |
   | sint64    | long       | int64  | 使用可变长编码方式。有符号的整型值。编码时比通常的 int64 高效。      |
   | fixed32   | int[1]     | uint32 | 总是 4 个字节。如果数值总是比总是比 228 大的话，这个类型会比 uint32 高效。 |
   | fixed64   | long[1]    | uint64 | 总是 8 个字节。如果数值总是比总是比 256 大的话，这个类型会比 uint64 高效。 |
   | sfixed32  | int        | int32  | 总是 4 个字节。                                |
   | sfixed64  | long       | int64  | 总是 8 个字节。                                |
   | bool      | boolean    | bool   |                                          |
   | string    | String     | string | 一个字符串必须是 UTF-8 编码或者 7-bit ASCII 编码的文本。   |
   | bytes     | ByteString | string | 可能包含任意顺序的字节数据。                           |

2. Optional的字段及默认值

3. 枚举

4. 使用其他消息类型

5. 嵌套类型

6. 更新一个消息类型

7. 扩展

8. 包

9. 定义服务

10. 选项

   1. `java_package` (fileoption)生成java类所在的包
   2. `java_outer_classname` (fileoption)要生成java类的名称
   3. `optimize_for` (fileoption)可以被设置为 SPEED, CODE_SIZE,or LITE_RUNTIME
      1. SPEED (default): protocol buffer 编译器将通过在消息类型上执行序列化、语法分析及其他通用的操作。这种代码是最优的
      2. CODE_SIZE: protocol buffer 编译器将会产生最少量的类，通过共享或基于反射的代码来实现序列化、语法分析及各种其它操作。采用该方式产生的代码将比 SPEED 要少得多， 但是操作要相对慢些。当然实现的类及其对外的 API 与 SPEED 模式都是一样的。这种方式经常用在一些包含大量的. proto 文件而且并不盲目追求速度的 应用中。
      3.  LITE_RUNTIME: protocol buffer 编译器依赖于运行时核心类库来生成代码（即采用 libprotobuf-lite 替代 libprotobuf）。这种核心类库由于忽略了一 些描述符及反射，要比全类库小得多。这种模式经常在移动手机平台应用多一些。编译器采用该模式产生的方法实现与 SPEED 模式不相上下，产生的类通过实现 MessageLite 接口，但它仅仅是 Messager 接口的一个子集。
   4. `packed` (field option): 如果该选项在一个整型基本类型上被设置为真，则采用更紧凑的编码方式(仅针对基本数值类型)
   5. `deprecated` (field option): 如果该选项被设置为 true，表明该字段已经被弃用了，在新代码中不建议使用。

11. 自定义选项

12. 生成访问类

   ```
   protoc --proto_path= --java_out=java Person.proto ./other/other.proto ./other/other1.proto
   ```

## 疑问

1. 怎么使用其他消息类型?
   - 导入定义
2. 设置的含义及用法
3. 包名的写法
4. 扩展使用int32类型,如何设置extension



## 相关文档

1. [Protobuf 语言指南](http://www.cnblogs.com/dkblog/archive/2012/03/27/2419010.html)
2. [使用 Protocol Buffers 代替 JSON 的五个原因](https://www.oschina.net/translate/choose-protocol-buffers)
3. [Protobuf 和 json 深度对比](http://cxshun.iteye.com/blog/1974498)
4. [Protocol Buffers官方文档](https://developers.google.com/protocol-buffers/)