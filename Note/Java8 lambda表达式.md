Java8 lambda表达式
---------------

###1. 定义
lambda表达式，本质上是一个匿名方法
###2. 格式：
参数列表／箭头／表达式语句

    new Thread(()->{
            System.out.println("测试");
        }).start();

###3. 类型：
lambda表达式的类型叫目标类型，即函数接口（只有一个显示声明的抽象方法的接口，一般用@FunctionalInterface标识），只有被转型为函数接口时才能当作Object使用，其中Jdk预定义了以下函数接口：

    1.Function<T,R>	R apply(T t);
    2.Consumer<T>	void accept(T t);
    3.Predicate<T>	boolean test(T t);

###4. 使用场景：
从形式上看lambda表达式只是节省了几句代码,但这并不是Java引入它的原因.Java的短期目标是配合集合类的内部迭代和批处理操作,长期目标是将Java向函数式语言这个方向引导.只是拥有函数式语言的特性,而不是完全变成函数式编程语言.所以Oracle并没有简单地使用内部类去实现lambda表达式

1.替换匿名内部类
2.集合类批处理／流操作

    a)Stream的一系列方法，例如：
    i.filter 接收predicate类型
    ii.foreach 接收consumer类型
    iii.map 接收function类型
    iv.以及配合Collectors的操作


    /**给出一个String类型的数组，找出其中所有不重复的素数
     * @param numbers
     */
    public void distinctPrimary(String... numbers) {
        List<String> l = Arrays.asList(numbers);
        List<Integer> r = l.stream()
                .map(e -> new Integer(e))
                .filter(e -> isPrime(e))
                .distinct()
                .collect(Collectors.toList());
        System.out.println("distinctPrimary result is: " + r);
    }

    /**给出一个String类型的数组，求其中所有不重复素数的和
     * @param numbers
     */
    public void distinctPrimarySum(String... numbers) {
        List<String> l = Arrays.asList(numbers);
        int sum = l.stream()
                .map(e -> new Integer(e))
                .filter(e -> isPrime(e))
                .distinct()
                .reduce(0, (x,y) -> x+y); // equivalent to .sum()
        System.out.println("distinctPrimarySum result is: " + sum);
    }

    /*// 统计年龄在25-35岁的男女人数、比例
    public void boysAndGirls(List<Person> persons) {
        Map<Integer, Integer> result = persons.parallelStream().filter(p -> p.getAge()>=25 && p.getAge()<=35).
                collect(
                        Collectors.groupingBy(p->p.getSex(), Collectors.summingInt(p->1))
                );
        System.out.print("boysAndGirls result is " + result);
        System.out.println(", ratio (male : female) is " + (float)result.get(Person.MALE)/result.get(Person.FEMALE));
    }*/

###5. 更多用法：
1.lambda表达式嵌套

     Callable<Runnable> callable = ()->()->{};

2.定义在条件表达式中

     Callable<Integer> c2 = true ? ()->45:()->34;

3.递归调用中

    protected UnaryOperator<Integer> factorial = i -> i == 0 ? 1 : i * this.factorial.apply( i - 1 );

###6. 其他：
1.捕获

    a)只有在用到外部类的方法或成员时，才持有外部类的引用
    b)effectively final

2.方法引用

    a)静态方法／实例方法／构造器方法／父类方法引用等
    
    //c1 与 c2 是一样的（静态方法引用）
    Comparator<Integer> c2 = (x, y) -> Integer.compare(x, y);
    Comparator<Integer> c1 = Integer::compare;
    
    //下面两句是一样的（实例方法引用1）
    persons.forEach(e -> System.out.println(e));
    persons.forEach(System.out::println);
    
    //下面两句是一样的（实例方法引用2）
    persons.forEach(person -> person.eat());
    persons.forEach(Person::eat);
    
    //下面两句是一样的（构造器引用）
    strList.stream().map(s -> new Integer(s));
    strList.stream().map(Integer::new);

3.接口变化

    a)可以有默认方法／静态方法

4.生成器函数

    Stream.generate()
    下面这个例子生成并打印5个随机数：
    Stream.generate(Math::random).limit(5).forEach(System.out::println);

###7. 参考
[Lambda Expressions](http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)

[Java8 Lambda表达式教程](http://blog.csdn.net/ioriogami/article/details/12782141)
