# Java 代理

## 静态代理

代理对象与被代理对象一起实现相同的接口或父类

```java
public class TestProxy2 implements BaseTest {
    @Override
    public void doTest() {
        Dog dog = new Dog();
        DogProxy dogProxy = new DogProxy(dog);
        dogProxy.run();
        dogProxy.eat();
    }
    
    class DogProxy implements IAnimal{
        Dog dog;
        DogProxy(Dog dog){
            this.dog = dog;
        }


        @Override
        public void run() {
            System.out.println("before method call");
            dog.run();
            System.out.println("after method called");
        }

        @Override
        public void eat() {
            System.out.println("before method call");
            dog.eat();
            System.out.println("after method called");
        }
    }
}
```

优点:不修改目标对象的前提下对目标进行扩展

缺点:需要实现相同的接口,所有会有很多代理类.如果接口添加了方法,目标和代理都要维护

## jdk 动态代理

利用 jdk 的 API, 动态在内存中创建代理对象

```java
public class TestProxy implements BaseTest {
    @Override
    public void doTest() {
        Dog dog = new Dog();
        ProxyHandler proxyHandler = new ProxyHandler(dog);
        IAnimal dogProxy = (IAnimal) Proxy.newProxyInstance(dog.getClass().getClassLoader(), dog.getClass().getInterfaces(),
                proxyHandler);
        dogProxy.eat();
        dogProxy.run();
    }

    class ProxyHandler implements InvocationHandler {
        private IAnimal animal;
        ProxyHandler(IAnimal animal){
            this.animal = animal;
        }

        @Override
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            System.out.println("before method call");
            method.invoke(animal,args);
            System.out.println("after method called");
            return null;
        }
    }

}
```

优点:代理对象不需要实现接口

缺点:目标对象一定要实现接口,否则不能用动态代理

## cglib 动态代理

以目标对象子类的方式实现动态代理,又叫子类代理

Cglib包的底层是通过使用一个小而快的字节码处理框架ASM来转换字节码并生成新的类.不鼓励直接使用ASM,因为它要求你必须对JVM内部结构包括class文件的格式和指令集都很熟悉.

```
public class TestProxy1 implements BaseTest {
    @Override
    public void doTest() {
        Dog dog = new Dog();
        Dog proxy = new DogProxy<Dog>(dog).getProxy();

        proxy.eat();
        proxy.run();
        proxy.bark();
    }

    class DogProxy<T> implements MethodInterceptor{
        private T dog;
        DogProxy(T dog){
            this.dog = dog;
        }

        @Override
        public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
            System.out.println("before method call");
            method.invoke(dog,objects);
            System.out.println("after method called");
            return null;
        }

        public T getProxy(){
            Enhancer enhancer = new Enhancer();
            enhancer.setSuperclass(dog.getClass());
            enhancer.setCallback(this);
            return (T)enhancer.create();
        }
    }
}
```

需要使用到 cglib 库,`compile group: 'cglib', name: 'cglib', version: '3.2.6'`

