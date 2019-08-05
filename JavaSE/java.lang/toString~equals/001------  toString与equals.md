### toString  与  equals 

> toString  (Object非私有方法之一)

```java
如果我们不想创建对象打印只是"Class@地址"的话那么可以重写toString方法，将格式定义成我们想要的.
```

> equals  (Object非私有方法之一)

```java
当我们不重写equals方法时，则只能使用Object的equals方法，而这个equals方法内部只使用了==，当我们创建俩个对象时堆里分配的地址都是不一样的，但值是相同的，"我们只想比较对象内的值"，通过"未重写的equals比较结果肯定是false",因为比较的是那个变量保存的地址值，而地址刚刚说了都是不一样的，所以这并不是我们想要的结果，所以我们"如果想比较成功则需要重写equals方法"，将该对象内的值通过get方法调用出来比较.
```

> 举例

```java
class javaBean{
    private int temp = 10;
    public int getTemp(){
        return temp;
    }
    @Override
    public boolean equals(Object obj){
        if(!(obj instanceof javaBean)){
            throw new ClassCastException("类转换异常");
        }
        javaBean value = (javaBean)obj;
        return this.temp == value.getTemp();
    }
}
public class Main{
    public static void main(String args[]){
        System.out.println(new javaBean().equals(new javaBean()));
    }
}
```

> 总结

```java
1.toString方法可以统一我们对象的书写格式
2.equals要比较值，就要重写
```

