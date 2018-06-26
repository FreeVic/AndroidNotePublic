# Java 泛型

## 简介

泛型的本质是参数化类型,也就是说变量的类型是一个参数

```java
package srcj.genericity;

import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.List;

import javafx.util.Pair;

/**
 * Created by 2018/6/22.
 */

/**
 *
 * 泛型类 在类名后添加<T>
 *
 */
class Holder<T> {


    private T t;

    public Holder(T t){
        this.t = t;
    }

    public T getValue(){
        return t;
    }

    public void setValue(T t){
        this.t = t;
    }

    /**
     * 泛型方法,返回值前声明泛型
     * @param key
     * @param value
     * @param <K>
     * @param <V>
     * @return
     */
    public <K,V> Pair<K,V> getPair(K key,V value){
        return new Pair(key,value);
    }

    /**
     * 使用 ArrayList 构建 泛型数组
     * @param <K>
     * @return
     */
    public <K> GenericArray<K> createArray(Class<K> kClass,int size){
        return new GenericArray<K>(size);
    }

    /**
     * 传入类型 构建 泛型数组
     * @param kClass
     * @param size
     * @param <K>
     * @return
     */
    public <K> K[] createArray1(Class<K> kClass,int size){
        return (K[]) Array.newInstance(kClass,size);
    }

    @Override
    public String toString() {
        String simpleName = t.getClass().getSimpleName();
        return "This is a "+simpleName+" holder";
    }
}

class GenericArray<T>{
    private List<T> array;
    GenericArray(int size){
        array = new ArrayList(size);
    }

    public T get(int index){
        return array.get(index);
    }

    public void set(int index,T t){
        array.set(index,t);
    }
}

```

## 类型擦除

java 中的泛型使用了类型擦除,以至于功能受到限制,只能说是伪泛型.类型参数只存在于编译期,在运行时,虚拟机并不知道泛型的存在

```
Class c1 = new ArrayList<String>().getClass();
Class c2 = new ArrayList<Integer>().getClass();
System.out.println(c1 == c2);
```

代码输出的结果是 true

## 协变

java 数组支持协变

```
// 尽管 Apple[] 可以 “向上转型” 为 Fruit[]，但数组元素的实际类型还是 Apple
Fruit[] fruits = new Apple[10];
```

泛型 不支持协变

```
// 尽管 Apple 是 Fruit 的子类型，但是 ArrayList<Apple> 不是 ArrayList<Fruit> 的子类型
List<Fruit> fruits = new ArrayList<Apple>(); // 不能编译
```

## 通配符

extends 和 the ? super 通配符的特征，我们可以得出以下结论：

- 如果你想从一个数据类型里获取数据，使用 ? extends 通配符（能取不能存）
- 如果你想把对象写入一个数据结构里，使用 ? super 通配符（能存不能取）
- 如果你既想存，又想取，那就别用通配符。