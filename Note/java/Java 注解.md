# Java 注解

Java注解是附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能

## 定义

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface FruitProvider {
    int id() default 0;
    String from() default "";
}
```

- 通过`@interface`定义
- 所有方法没有 只允许 public abstract 修饰符,没有方法体 参数,不允许抛异常
- 返回值只能是 基本类型 String Class Annotation enum 或他们的数组
- 如果只有一个属性,可以用 value() 函数
- default 表示默认值

## 元注解

**@Documented** 是否会保存到 javadoc 中

**@Retention** 保留时间

```
SOURCE 源码时
CLASS 编译时(默认)
RUNTIME 运行时
```

**@Target** 标识修饰哪些程序元素

```
TYPE
FIELD
METHOD
CONSTRUCTOR
PARAMETER
未添加表示修饰所有
```

**@Inherited** 是否可以被继承,默认 false

## 解析

### 运行时

反射获取注解的信息

```java
class FruitAnnotationUtil {
    public static String getInfo(Class clazz){
        StringBuilder sb = new StringBuilder();
        sb.append(clazz.getSimpleName()).append("\n");
        Field[] fields = clazz.getDeclaredFields();
        for(Field field:fields){
            if(field.isAnnotationPresent(FruitLocalName.class)){
                FruitLocalName fruitLocalName = field.getAnnotation(FruitLocalName.class);
                sb.append("本地名称="+fruitLocalName.value()).append("\n");

            }else if(field.isAnnotationPresent(FruitColor.class)){
                FruitColor fruitColor = field.getAnnotation(FruitColor.class);
                sb.append("颜色="+fruitColor.value()).append("\n");

            }else if(field.isAnnotationPresent(FruitProvider.class)){
                FruitProvider fruitProvider = field.getAnnotation(FruitProvider.class);
                sb.append("产地:")
                        .append(" id="+fruitProvider.id())
                        .append(" from="+fruitProvider.from())
                        .append("\n");

            }else{

            }
        }
        return sb.toString();
    }
}
```

### 编译时

自定义类继承 AbstractProcessor 处理

Idea 中设置 `enable annotation processing`

```java
@SupportedAnnotationTypes(value = "srcj.annotation.FruitGlobalName")
class AnnotationHandler extends AbstractProcessor {
    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        for(TypeElement element:annotations){
            FruitGlobalName globalName = element.getAnnotation(FruitGlobalName.class);
            System.out.println("编译期 GlobalName="+globalName.value());
            // 修改源代码文件
        }
        return false;
    }
}
```

[Android 编写基于编译时注解的项目](https://blog.csdn.net/lmj623565791/article/details/51931859)
