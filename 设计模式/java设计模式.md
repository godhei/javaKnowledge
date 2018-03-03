## 单例模式
**定义：** 确保一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。  
**类型：** 创建类模式
#### 要素
* 私有的构造方法
* 指向自己实例的私有静态引用
* 以自己实例为返回值的静态的公有的方法  


<p>&nbsp;&nbsp;&nbsp;&nbsp;单例模式根据实例化对象时机的不同分为两种：一种是饿汉式单例，一种是懒汉式单例。饿汉式单例在例类被加载时候，就实例化一个对象交给自己的引用；而懒汉式在调用取得实例方法的时候才会实例化对象。代码如下：</p>

##### 饿汉式
<pre><code>
public class Singleton {  
    private static Singleton singleton = new Singleton();  
    private Singleton(){}  
    public static Singleton getInstance(){  
        return singleton;  
    }  
}
</code></pre>

##### 懒汉式
<pre><code>
public class Singleton {  
    private static Singleton singleton;  
    private Singleton(){}  

    public static synchronized Singleton getInstance(){  
        if(singleton==null){  
            singleton = new Singleton();  
        }  
        return singleton;  
    }  
}  
</code></pre>

#### 单例模式的优点：
* 在内存中只有一个对象，节省内存空间。
* 避免频繁的创建销毁对象，可以提高性能。
* 避免对共享资源的多重占用。
* 可以全局访问。

** 适用场景：** 由于单例模式的以上优点，所以是编程中用的比较多的一种设计模式。我总结了一下我所知道的适合使用单例模式的场景：
* 需要频繁实例化然后销毁的对象。
* 创建对象时耗时过多或者耗资源过多，但又经常用到的对象。
* 有状态的工具类对象。
* 频繁访问数据库或文件的对象。
* 以及其他我没用过的所有要求只有一个对象的场景。

** 单例模式注意事项：** 只能使用单例类提供的方法得到单例对象，不要使用反射，否则将会实例化一个新对象。不要做断开单例类对象与类中静态引用的危险操作。多线程使用单例使用共享资源时，注意线程安全问题。

** 关于java中单例模式的一些争议：**

###### 单例模式的对象长时间不用会被jvm垃圾收集器收集吗？
<p> &nbsp;&nbsp;&nbsp;&nbsp;看到不少资料中说：如果一个单例对象在内存中长久不用，会被jvm认为是一个垃圾，在执行垃圾收集的时候会被清理掉。对此这个说法，笔者持怀疑态度，笔者本人的观点是：在hotspot虚拟机1.6版本中，除非人为地断开单例中静态引用到单例对象的联接，否则jvm垃圾收集器是不会回收单例对象的。
</p>


###### 在一个jvm中会出现多个单例吗
<p>&nbsp;&nbsp;&nbsp;&nbsp;在分布式系统、多个类加载器、以及序列化的的情况下，会产生多个单例，这一点是无庸置疑的。那么在同一个jvm中，会不会产生单例呢？使用单例提供的getInstance()方法只能得到同一个单例，除非是使用反射方式，将会得到新的单例。代码如下
<pre><code>
Class c = Class.forName(Singleton.class.getName());
Constructor ct = c.getDeclaredConstructor();
ct.setAccessible(true);
Singleton singleton = (Singleton)ct.newInstance();
</code></pre>
这样，每次运行都会产生新的单例对象。所以运用单例模式时，一定注意不要使用反射产生新的单例对象。</p>

###### 懒汉式单例线程安全吗
<p>&nbsp;&nbsp;&nbsp;&nbsp;主要是网上的一些说法，懒汉式的单例模式是线程不安全的，即使是在实例化对象的方法上加synchronized关键字，也依然是危险的，但是笔者经过编码测试，发现加synchronized关键字修饰后，虽然对性能有部分影响，但是却是线程安全的，并不会产生实例化多个对象的情况。</p>

###### 单例模式只有饿汉式和懒汉式两种吗
<p>&nbsp;&nbsp;&nbsp;&nbsp;饿汉式单例和懒汉式单例只是两种比较主流和常用的单例模式方法，从理论上讲，任何可以实现一个类只有一个实例的设计模式，都可以称为单例模式。</p>

###### 单例类可以被继承吗
<p>&nbsp;&nbsp;&nbsp;&nbsp;饿汉式单例和懒汉式单例由于构造方法是private的，所以他们都是不可继承的，但是其他很多单例模式是可以继承的，例如登记式单例。</p>

###### 饿汉式单例好还是懒汉式单例好
<p> 在java中，饿汉式单例要优于懒汉式单例。C++中则一般使用懒汉式单例。</p>
