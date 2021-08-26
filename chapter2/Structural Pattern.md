Introduction 

结构型模式描述如何将类或对象按某种布局组成更大的结构。它分为类结构型模式和对象结构型模式，前者采用继承机制来组织接口和类，后者釆用组合或聚合来组合对象。

由于组合关系或聚合关系比继承关系耦合度低，满足“合成复用原则”，所以对象结构型模式比类结构型模式具有更大的灵活性。

结构型模式分为以下 7 种：

1. 代理（Proxy）模式：为某对象提供一种代理以控制对该对象的访问。即客户端通过代理间接地访问该对象，从而限制、增强或修改该对象的一些特性。
2. 适配器（Adapter）模式：将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。
3. 桥接（Bridge）模式：将抽象与实现分离，使它们可以独立变化。它是用组合关系代替继承关系来实现的，从而降低了抽象和实现这两个可变维度的耦合度。
4. 装饰（Decorator）模式：动态地给对象增加一些职责，即增加其额外的功能。
5. 外观（Facade）模式：为多个复杂的子系统提供一个一致的接口，使这些子系统更加容易被访问。
6. 享元（Flyweight）模式：运用共享技术来有效地支持大量细粒度对象的复用。
7. 组合（Composite）模式：将对象组合成树状层次结构，使用户对单个对象和组合对象具有一致的访问性。


以上 7 种结构型模式，除了[适配器模式](http://c.biancheng.net/view/1361.html)分为类结构型模式和对象结构型模式两种，其他的全部属于对象结构型模式，下面我们会分别、详细地介绍它们的特点、结构与应用。

# 1. 代理模式

在有些情况下，一个客户不能或者不想直接访问另一个对象，这时需要找一个中介帮忙完成某项任务，这个中介就是代理对象。例如，购买火车票不一定要去火车站买，可以通过 12306 网站或者去火车票代售点买。又如找女朋友、找保姆、找工作等都可以通过找中介完成。

在软件设计中，使用代理模式的例子也很多，例如，要访问的远程对象比较大（如视频或大图像等），其下载要花很多时间。还有因为安全原因需要屏蔽客户端直接访问真实对象，如某单位的内部数据库等。

## 1.1 代理模式的定义与特点

代理模式的定义：由于某些原因需要给某对象提供一个代理以控制对该对象的访问。这时，访问对象不适合或者不能直接引用目标对象，代理对象作为访问对象和目标对象之间的中介。

代理模式的主要优点有：

- 代理模式在客户端与目标对象之间起到一个中介作用和保护目标对象的作用；
- 代理对象可以扩展目标对象的功能；
- 代理模式能将客户端与目标对象分离，在一定程度上降低了系统的耦合度，增加了程序的可扩展性


其主要缺点是：

- 代理模式会造成系统设计中类的数量增加
- 在客户端和目标对象之间增加一个代理对象，会造成请求处理速度变慢；
- 增加了系统的复杂度；

> 那么如何解决以上提到的缺点呢？答案是可以使用动态代理方式

## 1.2 代理模式的结构与实现

代理模式的结构比较简单，主要是通过定义一个继承抽象主题的代理来包含真实主题，从而实现对真实主题的访问，下面来分析其基本结构和实现方法。

#### 1.2.1. 模式的结构

代理模式的主要角色如下:

1. 抽象主题（Subject）类：通过接口或抽象类声明真实主题和代理对象实现的业务方法。
2. 真实主题（Real Subject）类：实现了抽象主题中的具体业务，是代理对象所代表的真实对象，是最终要引用的对象。
3. 代理（Proxy）类：提供了与真实主题相同的接口，其内部含有对真实主题的引用，它可以访问、控制或扩展真实主题的功能。


其结构图如下图所示：



![代理模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260139232.gif)


在代码中，一般代理会被理解为代码增强，实际上就是在原代码逻辑前后增加一些代码逻辑，而使调用者无感知。

根据代理的创建时期，代理模式分为静态代理和动态代理。

- 静态：由程序员创建代理类或特定工具自动生成源代码再对其编译，在程序运行前代理类的 .class 文件就已经存在了。
- 动态：在程序运行时，运用反射机制动态创建而成

#### 1.2.2. 模式的实现

代理模式的实现代码如下：

```java
package proxy;public class ProxyTest {   
    public static void main(String[] args) {  
        Proxy proxy = new Proxy();    
        proxy.Request();  
    }}
//抽象主题
interface Subject { 
    void Request();
}
//真实主题
class RealSubject implements Subject { 
    public void Request() {     
        System.out.println("访问真实主题方法..."); 
    }}
//代理
class Proxy implements Subject { 
    private RealSubject realSubject;  
    public void Request() {    
        if (realSubject == null) {      
            realSubject = new RealSubject();   
        }     
        preRequest();     
        realSubject.Request();  
        postRequest();   
    }  
    public void preRequest() {      
        System.out.println("访问真实主题之前的预处理。"); 
    } 
    public void postRequest() {    
        System.out.println("访问真实主题之后的后续处理。");  
    }}
```

程序运行的结果如下：

```
访问真实主题之前的预处理。
访问真实主题方法...
访问真实主题之后的后续处理。
```

## 1.3 代理模式的应用实例

【例1】韶关“天街e角”公司

分析：本实例中的“婺源特产公司”经营许多婺源特产。而韶关“天街e角”公司是婺源特产公司特产的代理，通过调用婺源特产公司的 display() 方法显示代理产品，当然它可以增加一些额外的处理，如包裝或加价等。客户可通过“天街e角”代理公司间接访问“婺源特产公司”的产品，图 2 所示是公司的结构图。



![韶关“天街e角”公园的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260142149.gif)
程序代码如下：

```Java
package proxy;
import java.awt.*;
import javax.swing.*;
public class WySpecialtyProxy { 
    public static void main(String[] args) {  
        SgProxy proxy = new SgProxy();    
        proxy.display();   
    }}
//抽象主题：特产
interface Specialty {    
    void display();
}
//真实主题：婺源特产
class WySpecialty extends JFrame implements Specialty {  
    private static final long serialVersionUID = 1L;  
    public WySpecialty() {     
        super("韶关代理婺源特产测试");   
        this.setLayout(new GridLayout(1, 1));    
        JLabel l1 = new JLabel(new ImageIcon("src/proxy/WuyuanSpecialty.jpg"));  
        this.add(l1);    
        this.pack();    
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
    }    
    public void display() {  
        this.setVisible(true);    
    }}
//代理：韶关代理
class SgProxy implements Specialty { 
    private WySpecialty realSubject = new WySpecialty();  
    public void display() {    
        preRequest();      
        realSubject.display();    
        postRequest();   
    }   
    public void preRequest() {    
        System.out.println("韶关代理婺源特产开始。");  
    }   
    public void postRequest() {    
        System.out.println("韶关代理婺源特产结束。"); 
    }}
```



## 1.4 代理模式的应用场景

当无法或不想直接引用某个对象或访问某个对象存在困难时，可以通过代理对象来间接访问。使用代理模式主要有两个目的：一是保护目标对象，二是增强目标对象。

前面分析了代理模式的结构与特点，现在来分析以下的应用场景。

- 远程代理，这种方式通常是为了隐藏目标对象存在于不同地址空间的事实，方便客户端访问。例如，用户申请某些网盘空间时，会在用户的文件系统中建立一个虚拟的硬盘，用户访问虚拟硬盘时实际访问的是网盘空间。
- 虚拟代理，这种方式通常用于要创建的目标对象开销很大时。例如，下载一幅很大的图像需要很长时间，因某种计算比较复杂而短时间无法完成，这时可以先用小比例的虚拟代理替换真实的对象，消除用户对服务器慢的感觉。
- 安全代理，这种方式通常用于控制不同种类客户对真实对象的访问权限。
- 智能指引，主要用于调用目标对象时，代理附加一些额外的处理功能。例如，增加计算真实对象的引用次数的功能，这样当该对象没有被引用时，就可以自动释放它。
- 延迟加载，指为了提高系统的性能，延迟对目标的加载。

# 2. 适配器模式

在现实生活中，经常出现两个对象因接口不兼容而不能在一起工作的实例，这时需要第三者进行适配。例如，讲中文的人同讲英文的人对话时需要一个翻译，用直流电的笔记本电脑接交流电源时需要一个电源适配器，用计算机访问照相机的 SD 内存卡时需要一个读卡器等。

在软件设计中也可能出现：需要开发的具有某种业务功能的组件在现有的组件库中已经存在，但它们与当前系统的接口规范不兼容，如果重新开发这些组件成本又很高，这时用适配器模式能很好地解决这些问题。

## 2.1 模式的定义与特点

适配器模式（Adapter）的定义如下：将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。适配器模式分为类结构型模式和对象结构型模式两种，前者类之间的耦合度比后者高，且要求程序员了解现有组件库中的相关组件的内部结构，所以应用相对较少些。

该模式的主要优点如下。

- 客户端通过适配器可以透明地调用目标接口。
- 复用了现存的类，程序员不需要修改原有代码而重用现有的适配者类。
- 将目标类和适配者类解耦，解决了目标类和适配者类接口不一致的问题。
- 在很多业务场景中符合开闭原则。
  其缺点是：

- 适配器编写过程需要结合业务场景全面考虑，可能会增加系统的复杂性。
- 增加代码阅读难度，降低代码可读性，过多使用适配器会使系统代码变得凌乱。

## 2.2 模式的结构与实现

类适配器模式可采用多重继承方式实现，如C++可定义一个适配器类来同时继承当前系统的业务接口和现有组件库中已经存在的组件接口；JAVA不支持多继承，但可以定义一个适配器类来实现当前系统的业务接口，同时又继承现有组件库中已经存在的组件。

对象适配器模式可釆用将现有组件库中已经实现的组件引入适配器类中，该类同时实现当前系统的业务接口。现在来介绍它们的基本结构。

#### 2.2.1. 模式的结构

适配器模式（Adapter）包含以下主要角色：

1. 目标（Target）接口：当前系统业务所期待的接口，它可以是抽象类或接口。
2. 适配者（Adaptee）类：它是被访问和适配的现存组件库中的组件接口。
3. 适配器（Adapter）类：它是一个转换器，通过继承或引用适配者的对象，把适配者接口转换成目标接口，让客户按目标接口的格式访问适配者。


类适配器模式的结构图如下图所示：



![类适配器模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260148387.gif)



对象适配器模式的结构图如下图所示：



![对象适配器模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260148872.gif)


#### 2.2.2. 模式的实现

(1) 类适配器模式的代码如下。

```Java
package adapter;
//目标接口
interface Target{ 
    public void request();
}
//适配者接口
class Adaptee{
    public void specificRequest() 
    {               
        System.out.println("适配者中的业务代码被调用！");  
    }}
//类适配器类
class ClassAdapter extends Adaptee implements Target{  
    public void request()    {  
        specificRequest();  
    }}
//客户端代码
public class ClassAdapterTest{ 
    public static void main(String[] args)    {  
        System.out.println("类适配器模式测试：");    
        Target target = new ClassAdapter();       
        target.request();  
    }}
```


程序的运行结果如下：

```
类适配器模式测试：
适配者中的业务代码被调用！
```


(2)对象适配器模式的代码如下。

```Java
package adapter;
//对象适配器类
class ObjectAdapter implements Target{  
    private Adaptee adaptee;  
    public ObjectAdapter(Adaptee adaptee)    {   
        this.adaptee=adaptee;  
    }   
    public void request()    {    
        adaptee.specificRequest();  
    }}
//客户端代码
public class ObjectAdapterTest{  
    public static void main(String[] args)    {  
        System.out.println("对象适配器模式测试：");       
        Adaptee adaptee = new Adaptee();      
        Target target = new ObjectAdapter(adaptee);  
        target.request();   
    }}
```


说明：对象适配器模式中的“目标接口”和“适配者类”的代码同类适配器模式一样，只要修改适配器类和客户端的代码即可。

程序的运行结果如下：

```
对象适配器模式测试：
适配者中的业务代码被调用！
```

## 2.3 模式的应用实例

**【例用适配器模式（Adapter）模拟新能源汽车的发动机】**

分析：新能源汽车的发动机有电能发动机（Electric Motor）和光能发动机（Optical Motor）等，各种发动机的驱动方法不同，例如，电能发动机的驱动方法 electricDrive() 是用电能驱动，而光能发动机的驱动方法 opticalDrive() 是用光能驱动，它们是适配器模式中被访问的适配者。

客户端希望用统一的发动机驱动方法 drive() 访问这两种发动机，所以必须定义一个统一的目标接口 Motor，然后再定义电能适配器（Electric Adapter）和光能适配器（Optical Adapter）去适配这两种发动机。

我们把客户端想访问的新能源发动机的适配器的名称放在 XML 配置文件中，客户端可以通过对象生成器类 ReadXML 去读取。这样，客户端就可以通过 Motor 接口随便使用任意一种新能源发动机去驱动汽车，下图是其结构图：



![发动机适配器的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260144424.gif)


程序代码如下：

```java
package adapter;
//目标：发动机
interface Motor{  
    public void drive();
}
//适配者1：电能发动机
class ElectricMotor{  
    public void electricDrive()    { 
        System.out.println("电能发动机驱动汽车！");    
    }}
//适配者2：光能发动机
class OpticalMotor{   
    public void opticalDrive()    {    
        System.out.println("光能发动机驱动汽车！"); 
    }}
//电能适配器
class ElectricAdapter implements Motor{   
    private ElectricMotor emotor;   
    public ElectricAdapter()    {     
        emotor=new ElectricMotor();  
    }   
    public void drive()    {    
        emotor.electricDrive();  
    }}
//光能适配器
class OpticalAdapter implements Motor{  
    private OpticalMotor omotor;  
    public OpticalAdapter()    {     
        omotor=new OpticalMotor();   
    }   
    public void drive()    {   
        omotor.opticalDrive();   
    }}
//客户端代码
public class MotorAdapterTest{
    public static void main(String[] args)    {   
        System.out.println("适配器模式测试：");   
        Motor motor=(Motor)ReadXML.getObject();  
        motor.drive();  
    }}
package adapter;
import javax.xml.parsers.*;
import org.w3c.dom.*;
import java.io.*;
class ReadXML{  
    public static Object getObject()    {    
        try    {       
            DocumentBuilderFactory dFactory=DocumentBuilderFactory.newInstance();    
            DocumentBuilder builder=dFactory.newDocumentBuilder();        
            Document doc;                   
            doc=builder.parse(new File("src/adapter/config.xml"));       
            NodeList nl=doc.getElementsByTagName("className");       
            Node classNode=nl.item(0).getFirstChild();        
            String cName="adapter."+classNode.getNodeValue();    
            Class<?> c=Class.forName(cName);            
            Object obj=c.newInstance();         
            return obj;       
        }          
        catch(Exception e)         {          
            e.printStackTrace();        
            return null;       
        }   
    }}
```


程序的运行结果如下：

```
适配器模式测试：
电能发动机驱动汽车！
```


注意：如果将配置文件中的 ElectricAdapter 改为 OpticalAdapter，则运行结果如下：

```
适配器模式测试：
光能发动机驱动汽车！
```

## 2.4 模式的应用场景

适配器模式（Adapter）通常适用于以下场景。

- 以前开发的系统存在满足新系统功能需求的类，但其接口同新系统的接口不一致。
- 使用第三方提供的组件，但组件接口定义和自己要求的接口定义不同。



# 3.桥接模式

在现实生活中，某些类具有两个或多个维度的变化，如图形既可按形状分，又可按颜色分。如何设计类似于 Photoshop 这样的软件，能画不同形状和不同颜色的图形呢？如果用继承方式，m 种形状和 n 种颜色的图形就有 m×n 种，不但对应的子类很多，而且扩展困难。

当然，这样的例子还有很多，如不同颜色和字体的文字、不同品牌和功率的汽车、不同性别和职业的男女、支持不同平台和不同文件格式的媒体播放器等。如果用桥接模式就能很好地解决这些问题。

## 3.1 桥接模式的定义与特点

桥接（Bridge）模式的定义如下：将抽象与实现分离，使它们可以独立变化。它是用组合关系代替继承关系来实现，从而降低了抽象和实现这两个可变维度的耦合度。

通过上面的讲解，我们能很好的感觉到桥接模式遵循了里氏替换原则和依赖倒置原则，最终实现了开闭原则，对修改关闭，对扩展开放。这里将桥接模式的优缺点总结如下。

桥接（Bridge）模式的优点是：

- 抽象与实现分离，扩展能力强
- 符合开闭原则
- 符合合成复用原则
- 其实现细节对客户透明


缺点是：由于聚合关系建立在抽象层，要求开发者针对抽象化进行设计与编程，能正确地识别出系统中两个独立变化的维度，这增加了系统的理解与设计难度。

## 3.2 桥接模式的结构与实现

可以将抽象化部分与实现化部分分开，取消二者的继承关系，改用组合关系。

#### 3.2.1 模式的结构

桥接（Bridge）模式包含以下主要角色。

1. 抽象化（Abstraction）角色：定义抽象类，并包含一个对实现化对象的引用。
2. 扩展抽象化（Refined Abstraction）角色：是抽象化角色的子类，实现父类中的业务方法，并通过组合关系调用实现化角色中的业务方法。
3. 实现化（Implementor）角色：定义实现化角色的接口，供扩展抽象化角色调用。
4. 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现。


其结构图如下图所示：



![桥接模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260153679.gif)


#### 3.2.2. 模式的实现

桥接模式的代码如下：

```java
package bridge;
public class BridgeTest {   
    public static void main(String[] args) { 
        Implementor imple = new ConcreteImplementorA();     
        Abstraction abs = new RefinedAbstraction(imple);   
        abs.Operation();  
    }}
//实现化角色
interface Implementor {   
    public void OperationImpl();}
//具体实现化角色
class ConcreteImplementorA implements Implementor {  
    public void OperationImpl() {    
        System.out.println("具体实现化(Concrete Implementor)角色被访问");  
    }}
//抽象化角色
abstract class Abstraction {    
    protected Implementor imple;
    protected Abstraction(Implementor imple) {  
        this.imple = imple;  
    }   
    public abstract void Operation();}
//扩展抽象化角色
class RefinedAbstraction extends Abstraction { 
    protected RefinedAbstraction(Implementor imple) {  
        super(imple); 
    }  
    public void Operation() {  
        System.out.println("扩展抽象化(Refined Abstraction)角色被访问");    
        imple.OperationImpl();   
    }}
```


程序的运行结果如下：

```
扩展抽象化(Refined Abstraction)角色被访问
具体实现化(Concrete Implementor)角色被访问
```

## 3.3 桥接模式的应用实例

**【例】用桥接（Bridge）模式模拟女士皮包的选购**

分析：女士皮包有很多种，可以按用途分、按皮质分、按品牌分、按颜色分、按大小分等，存在多个维度的变化，所以采用桥接模式来实现女士皮包的选购比较合适。

本实例按用途分可选钱包（Wallet）和挎包（HandBag），按颜色分可选黄色（Yellow）和红色（Red）。可以按两个维度定义为颜色类和包类。

颜色类（Color）是一个维度，定义为实现化角色，它有两个具体实现化角色：黄色和红色，通过 getColor() 方法可以选择颜色；包类（Bag）是另一个维度，定义为抽象化角色，它有两个扩展抽象化角色：挎包和钱包，它包含了颜色类对象，通过 getName() 方法可以选择相关颜色的挎包和钱包。

客户类通过 ReadXML 类从 XML 配置文件中获取包信息，并把选到的产品通过窗体显示出现，图 2 所示是其结构图。



![女士皮包选购的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260155767.gif)

程序代码如下：

```java
package bridge;
import org.w3c.dom.NodeList;
import javax.swing.*;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import java.awt.*;
public class BagManage {    
    public static void main(String[] args) {    
        Color color;    
        Bag bag;   
        color = (Color) ReadXML.getObject("color");   
        bag = (Bag) ReadXML.getObject("bag");    
        bag.setColor(color);      
        String name = bag.getName();     
        show(name);  
    }  
    public static void show(String name) { 
        JFrame jf = new JFrame("桥接模式测试");  
        Container contentPane = jf.getContentPane();  
        JPanel p = new JPanel();   
        JLabel l = new JLabel(new ImageIcon("src/bridge/" + name + ".jpg"));   
        p.setLayout(new GridLayout(1, 1));     
        p.setBorder(BorderFactory.createTitledBorder("女士皮包"));   
        p.add(l);     
        contentPane.add(p, BorderLayout.CENTER);  
        jf.pack();   
        jf.setVisible(true);      
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  
    }}
//实现化角色：颜色
interface Color {  
    String getColor();}
//具体实现化角色：黄色
class Yellow implements Color { 
    public String getColor() {   
        return "yellow";   
    }}
//具体实现化角色：红色
class Red implements Color {   
    public String getColor() {  
        return "red";  
    }}
//抽象化角色：包
abstract class Bag { 
    protected Color color; 
    public void setColor(Color color) {
        this.color = color;   
    }   
    public abstract String getName();}
//扩展抽象化角色：挎包
class HandBag extends Bag { 
    public String getName() {    
        return color.getColor() + "HandBag"; 
    }}
//扩展抽象化角色：钱包
class Wallet extends Bag { 
    public String getName() {  
        return color.getColor() + "Wallet"; 
    }}
package bridge;    
import javax.xml.parsers.*;   
import org.w3c.dom.*;   
import java.io.*;
class ReadXML {  
    public static Object getObject(String args) {   
        try {         
            DocumentBuilderFactory dFactory = DocumentBuilderFactory.newInstance();    
            DocumentBuilder builder = dFactory.newDocumentBuilder();   
            Document doc;   
            doc = builder.parse(new File("src/bridge/config.xml"));  
            NodeList nl = doc.getElementsByTagName("className");       
            Node classNode = null;   
            if (args.equals("color")) {          
                classNode = nl.item(0).getFirstChild();     
            } else if (args.equals("bag")) {      
                classNode = nl.item(1).getFirstChild();      
            }         
            String cName = "bridge." + classNode.getNodeValue();        
            Class<?> c = Class.forName(cName);  
            Object obj = c.newInstance();     
            return obj;    
        } catch (Exception e) {      
            e.printStackTrace();    
            return null;    
        }  
    }}
```




## 3.4 桥接模式的应用场景

当一个类内部具备两种或多种变化维度时，使用桥接模式可以解耦这些变化的维度，使高层代码架构稳定。

桥接模式通常适用于以下场景。

1. 当一个类存在两个独立变化的维度，且这两个维度都需要进行扩展时。
2. 当一个系统不希望使用继承或因为多层次继承导致系统类的个数急剧增加时。
3. 当一个系统需要在构件的抽象化角色和具体化角色之间增加更多的灵活性时。


桥接模式的一个常见使用场景就是替换继承。我们知道，继承拥有很多优点，比如，抽象、封装、多态等，父类封装共性，子类实现特性。继承可以很好的实现代码复用（封装）的功能，但这也是继承的一大缺点。

因为父类拥有的方法，子类也会继承得到，无论子类需不需要，这说明继承具备强侵入性（父类代码侵入子类），同时会导致子类臃肿。因此，在设计模式中，有一个原则为优先使用组合/聚合，而不是继承。



![img](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260145886.png)


很多时候，我们分不清该使用继承还是组合/聚合或其他方式等，其实可以从现实语义进行思考。因为软件最终还是提供给现实生活中的人使用的，是服务于人类社会的，软件是具备现实场景的。当我们从纯代码角度无法看清问题时，现实角度可能会提供更加开阔的思路。

# 4.装饰器模式

上班族大多都有睡懒觉的习惯，每天早上上班时间都很紧张，于是很多人为了多睡一会，就会用方便的方式解决早餐问题。有些人早餐可能会吃煎饼，煎饼中可以加鸡蛋，也可以加香肠，但是不管怎么“加码”，都还是一个煎饼。在现实生活中，常常需要对现有产品增加新的功能或美化其外观，如房子装修、相片加相框等，都是装饰器模式。

在软件开发过程中，有时想用一些现存的组件。这些组件可能只是完成了一些核心功能。但在不改变其结构的情况下，可以动态地扩展其功能。所有这些都可以釆用装饰器模式来实现。

## 4.1 装饰器模式的定义与特点

装饰器（Decorator）模式的定义：指在不改变现有对象结构的情况下，动态地给该对象增加一些职责（即增加其额外功能）的模式，它属于对象结构型模式。

装饰器模式的主要优点有：

- 装饰器是继承的有力补充，比继承灵活，在不改变原有对象的情况下，动态的给一个对象扩展功能，即插即用
- 通过使用不用装饰类及这些装饰类的排列组合，可以实现不同效果
- 装饰器模式完全遵守开闭原则


其主要缺点是：装饰器模式会增加许多子类，过度使用会增加程序得复杂性。

## 4.2 装饰器模式的结构与实现

通常情况下，扩展一个类的功能会使用继承方式来实现。但继承具有静态特征，耦合度高，并且随着扩展功能的增多，子类会很膨胀。如果使用组合关系来创建一个包装对象（即装饰对象）来包裹真实对象，并在保持真实对象的类结构不变的前提下，为其提供额外的功能，这就是装饰器模式的目标。下面来分析其基本结构和实现方法。

#### 4.2.1. 模式的结构

装饰器模式主要包含以下角色。

1. 抽象构件（Component）角色：定义一个抽象接口以规范准备接收附加责任的对象。
2. 具体构件（ConcreteComponent）角色：实现抽象构件，通过装饰角色为其添加一些职责。
3. 抽象装饰（Decorator）角色：继承抽象构件，并包含具体构件的实例，可以通过其子类扩展具体构件的功能。
4. 具体装饰（ConcreteDecorator）角色：实现抽象装饰的相关方法，并给具体构件对象添加附加的责任。


装饰器模式的结构图如下图：



![装饰模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260159472.gif)


#### 4.2.2. 模式的实现

装饰器模式的实现代码如下：

```java
package decorator;
public class DecoratorPattern {  
    public static void main(String[] args) { 
        Component p = new ConcreteComponent();   
        p.operation();   
        System.out.println("---------------------------------");  
        Component d = new ConcreteDecorator(p);  
        d.operation();  
    }}
//抽象构件角色
interface Component { 
    public void operation();}
//具体构件角色
class ConcreteComponent implements Component {  
    public ConcreteComponent() {    
        System.out.println("创建具体构件角色"); 
    }  
    public void operation() {    
        System.out.println("调用具体构件角色的方法operation()");   
    }}
//抽象装饰角色
class Decorator implements Component {   
    private Component component;  
    public Decorator(Component component) {   
        this.component = component;  
    }  
    public void operation() {    
        component.operation();  
    }}
//具体装饰角色
class ConcreteDecorator extends Decorator { 
    public ConcreteDecorator(Component component) {    
        super(component); 
    } 
    public void operation() {   
        super.operation();    
        addedFunction();  
    }   
    public void addedFunction() { 
        System.out.println("为具体构件角色增加额外的功能addedFunction()"); 
    }}
```


程序运行结果如下：

```
创建具体构件角色
调用具体构件角色的方法operation()
---------------------------------
调用具体构件角色的方法operation()
为具体构件角色增加额外的功能addedFunction()
```

## 4.3 装饰器模式的应用实例

**【例】用装饰器模式实现游戏角色“莫莉卡·安斯兰”的变身**

分析：在《恶魔战士》中，游戏角色“莫莉卡·安斯兰”的原身是一个可爱少女，但当她变身时，会变成头顶及背部延伸出蝙蝠状飞翼的女妖，当然她还可以变为穿着漂亮外衣的少女。这些都可用装饰器模式来实现，在本实例中的“莫莉卡”原身有 setImage(String t) 方法决定其显示方式，而其 变身“蝙蝠状女妖”和“着装少女”可以用 setChanger() 方法来改变其外观，原身与变身后的效果用 display() 方法来显示，下图所示是其结构图：



![游戏角色“莫莉卡·安斯兰”的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260145212.gif)

程序代码如下：

```java
package decorator;
import java.awt.*;
import javax.swing.*;
public class MorriganAensland { 
    public static void main(String[] args) {   
        Morrigan m0 = new original();  
        m0.display();   
        Morrigan m1 = new Succubus(m0);  
        m1.display();     
        Morrigan m2 = new Girl(m0);    
        m2.display();   
    }}
//抽象构件角色：莫莉卡
interface Morrigan {  
    public void display();
}
//具体构件角色：原身
class original extends JFrame implements Morrigan {  
    private static final long serialVersionUID = 1L;  
    private String t = "Morrigan0.jpg"; 
    public original() {      
        super("《恶魔战士》中的莫莉卡·安斯兰");  
    }  
    public void setImage(String t) {  
        this.t = t;  
    }   
    public void display() {     
        this.setLayout(new FlowLayout());   
        JLabel l1 = new JLabel(new ImageIcon("src/decorator/" + t));    
        this.add(l1);      
        this.pack();  
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);    
        this.setVisible(true);  
    }}
//抽象装饰角色：变形
class Changer implements Morrigan {
    Morrigan m;  
    public Changer(Morrigan m) {  
        this.m = m;  
    }   
    public void display() {    
        m.display(); 
    }}
//具体装饰角色：女妖
class Succubus extends Changer { 
    public Succubus(Morrigan m) {   
        super(m);    
    } 
    public void display() { 
        setChanger();   
        super.display();
    }   
    public void setChanger() {   
        ((original) super.m).setImage("Morrigan1.jpg");
    }}
//具体装饰角色：少女
class Girl extends Changer {  
    public Girl(Morrigan m) {  
        super(m); 
    }    
    public void display() {     
        setChanger();
        super.display();  
    }  
    public void setChanger() { 
        ((original) super.m).setImage("Morrigan2.jpg");  
    }}
```




## 4.4 装饰器模式的应用场景

前面讲解了关于装饰器模式的结构与特点，下面介绍其适用的应用场景，装饰器模式通常在以下几种情况使用。

- 当需要给一个现有类添加附加职责，而又不能采用生成子类的方法进行扩充时。例如，该类被隐藏或者该类是终极类或者采用继承方式会产生大量的子类。
- 当需要通过对现有的一组基本功能进行排列组合而产生非常多的功能时，采用继承关系很难实现，而采用装饰器模式却很好实现。
- 当对象的功能要求可以动态地添加，也可以再动态地撤销时。


装饰器模式在Java语言中的最著名的应用莫过于 Java I/O 标准库的设计了。例如，InputStream 的子类 FilterInputStream，OutputStream 的子类 FilterOutputStream，Reader 的子类 BufferedReader 以及 FilterReader，还有 Writer 的子类 BufferedWriter、FilterWriter 以及 PrintWriter 等，它们都是抽象装饰类。

下面代码是为 FileReader 增加缓冲区而采用的装饰类 BufferedReader 的例子：

```java
BufferedReader in = new BufferedReader(new FileReader("filename.txt"));
String s = in.readLine();
```

# 5 外观模式

在现实生活中，常常存在办事较复杂的例子，如办房产证或注册一家公司，有时要同多个部门联系，这时要是有一个综合部门能解决一切手续问题就好了。

软件设计也是这样，当一个系统的功能越来越强，子系统会越来越多，客户对系统的访问也变得越来越复杂。这时如果系统内部发生改变，客户端也要跟着改变，这违背了“开闭原则”，也违背了“迪米特法则”，所以有必要为多个子系统提供一个统一的接口，从而降低系统的耦合度，这就是外观模式的目标。

下图给出了客户去当地房产局办理房产证过户要遇到的相关部门：



![办理房产证过户的相关部门](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260203159.gif)


## 5.1 外观模式的定义与特点

外观（Facade）模式又叫作门面模式，是一种通过为多个复杂的子系统提供一个一致的接口，而使这些子系统更加容易被访问的模式。该模式对外有一个统一接口，外部应用程序不用关心内部子系统的具体细节，这样会大大降低应用程序的复杂度，提高了程序的可维护性。

在日常编码工作中，我们都在有意无意的大量使用外观模式。只要是高层模块需要调度多个子系统（2个以上的类对象），我们都会自觉地创建一个新的类封装这些子系统，提供精简的接口，让高层模块可以更加容易地间接调用这些子系统的功能。尤其是现阶段各种第三方SDK、开源类库，很大概率都会使用外观模式。

外观（Facade）模式是“迪米特法则”的典型应用，它有以下主要优点。

1. 降低了子系统与客户端之间的耦合度，使得子系统的变化不会影响调用它的客户类。
2. 对客户屏蔽了子系统组件，减少了客户处理的对象数目，并使得子系统使用起来更加容易。
3. 降低了大型软件系统中的编译依赖性，简化了系统在不同平台之间的移植过程，因为编译一个子系统不会影响其他的子系统，也不会影响外观对象。


外观（Facade）模式的主要缺点如下。

1. 不能很好地限制客户使用子系统类，很容易带来未知风险。
2. 增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。

## 5.2 外观模式的结构与实现

外观（Facade）模式的结构比较简单，主要是定义了一个高层接口。它包含了对各个子系统的引用，客户端可以通过它访问各个子系统的功能。现在来分析其基本结构和实现方法。

#### 5.2.1. 模式的结构

外观（Facade）模式包含以下主要角色。

1. 外观（Facade）角色：为多个子系统对外提供一个共同的接口。
2. 子系统（Sub System）角色：实现系统的部分功能，客户可以通过外观角色访问它。
3. 客户（Client）角色：通过一个外观角色访问各个子系统的功能。


其结构图如下图所示：



![外观模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260146632.gif)


#### 5.2.2. 模式的实现

外观模式的实现代码如下：

```java
package facade;
public class FacadePattern {  
    public static void main(String[] args) {  
        Facade f = new Facade();  
        f.method();   
    }}
//外观角色
class Facade {   
    private SubSystem01 obj1 = new SubSystem01(); 
    private SubSystem02 obj2 = new SubSystem02(); 
    private SubSystem03 obj3 = new SubSystem03();
    public void method() {       
        obj1.method1();     
        obj2.method2();     
        obj3.method3();   
    }}
//子系统角色
class SubSystem01 {  
    public void method1() {   
        System.out.println("子系统01的method1()被调用！"); 
    }}
//子系统角色
class SubSystem02 {  
    public void method2() { 
        System.out.println("子系统02的method2()被调用！");  
    }}
//子系统角色
class SubSystem03 {
    public void method3() { 
        System.out.println("子系统03的method3()被调用！"); 
    }}
```


程序运行结果如下：

```
子系统01的method1()被调用！
子系统02的method2()被调用！
子系统03的method3()被调用！
```

## 5.3 外观模式的应用实例

**【例】用“外观模式”设计一个婺源特产的选购界面**

分析：本实例的外观角色 WySpecialty 是 JPanel 的子类，它拥有 8 个子系统角色 Specialty1~Specialty8，它们是图标类（ImageIcon）的子类对象，用来保存该婺源特产的图标。

外观类（WySpecialty）用 JTree 组件来管理婺源特产的名称，并定义一个事件处理方法 valueClianged(TreeSelectionEvent e)，当用户从树中选择特产时，该特产的图标对象保存在标签（JLabd）对象中。

客户窗体对象用分割面板来实现，左边放外观角色的目录树，右边放显示所选特产图像的标签。其结构图如下图所示：



![婺源特产管理界面的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260205188.gif)

程序代码如下：

```java
package facade;
import java.awt.*;
import javax.swing.*;
import javax.swing.event.*;
import javax.swing.tree.DefaultMutableTreeNode;
public class WySpecialtyFacade { 
    public static void main(String[] args) {   
        JFrame f = new JFrame("外观模式: 婺源特产选择测试");   
        Container cp = f.getContentPane(); 
        WySpecialty wys = new WySpecialty();  
        JScrollPane treeView = new JScrollPane(wys.tree);   
        JScrollPane scrollpane = new JScrollPane(wys.label);    
        JSplitPane splitpane = new JSplitPane(JSplitPane.HORIZONTAL_SPLIT, true, treeView, scrollpane); 
        //分割面版     
        splitpane.setDividerLocation(230);   
        //设置splitpane的分隔线位置      
        splitpane.setOneTouchExpandable(true); 
        //设置splitpane可以展开或收起         
        cp.add(splitpane);      
        f.setSize(650, 350);     
        f.setVisible(true);     
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
    }}
class WySpecialty extends JPanel implements TreeSelectionListener {  
    private static final long serialVersionUID = 1L; 
    final JTree tree; 
    JLabel label;
    private Specialty1 s1 = new Specialty1();  
    private Specialty2 s2 = new Specialty2(); 
    private Specialty3 s3 = new Specialty3();  
    private Specialty4 s4 = new Specialty4();   
    private Specialty5 s5 = new Specialty5();   
    private Specialty6 s6 = new Specialty6(); 
    private Specialty7 s7 = new Specialty7(); 
    private Specialty8 s8 = new Specialty8();   
    WySpecialty() {  
        DefaultMutableTreeNode top = new DefaultMutableTreeNode("婺源特产");    
        DefaultMutableTreeNode node1 = null, node2 = null, tempNode = null;   
        node1 = new DefaultMutableTreeNode("婺源四大特产（红、绿、黑、白）");     
        tempNode = new DefaultMutableTreeNode("婺源荷包红鲤鱼");  
        node1.add(tempNode);    
        tempNode = new DefaultMutableTreeNode("婺源绿茶");    
        node1.add(tempNode);    
        tempNode = new DefaultMutableTreeNode("婺源龙尾砚");   
        node1.add(tempNode);    
        tempNode = new DefaultMutableTreeNode("婺源江湾雪梨");  
        node1.add(tempNode);    
        top.add(node1);       
        node2 = new DefaultMutableTreeNode("婺源其它土特产");   
        tempNode = new DefaultMutableTreeNode("婺源酒糟鱼");    
        node2.add(tempNode);      
        tempNode = new DefaultMutableTreeNode("婺源糟米子糕");    
        node2.add(tempNode); 
        tempNode = new DefaultMutableTreeNode("婺源清明果");      
        node2.add(tempNode); 
        tempNode = new DefaultMutableTreeNode("婺源油煎灯");     
        node2.add(tempNode); 
        top.add(node2);    
        tree = new JTree(top);     
        tree.addTreeSelectionListener(this);  
        label = new JLabel();   
    }  
    public void valueChanged(TreeSelectionEvent e) {   
        if (e.getSource() == tree) {  
            DefaultMutableTreeNode node = (DefaultMutableTreeNode) tree.getLastSelectedPathComponent(); 
            if (node == null) return;       
            if (node.isLeaf()) {       
                Object object = node.getUserObject();        
                String sele = object.toString();    
                label.setText(sele);         
                label.setHorizontalTextPosition(JLabel.CENTER);     
                label.setVerticalTextPosition(JLabel.BOTTOM);       
                sele = sele.substring(2, 4);         
                if (sele.equalsIgnoreCase("荷包")) label.setIcon(s1);       
                else if (sele.equalsIgnoreCase("绿茶")) label.setIcon(s2);     
                else if (sele.equalsIgnoreCase("龙尾")) label.setIcon(s3);      
                else if (sele.equalsIgnoreCase("江湾")) label.setIcon(s4);      
                else if (sele.equalsIgnoreCase("酒糟")) label.setIcon(s5);        
                else if (sele.equalsIgnoreCase("糟米")) label.setIcon(s6);      
                else if (sele.equalsIgnoreCase("清明")) label.setIcon(s7);          
                else if (sele.equalsIgnoreCase("油煎")) label.setIcon(s8);      
                label.setHorizontalAlignment(JLabel.CENTER);    
            }     
        }   
    }}
class Specialty1 extends ImageIcon { 
    private static final long serialVersionUID = 1L; 
    Specialty1() {     
        super("src/facade/WyImage/Specialty11.jpg");  
    }}
class Specialty2 extends ImageIcon {   
    private static final long serialVersionUID = 1L;    
    Specialty2() {     
        super("src/facade/WyImage/Specialty12.jpg");  
    }}
class Specialty3 extends ImageIcon { 
    private static final long serialVersionUID = 1L;   
    Specialty3() {      
        super("src/facade/WyImage/Specialty13.jpg"); 
    }}
class Specialty4 extends ImageIcon {   
    private static final long serialVersionUID = 1L;   
    Specialty4() {     
        super("src/facade/WyImage/Specialty14.jpg"); 
    }}
class Specialty5 extends ImageIcon {  
    private static final long serialVersionUID = 1L;    
    Specialty5() {      
        super("src/facade/WyImage/Specialty21.jpg");  
    }}
class Specialty6 extends ImageIcon {  
    private static final long serialVersionUID = 1L;  
    Specialty6() {      
        super("src/facade/WyImage/Specialty22.jpg");   
    }}class Specialty7 extends ImageIcon {   
    private static final long serialVersionUID = 1L;  
    Specialty7() {    
        super("src/facade/WyImage/Specialty23.jpg");  
    }}
class Specialty8 extends ImageIcon {  
    private static final long serialVersionUID = 1L;  
    Specialty8() {      
        super("src/facade/WyImage/Specialty24.jpg");  
    }}
```





## 5.4 外观模式的应用场景

通常在以下情况下可以考虑使用外观模式。

1. 对分层结构系统构建时，使用外观模式定义子系统中每层的入口点可以简化子系统之间的依赖关系。
2. 当一个复杂系统的子系统很多时，外观模式可以为系统设计一个简单的接口供外界访问。
3. 当客户端与多个子系统之间存在很大的联系时，引入外观模式可将它们分离，从而提高子系统的独立性和可移植性。

# 6.享元模式

在面向对象程序设计过程中，有时会面临要创建大量相同或相似对象实例的问题。创建那么多的对象将会耗费很多的系统资源，它是系统性能提高的一个瓶颈。

例如，围棋和五子棋中的黑白棋子，图像中的坐标点或颜色，局域网中的路由器、交换机和集线器，教室里的桌子和凳子等。这些对象有很多相似的地方，如果能把它们相同的部分提取出来共享，则能节省大量的系统资源，这就是享元模式的产生背景。

## 6.1 享元模式的定义与特点

享元（Flyweight）模式的定义：运用共享技术来有效地支持大量细粒度对象的复用。它通过共享已经存在的对象来大幅度减少需要创建的对象数量、避免大量相似类的开销，从而提高系统资源的利用率。

享元模式的主要优点是：相同对象只要保存一份，这降低了系统中对象的数量，从而降低了系统中细粒度对象给内存带来的压力。

其主要缺点是：

1. 为了使对象可以共享，需要将一些不能共享的状态外部化，这将增加程序的复杂性。
2. 读取享元模式的外部状态会使得运行时间稍微变长。

## 6.2 享元模式的结构与实现

享元模式的定义提出了两个要求，细粒度和共享对象。因为要求细粒度，所以不可避免地会使对象数量多且性质相近，此时我们就将这些对象的信息分为两个部分：内部状态和外部状态。

- 内部状态指对象共享出来的信息，存储在享元信息内部，并且不回随环境的改变而改变；
- 外部状态指对象得以依赖的一个标记，随环境的改变而改变，不可共享。

比如，连接池中的连接对象，保存在连接对象中的用户名、密码、连接URL等信息，在创建对象的时候就设置好了，不会随环境的改变而改变，这些为内部状态。而当每个连接要被回收利用时，我们需要将它标记为可用状态，这些为外部状态。

享元模式的本质是缓存共享对象，降低内存消耗。

#### 6.2.1. 模式的结构

享元模式的主要角色有如下。

1. 抽象享元角色（Flyweight）：是所有的具体享元类的基类，为具体享元规范需要实现的公共接口，非享元的外部状态以参数的形式通过方法传入。
2. 具体享元（Concrete Flyweight）角色：实现抽象享元角色中所规定的接口。
3. 非享元（Unsharable Flyweight)角色：是不可以共享的外部状态，它以参数的形式注入具体享元的相关方法中。
4. 享元工厂（Flyweight Factory）角色：负责创建和管理享元角色。当客户对象请求一个享元对象时，享元工厂检査系统中是否存在符合要求的享元对象，如果存在则提供给客户；如果不存在的话，则创建一个新的享元对象。


下图是享元模式的结构图，其中：

- UnsharedConcreteFlyweight 是非享元角色，里面包含了非共享的外部状态信息 info；
- Flyweight 是抽象享元角色，里面包含了享元方法 operation(UnsharedConcreteFlyweight state)，非享元的外部状态以参数的形式通过该方法传入；
- ConcreteFlyweight 是具体享元角色，包含了关键字 key，它实现了抽象享元接口；
- FlyweightFactory 是享元工厂角色，它是关键字 key 来管理具体享元；
- 客户角色通过享元工厂获取具体享元，并访问具体享元的相关方法。



![享元模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260210558.gif)


#### 6.2.2. 模式的实现

享元模式的实现代码如下：

```java
public class FlyweightPattern { 
    public static void main(String[] args) {  
        FlyweightFactory factory = new FlyweightFactory();    
        Flyweight f01 = factory.getFlyweight("a"); 
        Flyweight f02 = factory.getFlyweight("a"); 
        Flyweight f03 = factory.getFlyweight("a"); 
        Flyweight f11 = factory.getFlyweight("b");  
        Flyweight f12 = factory.getFlyweight("b");       
        f01.operation(new UnsharedConcreteFlyweight("第1次调用a。"));  
        f02.operation(new UnsharedConcreteFlyweight("第2次调用a。")); 
        f03.operation(new UnsharedConcreteFlyweight("第3次调用a。"));    
        f11.operation(new UnsharedConcreteFlyweight("第1次调用b。")); 
        f12.operation(new UnsharedConcreteFlyweight("第2次调用b。"));  
    }}
//非享元角色
class UnsharedConcreteFlyweight {  
    private String info;  
    UnsharedConcreteFlyweight(String info) { 
        this.info = info;  
    } 
    public String getInfo() {  
        return info;    
    }  
    public void setInfo(String info) { 
        this.info = info;  
    }}
//抽象享元角色
interface Flyweight { 
    public void operation(UnsharedConcreteFlyweight state);}
//具体享元角色
class ConcreteFlyweight implements Flyweight { 
    private String key;  
    ConcreteFlyweight(String key) {  
        this.key = key;   
        System.out.println("具体享元" + key + "被创建！"); 
    }   
    public void operation(UnsharedConcreteFlyweight outState) {     
        System.out.print("具体享元" + key + "被调用，");   
        System.out.println("非享元信息是:" + outState.getInfo());   
    }}
//享元工厂角色
class FlyweightFactory { 
    private HashMap<String, Flyweight> flyweights = new HashMap<String, Flyweight>();   
    public Flyweight getFlyweight(String key) {  
        Flyweight flyweight = (Flyweight) flyweights.get(key);      
        if (flyweight != null) {      
            System.out.println("具体享元" + key + "已经存在，被成功获取！");  
        } else {        
            flyweight = new ConcreteFlyweight(key);    
            flyweights.put(key, flyweight); 
        }       
        return flyweight;  
    }}
```

程序运行结果如下：

```
具体享元a被创建！
具体享元a已经存在，被成功获取！
具体享元a已经存在，被成功获取！
具体享元b被创建！
具体享元b已经存在，被成功获取！
具体享元a被调用，非享元信息是:第1次调用a。
具体享元a被调用，非享元信息是:第2次调用a。
具体享元a被调用，非享元信息是:第3次调用a。
具体享元b被调用，非享元信息是:第1次调用b。
具体享元b被调用，非享元信息是:第2次调用b。
```

## 6.3 享元模式的应用实例

**【例】享元模式在五子棋游戏中的应用**

分析：五子棋同围棋一样，包含多个“黑”或“白”颜色的棋子，所以用享元模式比较好。

本实例中:

- 棋子（ChessPieces）类是抽象享元角色，它包含了一个落子的 DownPieces(Graphics g,Point pt) 方法；
- 白子（WhitePieces）和黑子（BlackPieces）类是具体享元角色，它实现了落子方法；
- Point 是非享元角色，它指定了落子的位置；
- WeiqiFactory 是享元工厂角色，它通过 ArrayList 来管理棋子，并且提供了获取白子或者黑子的 getChessPieces(String type) 方法；
- 客户类（Chessboard）利用 Graphics 组件在框架窗体中绘制一个棋盘，并实现 mouseClicked(MouseEvent e) 事件处理方法，该方法根据用户的选择从享元工厂中获取白子或者黑子并落在棋盘上。


下图是其结构图：



![五子棋游戏的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260212790.gif)

程序代码如下：

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.util.ArrayList;
public class WzqGame {  
    public static void main(String[] args) {      
        new Chessboard();  
    }}
//棋盘
class Chessboard extends MouseAdapter {  
    WeiqiFactory wf;   
    JFrame f;   
    Graphics g;   
    JRadioButton wz;    
    JRadioButton bz;  
    private final int x = 50;   
    private final int y = 50;   
    private final int w = 40;  
    //小方格宽度和高度  
    private final int rw = 400;   
    //棋盘宽度和高度  
    Chessboard() {       
        wf = new WeiqiFactory();    
        f = new JFrame("享元模式在五子棋游戏中的应用");   
        f.setBounds(100, 100, 500, 550);  
        f.setVisible(true);        f.setResizable(false);        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);        JPanel SouthJP = new JPanel();        f.add("South", SouthJP);        wz = new JRadioButton("白子");        bz = new JRadioButton("黑子", true);        ButtonGroup group = new ButtonGroup();        group.add(wz);        group.add(bz);        SouthJP.add(wz);        SouthJP.add(bz);        JPanel CenterJP = new JPanel();        CenterJP.setLayout(null);        CenterJP.setSize(500, 500);        CenterJP.addMouseListener(this);        f.add("Center", CenterJP);        try {            Thread.sleep(500);        } catch (InterruptedException e) {            e.printStackTrace();        }        g = CenterJP.getGraphics();        g.setColor(Color.BLUE);        g.drawRect(x, y, rw, rw);        for (int i = 1; i < 10; i++) {            //绘制第i条竖直线            g.drawLine(x + (i * w), y, x + (i * w), y + rw);            //绘制第i条水平线            g.drawLine(x, y + (i * w), x + rw, y + (i * w));        }    }    public void mouseClicked(MouseEvent e) {        Point pt = new Point(e.getX() - 15, e.getY() - 15);        if (wz.isSelected()) {            ChessPieces c1 = wf.getChessPieces("w");            c1.DownPieces(g, pt);        } else if (bz.isSelected()) {            ChessPieces c2 = wf.getChessPieces("b");            c2.DownPieces(g, pt);        }    }}//抽象享元角色：棋子interface ChessPieces {    public void DownPieces(Graphics g, Point pt);    //下子}//具体享元角色：白子class WhitePieces implements ChessPieces {    public void DownPieces(Graphics g, Point pt) {        g.setColor(Color.WHITE);        g.fillOval(pt.x, pt.y, 30, 30);    }}//具体享元角色：黑子class BlackPieces implements ChessPieces {    public void DownPieces(Graphics g, Point pt) {        g.setColor(Color.BLACK);        g.fillOval(pt.x, pt.y, 30, 30);    }}//享元工厂角色class WeiqiFactory {    private ArrayList<ChessPieces> qz;    public WeiqiFactory() {        qz = new ArrayList<ChessPieces>();        ChessPieces w = new WhitePieces();        qz.add(w);        ChessPieces b = new BlackPieces();        qz.add(b);    }    public ChessPieces getChessPieces(String type) {        if (type.equalsIgnoreCase("w")) {            return (ChessPieces) qz.get(0);        } else if (type.equalsIgnoreCase("b")) {            return (ChessPieces) qz.get(1);        } else {            return null;        }    }}
```

程序运行结果如图 3 所示。

![五子棋游戏的运行结果](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260146574.gif)
图3 五子棋游戏的运行结果

## 6.4 享元模式的应用场景

当系统中多处需要同一组信息时，可以把这些信息封装到一个对象中，然后对该对象进行缓存，这样，一个对象就可以提供给多出需要使用的地方，避免大量同一对象的多次创建，降低大量内存空间的消耗。

享元模式其实是工厂方法模式的一个改进机制，享元模式同样要求创建一个或一组对象，并且就是通过工厂方法模式生成对象的，只不过享元模式为工厂方法模式增加了缓存这一功能。

前面分析了享元模式的结构与特点，下面分析它适用的应用场景。享元模式是通过减少内存中对象的数量来节省内存空间的，所以以下几种情形适合采用享元模式：

1. 系统中存在大量相同或相似的对象，这些对象耗费大量的内存资源。

2. 大部分的对象可以按照内部状态进行分组，且可将不同部分外部化，这样每一个组只需保存一个内部状态。

3. 由于享元模式需要额外维护一个保存享元的数据结构，所以应当在有足够多的享元实例时才值得使用享元模式。

   

# 7.组合模式

在现实生活中，存在很多“部分-整体”的关系，例如，大学中的部门与学院、总公司中的部门与分公司、学习用品中的书与书包、生活用品中的衣服与衣柜、以及厨房中的锅碗瓢盆等。在软件开发中也是这样，例如，文件系统中的文件与文件夹、窗体程序中的简单控件与容器控件等。对这些简单对象与复合对象的处理，如果用组合模式来实现会很方便。

## 7.1 组合模式的定义与特点

组合（Composite Pattern）模式的定义：有时又叫作整体-部分（Part-Whole）模式，它是一种将对象组合成树状的层次结构的模式，用来表示“整体-部分”的关系，使用户对单个对象和组合对象具有一致的访问性，属于结构型设计模式。

组合模式一般用来描述整体与部分的关系，它将对象组织到树形结构中，顶层的节点被称为根节点，根节点下面可以包含树枝节点和叶子节点，树枝节点下面又可以包含树枝节点和叶子节点，树形结构图如下：

![组合模式树形结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260147234.png)


由上图可以看出，其实根节点和树枝节点本质上属于同一种数据类型，可以作为容器使用；而叶子节点与树枝节点在语义上不属于用一种类型。但是在组合模式中，会把树枝节点和叶子节点看作属于同一种数据类型（用统一接口定义），让它们具备一致行为。

这样，在组合模式中，整个树形结构中的对象都属于同一种类型，带来的好处就是用户不需要辨别是树枝节点还是叶子节点，可以直接进行操作，给用户的使用带来极大的便利。

组合模式的主要优点有：

1. 组合模式使得客户端代码可以一致地处理单个对象和组合对象，无须关心自己处理的是单个对象，还是组合对象，这简化了客户端代码；
2. 更容易在组合体内加入新的对象，客户端不会因为加入了新的对象而更改源代码，满足“开闭原则”；


其主要缺点是：

1. 设计较复杂，客户端需要花更多时间理清类之间的层次关系；
2. 不容易限制容器中的构件；
3. 不容易用继承的方法来增加构件的新功能。

## 7.2 组合模式的结构与实现

组合模式的结构不是很复杂，下面对它的结构和实现进行分析。

### 7.2.1 模式的结构

组合模式包含以下主要角色。

1. 抽象构件（Component）角色：它的主要作用是为树叶构件和树枝构件声明公共接口，并实现它们的默认行为。在透明式的组合模式中抽象构件还声明访问和管理子类的接口；在安全式的组合模式中不声明访问和管理子类的接口，管理工作由树枝构件完成。（总的抽象类或接口，定义一些通用的方法，比如新增、删除）
2. 树叶构件（Leaf）角色：是组合中的叶节点对象，它没有子节点，用于继承或实现抽象构件。
3. 树枝构件（Composite）角色 / 中间构件：是组合中的分支节点对象，它有子节点，用于继承和实现抽象构件。它的主要作用是存储和管理子部件，通常包含 Add()、Remove()、GetChild() 等方法。


组合模式分为透明式的组合模式和安全式的组合模式。

#### 7.2.1.1 透明方式

在该方式中，由于抽象构件声明了所有子类中的全部方法，所以客户端无须区别树叶对象和树枝对象，对客户端来说是透明的。但其缺点是：树叶构件本来没有 Add()、Remove() 及 GetChild() 方法，却要实现它们（空实现或抛异常），这样会带来一些安全性问题。其结构图如下图所示：



![透明式的组合模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260220378.gif)


#### 7.2.1.2 安全方式

在该方式中，将管理子构件的方法移到树枝构件中，抽象构件和树叶构件没有对子对象的管理方法，这样就避免了上一种方式的安全性问题，但由于叶子和分支有不同的接口，客户端在调用时要知道树叶对象和树枝对象的存在，所以失去了透明性。其结构图如下图所示：



![安全式的组合模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260220206.gif)


### 7.2.2 模式的实现

假如要访问集合 c0={leaf1,{leaf2,leaf3}} 中的元素，其对应的树状图如图 3 所示。



![集合c0的树状图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260221999.gif)


#### 7.2.2.1 透明组合模式

下面为透明式的组合模式的实现代码：

```java
public class CompositePattern { 
    public static void main(String[] args) {  
        Component c0 = new Composite();     
        Component c1 = new Composite();     
        Component leaf1 = new Leaf("1");     
        Component leaf2 = new Leaf("2");      
        Component leaf3 = new Leaf("3");     
        c0.add(leaf1);     
        c0.add(c1);    
        c1.add(leaf2);     
        c1.add(leaf3);     
        c0.operation(); 
    }}
//抽象构件
interface Component { 
    public void add(Component c);   
    public void remove(Component c); 
    public Component getChild(int i);  
    public void operation();}
//树叶构件
class Leaf implements Component { 
    private String name;   
    public Leaf(String name) {   
        this.name = name;  
    }   
    public void add(Component c) { 
    }   
    public void remove(Component c) {
    }  
    public Component getChild(int i) {  
        return null;  
    }  
    public void operation() {     
        System.out.println("树叶" + name + "：被访问！");  
    }}
//树枝构件
class Composite implements Component {  
    private ArrayList<Component> children = new ArrayList<Component>();   
    public void add(Component c) {    
        children.add(c);   
    }  
    public void remove(Component c) {  
        children.remove(c); 
    }   
    public Component getChild(int i) {    
        return children.get(i);  
    }  
    public void operation() {   
        for (Object obj : children) {   
            ((Component) obj).operation();   
        }  
    }}
```

程序运行结果如下：

```
树叶1：被访问！
树叶2：被访问！
树叶3：被访问！
```

#### 7.2.2.2 安全组合模式

安全式的组合模式与透明式组合模式的实现代码类似，只要对其做简单修改就可以了，代码如下。

首先修改 Component 代码，只保留层次的公共行为。

```java
interface Component {    public void operation();}
```

然后修改客户端代码，将树枝构件类型更改为 Composite 类型，以便获取管理子类操作的方法。

```java
public class CompositePattern {   
    public static void main(String[] args) {     
        Composite c0 = new Composite();    
        Composite c1 = new Composite();    
        Component leaf1 = new Leaf("1");      
        Component leaf2 = new Leaf("2");    
        Component leaf3 = new Leaf("3");  
        c0.add(leaf1);    
        c0.add(c1);     
        c1.add(leaf2);     
        c1.add(leaf3);    
        c0.operation();  
    }}
```

## 7.3 组合模式的应用实例

**【例】用组合模式实现当用户在商店购物后，显示其所选商品信息，并计算所选商品总价的功能**

说明：假如李先生到韶关“天街e角”生活用品店购物，用 1 个红色小袋子装了 2 包婺源特产（单价 7.9 元）、1 张婺源地图（单价 9.9 元）；用 1 个白色小袋子装了 2 包韶关香藉（单价 68 元）和 3 包韶关红茶（单价 180 元）；用 1 个中袋子装了前面的红色小袋子和 1 个景德镇瓷器（单价 380 元）；用 1 个大袋子装了前面的中袋子、白色小袋子和 1 双李宁牌运动鞋（单价 198 元）。

最后“大袋子”中的内容有：{1 双李宁牌运动鞋（单价 198 元）、白色小袋子{2 包韶关香菇（单价 68 元）、3 包韶关红茶（单价 180 元）}、中袋子{1 个景德镇瓷器（单价 380 元）、红色小袋子{2 包婺源特产（单价 7.9 元）、1 张婺源地图（单价 9.9 元）}}}，现在要求编程显示李先生放在大袋子中的所有商品信息并计算要支付的总价。

本实例可按安全组合模式设计，其结构图如下图所示：



![韶关“天街e角”店购物的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260223100.gif)
程序代码如下：

```java
package composite;import java.util.ArrayList;public class ShoppingTest {  
    public static void main(String[] args) {   
        float s = 0;    
        Bags BigBag, mediumBag, smallRedBag, smallWhiteBag;    
        Goods sp;    
        BigBag = new Bags("大袋子"); 
        mediumBag = new Bags("中袋子");    
        smallRedBag = new Bags("红色小袋子");      
        smallWhiteBag = new Bags("白色小袋子");     
        sp = new Goods("婺源特产", 2, 7.9f);     
        smallRedBag.add(sp);       
        sp = new Goods("婺源地图", 1, 9.9f);  
        smallRedBag.add(sp);     
        sp = new Goods("韶关香菇", 2, 68);     
        smallWhiteBag.add(sp);     
        sp = new Goods("韶关红茶", 3, 180);  
        smallWhiteBag.add(sp);
        sp = new Goods("景德镇瓷器", 1, 380);      
        mediumBag.add(sp);       
        mediumBag.add(smallRedBag);    
        sp = new Goods("李宁牌运动鞋", 1, 198); 
        BigBag.add(sp);      
        BigBag.add(smallWhiteBag);    
        BigBag.add(mediumBag);    
        System.out.println("您选购的商品有：");   
        BigBag.show();     
        s = BigBag.calculation();    
        System.out.println("要支付的总价是：" + s + "元"); 
    }}
//抽象构件：物品
interface Articles { 
    public float calculation();
    //计算  
    public void show();}
//树叶构件：商品
class Goods implements Articles {
    private String name;    
    //名字 
    private int quantity; 
    //数量  
    private float unitPrice;
    //单价   
    public Goods(String name, int quantity, float unitPrice) {   
        this.name = name;     
        this.quantity = quantity;  
        this.unitPrice = unitPrice;
    }  
    public float calculation() { 
        return quantity * unitPrice;  
    }   
    public void show() {    
        System.out.println(name + "(数量：" + quantity + "，单价：" + unitPrice + "元)");   
    }}
//树枝构件：袋子
class Bags implements Articles { 
    private String name;     //名字  
    private ArrayList<Articles> bags = new ArrayList<Articles>(); 
    public Bags(String name) {   
        this.name = name;  
    }    public void add(Articles c) {  
        bags.add(c);   
    }  
    public void remove(Articles c) {  
        bags.remove(c);  
    }   
    public Articles getChild(int i) {   
        return bags.get(i);   
    }  
    public float calculation() {     
        float s = 0;     
        for (Object obj : bags) {      
            s += ((Articles) obj).calculation();   
        }      
        return s;   
    }    
    public void show() {    
        for (Object obj : bags) { 
            ((Articles) obj).show();  
        }   
     }}
```

程序运行结果如下：

```
您选购的商品有：
李宁牌运动鞋(数量：1，单价：198.0元)
韶关香菇(数量：2，单价：68.0元)
韶关红茶(数量：3，单价：180.0元)
景德镇瓷器(数量：1，单价：380.0元)
婺源特产(数量：2，单价：7.9元)
婺源地图(数量：1，单价：9.9元)
要支付的总价是：1279.7元
```

## 7.4 组合模式的应用场景

前面分析了组合模式的结构与特点，下面分析它适用的以下应用场景。

1. 在需要表示一个对象整体与部分的层次结构的场合。
2. 要求对用户隐藏组合对象与单个对象的不同，用户可以用统一的接口使用组合结构中的所有对象的场合。
