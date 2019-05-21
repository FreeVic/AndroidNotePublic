# Kotlin实战

### 扩展函数实现细节

本质是 java 的静态方法

### lambda 实现细节

如果 lambda 传递给普通的期望函数式接口的方法,则 lambda会被编译成一个匿名类

如果 lambda 传递给标记成inline 的函数,则不会创建任何匿名类,而是用 lambda 的真实代码替换每一次调用.

Kotlin 1.0开始,每个 lambda 表达式都会被编译成一个匿名类,内联(inline) lambda 除外.而大多数的库函数都是inline 的

### 变量捕获实现细节

变量捕获本质是创建一个包装类存储要改变的值的引用

```kotlin
class Ref<T>(var value:T)
val counter = Ref(0)
var ic = { counter.value++ }
```

### lambda和显示匿名内部类的区别

```kotlin
postponeComputation(1000,object:Runnaable{
    override fun run)(){
        println(42)
    }
})

postponeComputation(1000){println(42)}
val runnable = Runnable {println(42)}
postponeComputation(1000,runnable)
```

显示地声明对象,则每次调用都会生成一个新的对象

使用lambda,如果 lambda 没有访问任何来自定义它的函数的变量,则相应的匿名类实例可以在多次调用之间重用

同样的,如果 lambda 捕捉了一个变量,则每次调用也会创建一个新的对象保存这个变量,并且每次调用的 Runnable 也是一个新的实例

### 内联(inline)函数的限制

函数被声明成 inline 的,意味着这个函数以及传给他的 lambda 的字节码会被一起内联到该函数调用的地方

如果要内联的函数很大,则字节码拷贝到每一次调用的地方势必极大增加字节码的长度,所以**内联函数应该尽可能小**

### ::class.java ::javaClass区别

```kotlin
/**
 * Returns a Java [Class] instance corresponding to the given [KClass] instance.
 */
@Suppress("UPPER_BOUND_VIOLATED")
public val <T> KClass<T>.java: Class<T>
    @JvmName("getJavaClass")
    get() = (this as ClassBasedDeclarationContainer).jClass as Class<T>
```

```kotlin
/**
 * Returns the runtime Java class of this object.
 */
public inline val <T: Any> T.javaClass : Class<T>
    @Suppress("UsePropertyAccessSyntax")
    get() = (this as java.lang.Object).getClass() as Class<T>
```

`::class.java` 返回的是类的字节码

`::javaClass` 是一个成员引用,所以返回的是 KProperty0类型,`::javaClass.get()`则是类的字节码

`.javaClass` 返回的是类的字节码

```kotlin
val v1 = Boolean::class.java
val v2 = Boolean::class.javaObjectType
val v3 = Boolean::class.javaPrimitiveType
val v4 = true.javaClass
val v5 = Boolean::class
v1 == v3 == v4 == v5 != v2
```

## 注意

- 调用 java 静态变量需要当心,二进制文件里是直接写死的
- 调用 java 时无法推断可空类型,默认都是非空类型,所以一般都要写可空类型,防止运行时崩溃
  - 实现 java 中的接口类型,方法参数
  - 调用 Java 中的方法,返回参数
  - 