### ThreadLocal

```txt
ThreadLocal 的中文意思是 本地线程  但它并非一个Thread 它只是一个线程本地变量，
用于将线程对象中的数据产生多个隔离，意思就是说原本线程对象中只有一个对象，若该对象应该给
多方其他数据使用则多方其他数据会对该对象产生很多修改，导致数据不统一，而使用ThreadLocal
可以将该对象产生多个对象副本，与对应的线程绑定，Key(线程对象)-value(存储的数据)

底层原理：
    每个线程都有自己的ThreadLocals[ThreadLocalMap],当使用ThreadLocal时，
    调用set[将当前线程的此线程局部变量的副本设置为指定的值。]方法，内部是先获取到当前线程对象
    然后判断此线程对象的ThreadLocals是否为空,
    若为空则将当前ThreadLocal作为键，传入的值作为value一同追加到当前线程中的Threadlocals中
    若第二次再调用set则会使用当前线程内的ThreadLocal对象，将开始调用的ThreadLocal作为键找到老的值，将新值取而代之.
    
    源代码：
    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
    总结：
        就相当于一个ThreadLocal[键] 拥有多个value,但多个value被分为一个一个value在不同的线程中
```


> 注意

```java
一个线程 对 ThreadLocal 只能set 一个 若继续set则会覆盖上一个set

ThreadLocal s=  ThreadLocal.withInitial(()->{
            return null;
        });
s.set(123);
s.set(1234);
System.out.println(s.get());
```



该ThreadLocal 常用方法

```java
get() //返回当前线程的此线程局部变量的副本中的值。 
initialValue() //返回此线程局部变量的当前线程的“初始值”。 
remove() //删除此线程局部变量的当前线程的值。 
set(T value) //将当前线程的此线程局部变量的副本设置为指定的值。 
    
若不想使用ThreadLocal的构造方法，可以通过类名点该方法获取一个ThreadLocal
withInitial(Supplier<? extends S> supplier) //创建线程局部变量。 
```

> 静态 创建 ThreadLocal 对象

```java
ThreadLocal s=  ThreadLocal.withInitial(()->{
     return null;
});
s.set(123);
System.out.println(s.get());
```

ThreadLocal 的底层实现

 ```text
ThreadLocal 中 有一个 静态内部类 称：static class ThreadLocalMap
当我们将数据传入set时，首先set判断当前线程是否为null 若不为null 则将
当前的ThreadLocal 与 传入的数据value 加入到map中
 ```

