##工厂模式
代码编写出来是为了给别人调用的：
   - 调用者（client）跟代码编写者（provide），可能是同一个人，也有可能是不同的人。
   - 提供给调用者的代码，有可能是源码可见，也可能是源码不可见，不可修改的（比如 jar 包）。
 所以，为了简化代码的协作使用及管理维护，必须想尽办法简化代码逻辑，实现必要的分离。

### 最原始方法
比如说，有一系列的代码，是用来创建不同品牌的手机，代码是这样的。
````
    public class Iphone {
        public void xxx () {}
    }
    public class Huawei {
        public void yyy () {}
    }
    public class Lennovo {}
    public class Xiaomi {}
    public class Vivo {}
    
````
如果这样的代码提供给客户端调用，那么提供者必须将 **所有类的名称**以及**对应的方法**暴露给客户端。

客户端的调用示例如下：
~~~~
    Iphone phone1 = new Iphone();
    phone1.xxx();
    
    Huawei phone2 = new Huawei();
    phone2.yyy();
~~~~
这样的方式非常原始，也很简单，但是代码的逻辑不清晰，暴露的内容过多。


解决的方案
   - 抽象逻辑，提供接口
---
### **有了接口之后**
为了减少方法调用的复杂度，也为了便于抽象跟代码管理，我们额外提供一个接口：
~~~
public interface Phone {
    void play();
}
~~~
然后，将所有手机类都实现 Phone 接口，将暴露给客户端调用的逻辑都封装在 play 方法里。

那么，客户端需要知道的调用API就减少到了两种：
- Phone接口的信息
- Phone接口有哪些实现类

调用的逻辑就变简单了：
~~~
Phone phone1 = new Iphone();
phone1.play();

Phone phone2 = new Lianxiang();
phone2.play();

Phone phone3 = new Xiaomii();
phone3.play();
~~~
这种方式有缺点：
- 客户端，必须要知道手机类的具体名字。
- 客户端的调用，跟提供的代码是耦合的。

所以，自然产生了简单工厂的这种策略

### **简单工厂**
在中间加一层
~~~
public class PhoneFactory {
    public Phone createPhone(String tag) {
        if (tag.equals("lx")) {
            return new Lenovo();
        } else if (tag.equals("pg")) {
            return new Iphone();
        } else if (tag.equals("hw")) {
            return new Huawei();
        }
    }
}
~~~
客户端的调用：
~~~
PhoneFactory pf = new PhoneFactory();

pf.createPhone("hw").play();
pf.createPhone("pg").play();
pf.createPhone("xm").play();
~~~

简单工厂模式，本身已经为解耦合做出了很好的方案。但是它有缺点：
- PhoneFactory 代码跟 Phone 代码紧耦合。
- 每次要添加/删除/修改某一个 Phone，都需要修改 PhoneFactory 这个类。

所以，自然产生了**工厂方法模式**这种策略.....

    

