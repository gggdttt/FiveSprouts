> 本章节主要的Reference源: https://www.runoob.com/ ，http://c.biancheng.net/
>

设计模式（Design pattern）是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的，设计模式可以看作是一种工程思维。

设计模式是一套被反复使用的、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了<u>重用代码、让代码更容易被他人理解、保证代码可靠性</u>。 毫无疑问，设计模式于己于他人于系统都是多赢的。项目中合理地运用设计模式可以完美地解决很多问题，每种模式在现实中都有相应的原理来与之对应，每种模式都描述了一个在我们周围不断重复发生的问题，以及该问题的核心解决方案。

在 1994 年，由 Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides 四人合著出版了一本名为 **Design Patterns - Elements of Reusable Object-Oriented Software（中文译名：设计模式 - 可复用的面向对象软件元素）** 的书，该书首次提到了软件开发中设计模式的概念。

他们所提出的设计模式主要是基于以下的面向对象设计原则：

- 对接口编程而不是对实现编程。
- 优先使用对象组合而不是继承。

> 这里需要对面向对象编程（Object-Oriented Development) 有一定的了解，特别是对Class，extend以及implement有一定的理解，推荐在学习Java的同时阅读这部分内容。



## Usage of Design Pattern

设计模式在软件开发中的两个主要用途：

1. 开发人员的共同约束/平台

   设计模式提供了一个标准的术语系统，且具体到特定的情景。例如，单例设计模式意味着使用单个对象，这样所有熟悉单例设计模式的开发人员都能使用单个对象，并且可以通过这种方式告诉对方，程序使用的是单例模式。

2. 最佳的实践

   设计模式已经经历了很长一段时间的发展，它们提供了软件开发过程中面临的一般问题的最佳解决方案。学习这些模式有助于经验不足的开发人员通过一种简单快捷的方式来学习软件设计。

## Types of Design Pattern

根据设计模式的参考书 **Design Patterns - Elements of Reusable Object-Oriented Software（中文译名：设计模式 - 可复用的面向对象软件元素）** 中所提到的，总共有 23 种设计模式。这些模式可以分为三大类：创建型模式（Creational Patterns）、结构型模式（Structural Patterns）、行为型模式（Behavioral Patterns）。当然，我们还会讨论另一类设计模式：J2EE 设计模式。

> J2EE的全称是Java 2 Platform Enterprise Edition，它是由SUN公司领导、各厂家共同制定并得到广泛认可的工业标准，或者说，它是在SUN公司领导下，多家公司参与共同制定的企业级分布式应用程序开发规范。 目前，J2EE是市场上主流的企业级分布式应用平台的解决方案 。

| 序号 | 模式 & 描述                                                  | 包括                                                         |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1    | **创建型模式** 这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。 | 工厂模式（Factory Pattern）抽象工厂模式（Abstract Factory Pattern）单例模式（Singleton Pattern）建造者模式（Builder Pattern）原型模式（Prototype Pattern） |
| 2    | **结构型模式** 这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。 | 适配器模式（Adapter Pattern）桥接模式（Bridge Pattern）过滤器模式（Filter、Criteria Pattern）组合模式（Composite Pattern）装饰器模式（Decorator Pattern）外观模式（Facade Pattern）享元模式（Flyweight Pattern）代理模式（Proxy Pattern） |
| 3    | **行为型模式** 这些设计模式特别关注对象之间的通信。          | 责任链模式（Chain of Responsibility Pattern）命令模式（Command Pattern）解释器模式（Interpreter Pattern）迭代器模式（Iterator Pattern）中介者模式（Mediator Pattern）备忘录模式（Memento Pattern）观察者模式（Observer Pattern）状态模式（State Pattern）空对象模式（Null Object Pattern）策略模式（Strategy Pattern）模板模式（Template Pattern）访问者模式（Visitor Pattern） |
| 4    | **J2EE 模式** 这些设计模式特别关注表示层。这些模式是由 Sun Java Center 鉴定的。 | MVC 模式（MVC Pattern）业务代表模式（Business Delegate Pattern）组合实体模式（Composite Entity Pattern）数据访问对象模式（Data Access Object Pattern）前端控制器模式（Front Controller Pattern）拦截过滤器模式（Intercepting Filter Pattern）服务定位器模式（Service Locator Pattern）传输对象模式（Transfer Object Pattern） |



## Six Principles of Design Pattern

| 设计原则     | 一句话归纳                                                   | 目的                                       |
| ------------ | ------------------------------------------------------------ | ------------------------------------------ |
| 开闭原则     | 对扩展开放，对修改关闭                                       | 降低维护带来的新风险                       |
| 依赖倒置原则 | 高层不应该依赖低层，要面向接口编程                           | 更利于代码结构的升级扩展                   |
| 单一职责原则 | 一个类只干一件事，实现类要单一                               | 便于理解，提高代码的可读性                 |
| 接口隔离原则 | 一个接口只干一件事，接口要精简单一                           | 功能解耦，高聚合、低耦合                   |
| 迪米特法则   | 不该知道的不要知道，一个类应该保持对其它对象最少的了解，降低耦合度 | 只和朋友交流，不和陌生人说话，减少代码臃肿 |
| 里氏替换原则 | 不要破坏继承体系，子类重写方法功能发生改变，不应该影响父类方法的含义 | 防止继承泛滥                               |
| 合成复用原则 | 尽量使用组合或者聚合关系实现代码复用，少使用继承             | 降低代码耦合                               |

### 1、开闭原则（Open Close Principle）

开闭原则的意思是：**对扩展开放，对修改关闭**。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类。

> 可以理解为，一旦一个代码通过了review，那么它将一直存在且不会被修改，除非真的出现重大bug，后续功能的实现都通过对其进行扩展来实现，这要求对需求分类到不可再分，即每个功能模块之间没有交集。当然在真正开发中很难完全做到这一点，但是至少在设计阶段需要严格遵守这个原则。

可以通过“抽象约束、封装变化”来实现开闭原则，即通过接口或者抽象类为软件实体定义一个相对稳定的抽象层，而将相同的可变因素封装在相同的具体实现类中。

因为抽象灵活性好，适应性广，只要抽象的合理，可以基本保持软件架构的稳定。而软件中易变的细节可以从抽象派生来的实现类来进行扩展，当软件需要发生变化时，只需要根据需求重新派生一个实现类来扩展就可以了。

下面以 Windows 的桌面主题为例介绍开闭原则的应用。

#### 【**Windows 的桌面主题设计**】

分析：Windows 的主题是桌面背景图片、窗口颜色和声音等元素的组合。用户可以根据自己的喜爱更换自己的桌面主题，也可以从网上下载新的主题。这些主题有共同的特点，可以为其定义一个抽象类（Abstract Subject），而每个具体的主题（Specific Subject）是其子类。用户窗体可以根据需要选择或者增加新的主题，而不需要修改原代码，所以它是满足开闭原则的，其类图如下图所示。



![Windows的桌面主题类图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251530266.gif)

### 2、里氏代换原则（Liskov Substitution Principle）

里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。*LSP* 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。<u>里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化</u>，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

> 这里需要对[继承](https://zhuanlan.zhihu.com/p/37287325)有一个初步的理解。可以说开闭原则和里氏代换原则会一起被使用，因为开闭原则的扩展性需要通过里氏代换原则来实现。

#### 里氏代换原则实现方法

里氏替换原则通俗来讲就是：子类可以扩展父类的功能，但不能改变父类原有的功能。也就是说：子类继承父类时，除添加新的方法完成新增功能外，尽量不要重写父类的方法。

根据上述理解，对里氏替换原则的定义可以总结如下：

- 子类可以实现父类的抽象方法，但不能覆盖父类的非抽象方法
- 子类中可以增加自己特有的方法
- 当子类的方法重载父类的方法时，方法的前置条件（即方法的输入参数）要比父类的方法更宽松
- 当子类的方法实现父类的方法时（重写/重载或实现抽象方法），方法的后置条件（即方法的的输出/返回值）要比父类的方法更严格或相等


通过重写父类的方法来完成新的功能写起来虽然简单，但是整个继承体系的可复用性会比较差，特别是运用多态比较频繁时，程序运行出错的概率会非常大。

如果程序违背了里氏替换原则，则继承类的对象在基类出现的地方会出现运行错误。这时其修正方法是：取消原来的继承关系，重新设计它们之间的关系。

关于里氏替换原则的例子，最有名的是“正方形不是长方形”。当然，生活中也有很多类似的例子，例如，企鹅、鸵鸟和几维鸟从生物学的角度来划分，它们属于鸟类；但从类的继承关系来看，由于它们不能继承“鸟”会飞的功能，所以它们不能定义成“鸟”的子类。同样，由于“气球鱼”不会游泳，所以不能定义成“鱼”的子类；“玩具炮”炸不了敌人，所以不能定义成“炮”的子类等。

下面以“几维鸟不是鸟”为例来说明里氏替换原则。

#### 【例】里氏替换原则在“几维鸟不是鸟”实例中的应用

分析：鸟一般都会飞行，如燕子的飞行速度大概是每小时 120 千米。但是新西兰的几维鸟由于翅膀退化无法飞行。假如要设计一个实例，计算这两种鸟飞行 300 千米要花费的时间。显然，拿燕子来测试这段代码，结果正确，能计算出所需要的时间；但拿几维鸟来测试，结果会发生“除零异常”或是“无穷大”，明显不符合预期，其类图如下图所示。



![“几维鸟不是鸟”实例的类图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251606506.gif)


程序代码如下：

```java
package principle;
public class LSPtest {    
    public static void main(String[] args) {        
        Bird bird1 = new Swallow();        
        Bird bird2 = new BrownKiwi();        
        bird1.setSpeed(120);        
        bird2.setSpeed(120);        
        System.out.println("The distance is 300 kms：");
        try {            
            System.out.println("Swallow needs " + bird1.getFlyTime(300) + "hours.");          
            System.out.println("BrownKiwi needs " + bird2.getFlyTime(300) + "hours.");        
        } catch (Exception err) {            
            System.out.println("An error occurrs!");        
        }    
    }}
//鸟类
class Bird {    
    double flySpeed;    
    public void setSpeed(double speed) {        
        flySpeed = speed;    
    }    
    public double getFlyTime(double distance) {        
        return (distance / flySpeed);    
    }}
//燕子类
class Swallow extends Bird {}
//几维鸟类
class BrownKiwi extends Bird {    
    public void setSpeed(double speed) {        
        flySpeed = 0;    
    }}
```


程序的运行结果如下：

```
The distance is 300 kms：
Swallow needs 2.5 hours.
BrownKiwi needs Infinity hours。
```


程序运行错误的原因是：几维鸟类重写了鸟类的 setSpeed(double speed) 方法，这违背了里氏替换原则。正确的做法是：取消几维鸟原来的继承关系，定义鸟和几维鸟的更一般的父类，如动物类，它们都有奔跑的能力。几维鸟的飞行速度虽然为 0，但奔跑速度不为 0，可以计算出其奔跑 300 千米所要花费的时间。其类图如下图所示。



![“几维鸟是动物”实例的类图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251533234.gif)

### **3、依赖倒转原则（Dependence Inversion Principle）**

这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。

> 需要对接口的概念有一个基本认识，在理解[接口](https://www.zhihu.com/question/20111251)的基础上就能理解DIP了。

#### 实现方法

依赖倒置原则的目的是通过要面向接口的编程来降低类间的耦合性，所以我们在实际编程中只要遵循以下4点，就能在项目中满足这个规则。

1. 每个类尽量提供接口或抽象类，或者两者都具备。
2. 变量的声明类型尽量是接口或者是抽象类。
3. 任何类都不应该从具体类派生。
4. 使用继承时尽量遵循里氏替换原则。


下面以“顾客购物程序”为例来说明依赖倒置原则的应用。

**【例】依赖倒置原则在“顾客购物程序”中的应用。**

分析：本程序反映了 “顾客类”与“商店类”的关系。商店类中有 sell() 方法，顾客类通过该方法购物以下代码定义了顾客类通过网店 1 Shop1 购物：

```java
class Customer {    
    public void shopping(Shop1 shop) {        
        //购物        
        System.out.println(shop.sell());    
    }}
```

但是，这种设计存在缺点，如果该顾客想从另外一家商店购物，就要将该顾客的代码修改如下：

```java
class Customer {    
    public void shopping(Shop2 shop) {        
        //购物        
        System.out.println(shop.sell());    
    }}
```

顾客每更换一家商店，都要修改一次代码，这明显违背了开闭原则。存在以上缺点的原因是：顾客类设计时同具体的商店类绑定了，这违背了依赖倒置原则。解决方法是：定义“网店1”和“网店2”的共同接口 Shop，顾客类面向该接口编程，其代码修改如下：

```java
class Customer {    
    public void shopping(Shop shop) {        
        //购物        
        System.out.println(shop.sell());    
    }}
```

这样，不管顾客类 Customer 访问什么商店，或者增加新的商店，都不需要修改原有代码了，其类图如下图所示。



![顾客购物程序的类图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251548397.gif)


程序代码如下：

```java
package principle;
public class DIPtest {    
    public static void main(String[] args) {       
        Customer wang = new Customer();        
        System.out.println("顾客购买以下商品：");        
        wang.shopping(new ShaoguanShop());        
        wang.shopping(new WuyuanShop());    }}
//商店
interface Shop {    
    public String sell(); 
    //卖
}
//韶关网店
class Shop1 implements Shop {    
    public String sell() {        
        return "韶关土特产：香菇、木耳……";    
    }}
//婺源网店
class Shop2 implements Shop {    
    public String sell() {        
        return "婺源土特产：绿茶、酒糟鱼……";    
    }}//顾客
class Customer {    
    public void shopping(Shop shop) {        
        //购物        
        System.out.println(shop.sell());    
    }}
```

程序的运行结果如下：

```
顾客购买以下商品：
韶关土特产：香菇、木耳……
婺源土特产：绿茶、酒糟鱼……
```

### **4、接口隔离原则（Interface Segregation Principle）**

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合，即高内聚低耦合。

> 可以和第五条迪米特法则结合起来理解。

#### 实现方法

在具体应用接口隔离原则时，应该根据以下几个规则来衡量。

- 接口尽量小，但是要有限度。一个接口只服务于一个子模块或业务逻辑。
- 为依赖接口的类定制服务。只提供调用者需要的方法，屏蔽不需要的方法。
- 了解环境，拒绝盲从。每个项目或产品都有选定的环境因素，环境不同，接口拆分的标准就不同深入了解业务逻辑。
- 提高内聚，减少对外交互。使接口用最少的方法去完成最多的事情。


下面以学生成绩管理程序为例介绍接口隔离原则的应用。

**【例】学生成绩管理程序**

分析：学生成绩管理程序一般包含插入成绩、删除成绩、修改成绩、计算总分、计算均分、打印成绩信息、査询成绩信息等功能，如果将这些功能全部放到一个接口中显然不太合理，正确的做法是将它们分别放在输入模块、统计模块和打印模块等 3 个模块中，其类图如下图 所示。

![学生成绩管理程序的类图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251555685.gif)

程序代码如下：

```java 
package principle;
public class ISPtest {    
    public static void main(String[] args) {        
        InputModule input = StuScoreList.getInputModule();        
        CountModule count = StuScoreList.getCountModule();        
        PrintModule print = StuScoreList.getPrintModule();        
        input.insert();        
        count.countTotalScore();        
        print.printStuInfo();         
        //print.delete();    
    }}
//输入模块接口
interface InputModule {    
    void insert();    
    void delete();    
    void modify();
}
//统计模块接口
interface CountModule {    
    void countTotalScore();    
    void countAverage();
}
//打印模块接口
interface PrintModule {    
    void printStuInfo();    
    void queryStuInfo();}
//实现类
class StuScoreList implements InputModule, CountModule, PrintModule {    
    private StuScoreList() {    }    
    public static InputModule getInputModule() {        
        return (InputModule) new StuScoreList();    
    }    
    public static CountModule getCountModule() {        
        return (CountModule) new StuScoreList();    
    }    
    public static PrintModule getPrintModule() {        
        return (PrintModule) new StuScoreList();    
    }    
    public void insert() {        
        System.out.println("输入模块的insert()方法被调用！");    
    }    
    public void delete() {        
        System.out.println("输入模块的delete()方法被调用！");    
    }    
    public void modify() {        
        System.out.println("输入模块的modify()方法被调用！");    
    }    
    public void countTotalScore() {        
        System.out.println("统计模块的countTotalScore()方法被调用！");    
    }
    public void countAverage() {        
        System.out.println("统计模块的countAverage()方法被调用！");    
    }    
    public void printStuInfo() {        
        System.out.println("打印模块的printStuInfo()方法被调用！");    
    }    
    public void queryStuInfo() {        
        System.out.println("打印模块的queryStuInfo()方法被调用！");    
    }}
```

程序的运行结果如下：

```
输入模块的insert()方法被调用！
统计模块的countTotalScore()方法被调用！
打印模块的printStuInfo()方法被调用！
```

### **5、迪米特法则/最少知道原则（Demeter Principle）**

最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

> 现代软件工程大部分项目都是按照功能模块来划分，这样就算一个模块出现了重大bug也可以最快速定位以及尽可能减少对其他功能模块的影响。

#### 实现方法

从迪米特法则的定义和特点可知，它强调以下两点：

1. 从依赖者的角度来说，只依赖应该依赖的对象。
2. 从被依赖者的角度说，只暴露应该暴露的方法。

所以，在运用迪米特法则时要注意以下 6 点：

1. 在类的划分上，应该创建弱耦合的类。类与类之间的耦合越弱，就越有利于实现可复用的目标
2. 在类的结构设计上，尽量降低类成员的访问权限
3. 在类的设计上，优先考虑将一个类设置成不变类
4. 在对其他类的引用上，将引用其他对象的次数降到最低
5. 不暴露类的属性成员，而应该提供相应的访问器（set 和 get 方法）
6. 谨慎使用序列化（Serializable）功能


**【例】明星与经纪人的关系实例**

分析：明星由于全身心投入艺术，所以许多日常事务由经纪人负责处理，如与粉丝的见面会，与媒体公司的业务洽淡等。这里的经纪人是明星的朋友，而粉丝和媒体公司是陌生人，所以适合使用迪米特法则，其类图如下图 所示。



![明星与经纪人的关系图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251601678.gif)

程序代码如下：

```java
package principle;
public class LoDtest {    
    public static void main(String[] args) {        
        Agent agent = new Agent();       
        agent.setStar(new Star("林心如"));      
        agent.setFans(new Fans("粉丝韩丞"));   
        agent.setCompany(new Company("中国传媒有限公司"));    
        agent.meeting();      
        agent.business();  
    }}
//经纪人
class Agent {    
    private Star myStar;   
    private Fans myFans; 
    private Company myCompany;   
    public void setStar(Star myStar) {      
        this.myStar = myStar;  
    }   
    public void setFans(Fans myFans) {    
        this.myFans = myFans;  
    } 
    public void setCompany(Company myCompany) {  
        this.myCompany = myCompany;   
    }   
    public void meeting() {   
        System.out.println(myFans.getName() + "与明星" + myStar.getName() + "见面了。"); 
    }    
    public void business() {    
        System.out.println(myCompany.getName() + "与明星" + myStar.getName() + "洽淡业务。"); 
    }}
//明星
class Star {   
    private String name;   
    Star(String name) {     
        this.name = name;  
    }   
    public String getName() {       
        return name;  
    }}
//粉丝
class Fans { 
    private String name;  
    Fans(String name) {   
        this.name = name; 
    }    
    public String getName() {  
        return name;  
    }}
//媒体公司
class Company { 
    private String name;    
    Company(String name) {     
        this.name = name; 
    }   
    public String getName() {  
        return name;
    }}
```


程序的运行结果如下：

```
粉丝韩丞与明星林心如见面了。
中国传媒有限公司与明星林心如洽淡业务。
```

### **6、合成复用原则（Composite Reuse Principle）**

合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。即用类似搭建乐高的形式，来组合之前我们已经定义好的最小的类、功能模块组合以实现新的功能模块，而不是通过继承的方式（会极大提高耦合度且不方便复用）。

#### 合成复用原则的实现方法

合成复用原则是通过将已有的对象纳入新对象中，作为新对象的成员对象来实现的，新对象可以调用已有对象的功能，从而达到复用。

下面以汽车分类管理程序为例来介绍合成复用原则的应用。

**【例】汽车分类管理程序**

分析：汽车按“动力源”划分可分为汽油汽车、电动汽车等；按“颜色”划分可分为白色汽车、黑色汽车和红色汽车等。如果同时考虑这两种分类，其组合就很多。图 1 所示是用继承关系实现的汽车分类的类图。



![用继承关系实现的汽车分类的类图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251606756.gif)

从图中 可以看出用继承关系实现会产生很多子类，而且增加新的“动力源”或者增加新的“颜色”都要修改源代码，这违背了开闭原则，显然不可取。但如果改用组合关系实现就能很好地解决以上问题，其类图如图 2 所示。



![用组合关系实现的汽车分类的类图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108251605527.gif)


## Conclusion 

设计模式不是为每个人准备的，而是基于业务来选择设计模式，需要时就能想到它。要明白一点，技术永远为业务服务，技术只是满足业务需要的一个工具。我们需要掌握每种设计模式的应用场景、特征、优缺点，以及每种设计模式的关联关系，这样就能够很好地满足日常业务的需要。

许多设计模式的功能类似，界限不是特别清楚（为了能让大家更好的理解，每个章节后面都列出了类似功能设计模式之间的对比）。大家不要疑惑，设计模式不是为了特定场景而生的，而是为了让大家可以更好和更快地开发。

当然，设计模式只是实现了七大设计原则的具体方式，套用太多设计模式只会陷入模式套路陷阱，最后代码写的凌乱不堪，合适的才是最好的。

在实际工作中很少会规定必须使用哪种设计模式，这样只会限制别人。不能为了使用设计模式而去做架构，而是有了做架构的需求后，发现它符合某一类设计模式的结构，在将两者结合(不能本末倒置)。

设计模式要活学活用，不要生搬硬套。想要游刃有余地使用设计模式，需要打下牢固的程序设计语言基础、夯实自己的编程思想、积累大量的时间经验、提高开发能力。目的都是让程序<u>低耦合，高复用，高内聚，易扩展，易维护</u>。

### 1. 需求驱动

不仅仅是功能性需求，需求驱动还包括性能和运行时的需求，如软件的可维护性和可复用性等方面。设计模式是针对软件设计的，而软件设计是针对需求的，一定不要为了使用设计模式而使用设计模式，否则可能会使设计变得复杂，使软件难以调试和维护。

### 2. 分析成功的模式应用项目

对现有的应用实例进行分析是一个很好的学习途径，应当注意学习已有的项目，而不仅是学习设计模式如何实现，更重要的是注意在什么场合使用设计模式。

### 3. 充分了解所使用的开发平台

设计模式大部分都是针对面向对象的软件设计，因此在理论上适合任何面向对象的语言，但随着技术的发展和编程环境的改善，设计模式的实现方式会有很大的差别。在一些平台下，某些设计模式是自然实现的。

不仅指编程语言，平台还包括平台引入的技术。例如，Java EE 引入了反射机制和依赖注入，这些技术的使用使设计模式的实现方式产生了改变。

### 4. 在编程中领悟设计模式

软件开发是一项实践工作，最直接的方法就是编程。没有从来不下棋却熟悉定式的围棋高手，也没有不会编程就能成为架构设计师的先例。掌握设计模式是水到渠成的事情，除了理论只是和实践积累，可能会“渐悟”或者“顿悟”。

### 5.避免设计过度

设计模式解决的是设计不足的问题，但同时也要避免设计过度。一定要牢记简洁原则，要知道设计模式是为了使设计简单，而不是更复杂。如果引入设计模式使得设计变得复杂，只能说我们把简单问题复杂化了，问题本身不需要设计模式。

这里需要把握的是需求变化的程度，一定要区分需求的稳定部分和可变部分。一个软件必然有稳定部分，这个部分就是核心业务逻辑。如果核心业务逻辑发生变化，软件就没有存在的必要，核心业务逻辑是我们需要固化的。对于可变的部分，需要判断可能发生变化的程度来确定设计策略和设计风险。要知道，设计过度与设计不足同样对项目有害。

> 学习设计模式，死记硬背是没用的，还要从实践中理解，本教程后面会结合实例和源码来讲解如何使用设计模式。

需要特别声明的是，在日常应用中，设计模式从来都不是单个设计模式独立使用的。在实际应用中，通常多个设计模式混合使用，你中有我，我中有你。