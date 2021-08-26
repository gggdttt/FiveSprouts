Introduction

行为型模式用于描述程序在运行时复杂的流程控制，即描述多个类或对象之间怎样相互协作共同完成单个对象都无法单独完成的任务，它涉及算法与对象间职责的分配。

行为型模式分为类行为模式和对象行为模式，前者采用继承机制来在类间分派行为，后者采用组合或聚合在对象间分配行为。由于组合关系或聚合关系比继承关系耦合度低，满足“合成复用原则”，所以对象行为模式比类行为模式具有更大的灵活性。

行为型模式是 GoF设计模式中最为庞大的一类，它包含以下 11 种模式：

1. 模板方法（Template Method）模式：定义一个操作中的算法骨架，将算法的一些步骤延迟到子类中，使得子类在可以不改变该算法结构的情况下重定义该算法的某些特定步骤
2. 策略（Strategy）模式：定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的改变不会影响使用算法的客户
3. 命令（Command）模式：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开
4. 职责链（Chain of Responsibility）模式：把请求从链中的一个对象传到下一个对象，直到请求被响应为止。通过这种方式去除对象之间的耦合
5. 状态（State）模式：允许一个对象在其内部状态发生改变时改变其行为能力
6. 观察者（Observer）模式：多个对象间存在一对多关系，当一个对象发生改变时，把这种改变通知给其他多个对象，从而影响其他对象的行为
7. 中介者（Mediator）模式：定义一个中介对象来简化原有对象之间的交互关系，降低系统中对象间的耦合度，使原有对象之间不必相互了解
8. 迭代器（Iterator）模式：提供一种方法来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示
9. 访问者（Visitor）模式：在不改变集合元素的前提下，为一个集合中的每个元素提供多种访问方式，即每个元素有多个访问者对象访问
10. 备忘录（Memento）模式：在不破坏封装性的前提下，获取并保存一个对象的内部状态，以便以后恢复它
11. 解释器（Interpreter）模式：提供如何定义语言的文法，以及对语言句子的解释方法，即解释器


以上 11 种行为型模式，除了模板方法模式和解释器模式是类行为型模式，其他的全部属于对象行为型模式，下面我们将详细介绍它们的特点、结构与应用。

# 1.模板方法（Template Method）模式

在面向对象程序设计过程中，程序员常常会遇到这种情况：设计一个系统时知道了算法所需的关键步骤，而且确定了这些步骤的执行顺序，但某些步骤的具体实现还未知，或者说某些步骤的实现与具体的环境相关。

例如，去银行办理业务一般要经过以下4个流程：取号、排队、办理具体业务、对银行工作人员进行评分等，其中取号、排队和对银行工作人员进行评分的业务对每个客户是一样的，可以在父类中实现，但是办理具体业务却因人而异，它可能是存款、取款或者转账等，可以延迟到子类中实现。

这样的例子在生活中还有很多，例如，一个人每天会起床、吃饭、做事、睡觉等，其中“做事”的内容每天可能不同。我们把这些规定了流程或格式的实例定义成模板，允许使用者根据自己的需求去更新它，例如，简历模板、论文模板、Word 中模板文件等。

以下介绍的模板方法模式将解决以上类似的问题。

## 1.1 模式的定义与特点

模板方法（Template Method）模式的定义如下：定义一个操作中的算法骨架，而将算法的一些步骤延迟到子类中，使得子类可以不改变该算法结构的情况下重定义该算法的某些特定步骤。它是一种类行为型模式。

该模式的主要优点如下：

1. 它封装了不变部分，扩展可变部分。它把认为是不变部分的算法封装到父类中实现，而把可变部分算法由子类继承实现，便于子类继续扩展
2. 它在父类中提取了公共的部分代码，便于代码复用
3. 部分方法是由子类实现的，因此子类可以通过扩展方式增加相应的功能，符合开闭原则


该模式的主要缺点如下：

1. 对每个不同的实现都需要定义一个子类，这会导致类的个数增加，系统更加庞大，设计也更加抽象，间接地增加了系统实现的复杂度
2. 父类中的抽象方法由子类实现，子类执行的结果会影响父类的结果，这导致一种反向的控制结构，它提高了代码阅读的难度
3. 由于继承关系自身的缺点，如果父类添加新的抽象方法，则所有子类都要改一遍

## 1.2 模式的结构与实现

模板方法模式需要注意抽象类与具体子类之间的协作。它用到了虚函数的多态性技术以及“不用调用我，让我来调用你”的反向控制技术。现在来介绍它们的基本结构。

### 1.2.1. 模式的结构

模板方法模式包含以下主要角色：

#### 1.2.1.1 抽象类/抽象模板（Abstract Class）

抽象模板类，负责给出一个算法的轮廓和骨架。它由一个模板方法和若干个基本方法构成。这些方法的定义如下。

① 模板方法：定义了算法的骨架，按某种顺序调用其包含的基本方法。

② 基本方法：是整个算法中的一个步骤，包含以下几种类型。

- 抽象方法：在抽象类中声明，由具体子类实现。
- 具体方法：在抽象类中已经实现，在具体子类中可以继承或重写它。
- 钩子方法：在抽象类中已经实现，包括用于判断的逻辑方法和需要子类重写的空方法两种。

#### 1.2.1.2 具体子类/具体实现（Concrete Class）

具体实现类，实现抽象类中所定义的抽象方法和钩子方法，它们是一个顶级逻辑的一个组成步骤。

模板方法模式的结构图如下图所示：



![模板方法模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260256161.gif)


### 1.2.2. 模式的实现

模板方法模式的代码如下：

```java
public class TemplateMethodPattern {  
    public static void main(String[] args) {    
        AbstractClass tm = new ConcreteClass();    
        tm.TemplateMethod();  
    }}
//抽象类
abstract class AbstractClass {  
    //模板方法  
    public void TemplateMethod() {   
        SpecificMethod();    
        abstractMethod1();      
        abstractMethod2();   
    }  
    //具体方法  
    public void SpecificMethod() {    
        System.out.println("抽象类中的具体方法被调用..."); 
    } 
    //抽象方法1 
    public abstract void abstractMethod1();  
    //抽象方法2 
    public abstract void abstractMethod2();}
//具体子类
class ConcreteClass extends AbstractClass { 
    public void abstractMethod1() {   
        System.out.println("抽象方法1的实现被调用...");
    }  
    public void abstractMethod2() {    
        System.out.println("抽象方法2的实现被调用...");  
    }}
```

程序的运行结果如下：

```
抽象类中的具体方法被调用...
抽象方法1的实现被调用...
抽象方法2的实现被调用...
```

## 1.3 模式的应用实例

**【例】用模板方法模式实现出国留学手续设计程序:**

分析：出国留学手续一般经过以下流程：索取学校资料，提出入学申请，办理因私出国护照、出境卡和公证，申请签证，体检、订机票、准备行装，抵达目标学校等，其中有些业务对各个学校是一样的，但有些业务因学校不同而不同，所以比较适合用模板方法模式来实现。

在本实例中，我们先定义一个出国留学的抽象类 StudyAbroad，里面包含了一个模板方法 TemplateMethod()，该方法中包含了办理出国留学手续流程中的各个基本方法，其中有些方法的处理由于各国都一样，所以在抽象类中就可以实现，但有些方法的处理各国是不同的，必须在其具体子类（如美国留学类 StudyInAmerica）中实现。如果再增加一个国家，只要增加一个子类就可以了，下图 所示是其结构图。



![出国留学手续设计程序的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260257134.gif)

程序代码如下：

```java
public class StudyAbroadProcess {    
    public static void main(String[] args) { 
        StudyAbroad tm = new StudyInAmerica();    
        tm.TemplateMethod(); 
    }}
//抽象类: 出国留学
abstract class StudyAbroad {  
    public void TemplateMethod() //模板方法   
    {      
        LookingForSchool(); //索取学校资料   
        ApplyForEnrol();    //入学申请     
        ApplyForPassport(); //办理因私出国护照、出境卡和公证      
        ApplyForVisa();     //申请签证    
        ReadyGoAbroad();    //体检、订机票、准备行装    
        Arriving();         //抵达  
    }  
    public void ApplyForPassport() {    
        System.out.println("三.办理因私出国护照、出境卡和公证：");    
        System.out.println("  1）持录取通知书、本人户口簿或身份证向户口所在地公安机关申请办理因私出国护照和出境卡。");   
        System.out.println("  2）办理出生公证书，学历、学位和成绩公证，经历证书，亲属关系公证，经济担保公证。"); 
    }  
    public void ApplyForVisa() {     
        System.out.println("四.申请签证：");        
        System.out.println("  1）准备申请国外境签证所需的各种资料，包括个人学历、成绩单、工作经历的证明；个人及家庭收入、资金和财产证明；家庭成员的关系证明等；");     
        System.out.println("  2）向拟留学国家驻华使(领)馆申请入境签证。申请时需按要求填写有关表格，递交必需的证明材料，缴纳签证。有的国家(比如美国、英国、加拿大等)在申请签证时会要求申请人前往使(领)馆进行面试。");  
    } 
    public void ReadyGoAbroad() {   
        System.out.println("五.体检、订机票、准备行装：");   
        System.out.println("  1）进行身体检查、免疫检查和接种传染病疫苗；");      
        System.out.println("  2）确定机票时间、航班和转机地点。");  
    }    
    public abstract void LookingForSchool();//索取学校资料  
    public abstract void ApplyForEnrol();   //入学申请  
    public abstract void Arriving();        //抵达
}
//具体子类: 美国留学
class StudyInAmerica extends StudyAbroad {    
    @Override 
    public void LookingForSchool() {     
        System.out.println("一.索取学校以下资料：");    
        System.out.println("  1）对留学意向国家的政治、经济、文化背景和教育体制、学术水平进行较为全面的了解；");  
        System.out.println("  2）全面了解和掌握国外学校的情况，包括历史、学费、学制、专业、师资配备、教学设施、学术地位、学生人数等；");    
        System.out.println("  3）了解该学校的住宿、交通、医疗保险情况如何；"); 
        System.out.println("  4）该学校在中国是否有授权代理招生的留学中介公司？");   
        System.out.println("  5）掌握留学签证情况；");    
        System.out.println("  6）该国政府是否允许留学生合法打工？");     
        System.out.println("  8）毕业之后可否移民？");    
        System.out.println("  9）文凭是否受到我国认可？"); 
    }  
    @Override   
    public void ApplyForEnrol() {     
        System.out.println("二.入学申请：");    
        System.out.println("  1）填写报名表；");  
        System.out.println("  2）将报名表、个人学历证明、最近的学习成绩单、推荐信、个人简历、托福或雅思语言考试成绩单等资料寄往所申请的学校；");      
        System.out.println("  3）为了给签证办理留有充裕的时间，建议越早申请越好，一般提前1年就比较从容。");  
    }   
    @Override  
    public void Arriving() {   
        System.out.println("六.抵达目标学校：");
        System.out.println("  1）安排住宿；");   
        System.out.println("  2）了解校园及周边环境。");
    }}
```

程序的运行结果如下：

```
一.索取学校以下资料：
  1）对留学意向国家的政治、经济、文化背景和教育体制、学术水平进行较为全面的了解；
  2）全面了解和掌握国外学校的情况，包括历史、学费、学制、专业、师资配备、教学设施、学术地位、学生人数等；
  3）了解该学校的住宿、交通、医疗保险情况如何；
  4）该学校在中国是否有授权代理招生的留学中介公司？
  5）掌握留学签证情况；
  6）该国政府是否允许留学生合法打工？
  8）毕业之后可否移民？
  9）文凭是否受到我国认可？
二.入学申请：
  1）填写报名表；
  2）将报名表、个人学历证明、最近的学习成绩单、推荐信、个人简历、托福或雅思语言考试成绩单等资料寄往所申请的学校；
  3）为了给签证办理留有充裕的时间，建议越早申请越好，一般提前1年就比较从容。
三.办理因私出国护照、出境卡和公证：
  1）持录取通知书、本人户口簿或身份证向户口所在地公安机关申请办理因私出国护照和出境卡。
  2）办理出生公证书，学历、学位和成绩公证，经历证书，亲属关系公证，经济担保公证。
四.申请签证：
  1）准备申请国外境签证所需的各种资料，包括个人学历、成绩单、工作经历的证明；个人及家庭收入、资金和财产证明；家庭成员的关系证明等；
  2）向拟留学国家驻华使(领)馆申请入境签证。申请时需按要求填写有关表格，递交必需的证明材料，缴纳签证。有的国家(比如美国、英国、加拿大等)在申请签证时会要求申请人前往使(领)馆进行面试。
五.体检、订机票、准备行装：
  1）进行身体检查、免疫检查和接种传染病疫苗；
  2）确定机票时间、航班和转机地点。
六.抵达目标学校：
  1）安排住宿；
  2）了解校园及周边环境。
```

## 1.4 模式的应用场景

模板方法模式通常适用于以下场景。

1. 算法的整体步骤很固定，但其中个别部分易变时，这时候可以使用模板方法模式，将容易变的部分抽象出来，供子类实现。
2. 当多个子类存在公共的行为时，可以将其提取出来并集中到一个公共父类中以避免代码重复。首先，要识别现有代码中的不同之处，并且将不同之处分离为新的操作。最后，用一个调用这些新的操作的模板方法来替换这些不同的代码。
3. 当需要控制子类的扩展时，模板方法只在特定点调用钩子操作，这样就只允许在这些点进行扩展。在面向对象程序设计过程中，程序员常常会遇到这种情况：设计一个系统时知道了算法所需的关键步骤，而且确定了这些步骤的执行顺序，但某些步骤的具体实现还未知，或者说某些步骤的实现与具体的环境相关。

# 2.策略（Strategy）模式

在现实生活中常常遇到实现某种目标存在多种策略可供选择的情况，例如，出行旅游可以乘坐飞机、乘坐火车、骑自行车或自己开私家车等，超市促销可以釆用打折、送商品、送积分等方法。

在软件开发中也常常遇到类似的情况，当实现某一个功能存在多种算法或者策略，我们可以根据环境或者条件的不同选择不同的算法或者策略来完成该功能，如数据排序策略有冒泡排序、选择排序、插入排序、二叉树排序等。

如果使用多重条件转移语句实现（即硬编码），不但使条件语句变得很复杂，而且增加、删除或更换算法要修改原代码，不易维护，违背开闭原则。如果采用策略模式就能很好解决该问题。

## 2.1 策略模式的定义与特点

策略（Strategy）模式的定义：该模式定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的变化不会影响使用算法的客户。策略模式属于对象行为模式，它通过对算法进行封装，把使用算法的责任和算法的实现分割开来，并委派给不同的对象对这些算法进行管理。

策略模式的主要优点如下:

1. 多重条件语句不易维护，而使用策略模式可以避免使用多重条件语句，如 if...else 语句、switch...case 语句。
2. 策略模式提供了一系列的可供重用的算法族，恰当使用继承可以把算法族的公共代码转移到父类里面，从而避免重复的代码。
3. 策略模式可以提供相同行为的不同实现，客户可以根据不同时间或空间要求选择不同的。
4. 策略模式提供了对开闭原则的完美支持，可以在不修改原代码的情况下，灵活增加新算法。
5. 策略模式把算法的使用放到环境类中，而算法的实现移到具体策略类中，实现了二者的分离。


其主要缺点如下:

1. 客户端必须理解所有策略算法的区别，以便适时选择恰当的算法类。
2. 策略模式造成很多的策略类，增加维护难度。

## 2.2 策略模式的结构与实现

策略模式是准备一组算法，并将这组算法封装到一系列的策略类里面，作为一个抽象策略类的子类。策略模式的重心不是如何实现算法，而是如何组织这些算法，从而让程序结构更加灵活，具有更好的维护性和扩展性，现在我们来分析其基本结构和实现方法。

#### 2.2.1. 模式的结构

策略模式的主要角色如下:

1. 抽象策略（Strategy）类：定义了一个公共接口，各种不同的算法以不同的方式实现这个接口，环境角色使用这个接口调用不同的算法，一般使用接口或抽象类实现。
2. 具体策略（Concrete Strategy）类：实现了抽象策略定义的接口，提供具体的算法实现。
3. 环境（Context）类：持有一个策略类的引用，最终给客户端调用。





![策略模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260250802.gif)


#### 2.2.2. 模式的实现

策略模式的实现代码如下：

```java
public class StrategyPattern {   
    public static void main(String[] args) {    
        Context c = new Context();       
        Strategy s = new ConcreteStrategyA();   
        c.setStrategy(s);     
        c.strategyMethod();     
        System.out.println("-----------------");  
        s = new ConcreteStrategyB();    
        c.setStrategy(s);    
        c.strategyMethod();  
    }}
//抽象策略类
interface Strategy {  
    public void strategyMethod();  
    //策略方法
}
//具体策略类A
class ConcreteStrategyA implements Strategy {  
    public void strategyMethod() {   
        System.out.println("具体策略A的策略方法被访问！");  
    }}
//具体策略类B
class ConcreteStrategyB implements Strategy { 
    public void strategyMethod() {     
        System.out.println("具体策略B的策略方法被访问！");   
    }}
//环境类
class Context {  
    private Strategy strategy;   
    public Strategy getStrategy() {  
        return strategy;   
    }  
    public void setStrategy(Strategy strategy) {  
        this.strategy = strategy;  
    }  
    public void strategyMethod() {   
        strategy.strategyMethod();  
    }}
```

程序运行结果如下：

```
具体策略A的策略方法被访问！
-----------------
具体策略B的策略方法被访问！
```

## 2.3 策略模式的应用实例

**【例】策略模式在“大闸蟹”做菜中的应用**

分析：关于大闸蟹的做法有很多种，我们以清蒸大闸蟹和红烧大闸蟹两种方法为例，介绍策略模式的应用。

首先，定义一个大闸蟹加工的抽象策略类（CrabCooking），里面包含了一个做菜的抽象方法 CookingMethod()；然后，定义清蒸大闸蟹（SteamedCrabs）和红烧大闸蟹（BraisedCrabs）的具体策略类，它们实现了抽象策略类中的抽象方法；本案例将具体策略类定义成 JLabel 的子类；最后，定义一个厨房（Kitchen）环境类，它具有设置和选择做菜策略的方法；客户类通过厨房类获取做菜策略，并把做菜结果图在窗体中显示出来，下图所示是其结构图：



![大闸蟹做菜策略的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260302042.gif)


程序代码如下：

```java
import java.awt.*;
import java.awt.event.*;
import javax.swing.*
    ;public class CrabCookingStrategy implements ItemListener {   
        private JFrame f;  
        private JRadioButton qz, hs;   
        private JPanel CenterJP, SouthJP;  
        private Kitchen cf;    //厨房   
        private CrabCooking qzx, hsx;    //大闸蟹加工者   
        CrabCookingStrategy() {    
            f = new JFrame("策略模式在大闸蟹做菜中的应用");    
            f.setBounds(100, 100, 500, 400);   
            f.setVisible(true);   
            f.setResizable(false);      
            f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);   
            SouthJP = new JPanel();    
            CenterJP = new JPanel();     
            f.add("South", SouthJP);       
            f.add("Center", CenterJP);      
            qz = new JRadioButton("清蒸大闸蟹"); 
            hs = new JRadioButton("红烧大闸蟹");   
            qz.addItemListener(this);      
            hs.addItemListener(this);     
            ButtonGroup group = new ButtonGroup();       
            group.add(qz);     
            group.add(hs);     
            SouthJP.add(qz);      
            SouthJP.add(hs);      
            //---------------------------------    
            cf = new Kitchen();    //厨房   
            qzx = new SteamedCrabs();    //清蒸大闸蟹类  
            hsx = new BraisedCrabs();    //红烧大闸蟹类  
        }   
        public void itemStateChanged(ItemEvent e) {   
            JRadioButton jc = (JRadioButton) e.getSource();    
            if (jc == qz) {       
                cf.setStrategy(qzx);    
                cf.CookingMethod(); //清蒸      
            } else if (jc == hs) {          
                cf.setStrategy(hsx);        
                cf.CookingMethod(); //红烧   
            }     
            CenterJP.removeAll();   
            CenterJP.repaint();      
            CenterJP.add((Component) cf.getStrategy());      
            f.setVisible(true);   
        }   
        public static void main(String[] args) {  
            new CrabCookingStrategy();  
        }}
//抽象策略类：大闸蟹加工类
interface CrabCooking {   
    public void CookingMethod(); 
    //做菜方法
}
//具体策略类：清蒸大闸蟹
class SteamedCrabs extends JLabel implements CrabCooking { 
    private static final long serialVersionUID = 1L;   
    public void CookingMethod() {   
        this.setIcon(new ImageIcon("src/strategy/SteamedCrabs.jpg"));      
        this.setHorizontalAlignment(CENTER); 
    }}
//具体策略类：红烧大闸蟹
class BraisedCrabs extends JLabel implements CrabCooking {  
    private static final long serialVersionUID = 1L;  
    public void CookingMethod() {    
        this.setIcon(new ImageIcon("src/strategy/BraisedCrabs.jpg"));      
        this.setHorizontalAlignment(CENTER);    
    }}
//环境类：厨房
class Kitchen {  
    private CrabCooking strategy; 
    //抽象策略   
    public void setStrategy(CrabCooking strategy) { 
        this.strategy = strategy;  
    }  
    public CrabCooking getStrategy() {   
        return strategy; 
    }   
    public void CookingMethod() {    
        strategy.CookingMethod();    //做菜  
    }}
```


**【例】用策略模式实现从韶关去婺源旅游的出行方式**

分析：从韶关去婺源旅游有以下几种出行方式：坐火车、坐汽车和自驾车，所以该实例用策略模式比较适合，下图所示是其结构图：



![婺源旅游结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260306510.gif)


## 2.4 策略模式的应用场景

策略模式在很多地方用到，如 Java SE 中的容器布局管理就是一个典型的实例，Java SE 中的每个容器都存在多种布局供用户选择。在程序设计中，通常在以下几种情况中使用策略模式较多。

1. 一个系统需要动态地在几种算法中选择一种时，可将每个算法封装到策略类中。
2. 一个类定义了多种行为，并且这些行为在这个类的操作中以多个条件语句的形式出现，可将每个条件分支移入它们各自的策略类中以代替这些条件语句。
3. 系统中各算法彼此完全独立，且要求对客户隐藏具体算法的实现细节时。
4. 系统要求使用算法的客户不应该知道其操作的数据时，可使用策略模式来隐藏与算法相关的数据结构。
5. 多个类只区别在表现行为不同，可以使用策略模式，在运行时动态选择具体要执行的行为。

# 3.命令（Command）模式

在软件开发系统中，“方法的请求者”与“方法的实现者”之间经常存在紧密的耦合关系，这不利于软件功能的扩展与维护。例如，想对方法进行“撤销、重做、记录”等处理都很不方便，因此“如何将方法的请求者与实现者解耦？”变得很重要，命令模式就能很好地解决这个问题。

在现实生活中，命令模式的例子也很多。比如看电视时，我们只需要轻轻一按遥控器就能完成频道的切换，这就是命令模式，将换台请求和换台处理完全解耦了。电视机遥控器（命令发送者）通过按钮（具体命令）来遥控电视机（命令接收者）。

再比如，我们去餐厅吃饭，菜单不是等到客人来了之后才定制的，而是已经预先配置好的。这样，客人来了就只需要点菜，而不是任由客人临时定制。餐厅提供的菜单就相当于把请求和处理进行了解耦，这就是命令模式的体现。

## 3.1 命令模式的定义与特点

命令（Command）模式的定义如下：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。这样两者之间通过命令对象进行沟通，这样方便将命令对象进行储存、传递、调用、增加与管理。

命令模式的主要优点如下：

1. 通过引入中间件（抽象接口）降低系统的耦合度
2. 扩展性良好，增加或删除命令非常方便。采用命令模式增加与删除命令不会影响其他类，且满足“开闭原则”
3. 可以实现宏命令。命令模式可以与组合模式结合，将多个命令装配成一个组合命令，即宏命令
4. 方便实现 Undo 和 Redo 操作。命令模式可以与后面介绍的备忘录模式结合，实现命令的撤销与恢复
5. 可以在现有命令的基础上，增加额外功能。比如日志记录，结合装饰器模式会更加灵活


其缺点是：

1. 可能产生大量具体的命令类。因为每一个具体操作都需要设计一个具体命令类，这会增加系统的复杂性
2. 命令模式的结果其实就是接收方的执行结果，但是为了以命令的形式进行架构、解耦请求与实现，引入了额外类型结构（引入了请求方与抽象命令接口），增加了理解上的困难。不过<u>这也是设计模式的通病，抽象必然会额外增加类的数量</u>，代码抽离肯定比代码聚合更加难理解。

## 3.2 命令模式的结构与实现

可以将系统中的相关操作抽象成命令，使调用者与实现者相关分离，其结构如下。

#### 3.2.1. 模式的结构

命令模式包含以下主要角色：

1. 抽象命令类（Command）角色：声明执行命令的接口，拥有执行命令的抽象方法 execute()。
2. 具体命令类（Concrete Command）角色：是抽象命令类的具体实现类，它拥有接收者对象，并通过调用接收者的功能来完成命令要执行的操作。
3. 实现者/接收者（Receiver）角色：执行命令功能的相关操作，是具体命令对象业务的真正实现者。
4. 调用者/请求者（Invoker）角色：是请求的发送者，它通常拥有很多的命令对象，并通过访问命令对象来执行相关请求，它不直接访问接收者。


其结构图如下图所示：



![命令模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260251591.gif)

#### 3.2.2. 模式的实现

命令模式的代码如下：

```java
package command;
public class CommandPattern {  
    public static void main(String[] args) {  
        Command cmd = new ConcreteCommand();  
        Invoker ir = new Invoker(cmd);     
        System.out.println("客户访问调用者的call()方法...");    
        ir.call();    
    }}
//调用者
class Invoker {  
    private Command command;  
    public Invoker(Command command) { 
        this.command = command;  
    }  
    public void setCommand(Command command) { 
        this.command = command;   
    }  
    public void call() {  
        System.out.println("调用者执行命令command...");   
        command.execute();  
    }}
//抽象命令
interface Command {  
    public abstract void execute();}
//具体命令
class ConcreteCommand implements Command { 
    private Receiver receiver;  
    ConcreteCommand() {   
        receiver = new Receiver();   
    }   
    public void execute() { 
        receiver.action();   
    }}
//接收者
class Receiver {    
    public void action() {  
        System.out.println("接收者的action()方法被调用..."); 
    }}
```

程序的运行结果如下：

```
客户访问调用者的call()方法...
调用者执行命令command...
接收者的action()方法被调用...
```

## 3.3 命令模式的应用实例

**【例】用命令模式实现客户去餐馆吃早餐的实例**

分析：客户去餐馆可选择的早餐有肠粉、河粉和馄饨等，客户可向服务员选择以上早餐中的若干种，服务员将客户的请求交给相关的厨师去做。这里的点早餐相当于“命令”，服务员相当于“调用者”，厨师相当于“接收者”，所以用命令模式实现比较合适。

- 首先，定义一个早餐类（Breakfast），它是抽象命令类，有抽象方法 cooking()，说明要做什么
- 再定义其子类肠粉类（ChangFen）、馄饨类（HunTun）和河粉类（HeFen），它们是具体命令类，实现早餐类的 cooking() 方法，但它们不会具体做，而是交给具体的厨师去做
- 具体厨师类有肠粉厨师（ChangFenChef）、馄饨厨师（HunTunChef）和河粉厨师（HeFenChef），他们是命令的接收者


本实例把每个厨师类定义为 JFrame 的子类；最后，定义服务员类（Waiter），它接收客户的做菜请求，并发出做菜的命令。客户类是通过服务员类来点菜的，下图所示是其结构图。



![客户在餐馆吃早餐的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q1161134341E.gif)

程序代码如下：

```java
package command;
import javax.swing.*;
public class CookingCommand {  
    public static void main(String[] args) {   
        Breakfast food1 = new ChangFen();      
        Breakfast food2 = new HunTun();     
        Breakfast food3 = new HeFen();    
        Waiter fwy = new Waiter();     
        fwy.setChangFen(food1);//设置肠粉菜单   
        fwy.setHunTun(food2);  //设置河粉菜单       
        fwy.setHeFen(food3);   //设置馄饨菜单     
        fwy.chooseChangFen();  //选择肠粉      
        fwy.chooseHeFen();     //选择河粉       
        fwy.chooseHunTun();    //选择馄饨   
    }}
//调用者：服务员
class Waiter {  
    private Breakfast changFen, hunTun, heFen;   
    public void setChangFen(Breakfast f) {   
        changFen = f;
    }  
    public void setHunTun(Breakfast f) {  
        hunTun = f;   
    }   
    public void setHeFen(Breakfast f) {     
        heFen = f; 
    }   
    public void chooseChangFen() {     
        changFen.cooking();   
    }  
    public void chooseHunTun() {    
        hunTun.cooking();  
    }  
    public void chooseHeFen() {   
        heFen.cooking();   
    }}
//抽象命令：早餐
interface Breakfast {    
    public abstract void cooking();}
//具体命令：肠粉
class ChangFen implements Breakfast {   
    private ChangFenChef receiver;  
    ChangFen() {  
        receiver = new ChangFenChef(); 
    } 
    public void cooking() {      
        receiver.cooking();   
    }}
//具体命令：馄饨
class HunTun implements Breakfast {
    private HunTunChef receiver; 
    HunTun() {     
        receiver = new HunTunChef(); 
    }   
    public void cooking() {    
        receiver.cooking();
    }}
//具体命令：河粉
class HeFen implements Breakfast { 
    private HeFenChef receiver;  
    HeFen() {      
        receiver = new HeFenChef();
    } 
    public void cooking() { 
        receiver.cooking(); 
    }}
//接收者：肠粉厨师
class ChangFenChef extends JFrame {   
    private static final long serialVersionUID = 1L;   
    JLabel l = new JLabel();  
    ChangFenChef() {    
        super("煮肠粉");    
        l.setIcon(new ImageIcon("src/command/ChangFen.jpg")); 
        this.add(l);
        this.setLocation(30, 30);    
        this.pack();      
        this.setResizable(false);   
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  
    } 
    public void cooking() {  
        this.setVisible(true);  
    }}
//接收者：馄饨厨师
class HunTunChef extends JFrame {    
    private static final long serialVersionUID = 1L; 
    JLabel l = new JLabel();   
    HunTunChef() {     
        super("煮馄饨");    
        l.setIcon(new ImageIcon("src/command/HunTun.jpg"));     
        this.add(l);    
        this.setLocation(350, 50);   
        this.pack();      
        this.setResizable(false);     
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  
    }   
    public void cooking() {    
        this.setVisible(true); 
    }}
//接收者：河粉厨师
class HeFenChef extends JFrame {  
    private static final long serialVersionUID = 1L;   
    JLabel l = new JLabel();
    HeFenChef() {        
        super("煮河粉");  
        l.setIcon(new ImageIcon("src/command/HeFen.jpg")); 
        this.add(l);     
        this.setLocation(200, 280);    
        this.pack();     
        this.setResizable(false);      
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
    } 
    public void cooking() {   
        this.setVisible(true); 
    }}
```



## 3.4 命令模式的应用场景

当系统的某项操作具备命令语义，且命令实现不稳定（变化）时，可以通过命令模式解耦请求与实现。使用抽象命令接口使请求方的代码架构稳定，封装接收方具体命令的实现细节。接收方与抽象命令呈现弱耦合（内部方法无需一致），具备良好的扩展性。

命令模式通常适用于以下场景。

1. 请求调用者需要与请求接收者解耦时，命令模式可以使调用者和接收者不直接交互。
2. 系统随机请求命令或经常增加、删除命令时，命令模式可以方便地实现这些功能。
3. 当系统需要执行一组操作时，命令模式可以定义宏命令来实现该功能。
4. 当系统需要支持命令的撤销（Undo）操作和恢复（Redo）操作时，可以将命令对象存储起来，采用备忘录模式来实现。

# 4.职责链（Chain of Responsibility）模式

在现实生活中，一个事件需要经过多个对象处理是很常见的场景。例如，采购审批流程、请假流程等。公司员工请假，可批假的领导有部门负责人、副总经理、总经理等，但每个领导能批准的天数不同，员工必须根据需要请假的天数去找不同的领导签名，也就是说员工必须记住每个领导的姓名、电话和地址等信息，这无疑增加了难度。

在计算机软硬件中也有相关例子，如总线网中数据报传送，每台计算机根据目标地址是否同自己的地址相同来决定是否接收；还有异常处理中，处理程序根据异常的类型决定自己是否处理该异常；还有Struts2的拦截器、JSP和Servlet 的 Filter 等，所有这些，都可以考虑使用责任链模式来实现。

## 4.1 模式的定义与特点

责任链（Chain of Responsibility）模式的定义：为了避免请求发送者与多个请求处理者耦合在一起，于是将所有请求的处理者通过前一对象记住其下一个对象的引用而连成一条链；当有请求发生时，可将请求沿着这条链传递，直到有对象处理它为止。

注意：责任链模式也叫职责链模式。

在责任链模式中，客户只需要将请求发送到责任链上即可，无须关心请求的处理细节和请求的传递过程，请求会自动进行传递。所以责任链将请求的发送者和请求的处理者解耦了。

责任链模式是一种对象行为型模式，其主要优点如下：

1. 降低了对象之间的耦合度。该模式使得一个对象无须知道到底是哪一个对象处理其请求以及链的结构，发送者和接收者也无须拥有对方的明确信息。
2. 增强了系统的可扩展性。可以根据需要增加新的请求处理类，满足开闭原则。
3. 增强了给对象指派职责的灵活性。当工作流程发生变化，可以动态地改变链内的成员或者调动它们的次序，也可动态地新增或者删除责任。
4. 责任链简化了对象之间的连接。每个对象只需保持一个指向其后继者的引用，不需保持其他所有处理者的引用，这避免了使用众多的 if 或者 if···else 语句。
5. 责任分担。每个类只需要处理自己该处理的工作，不该处理的传递给下一个对象完成，明确各类的责任范围，符合类的单一职责原则。


其主要缺点如下：

1. 不能保证每个请求一定被处理。由于一个请求没有明确的接收者，所以不能保证它一定会被处理，该请求可能一直传到链的末端都得不到处理。
2. 对比较长的职责链，请求的处理可能涉及多个处理对象，系统性能将受到一定影响。
3. 职责链建立的合理性要靠客户端来保证，增加了客户端的复杂性，可能会由于职责链的错误设置而导致系统出错，如可能会造成循环调用。

## 4.2 模式的结构与实现

通常情况下，可以通过数据链表来实现职责链模式的数据结构。

#### 4.2.1. 模式的结构

职责链模式主要包含以下角色：

1. 抽象处理者（Handler）角色：定义一个处理请求的接口，包含抽象处理方法和一个后继连接
2. 具体处理者（Concrete Handler）角色：实现抽象处理者的处理方法，判断能否处理本次请求，如果可以处理请求则处理，否则将该请求转给它的后继者
3. 客户类（Client）角色：创建处理链，并向链头的具体处理者对象提交请求，它不关心处理细节和请求的传递过程

责任链模式的本质是解耦请求与处理，让请求在处理链中能进行传递与被处理；理解责任链模式应当理解其模式，而不是其具体实现。责任链模式的独到之处是将其节点处理者组合成了链式结构，并允许节点自身决定是否进行请求处理或转发，相当于让请求流动起来。

其结构图如下图所示。



![责任链模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260313085.gif)


客户端可按下图所示设置。

![责任链](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260313082.gif)


#### 4.2.2. 模式的实现

职责链模式的实现代码如下：

```java
package chainOfResponsibility;
public class ChainOfResponsibilityPattern {  
    public static void main(String[] args) {   
        //组装责任链      
        Handler handler1 = new ConcreteHandler1();  
        Handler handler2 = new ConcreteHandler2();   
        handler1.setNext(handler2);        //提交请求     
        handler1.handleRequest("two");   
    }}
//抽象处理者角色
abstract class Handler {  
    private Handler next;   
    public void setNext(Handler next) {    
        this.next = next;   
    } 
    public Handler getNext() {  
        return next;   
    }  
    //处理请求的方法  
    public abstract void handleRequest(String request);}
//具体处理者角色
1class ConcreteHandler1 extends Handler {   
    public void handleRequest(String request) {    
        if (request.equals("one")) {       
            System.out.println("具体处理者1负责处理该请求！");    
        } else {         
            if (getNext() != null) {    
                getNext().handleRequest(request);      
            } else {          
                System.out.println("没有人处理该请求！");        
            }   
        }  
    }}//具体处理者角色2
class ConcreteHandler2 extends Handler {  
    public void handleRequest(String request) {    
        if (request.equals("two")) {        
            System.out.println("具体处理者2负责处理该请求！");     
        } else {         
            if (getNext() != null) {      
                getNext().handleRequest(request);     
            } else {       
                System.out.println("没有人处理该请求！");   
            }    
        } 
    }}
```

程序运行结果如下：

```
具体处理者2负责处理该请求！
```

在上面代码中，我们把消息硬编码为 String 类型，而在真实业务中，消息是具备多样性的，可以是 int、String 或者自定义类型。因此，在上面代码的基础上，可以对消息类型进行抽象 Request，增强了消息的兼容性。

## 4.3 模式的应用实例

**【例】用责任链模式设计一个请假条审批模块**

分析：假如规定学生请假小于或等于 2 天，班主任可以批准；小于或等于 7 天，系主任可以批准；小于或等于 10 天，院长可以批准；其他情况不予批准；这个实例适合使用职责链模式实现。

首先，定义一个领导类（Leader），它是抽象处理者，包含了一个指向下一位领导的指针 next 和一个处理假条的抽象处理方法 handleRequest(int LeaveDays)；然后，定义班主任类（ClassAdviser）、系主任类（DepartmentHead）和院长类（Dean），它们是抽象处理者的子类，是具体处理者，必须根据自己的权力去实现父类的 handleRequest(int LeaveDays) 方法，如果无权处理就将假条交给下一位具体处理者，直到最后；客户类负责创建处理链，并将假条交给链头的具体处理者（班主任）。下图所示是其结构图：



![请假条审批模块的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260315755.gif)

程序代码如下：

```java
package chainOfResponsibility;
public class LeaveApprovalTest {  
    public static void main(String[] args) {    
        //组装责任链   
        Leader teacher1 = new ClassAdviser();  
        Leader teacher2 = new DepartmentHead();       
        Leader teacher3 = new Dean();      
        //Leader teacher4=new DeanOfStudies();   
        teacher1.setNext(teacher2);    
        teacher2.setNext(teacher3);       
        //teacher3.setNext(teacher4);     
        //提交请求   
        teacher1.handleRequest(8);
    }}
//抽象处理者：领导类
abstract class Leader {  
    private Leader next;    
    public void setNext(Leader next) {    
        this.next = next;    
    }   
    public Leader getNext() {     
        return next;  
    } 
    //处理请求的方法   
    public abstract void handleRequest(int LeaveDays);}
//具体处理者1：班主任类
class ClassAdviser extends Leader {    
    public void handleRequest(int LeaveDays) {     
        if (LeaveDays <= 2) {     
            System.out.println("班主任批准您请假" + LeaveDays + "天。");     
        } else {        
            if (getNext() != null) {       
                getNext().handleRequest(LeaveDays);     
            } else {      
                System.out.println("请假天数太多，没有人批准该假条！");   
            } 
        }   
    }}
//具体处理者2：系主任类
class DepartmentHead extends Leader {  
    public void handleRequest(int LeaveDays) {    
        if (LeaveDays <= 7) {   
            System.out.println("系主任批准您请假" + LeaveDays + "天。");      
        } else {      
            if (getNext() != null) {      
                getNext().handleRequest(LeaveDays);  
            } else {        
                System.out.println("请假天数太多，没有人批准该假条！");    
            }
        }  
    }}
//具体处理者3：院长类
class Dean extends Leader { 
    public void handleRequest(int LeaveDays) {    
        if (LeaveDays <= 10) {        
            System.out.println("院长批准您请假" + LeaveDays + "天。");   
        } else {         
            if (getNext() != null) {        
                getNext().handleRequest(LeaveDays);  
            } else {              
                System.out.println("请假天数太多，没有人批准该假条！");     
            }      
        }   
    }}
//具体处理者4：教务处长类
class DeanOfStudies extends Leader {  
    public void handleRequest(int LeaveDays) {    
        if (LeaveDays <= 20) {        
            System.out.println("教务处长批准您请假" + LeaveDays + "天。");    
        } else {        
            if (getNext() != null) {   
                getNext().handleRequest(LeaveDays);    
            } else {     
                System.out.println("请假天数太多，没有人批准该假条！");       
            }       
        }  
    }}
```

程序运行结果如下：

```
院长批准您请假8天。
```


假如增加一个教务处长类，可以批准学生请假 20 天，也非常简单，代码如下：

```java
//具体处理者4:教务处长类
class DeanOfStudies extends Leader {  
    public void handleRequest(int LeaveDays) {   
        if (LeaveDays <= 20) {          
            System.out.println("教务处长批准您请假" + LeaveDays + "天。");      
        } else {        
            if (getNext() != null) {      
                getNext().handleRequest(LeaveDays);   
            } else {              
                System.out.println("请假天数太多，没有人批准该假条！");     
            }   
        } 
    }}
```

## 4.4 模式的应用场景

前边已经讲述了关于责任链模式的结构与特点，下面介绍其应用场景，责任链模式通常在以下几种情况使用。

1. 多个对象可以处理一个请求，但具体由哪个对象处理该请求在运行时自动确定。
2. 可动态指定一组对象处理请求，或添加新的处理者。
3. 需要在不明确指定请求处理者的情况下，向多个处理者中的一个提交请求。

## 4.5 模式的扩展

职责链模式存在以下两种情况。

1. 纯的职责链模式：一个请求必须被某一个处理者对象所接收，且一个具体处理者对某个请求的处理只能采用以下两种行为之一：自己处理（承担责任）；把责任推给下家处理。
2. 不纯的职责链模式：允许出现某一个具体处理者对象在承担了请求的一部分责任后又将剩余的责任传给下家的情况，且一个请求可以最终不被任何接收端对象所接收。

# 5.状态（State）模式

在软件开发过程中，应用程序中的部分对象可能会根据不同的情况做出不同的行为，我们把这种对象称为有状态的对象，而把影响对象行为的一个或多个动态变化的属性称为状态。当有状态的对象与外部事件产生互动时，其内部状态就会发生改变，从而使其行为也发生改变。如人都有高兴和伤心的时候，不同的情绪有不同的行为，当然外界也会影响其情绪变化。

对这种有状态的对象编程，传统的解决方案是：将这些所有可能发生的情况全都考虑到，然后使用 if-else 或 switch-case 语句来做状态判断，再进行不同情况的处理。但是显然这种做法对复杂的状态判断存在天然弊端，条件判断语句会过于臃肿，可读性差，且不具备扩展性，维护难度也大。且增加新的状态时要添加新的 if-else 语句，这违背了“开闭原则”，不利于程序的扩展。

以上问题如果采用“状态模式”就能很好地得到解决。状态模式的解决思想是：当控制一个对象状态转换的条件表达式过于复杂时，把相关“判断逻辑”提取出来，用各个不同的类进行表示，系统处于哪种情况，直接使用相应的状态类对象进行处理，这样能把原来复杂的逻辑判断简单化，消除了 if-else、switch-case 等冗余语句，代码更有层次性，并且具备良好的扩展力。

## 5.1 状态模式的定义与特点

状态（State）模式的定义：对有状态的对象，把复杂的“判断逻辑”提取到不同的状态对象中，允许状态对象在其内部状态发生改变时改变其行为。

状态模式是一种对象行为型模式，其主要优点如下：

1. 结构清晰，状态模式将与特定状态相关的行为局部化到一个状态中，并且将不同状态的行为分割开来，满足“单一职责原则”
2. 将状态转换显示化，减少对象间的相互依赖。将不同的状态引入独立的对象中会使得状态转换变得更加明确，且减少对象间的相互依赖
3. 状态类职责明确，有利于程序的扩展。通过定义新的子类很容易地增加新的状态和转换


状态模式的主要缺点如下：

1. 状态模式的使用必然会增加系统的类与对象的个数
2. 状态模式的结构与实现都较为复杂，如果使用不当会导致程序结构和代码的混乱
3. 状态模式对开闭原则的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源码，否则无法切换到新增状态，而且修改某个状态类的行为也需要修改对应类的源码

## 5.2 状态模式的结构与实现

状态模式把受环境改变的对象行为包装在不同的状态对象里，其意图是让一个对象在其内部状态改变的时候，其行为也随之改变。现在我们来分析其基本结构和实现方法。

#### 5.2.1. 模式的结构

状态模式包含以下主要角色。

1. 环境类（Context）角色：也称为上下文，它定义了客户端需要的接口，内部维护一个当前状态，并负责具体状态的切换
2. 抽象状态（State）角色：定义一个接口，用以封装环境对象中的特定状态所对应的行为，可以有一个或多个行为
3. 具体状态（Concrete State）角色：实现抽象状态所对应的行为，并且在需要的情况下进行状态切换


其结构图下图所示:

![状态模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260318345.gif)


#### 5.2.2. 模式的实现

状态模式的实现代码如下：

```java
public class StatePatternClient {   
    public static void main(String[] args) {   
        Context context = new Context();    //创建环境    
        context.Handle();    //处理请求    
        context.Handle();  
        context.Handle();     
        context.Handle();   
    }}
//环境类
class Context { 
    private State state;   
    //定义环境类的初始状态 
    public Context() {    
        this.state = new ConcreteStateA();   
    }    
    //设置新状态  
    public void setState(State state) {   
        this.state = state;  
    }   
    //读取状态  
    public State getState() {     
        return (state);   
    }    
    //对请求做处理 
    public void Handle() {  
        state.Handle(this);   
    }}
//抽象状态类
abstract class State { 
    public abstract void Handle(Context context);}
//具体状态A类
class ConcreteStateA extends State {  
    public void Handle(Context context) {   
        System.out.println("当前状态是 A.");  
        context.setState(new ConcreteStateB()); 
    }}
//具体状态B类
class ConcreteStateB extends State {  
    public void Handle(Context context) {        
        System.out.println("当前状态是 B.");     
        context.setState(new ConcreteStateA());  
    }}
```


程序运行结果如下：

```
当前状态是 A.
当前状态是 B.
当前状态是 A.
当前状态是 B.
```

## 5.3 状态模式的应用实例

**【例】用“状态模式”设计一个学生成绩的状态转换程序**

分析：本实例包含了“不及格”“中等”和“优秀” 3 种状态，当学生的分数小于 60 分时为“不及格”状态，当分数大于等于 60 分且小于 90 分时为“中等”状态，当分数大于等于 90 分时为“优秀”状态，我们用状态模式来实现这个程序。

首先，定义一个抽象状态类（AbstractState），其中包含了环境属性、状态名属性和当前分数属性，以及加减分方法 addScore(intx) 和检查当前状态的抽象方法 checkState()。

然后，定义“不及格”状态类 LowState、“中等”状态类 MiddleState 和“优秀”状态类 HighState，它们是具体状态类，实现 checkState() 方法，负责检査自己的状态，并根据情况转换。

最后，定义环境类（ScoreContext），其中包含了当前状态对象和加减分的方法 add(int score)，客户类通过该方法来改变成绩状态。结构图如下图：

![学生成绩的状态转换程序的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260319191.gif)

程序代码如下：

```java
public class ScoreStateTest {  
    public static void main(String[] args) {     
        ScoreContext account = new ScoreContext(); 
        System.out.println("学生成绩状态测试：");     
        account.add(30);     
        account.add(40);      
        account.add(25);       
        account.add(-15);    
        account.add(-25);   
    }}
//环境类
class ScoreContext { 
    private AbstractState state;
    ScoreContext() {   
        state = new LowState(this);
    }   
    public void setState(AbstractState state) { 
        this.state = state;  
    }   
    public AbstractState getState() {  
        return state;  
    }   
    public void add(int score) {  
        state.addScore(score);  
    }}
//抽象状态类
abstract class AbstractState {  
    protected ScoreContext hj;  //环境  
    protected String stateName; //状态名   
    protected int score; //分数   
    public abstract void checkState(); //检查当前状态 
    public void addScore(int x) {   
        score += x;     
        System.out.print("加上：" + x + "分，\t当前分数：" + score);  
        checkState();    
        System.out.println("分，\t当前状态：" + hj.getState().stateName); 
    }}
//具体状态类：不及格
class LowState extends AbstractState {  
    public LowState(ScoreContext h) {     
        hj = h;     
        stateName = "不及格";  
        score = 0;  
    }   
    public LowState(AbstractState state) {     
        hj = state.hj;  
        stateName = "不及格";  
        score = state.score;
    } 
    public void checkState() {  
        if (score >= 90) {    
            hj.setState(new HighState(this));      
        } else if (score >= 60) {      
            hj.setState(new MiddleState(this));  
        }  
    }}
//具体状态类：中等
class MiddleState extends AbstractState {  
    public MiddleState(AbstractState state) {    
        hj = state.hj;  
        stateName = "中等";      
        score = state.score;
    } 
    public void checkState() {  
        if (score < 60) {   
            hj.setState(new LowState(this));   
        } else if (score >= 90) {    
            hj.setState(new HighState(this));     
        } 
    }}
//具体状态类：优秀
class HighState extends AbstractState { 
    public HighState(AbstractState state) {    
        hj = state.hj;      
        stateName = "优秀";        
        score = state.score; 
    }  
    public void checkState() {  
        if (score < 60) {  
            hj.setState(new LowState(this));  
        } else if (score < 90) {   
            hj.setState(new MiddleState(this));  
        } 
    }}
```

程序运行结果如下：

```
学生成绩状态测试：
加上：30分，    当前分数：30分，    当前状态：不及格
加上：40分，    当前分数：70分，    当前状态：中等
加上：25分，    当前分数：95分，    当前状态：优秀
加上：-15分，    当前分数：80分，    当前状态：中等
加上：-25分，    当前分数：55分，    当前状态：不及格
```



## 5.4 状态模式的应用场景

通常在以下情况下可以考虑使用状态模式。

- 当一个对象的行为取决于它的状态，并且它必须在运行时根据状态改变它的行为时，就可以考虑使用状态模式。
- 一个操作中含有庞大的分支结构，并且这些分支决定于对象的状态时。

## 5.5 状态模式的扩展

#### 5.5.1 状态模式与责任链模式的区别

状态模式和责任链模式都能消除 if-else 分支过多的问题。但在某些情况下，状态模式中的状态可以理解为责任，那么在这种情况下，两种模式都可以使用。

从定义来看，状态模式强调的是一个对象内在状态的改变，而责任链模式强调的是外部节点对象间的改变。

从代码实现上来看，两者最大的区别就是状态模式的各个状态对象知道自己要进入的下一个状态对象，而责任链模式并不清楚其下一个节点处理对象，因为链式组装由客户端负责。

#### 5.5.2 状态模式与策略模式的区别

状态模式和策略模式的 UML 类图架构几乎完全一样，但两者的应用场景是不一样的。策略模式的多种算法行为择其一都能满足，彼此之间是独立的，用户可自行更换策略算法，而状态模式的各个状态间存在相互关系，彼此之间在一定条件下存在自动切换状态的效果，并且用户无法指定状态，只能设置初始状态。

# 6.观察者（Observer）模式

在现实世界中，许多对象并不是独立存在的，其中一个对象的行为发生改变可能会导致一个或者多个其他对象的行为也发生改变。例如，某种商品的物价上涨时会导致部分商家高兴，而消费者伤心；还有，当我们开车到交叉路口时，遇到红灯会停，遇到绿灯会行。这样的例子还有很多，例如，股票价格与股民、微信公众号与微信用户、气象局的天气预报与听众、小偷与警察等。

在软件世界也是这样，例如，Excel 中的数据与折线图、饼状图、柱状图之间的关系；MVC 模式中的模型与视图的关系；事件模型中的事件源与事件处理者。所有这些，如果用观察者模式来实现就非常方便。

## 6.1 模式的定义与特点

观察者（Observer）模式的定义：指多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。这种模式有时又称作发布-订阅模式、模型-视图模式，它是对象行为型模式。

观察者模式是一种对象行为型模式，其主要优点如下:

1. 降低了目标与观察者之间的耦合关系，两者之间是抽象耦合关系。符合依赖倒置原则
2. 目标与观察者之间建立了一套触发机制


它的主要缺点如下:

1. 目标与观察者之间的依赖关系并没有完全解除，而且有可能出现循环引用
2. 当观察者对象很多时，通知的发布会花费很多时间，影响程序的效率

## 6.2 模式的结构与实现

实现观察者模式时要注意具体目标对象和具体观察者对象之间不能直接调用，否则将使两者之间紧密耦合起来，这违反了面向对象的设计原则。

#### 6.2.1. 模式的结构

观察者模式的主要角色如下:

1. 抽象主题（Subject）角色：也叫抽象目标类，它提供了一个用于保存观察者对象的聚集类和增加、删除观察者对象的方法，以及通知所有观察者的抽象方法
2. 具体主题（Concrete Subject）角色：也叫具体目标类，它实现抽象目标中的通知方法，当具体主题的内部状态发生改变时，通知所有注册过的观察者对象
3. 抽象观察者（Observer）角色：它是一个抽象类或接口，它包含了一个更新自己的抽象方法，当接到具体主题的更改通知时被调用
4. 具体观察者（Concrete Observer）角色：实现抽象观察者中定义的抽象方法，以便在得到目标的更改通知时更新自身的状态


观察者模式的结构图如下图：



![观察者模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260324190.gif)


#### 6.2.2. 模式的实现

观察者模式的实现代码如下：

```java
package net.biancheng.c.observer;
import java.util.*;
public class ObserverPattern { 
    public static void main(String[] args) {   
        Subject subject = new ConcreteSubject();
        Observer obs1 = new ConcreteObserver1();  
        Observer obs2 = new ConcreteObserver2();   
        subject.add(obs1);      
        subject.add(obs2);   
        subject.notifyObserver();   
    }}
//抽象目标
abstract class Subject {  
    protected List<Observer> observers = new ArrayList<Observer>();   
    //增加观察者方法   
    public void add(Observer observer) {     
        observers.add(observer);    } 
    //删除观察者方法 
    public void remove(Observer observer) {    
        observers.remove(observer);    }   
    public abstract void notifyObserver(); 
    //通知观察者方法
}
//具体目标
class ConcreteSubject extends Subject { 
    public void notifyObserver() {     
        System.out.println("具体目标发生改变...");      
        System.out.println("--------------");      
        for (Object obs : observers) {       
            ((Observer) obs).response();  
        }  
    }}
//抽象观察者
interface Observer {    
    void response();
    //反应
}
//具体观察者1
class ConcreteObserver1 implements Observer {  
    public void response() { 
        System.out.println("具体观察者1作出反应！");  
    }}
//具体观察者1
class ConcreteObserver2 implements Observer {   
    public void response() {    
        System.out.println("具体观察者2作出反应！");  
    }}
```

程序运行结果如下：

```
具体目标发生改变...
--------------
具体观察者1作出反应！
具体观察者2作出反应！
```

## 6.3 模式的应用实例

**【例】利用观察者模式设计一个程序，分析“人民币汇率”的升值或贬值对进口公司进口产品成本或出口公司的出口产品收入以及公司利润率的影响**

分析：当“人民币汇率”升值时，进口公司的进口产品成本降低且利润率提升，出口公司的出口产品收入降低且利润率降低；当“人民币汇率”贬值时，进口公司的进口产品成本提升且利润率降低，出口公司的出口产品收入提升且利润率提升。

这里的汇率（Rate）类是抽象目标类，它包含了保存观察者（Company）的 List 和增加/删除观察者的方法，以及有关汇率改变的抽象方法 change(int number)；而人民币汇率（RMBrate）类是具体目标， 它实现了父类的 change(int number) 方法，即当人民币汇率发生改变时通过相关公司；公司（Company）类是抽象观察者，它定义了一个有关汇率反应的抽象方法 response(int number)；进口公司（ImportCompany）类和出口公司（ExportCompany）类是具体观察者类，它们实现了父类的 response(int number) 方法，即当它们接收到汇率发生改变的通知时作为相应的反应。下图所示是其结构图：



![人民币汇率分析程序的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260326250.gif)

程序代码如下：

```java
package net.biancheng.c.observer;
import java.util.*;
public class RMBrateTest {  
    public static void main(String[] args) { 
        Rate rate = new RMBrate();  
        Company watcher1 = new ImportCompany();   
        Company watcher2 = new ExportCompany();     
        rate.add(watcher1);       
        rate.add(watcher2);     
        rate.change(10);     
        rate.change(-9);   
    }}
//抽象目标：汇率
abstract class Rate {   
    protected List<Company> companys = new ArrayList<Company>();  
    //增加观察者方法   
    public void add(Company company) {        companys.add(company);    }   
    //删除观察者方法  
    public void remove(Company company) {        companys.remove(company);    }   
    public abstract void change(int number);}
//具体目标：人民币汇率
class RMBrate extends Rate {   
    public void change(int number) {    
        for (Company obs : companys) {     
            ((Company) obs).response(number);   
        }  
    }}
//抽象观察者：公司
interface Company { 
    void response(int number);}
//具体观察者1：进口公司
class ImportCompany implements Company {
    public void response(int number) {    
        if (number > 0) {       
            System.out.println("人民币汇率升值" + number + "个基点，降低了进口产品成本，提升了进口公司利润率。"); 
        } else if (number < 0) {     
            System.out.println("人民币汇率贬值" + (-number) + "个基点，提升了进口产品成本，降低了进口公司利润率。");
        } 
    }}
//具体观察者2：出口公司
class ExportCompany implements Company {  
    public void response(int number) {   
        if (number > 0) {         
            System.out.println("人民币汇率升值" + number + "个基点，降低了出口产品收入，降低了出口公司的销售利润率。"); 
        } else if (number < 0) {     
            System.out.println("人民币汇率贬值" + (-number) + "个基点，提升了出口产品收入，提升了出口公司的销售利润率。");    
        }  
    }}

```


程序运行结果如下：

```
人民币汇率升值10个基点，降低了进口产品成本，提升了进口公司利润率。
人民币汇率升值10个基点，降低了出口产品收入，降低了出口公司的销售利润率。
人民币汇率贬值9个基点，提升了进口产品成本，降低了进口公司利润率。
人民币汇率贬值9个基点，提升了出口产品收入，提升了出口公司的销售利润率。
```

### 6.4 模式的应用场景

在软件系统中，当系统一方行为依赖另一方行为的变动时，可使用观察者模式松耦合联动双方，使得一方的变动可以通知到感兴趣的另一方对象，从而让另一方对象对此做出响应。

通过前面的分析与应用实例可知观察者模式适合以下几种情形:

1. 对象间存在一对多关系，一个对象的状态发生改变会影响其他对象。
2. 当一个抽象模型有两个方面，其中一个方面依赖于另一方面时，可将这二者封装在独立的对象中以使它们可以各自独立地改变和复用。
3. 实现类似广播机制的功能，不需要知道具体收听者，只需分发广播，系统中感兴趣的对象会自动接收该广播。
4. 多层级嵌套使用，形成一种链式触发机制，使得事件具备跨域（跨越两种观察者类型）通知。

# 7.中介者（Mediator）模式

在现实生活中，常常会出现好多对象之间存在复杂的交互关系，这种交互关系常常是“网状结构”，它要求每个对象都必须知道它需要交互的对象。例如，每个人必须记住他（她）所有朋友的电话；而且，朋友中如果有人的电话修改了，他（她）必须让其他所有的朋友一起修改，这叫作“牵一发而动全身”，非常复杂。

如果把这种“网状结构”改为“星形结构”的话，将大大降低它们之间的“耦合性”，这时只要找一个“中介者”就可以了。如前面所说的“每个人必须记住所有朋友电话”的问题，只要在网上建立一个每个朋友都可以访问的“通信录”就解决了。这样的例子还有很多，例如，你刚刚参加工作想租房，可以找“房屋中介”；或者，自己刚刚到一个陌生城市找工作，可以找“人才交流中心”帮忙。

在软件的开发过程中，这样的例子也很多，例如，在 MVC 框架中，控制器（C）就是模型（M）和视图（V）的中介者；还有大家常用的 QQ 聊天程序的“中介者”是 QQ 服务器。所有这些，都可以采用“中介者模式”来实现，它将大大降低对象之间的耦合性，提高系统的灵活性。

## 7.1 模式的定义与特点

中介者（Mediator）模式的定义：定义一个中介对象来封装一系列对象之间的交互，使原有对象之间的耦合松散，且可以独立地改变它们之间的交互。中介者模式又叫调停模式，它是迪米特法则的典型应用。

中介者模式是一种对象行为型模式，其主要优点如下:

1. 类之间各司其职，符合迪米特法则
2. 降低了对象之间的耦合性，使得对象易于独立地被复用
3. 将对象间的一对多关联转变为一对一的关联，提高系统的灵活性，使得系统易于维护和扩展


其主要缺点是：中介者模式将原本多个对象直接的相互依赖变成了中介者和多个同事类的依赖关系。当同事类越多时，中介者就会越臃肿，变得复杂且难以维护。

## 7.2 模式的结构与实现

中介者模式实现的关键是找出“中介者”，下面对它的结构和实现进行分析。

#### 7.2.1. 模式的结构

中介者模式包含以下主要角色:

1. 抽象中介者（Mediator）角色：它是中介者的接口，提供了同事对象注册与转发同事对象信息的抽象方法
2. 具体中介者（Concrete Mediator）角色：实现中介者接口，定义一个 List 来管理同事对象，协调各个同事角色之间的交互关系，因此它依赖于同事角色
3. 抽象同事类（Colleague）角色：定义同事类的接口，保存中介者对象，提供同事对象交互的抽象方法，实现所有相互影响的同事类的公共功能
4. 具体同事类（Concrete Colleague）角色：是抽象同事类的实现者，当需要与其他同事对象交互时，由中介者对象负责后续的交互


中介者模式的结构图如下图：



![中介者模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260329030.gif)


#### 7.2.2. 模式的实现

中介者模式的实现代码如下：

```java
package net.biancheng.c.mediator;
import java.util.*;
public class MediatorPattern {  
    public static void main(String[] args) {  
        Mediator md = new ConcreteMediator();    
        Colleague c1, c2;      
        c1 = new ConcreteColleague1();    
        c2 = new ConcreteColleague2();      
        md.register(c1);     
        md.register(c2);     
        c1.send();   
        System.out.println("-------------");     
        c2.send(); 
    }}
//抽象中介者
abstract class Mediator { 
    public abstract void register(Colleague colleague);  
    public abstract void relay(Colleague cl); 
    //转发
}
//具体中介者
class ConcreteMediator extends Mediator {   
    private List<Colleague> colleagues = new ArrayList<Colleague>();   
    public void register(Colleague colleague) {     
        if (!colleagues.contains(colleague)) {         
            colleagues.add(colleague);           
            colleague.setMedium(this);   
        }  
    }  
    public void relay(Colleague cl) {
        for (Colleague ob : colleagues) { 
            if (!ob.equals(cl)) {     
                ((Colleague) ob).receive();    
            }     
        }   
    }}
//抽象同事类
abstract class Colleague { 
    protected Mediator mediator;
    public void setMedium(Mediator mediator) {  
        this.mediator = mediator;  
    }   
    public abstract void receive();   
    public abstract void send();}
//具体同事类
class ConcreteColleague1 extends Colleague {   
    public void receive() {    
        System.out.println("具体同事类1收到请求。");
    }  
    public void send() {     
        System.out.println("具体同事类1发出请求。");    
        mediator.relay(this); 
        //请中介者转发   
    }}
//具体同事类
class ConcreteColleague2 extends Colleague {  
    public void receive() {  
        System.out.println("具体同事类2收到请求。");
    }   
    public void send() {     
        System.out.println("具体同事类2发出请求。");   
        mediator.relay(this); //请中介者转发   
    }}
```

程序的运行结果如下：

```
具体同事类1发出请求。
具体同事类2收到请求。
-------------
具体同事类2发出请求。
具体同事类1收到请求。
```

## 7.3 模式的应用实例

**【例】用中介者模式编写一个“韶关房地产交流平台”程序。**

说明：韶关房地产交流平台是“房地产中介公司”提供给“卖方客户”与“买方客户”进行信息交流的平台，比较适合用中介者模式来实现。

首先，定义一个中介公司（Medium）接口，它是抽象中介者，它包含了客户注册方法 register(Customer member) 和信息转发方法 relay(String from,String ad)；再定义一个韶关房地产中介（EstateMedium）公司，它是具体中介者类，它包含了保存客户信息的 List 对象，并实现了中介公司中的抽象方法。

然后，定义一个客户（Customer）类，它是抽象同事类，其中包含了中介者的对象，和发送信息的 send(String ad) 方法与接收信息的 receive(String from，String ad) 方法的接口，由于本程序是窗体程序，所以本类继承 JPmme 类，并实现动作事件的处理方法 actionPerformed(ActionEvent e)。

最后，定义卖方（Seller）类和买方（Buyer）类，它们是具体同事类，是客户（Customer）类的子类，它们实现了父类中的抽象方法，通过中介者类进行信息交流，其结构图如下图所示：



![韶关房地产交流平台的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260331992.gif)



程序代码如下：

```java
package net.biancheng.c.mediator;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.List;

public class DatingPlatform {
    public static void main(String[] args) {
        Medium md = new EstateMedium();    //房产中介   
        Customer member1, member2;
        member1 = new Seller("张三(卖方)");
        member2 = new Buyer("李四(买方)");
        md.register(member1); //客户注册      
        md.register(member2);
    }
}

//抽象中介者：中介公司
interface Medium {
    void register(Customer member);

    //客户注册   
    void relay(String from, String ad);
    //转发
}

//具体中介者：房地产中介
class EstateMedium implements Medium {
    private List<Customer> members = new ArrayList<Customer>();

    public void register(Customer member) {
        if (!members.contains(member)) {
            members.add(member);
            member.setMedium(this);
        }
    }

    public void relay(String from, String ad) {
        for (Customer ob : members) {
            String name = ob.getName();
            if (!name.equals(from)) {
                ((Customer) ob).receive(from, ad);
            }
        }
    }
}

//抽象同事类：客户
abstract class Customer extends JFrame implements ActionListener {
    private static final long serialVersionUID = -7219939540794786080L;
    protected Medium medium;
    protected String name;
    JTextField SentText;
    JTextArea ReceiveArea;

    public Customer(String name) {
        super(name);
        this.name = name;
    }

    void ClientWindow(int x, int y) {
        Container cp;
        JScrollPane sp;
        JPanel p1, p2;
        cp = this.getContentPane();
        SentText = new JTextField(18);
        ReceiveArea = new JTextArea(10, 18);
        ReceiveArea.setEditable(false);
        p1 = new JPanel();
        p1.setBorder(BorderFactory.createTitledBorder("接收内容："));
        p1.add(ReceiveArea);
        sp = new JScrollPane(p1);
        cp.add(sp, BorderLayout.NORTH);
        p2 = new JPanel();
        p2.setBorder(BorderFactory.createTitledBorder("发送内容："));
        p2.add(SentText);
        cp.add(p2, BorderLayout.SOUTH);
        SentText.addActionListener(this);
        this.setLocation(x, y);
        this.setSize(250, 330);
        this.setResizable(false); //窗口大小不可调整    
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        String tempInfo = SentText.getText().trim();
        SentText.setText("");
        this.send(tempInfo);
    }

    public String getName() {
        return name;
    }

    public void setMedium(Medium medium) {
        this.medium = medium;
    }

    public abstract void send(String ad);

    public abstract void receive(String from, String ad);
}

//具体同事类：卖方
class Seller extends Customer {
    private static final long serialVersionUID = -1443076716629516027L;

    public Seller(String name) {
        super(name);
        ClientWindow(50, 100);
    }

    public void send(String ad) {
        ReceiveArea.append("我(卖方)说: " + ad + "\n");        //使滚动条滚动到最底端    
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
        medium.relay(name, ad);
    }

    public void receive(String from, String ad) {
        ReceiveArea.append(from + "说: " + ad + "\n");
        //使滚动条滚动到最底端   
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
    }
}

//具体同事类：买方
class Buyer extends Customer {
    private static final long serialVersionUID = -474879276076308825L;

    public Buyer(String name) {
        super(name);
        ClientWindow(350, 100);
    }

    public void send(String ad) {
        ReceiveArea.append("我(买方)说: " + ad + "\n");
//使滚动条滚动到最底端       
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
        medium.relay(name, ad);
    }

    public void receive(String from, String ad) {
        ReceiveArea.append(from + "说: " + ad + "\n");
        //使滚动条滚动到最底端    
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
    }
}
```

程序的运行结果如下图：

![韶关房地产交流平台的运行结果](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260331688.gif)


## 7.4 模式的应用场景

前面分析了中介者模式的结构与特点，下面分析其以下应用场景。

- 当对象之间存在复杂的网状结构关系而导致依赖关系混乱且难以复用时。
- 当想创建一个运行于多个类之间的对象，又不想生成新的子类时。

## 7.5 模式的扩展

在实际开发中，通常采用以下两种方法来简化中介者模式，使开发变得更简单：

1. 不定义中介者接口，把具体中介者对象实现成为单例。
2. 同事对象不持有中介者，而是在需要的时候直接获取中介者对象并调用。

# 8.迭代器（Iterator）模式

在现实生活以及程序设计中，经常要访问一个聚合对象中的各个元素，如“数据结构”中的链表遍历，通常的做法是将链表的创建和遍历都放在同一个类中，但这种方式不利于程序的扩展，如果要更换遍历方法就必须修改程序源代码，这违背了 “开闭原则”。

既然将遍历方法封装在聚合类中不可取，那么聚合类中不提供遍历方法，将遍历方法由用户自己实现是否可行呢？答案是同样不可取，因为这种方式会存在两个缺点：

1. 暴露了聚合类的内部表示，使其数据不安全；
2. 增加了客户的负担。


“迭代器模式”能较好地克服以上缺点，它在客户访问类与聚合类之间插入一个迭代器，这分离了聚合对象与其遍历行为，对客户也隐藏了其内部细节，且满足“单一职责原则”和“开闭原则”，如Java中的 Collection、List、Set、Map 等都包含了迭代器。

迭代器模式在生活中应用的比较广泛，比如：物流系统中的传送带，不管传送的是什么物品，都会被打包成一个个箱子，并且有一个统一的二维码。这样我们不需要关心箱子里是什么，在分发时只需要一个个检查发送的目的地即可。再比如，我们平时乘坐交通工具，都是统一刷卡或者刷脸进站，而不需要关心是男性还是女性、是残疾人还是正常人等信息。

## 8.1 模式的定义与特点

迭代器（Iterator）模式的定义：提供一个对象来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示。迭代器模式是一种对象行为型模式，其主要优点如下：

1. 访问一个聚合对象的内容而无须暴露它的内部表示。
2. 遍历任务交由迭代器完成，这简化了聚合类。
3. 它支持以不同方式遍历一个聚合，甚至可以自定义迭代器的子类以支持新的遍历。
4. 增加新的聚合类和迭代器类都很方便，无须修改原有代码。
5. 封装性良好，为遍历不同的聚合结构提供一个统一的接口。


其主要缺点是：增加了类的个数，这在一定程度上增加了系统的复杂性。

在日常开发中，我们几乎不会自己写迭代器。除非需要定制一个自己实现的数据结构对应的迭代器，否则，开源框架提供的 API 完全够用。

## 8.2 模式的结构与实现

迭代器模式是通过将聚合对象的遍历行为分离出来，抽象成迭代器类来实现的，其目的是在不暴露聚合对象的内部结构的情况下，让外部代码透明地访问聚合的内部数据。现在我们来分析其基本结构与实现方法。

#### 8.2.1. 模式的结构

迭代器模式主要包含以下角色：

1. 抽象聚合（Aggregate）角色：定义存储、添加、删除聚合对象以及创建迭代器对象的接口
2. 具体聚合（ConcreteAggregate）角色：实现抽象聚合类，返回一个具体迭代器的实例
3. 抽象迭代器（Iterator）角色：定义访问和遍历聚合元素的接口，通常包含 hasNext()、first()、next() 等方法
4. 具体迭代器（Concretelterator）角色：实现抽象迭代器接口中所定义的方法，完成对聚合对象的遍历，记录遍历的当前位置


其结构图如下图：



![迭代器模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260253686.gif)


#### 8.2.2. 模式的实现

迭代器模式的实现代码如下：

```java
package net.biancheng.c.iterator;

import java.util.*;

public class IteratorPattern {
    public static void main(String[] args) {
        Aggregate ag = new ConcreteAggregate();
        ag.add("中山大学");
        ag.add("华南理工");
        ag.add("韶关学院");
        System.out.print("聚合的内容有：");
        Iterator it = ag.getIterator();
        while (it.hasNext()) {
            Object ob = it.next();
            System.out.print(ob.toString() + "\t");
        }
        Object ob = it.first();
        System.out.println("\nFirst：" + ob.toString());
    }
}

//抽象聚合
interface Aggregate {
    public void add(Object obj);

    public void remove(Object obj);

    public Iterator getIterator();
}

//具体聚合
class ConcreteAggregate implements Aggregate {
    private List<Object> list = new ArrayList<Object>();

    public void add(Object obj) {
        list.add(obj);
    }

    public void remove(Object obj) {
        list.remove(obj);
    }

    public Iterator getIterator() {
        return (new ConcreteIterator(list));
    }
}

//抽象迭代器
interface Iterator {
    Object first();

    Object next();

    boolean hasNext();
}

//具体迭代器
class ConcreteIterator implements Iterator {
    private List<Object> list = null;
    private int index = -1;

    public ConcreteIterator(List<Object> list) {
        this.list = list;
    }

    public boolean hasNext() {
        if (index < list.size() - 1) {
            return true;
        } else {
            return false;
        }
    }

    public Object first() {
        index = 0;
        Object obj = list.get(index);
        ;
        return obj;
    }

    public Object next() {
        Object obj = null;
        if (this.hasNext()) {
            obj = list.get(++index);
        }
        return obj;
    }
}
```

程序运行结果如下：

```
聚合的内容有：中山大学    华南理工    韶关学院   
First：中山大学
```

## 8.3 模式的应用实例

**【例】用迭代器模式编写一个浏览婺源旅游风景图的程序**

分析：婺源的名胜古迹较多，要设计一个查看相关景点图片和简介的程序，用“迭代器模式”设计比较合适。

首先，设计一个婺源景点（WyViewSpot）类来保存每张图片的名称与简介；再设计一个景点集（ViewSpotSet）接口，它是抽象聚合类，提供了增加和删除婺源景点的方法，以及获取迭代器的方法。

然后，定义一个婺源景点集（WyViewSpotSet）类，它是具体聚合类，用 ArrayList 来保存所有景点信息，并实现父类中的抽象方法；再定义婺源景点的抽象迭代器（ViewSpotltemtor）接口，其中包含了查看景点信息的相关方法。

最后，定义婺源景点的具体迭代器（WyViewSpotlterator）类，它实现了父类的抽象方法；客户端程序设计成窗口程序，它初始化婺源景点集（ViewSpotSet）中的数据，并实现 ActionListener 接口，它通过婺源景点迭代器（ViewSpotlterator）来査看婺源景点（WyViewSpot）的信息。其结构图如下图所示：



![婺源旅游风景图浏览程序的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260343732.gif)

程序代码如下：

```java
package net.biancheng.c.iterator;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

public class PictureIterator {
    public static void main(String[] args) {
        new PictureFrame();
    }
}

//相框类
class PictureFrame extends JFrame implements ActionListener {
    private static final long serialVersionUID = 1L;
    ViewSpotSet ag; //婺源景点集接口    
    ViewSpotIterator it;//婺源景点迭代器接口    
    WyViewSpot ob;    //婺源景点类    

    PictureFrame() {
        super("中国最美乡村“婺源”的部分风景图");
        this.setResizable(false);
        ag = new WyViewSpotSet();
        ag.add(new WyViewSpot("江湾", "江湾景区是婺源的一个国家5A级旅游景区，景区内有萧江宗祠、永思街、滕家老屋、婺源人家、乡贤园、百工坊等一大批古建筑，精美绝伦，做工精细。"));
        ag.add(new WyViewSpot("李坑", "李坑村是一个以李姓聚居为主的古村落，是国家4A级旅游景区，其建筑风格独特，是著名的徽派建筑，给人一种安静、祥和的感觉。"));
        ag.add(new WyViewSpot("思溪延村", "思溪延村位于婺源县思口镇境内，始建于南宋庆元五年（1199年），当时建村者俞氏以（鱼）思清溪水而名。"));
        ag.add(new WyViewSpot("晓起村", "晓起有“中国茶文化第一村”与“国家级生态示范村”之美誉，村屋多为清代建筑，风格各具特色，村中小巷均铺青石，曲曲折折，回环如棋局。"));
        ag.add(new WyViewSpot("菊径村", "菊径村形状为山环水绕型，小河成大半圆型，绕村庄将近一周，四周为高山环绕，符合中国的八卦“后山前水”设计，当地人称“脸盆村”。"));
        ag.add(new WyViewSpot("篁岭", "篁岭是著名的“晒秋”文化起源地，也是一座距今近六百历史的徽州古村；篁岭属典型山居村落，民居围绕水口呈扇形梯状错落排布。"));
        ag.add(new WyViewSpot("彩虹桥", "彩虹桥是婺源颇有特色的带顶的桥——廊桥，其不仅造型优美，而且它可在雨天里供行人歇脚，其名取自唐诗“两水夹明镜，双桥落彩虹”。"));
        ag.add(new WyViewSpot("卧龙谷", "卧龙谷是国家4A级旅游区，这里飞泉瀑流泄银吐玉、彩池幽潭碧绿清新、山峰岩石挺拔奇巧，活脱脱一幅天然泼墨山水画。"));
        it = ag.getIterator(); //获取婺源景点迭代器       
        ob = it.first();
        this.showPicture(ob.getName(), ob.getIntroduce());
    }    //显示图片  

    void showPicture(String Name, String Introduce) {
        Container cp = this.getContentPane();
        JPanel picturePanel = new JPanel();
        JPanel controlPanel = new JPanel();
        String FileName = "src/iterator/Picture/" + Name + ".jpg";
        JLabel lb = new JLabel(Name, new ImageIcon(FileName), JLabel.CENTER);
        JTextArea ta = new JTextArea(Introduce);
        lb.setHorizontalTextPosition(JLabel.CENTER);
        lb.setVerticalTextPosition(JLabel.TOP);
        lb.setFont(new Font("宋体", Font.BOLD, 20));
        ta.setLineWrap(true);
        ta.setEditable(false);        //ta.setBackground(Color.orange);     
        picturePanel.setLayout(new BorderLayout(5, 5));
        picturePanel.add("Center", lb);
        picturePanel.add("South", ta);
        JButton first, last, next, previous;
        first = new JButton("第一张");
        next = new JButton("下一张");
        previous = new JButton("上一张");
        last = new JButton("最末张");
        first.addActionListener(this);
        next.addActionListener(this);
        previous.addActionListener(this);
        last.addActionListener(this);
        controlPanel.add(first);
        controlPanel.add(next);
        controlPanel.add(previous);
        controlPanel.add(last);
        cp.add("Center", picturePanel);
        cp.add("South", controlPanel);
        this.setSize(630, 550);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    @Override
    public void actionPerformed(ActionEvent arg0) {
        String command = arg0.getActionCommand();
        if (command.equals("第一张")) {
            ob = it.first();
            this.showPicture(ob.getName(), ob.getIntroduce());
        } else if (command.equals("下一张")) {
            ob = it.next();
            this.showPicture(ob.getName(), ob.getIntroduce());
        } else if (command.equals("上一张")) {
            ob = it.previous();
            this.showPicture(ob.getName(), ob.getIntroduce());
        } else if (command.equals("最末张")) {
            ob = it.last();
            this.showPicture(ob.getName(), ob.getIntroduce());
        }
    }
}

//婺源景点类
class WyViewSpot {
    private String Name;
    private String Introduce;

    WyViewSpot(String Name, String Introduce) {
        this.Name = Name;
        this.Introduce = Introduce;
    }

    public String getName() {
        return Name;
    }

    public String getIntroduce() {
        return Introduce;
    }
}

//抽象聚合：婺源景点集接口
interface ViewSpotSet {
    void add(WyViewSpot obj);

    void remove(WyViewSpot obj);

    ViewSpotIterator getIterator();
}

//具体聚合：婺源景点集
class WyViewSpotSet implements ViewSpotSet {
    private ArrayList<WyViewSpot> list = new ArrayList<WyViewSpot>();

    public void add(WyViewSpot obj) {
        list.add(obj);
    }

    public void remove(WyViewSpot obj) {
        list.remove(obj);
    }

    public ViewSpotIterator getIterator() {
        return (new WyViewSpotIterator(list));
    }
}

//抽象迭代器：婺源景点迭代器接口
interface ViewSpotIterator {
    boolean hasNext();

    WyViewSpot first();

    WyViewSpot next();

    WyViewSpot previous();

    WyViewSpot last();
}

//具体迭代器：婺源景点迭代器
class WyViewSpotIterator implements ViewSpotIterator {
    private ArrayList<WyViewSpot> list = null;
    private int index = -1;
    WyViewSpot obj = null;

    public WyViewSpotIterator(ArrayList<WyViewSpot> list) {
        this.list = list;
    }

    public boolean hasNext() {
        if (index < list.size() - 1) {
            return true;
        } else {
            return false;
        }
    }

    public WyViewSpot first() {
        index = 0;
        obj = list.get(index);
        return obj;
    }

    public WyViewSpot next() {
        if (this.hasNext()) {
            obj = list.get(++index);
        }
        return obj;
    }

    public WyViewSpot previous() {
        if (index > 0) {
            obj = list.get(--index);
        }
        return obj;
    }

    public WyViewSpot last() {
        index = list.size() - 1;
        obj = list.get(index);
        return obj;
    }
}
```



## 8.4 模式的应用场景

前面介绍了关于迭代器模式的结构与特点，下面介绍其应用场景，迭代器模式通常在以下几种情况使用。

1. 当需要为聚合对象提供多种遍历方式时。
2. 当需要为遍历不同的聚合结构提供一个统一的接口时。
3. 当访问一个聚合对象的内容而无须暴露其内部细节的表示时。


由于聚合与迭代器的关系非常密切，所以大多数语言在实现聚合类时都提供了迭代器类，因此大数情况下使用语言中已有的聚合类的迭代器就已经够了。

# 9.访问者（Visitor）模式

在现实生活中，有些集合对象存在多种不同的元素，且每种元素也存在多种不同的访问者和处理方式。例如，公园中存在多个景点，也存在多个游客，不同的游客对同一个景点的评价可能不同；医院医生开的处方单中包含多种药元素，査看它的划价员和药房工作人员对它的处理方式也不同，划价员根据处方单上面的药品名和数量进行划价，药房工作人员根据处方单的内容进行抓药。

这样的例子还有很多，例如，电影或电视剧中的人物角色，不同的观众对他们的评价也不同；还有顾客在商场购物时放在“购物车”中的商品，顾客主要关心所选商品的性价比，而收银员关心的是商品的价格和数量。

这些被处理的数据元素相对稳定而访问方式多种多样的数据结构，如果用“访问者模式”来处理比较方便。访问者模式能把处理方法从数据结构中分离出来，并可以根据需要增加新的处理方法，且不用修改原来的程序代码与数据结构，这提高了程序的扩展性和灵活性。

## 9.1 模式的定义与特点

访问者（Visitor）模式的定义：将作用于某种数据结构中的各元素的操作分离出来封装成独立的类，使其在不改变数据结构的前提下可以添加作用于这些元素的新的操作，为数据结构中的每个元素提供多种访问方式。它将对数据的操作与数据结构进行分离，是行为类模式中最复杂的一种模式。

访问者（Visitor）模式是一种对象行为型模式，其主要优点如下:

1. 扩展性好。能够在不修改对象结构中的元素的情况下，为对象结构中的元素添加新的功能
2. 复用性好。可以通过访问者来定义整个对象结构通用的功能，从而提高系统的复用程度
3. 灵活性好。访问者模式将数据结构与作用于结构上的操作解耦，使得操作集合可相对自由地演化而不影响系统的数据结构
4. 符合单一职责原则。访问者模式把相关的行为封装在一起，构成一个访问者，使每一个访问者的功能都比较单一


访问者（Visitor）模式的主要缺点如下:

1. 增加新的元素类很困难。在访问者模式中，每增加一个新的元素类，都要在每一个具体访问者类中增加相应的具体操作，这违背了“开闭原则”
2. 破坏封装。访问者模式中具体元素对访问者公布细节，这破坏了对象的封装性
3. 违反了依赖倒置原则。访问者模式依赖了具体类，而没有依赖抽象类

## 9.2 模式的结构与实现

访问者（Visitor）模式实现的关键是如何将作用于元素的操作分离出来封装成独立的类，其基本结构与实现方法如下。

#### 9.2.1. 模式的结构

访问者模式包含以下主要角色:

1. 抽象访问者（Visitor）角色：定义一个访问具体元素的接口，为每个具体元素类对应一个访问操作 visit() ，该操作中的参数类型标识了被访问的具体元素
2. 具体访问者（ConcreteVisitor）角色：实现抽象访问者角色中声明的各个访问操作，确定访问者访问一个元素时该做什么
3. 抽象元素（Element）角色：声明一个包含接受操作 accept() 的接口，被接受的访问者对象作为 accept() 方法的参数
4. 具体元素（ConcreteElement）角色：实现抽象元素角色提供的 accept() 操作，其方法体通常都是 visitor.visit(this) ，另外具体元素中可能还包含本身业务逻辑的相关操作
5. 对象结构（Object Structure）角色：是一个包含元素角色的容器，提供让访问者对象遍历容器中的所有元素的方法，通常由 List、Set、Map 等聚合类实现


其结构图如下图所示：

![访问者（Visitor）模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260253054.gif)

#### 9.2.2. 模式的实现

访问者模式的实现代码如下：

```java
package net.biancheng.c.visitor;

import java.util.*;

public class VisitorPattern {
    public static void main(String[] args) {
        ObjectStructure os = new ObjectStructure();
        os.add(new ConcreteElementA());
        os.add(new ConcreteElementB());
        Visitor visitor = new ConcreteVisitorA();
        os.accept(visitor);
        System.out.println("------------------------");
        visitor = new ConcreteVisitorB();
        os.accept(visitor);
    }
}

//抽象访问者
interface Visitor {
    void visit(ConcreteElementA element);

    void visit(ConcreteElementB element);
}

//具体访问者A类
class ConcreteVisitorA implements Visitor {
    public void visit(ConcreteElementA element) {
        System.out.println("具体访问者A访问-->" + element.operationA());
    }

    public void visit(ConcreteElementB element) {
        System.out.println("具体访问者A访问-->" + element.operationB());
    }
}

//具体访问者B类
class ConcreteVisitorB implements Visitor {
    public void visit(ConcreteElementA element) {
        System.out.println("具体访问者B访问-->" + element.operationA());
    }

    public void visit(ConcreteElementB element) {
        System.out.println("具体访问者B访问-->" + element.operationB());
    }
}

//抽象元素类
interface Element {
    void accept(Visitor visitor);
}

//具体元素A类
class ConcreteElementA implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }

    public String operationA() {
        return "具体元素A的操作。";
    }
}

//具体元素B类
class ConcreteElementB implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }

    public String operationB() {
        return "具体元素B的操作。";
    }
}

//对象结构角色
class ObjectStructure {
    private List<Element> list = new ArrayList<Element>();

    public void accept(Visitor visitor) {
        Iterator<Element> i = list.iterator();
        while (i.hasNext()) {
            ((Element) i.next()).accept(visitor);
        }
    }

    public void add(Element element) {
        list.add(element);
    }

    public void remove(Element element) {
        list.remove(element);
    }
}
```

程序的运行结果如下：

```
具体访问者A访问-->具体元素A的操作。
具体访问者A访问-->具体元素B的操作。
------------------------
具体访问者B访问-->具体元素A的操作。
具体访问者B访问-->具体元素B的操作。
```

## 9.3 模式的应用实例

**【例】利用“访问者（Visitor）模式”模拟艺术公司与造币公司的功能**

分析：艺术公司利用“铜”可以设计出铜像，利用“纸”可以画出图画；造币公司利用“铜”可以印出铜币，利用“纸”可以印出纸币。对“铜”和“纸”这两种元素，两个公司的处理方法不同，所以该实例用访问者模式来实现比较适合。

首先，定义一个公司（Company）接口，它是抽象访问者，提供了两个根据纸（Paper）或铜（Cuprum）这两种元素创建作品的方法；再定义艺术公司（ArtCompany）类和造币公司（Mint）类，它们是具体访问者，实现了父接口的方法。

然后，定义一个材料（Material）接口，它是抽象元素，提供了 accept（Company visitor）方法来接受访问者（Company）对象访问；再定义纸（Paper）类和铜（Cuprum）类，它们是具体元素类，实现了父接口中的方法。

最后，定义一个材料集（SetMaterial）类，它是对象结构角色，拥有保存所有元素的容器 List，并提供让访问者对象遍历容器中的所有元素的 accept（Company visitor）方法；客户类设计成窗体程序，它提供材料集（SetMaterial）对象供访问者（Company）对象访问，实现了 ItemListener 接口，处理用户的事件请求。下图所示是其结构图：



![艺术公司与造币公司的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260350814.gif)

程序代码如下：

```java
package net.biancheng.c.visitor;

import javax.swing.*;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class VisitorProducer {
    public static void main(String[] args) {
        new MaterialWin();
    }
}

//窗体类
class MaterialWin extends JFrame implements ItemListener {
    private static final long serialVersionUID = 1L;
    JPanel CenterJP;
    SetMaterial os;
    //材料集对象   
    Company visitor1, visitor2;
    //访问者对象    
    String[] select;

    MaterialWin() {
        super("利用访问者模式设计艺术公司和造币公司");
        JRadioButton Art;
        JRadioButton mint;
        os = new SetMaterial();
        os.add(new Cuprum());
        os.add(new Paper());
        visitor1 = new ArtCompany();//艺术公司  
        visitor2 = new Mint(); //造币公司   
        this.setBounds(10, 10, 750, 350);
        this.setResizable(false);
        CenterJP = new JPanel();
        this.add("Center", CenterJP);
        JPanel SouthJP = new JPanel();
        JLabel yl = new JLabel("原材料有：铜和纸，请选择生产公司：");
        Art = new JRadioButton("艺术公司", true);
        mint = new JRadioButton("造币公司");
        Art.addItemListener(this);
        mint.addItemListener(this);
        ButtonGroup group = new ButtonGroup();
        group.add(Art);
        group.add(mint);
        SouthJP.add(yl);
        SouthJP.add(Art);
        SouthJP.add(mint);
        this.add("South", SouthJP);
        select = (os.accept(visitor1)).split(" ");    //获取产品名     
        showPicture(select[0], select[1]);    //显示产品   
    }

    //显示图片   
    void showPicture(String Cuprum, String paper) {
        CenterJP.removeAll();
        //清除面板内容  
        CenterJP.repaint();    //刷新屏幕    
        String FileName1 = "src/visitor/Picture/" + Cuprum + ".jpg";
        String FileName2 = "src/visitor/Picture/" + paper + ".jpg";
        JLabel lb = new JLabel(new ImageIcon(FileName1), JLabel.CENTER);
        JLabel rb = new JLabel(new ImageIcon(FileName2), JLabel.CENTER);
        CenterJP.add(lb);
        CenterJP.add(rb);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    @Override
    public void itemStateChanged(ItemEvent arg0) {
        JRadioButton jc = (JRadioButton) arg0.getSource();
        if (jc.isSelected()) {
            if (jc.getText() == "造币公司") {
                select = (os.accept(visitor2)).split(" ");
            } else {
                select = (os.accept(visitor1)).split(" ");
            }
            showPicture(select[0], select[1]);    //显示选择的产品    
        }
    }
}//抽象访问者:公司

interface Company {
    String create(Paper element);

    String create(Cuprum element);
}

//具体访问者：艺术公司
class ArtCompany implements Company {
    public String create(Paper element) {
        return "讲学图";
    }

    public String create(Cuprum element) {
        return "朱熹铜像";
    }
}

//具体访问者：造币公司
class Mint implements Company {
    public String create(Paper element) {
        return "纸币";
    }

    public String create(Cuprum element) {
        return "铜币";
    }
}

//抽象元素：材料
interface Material {
    String accept(Company visitor);
}

//具体元素：纸
class Paper implements Material {
    public String accept(Company visitor) {
        return (visitor.create(this));
    }
}

//具体元素：铜
class Cuprum implements Material {
    public String accept(Company visitor) {
        return (visitor.create(this));
    }
}

//对象结构角色:材料集
class SetMaterial {
    private List<Material> list = new ArrayList<Material>();

    public String accept(Company visitor) {
        Iterator<Material> i = list.iterator();
        String tmp = "";
        while (i.hasNext()) {
            tmp += ((Material) i.next()).accept(visitor) + " ";
        }
        return tmp; //返回某公司的作品集  
    }

    public void add(Material element) {
        list.add(element);
    }

    public void remove(Material element) {
        list.remove(element);
    }
}package net.biancheng.c.visitor;

import javax.swing.*;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class VisitorProducer {
    public static void main(String[] args) {
        new MaterialWin();
    }
}

//窗体类
class MaterialWin extends JFrame implements ItemListener {
    private static final long serialVersionUID = 1L;
    JPanel CenterJP;
    SetMaterial os;
    //材料集对象   
    Company visitor1, visitor2;
    //访问者对象    
    String[] select;

    MaterialWin() {
        super("利用访问者模式设计艺术公司和造币公司");
        JRadioButton Art;
        JRadioButton mint;
        os = new SetMaterial();
        os.add(new Cuprum());
        os.add(new Paper());
        visitor1 = new ArtCompany();//艺术公司  
        visitor2 = new Mint(); //造币公司   
        this.setBounds(10, 10, 750, 350);
        this.setResizable(false);
        CenterJP = new JPanel();
        this.add("Center", CenterJP);
        JPanel SouthJP = new JPanel();
        JLabel yl = new JLabel("原材料有：铜和纸，请选择生产公司：");
        Art = new JRadioButton("艺术公司", true);
        mint = new JRadioButton("造币公司");
        Art.addItemListener(this);
        mint.addItemListener(this);
        ButtonGroup group = new ButtonGroup();
        group.add(Art);
        group.add(mint);
        SouthJP.add(yl);
        SouthJP.add(Art);
        SouthJP.add(mint);
        this.add("South", SouthJP);
        select = (os.accept(visitor1)).split(" ");    //获取产品名     
        showPicture(select[0], select[1]);    //显示产品   
    }

    //显示图片   
    void showPicture(String Cuprum, String paper) {
        CenterJP.removeAll();
        //清除面板内容  
        CenterJP.repaint();    //刷新屏幕    
        String FileName1 = "src/visitor/Picture/" + Cuprum + ".jpg";
        String FileName2 = "src/visitor/Picture/" + paper + ".jpg";
        JLabel lb = new JLabel(new ImageIcon(FileName1), JLabel.CENTER);
        JLabel rb = new JLabel(new ImageIcon(FileName2), JLabel.CENTER);
        CenterJP.add(lb);
        CenterJP.add(rb);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    @Override
    public void itemStateChanged(ItemEvent arg0) {
        JRadioButton jc = (JRadioButton) arg0.getSource();
        if (jc.isSelected()) {
            if (jc.getText() == "造币公司") {
                select = (os.accept(visitor2)).split(" ");
            } else {
                select = (os.accept(visitor1)).split(" ");
            }
            showPicture(select[0], select[1]);    //显示选择的产品    
        }
    }
}//抽象访问者:公司

interface Company {
    String create(Paper element);

    String create(Cuprum element);
}

//具体访问者：艺术公司
class ArtCompany implements Company {
    public String create(Paper element) {
        return "讲学图";
    }

    public String create(Cuprum element) {
        return "朱熹铜像";
    }
}

//具体访问者：造币公司
class Mint implements Company {
    public String create(Paper element) {
        return "纸币";
    }

    public String create(Cuprum element) {
        return "铜币";
    }
}

//抽象元素：材料
interface Material {
    String accept(Company visitor);
}

//具体元素：纸
class Paper implements Material {
    public String accept(Company visitor) {
        return (visitor.create(this));
    }
}

//具体元素：铜
class Cuprum implements Material {
    public String accept(Company visitor) {
        return (visitor.create(this));
    }
}

//对象结构角色:材料集
class SetMaterial {
    private List<Material> list = new ArrayList<Material>();

    public String accept(Company visitor) {
        Iterator<Material> i = list.iterator();
        String tmp = "";
        while (i.hasNext()) {
            tmp += ((Material) i.next()).accept(visitor) + " ";
        }
        return tmp; //返回某公司的作品集  
    }

    public void add(Material element) {
        list.add(element);
    }

    public void remove(Material element) {
        list.remove(element);
    }
}
```



## 9.4 模式的应用场景

当系统中存在类型数量稳定（固定）的一类数据结构时，可以使用访问者模式方便地实现对该类型所有数据结构的不同操作，而又不会对数据产生任何副作用（脏数据）。

简而言之，就是当对集合中的不同类型数据（类型数量稳定）进行多种操作时，使用访问者模式。

通常在以下情况可以考虑使用访问者（Visitor）模式。

1. 对象结构相对稳定，但其操作算法经常变化的程序。
2. 对象结构中的对象需要提供多种不同且不相关的操作，而且要避免让这些操作的变化影响对象的结构。
3. 对象结构包含很多类型的对象，希望对这些对象实施一些依赖于其具体类型的操作。



## 9.5 模式的扩展

访问者（Visitor）模式是使用频率较高的一种设计模式，它常常同以下两种设计模式联用。

(1)与迭代器模式联用。因为访问者模式中的“对象结构”是一个包含元素角色的容器，当访问者遍历容器中的所有元素时，常常要用迭代器。如【例】中的对象结构是用 List 实现的，它通过 List 对象的 Iterator() 方法获取迭代器。如果对象结构中的聚合类没有提供迭代器，也可以用迭代器模式自定义一个。

(2)访问者（Visitor）模式同组合联用。因为访问者（Visitor）模式中的“元素对象”可能是叶子对象或者是容器对象，如果元素对象包含容器对象，就必须用到组合模式，其结构图如下图所示：



![包含组合模式的访问者模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260254089.gif)

# 10.备忘录（Memento）模式

每个人都有犯错误的时候，都希望有种“后悔药”能弥补自己的过失，让自己重新开始，但现实是残酷的。在计算机应用中，客户同样会常常犯错误，能否提供“后悔药”给他们呢？当然是可以的，而且是有必要的。这个功能由“备忘录模式”来实现。

其实很多应用软件都提供了这项功能，如 Word、记事本、Photoshop、Eclipse 等软件在编辑时按 Ctrl+Z 组合键时能撤销当前操作，使文档恢复到之前的状态；还有在 IE 中的后退键、数据库事务管理中的回滚操作、玩游戏时的中间结果存档功能、数据库与操作系统的备份操作、棋类游戏中的悔棋功能等都属于这类。

备忘录模式能记录一个对象的内部状态，当用户后悔时能撤销当前操作，使数据恢复到它原先的状态。

## 10.1 模式的定义与特点

备忘录（Memento）模式的定义：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便以后当需要时能将该对象恢复到原先保存的状态。该模式又叫快照模式。

备忘录模式是一种对象行为型模式，其主要优点如下：

- 提供了一种可以恢复状态的机制。当用户需要时能够比较方便地将数据恢复到某个历史的状态。
- 实现了内部状态的封装。除了创建它的发起人之外，其他对象都不能够访问这些状态信息。
- 简化了发起人类。发起人不需要管理和保存其内部状态的各个备份，所有状态信息都保存在备忘录中，并由管理者进行管理，这符合单一职责原则。


其主要缺点是：资源消耗大。如果要保存的内部状态信息过多或者特别频繁，将会占用比较大的内存资源。

## 10.2 模式的结构与实现

备忘录模式的核心是设计备忘录类以及用于管理备忘录的管理者类，现在我们来学习其结构与实现。

#### 10.2.1. 模式的结构

备忘录模式的主要角色如下:

1. 发起人（Originator）角色：记录当前时刻的内部状态信息，提供创建备忘录和恢复备忘录数据的功能，实现其他业务功能，它可以访问备忘录里的所有信息。
2. 备忘录（Memento）角色：负责存储发起人的内部状态，在需要的时候提供这些内部状态给发起人。
3. 管理者（Caretaker）角色：对备忘录进行管理，提供保存与获取备忘录的功能，但其不能对备忘录的内容进行访问与修改。


备忘录模式的结构图如下图所示:



![备忘录模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260354032.gif)


#### 10.2.2. 模式的实现

备忘录模式的实现代码如下：

```java
package net.biancheng.c.memento;

public class MementoPattern {
    public static void main(String[] args) {
        Originator or = new Originator();
        Caretaker cr = new Caretaker();
        or.setState("S0");
        System.out.println("初始状态:" + or.getState());
        cr.setMemento(or.createMemento()); //保存状态  
        or.setState("S1");
        System.out.println("新的状态:" + or.getState());
        or.restoreMemento(cr.getMemento()); //恢复状态   
        System.out.println("恢复状态:" + or.getState());
    }
}

//备忘录
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

//发起人
class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public Memento createMemento() {
        return new Memento(state);
    }

    public void restoreMemento(Memento m) {
        this.setState(m.getState());
    }
}

//管理者
class Caretaker {
    private Memento memento;

    public void setMemento(Memento m) {
        memento = m;
    }

    public Memento getMemento() {
        return memento;
    }
}
```

程序运行的结果如下：

```
初始状态:S0
新的状态:S1
恢复状态:S0
```

## 10.3 模式的应用实例

**【例】利用备忘录模式设计相亲游戏**

分析：假如有西施、王昭君、貂蝉、杨玉环四大美女同你相亲，你可以选择其中一位作为你的爱人；当然，如果你对前面的选择不满意，还可以重新选择，但希望你不要太花心；这个游戏提供后悔功能，用“备忘录模式”设计比较合适。

首先，先设计一个美女（Girl）类，它是备忘录角色，提供了获取和存储美女信息的功能；然后，设计一个相亲者（You）类，它是发起人角色，它记录当前时刻的内部状态信息（临时妻子的姓名），并提供创建备忘录和恢复备忘录数据的功能；最后，定义一个美女栈（GirlStack）类，它是管理者角色，负责对备忘录进行管理，用于保存相亲者（You）前面选过的美女信息，不过最多只能保存 4 个，提供后悔功能。

客户类设计成窗体程序，它包含美女栈（GirlStack）对象和相亲者（You）对象，它实现了 ActionListener 接口的事件处理方法 actionPerformed(ActionEvent e)，并将 4 大美女图像和相亲者（You）选择的美女图像在窗体中显示出来。下图 所示是其结构图:



![相亲游戏的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260355129.gif)

程序代码如下：

```java
package net.biancheng.c.memento;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class DatingGame {
    public static void main(String[] args) {
        new DatingGameWin();
    }
}

//客户窗体类
class DatingGameWin extends JFrame implements ActionListener {
    private static final long serialVersionUID = 1L;
    JPanel CenterJP, EastJP;
    JRadioButton girl1, girl2, girl3, girl4;
    JButton button1, button2;
    String FileName;
    JLabel g;
    You you;
    GirlStack girls;

    DatingGameWin() {
        super("利用备忘录模式设计相亲游戏");
        you = new You();
        girls = new GirlStack();
        this.setBounds(0, 0, 900, 380);
        this.setResizable(false);
        FileName = "src/memento/Photo/四大美女.jpg";
        g = new JLabel(new ImageIcon(FileName), JLabel.CENTER);
        CenterJP = new JPanel();
        CenterJP.setLayout(new GridLayout(1, 4));
        CenterJP.setBorder(BorderFactory.createTitledBorder("四大美女如下："));
        CenterJP.add(g);
        this.add("Center", CenterJP);
        EastJP = new JPanel();
        EastJP.setLayout(new GridLayout(1, 1));
        EastJP.setBorder(BorderFactory.createTitledBorder("您选择的爱人是："));
        this.add("East", EastJP);
        JPanel SouthJP = new JPanel();
        JLabel info = new JLabel("四大美女有“沉鱼落雁之容、闭月羞花之貌”，您选择谁？");
        girl1 = new JRadioButton("西施", true);
        girl2 = new JRadioButton("貂蝉");
        girl3 = new JRadioButton("王昭君");
        girl4 = new JRadioButton("杨玉环");
        button1 = new JButton("确定");
        button2 = new JButton("返回");
        ButtonGroup group = new ButtonGroup();
        group.add(girl1);
        group.add(girl2);
        group.add(girl3);
        group.add(girl4);
        SouthJP.add(info);
        SouthJP.add(girl1);
        SouthJP.add(girl2);
        SouthJP.add(girl3);
        SouthJP.add(girl4);
        SouthJP.add(button1);
        SouthJP.add(button2);
        button1.addActionListener(this);
        button2.addActionListener(this);
        this.add("South", SouthJP);
        showPicture("空白");
        you.setWife("空白");
        girls.push(you.createMemento());    //保存状态
    }

    //显示图片
    void showPicture(String name) {
        EastJP.removeAll(); //清除面板内容
        EastJP.repaint(); //刷新屏幕
        you.setWife(name);
        FileName = "src/memento/Photo/" + name + ".jpg";
        g = new JLabel(new ImageIcon(FileName), JLabel.CENTER);
        EastJP.add(g);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        boolean ok = false;
        if (e.getSource() == button1) {
            ok = girls.push(you.createMemento());    //保存状态
            if (ok && girl1.isSelected()) {
                showPicture("西施");
            } else if (ok && girl2.isSelected()) {
                showPicture("貂蝉");
            } else if (ok && girl3.isSelected()) {
                showPicture("王昭君");
            } else if (ok && girl4.isSelected()) {
                showPicture("杨玉环");
            }
        } else if (e.getSource() == button2) {
            you.restoreMemento(girls.pop());
            //恢复状态            showPicture(you.getWife());        }    }}
            //备忘录：美女
            class Girl {
                private String name;

                public Girl(String name) {
                    this.name = name;
                }

                public void setName(String name) {
                    this.name = name;
                }

                public String getName() {
                    return name;
                }
            }
            //发起人：您
            class You {
                private String wifeName;

                //妻子
                public void setWife(String name) {
                    wifeName = name;
                }

                public String getWife() {
                    return wifeName;
                }

                public Girl createMemento() {
                    return new Girl(wifeName);
                }

                public void restoreMemento(Girl p) {
                    setWife(p.getName());
                }
            }
            //管理者：美女栈
            class GirlStack {
                private Girl girl[];
                private int top;

                GirlStack() {
                    girl = new Girl[5];
                    top = -1;
                }

                public boolean push(Girl p) {
                    if (top >= 4) {
                        System.out.println("你太花心了，变来变去的！");
                        return false;
                    } else {
                        girl[++top] = p;
                        return true;
                    }
                }

                public Girl pop() {
                    if (top <= 0) {
                        System.out.println("美女栈空了！");
                        return girl[0];
                    } else return girl[top--];
                }
            }
```

## 10.4 模式的应用场景

前面学习了备忘录模式的定义与特点、结构与实现，现在来看该模式的以下应用场景。

1. 需要保存与恢复数据的场景，如玩游戏时的中间结果的存档功能。
2. 需要提供一个可回滚操作的场景，如 Word、记事本、Photoshop，Eclipse 等软件在编辑时按 Ctrl+Z 组合键，还有数据库中事务操作。

## 10.5 模式的扩展

在前面介绍的备忘录模式中，有单状态备份的例子，也有多状态备份的例子。下面介绍备忘录模式如何同[原型模式](http://c.biancheng.net/view/1343.html)混合使用。在备忘录模式中，通过定义“备忘录”来备份“发起人”的信息，而原型模式的 clone() 方法具有自备份功能，所以，如果让发起人实现 Cloneable 接口就有备份自己的功能，这时可以删除备忘录类，其结构图如下图 所示:



![带原型的备忘录模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260357886.gif)



实现代码如下：

```java
package net.biancheng.c.memento;

public class PrototypeMemento {
    public static void main(String[] args) {
        OriginatorPrototype or = new OriginatorPrototype();
        PrototypeCaretaker cr = new PrototypeCaretaker();
        or.setState("S0");
        System.out.println("初始状态:" + or.getState());
        cr.setMemento(or.createMemento()); //保存状态   
        or.setState("S1");
        System.out.println("新的状态:" + or.getState());
        or.restoreMemento(cr.getMemento()); //恢复状态     
        System.out.println("恢复状态:" + or.getState());
    }
}

//发起人原型
class OriginatorPrototype implements Cloneable {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public OriginatorPrototype createMemento() {
        return this.clone();
    }

    public void restoreMemento(OriginatorPrototype opt) {
        this.setState(opt.getState());
    }

    public OriginatorPrototype clone() {
        try {
            return (OriginatorPrototype) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return null;
    }
}

//原型管理者
class PrototypeCaretaker {
    private OriginatorPrototype opt;

    public void setMemento(OriginatorPrototype opt) {
        this.opt = opt;
    }

    public OriginatorPrototype getMemento() {
        return opt;
    }
}
```


程序的运行结果如下：

```
初始状态:S0
新的状态:S1
恢复状态:S0
```



# 11.解释器（Interpreter）模式

在软件开发中，会遇到有些问题多次重复出现，而且有一定的相似性和规律性。如果将它们归纳成一种简单的语言，那么这些问题实例将是该语言的一些句子，这样就可以用“编译原理”中的解释器模式来实现了。

虽然使用解释器模式的实例不是很多，但对于满足以上特点，且对运行效率要求不是很高的应用实例，如果用解释器模式来实现，其效果是非常好的，本文将介绍其工作原理与使用方法。

## 11.1 模式的定义与特点

解释器（Interpreter）模式的定义：给分析对象定义一个语言，并定义该语言的文法表示，再设计一个解析器来解释语言中的句子。也就是说，用编译语言的方式来分析应用中的实例。这种模式实现了文法表达式处理的接口，该接口解释一个特定的上下文。

这里提到的文法和句子的概念同编译原理中的描述相同，“文法”指语言的语法规则，而“句子”是语言集中的元素。例如，汉语中的句子有很多，“我是中国人”是其中的一个句子，可以用一棵语法树来直观地描述语言中的句子。

解释器模式是一种类行为型模式，其主要优点如下:

1. 扩展性好。由于在解释器模式中使用类来表示语言的文法规则，因此可以通过继承等机制来改变或扩展文法。
2. 容易实现。在语法树中的每个表达式节点类都是相似的，所以实现其文法较为容易。


解释器模式的主要缺点如下:

1. 执行效率较低。解释器模式中通常使用大量的循环和递归调用，当要解释的句子较复杂时，其运行速度很慢，且代码的调试过程也比较麻烦。
2. 会引起类膨胀。解释器模式中的每条规则至少需要定义一个类，当包含的文法规则很多时，类的个数将急剧增加，导致系统难以管理与维护。
3. 可应用的场景比较少。在软件开发中，需要定义语言文法的应用实例非常少，所以这种模式很少被使用到。

## 11.2 模式的结构与实现

解释器模式常用于对简单语言的编译或分析实例中，为了掌握好它的结构与实现，必须先了解编译原理中的“文法、句子、语法树”等相关概念:

* *文法

文法是用于描述语言的语法结构的形式规则。没有规矩不成方圆，例如，有些人认为完美爱情的准则是“相互吸引、感情专一、任何一方都没有恋爱经历”，虽然最后一条准则较苛刻，但任何事情都要有规则，语言也一样，不管它是机器语言还是自然语言，都有它自己的文法规则。例如，中文中的“句子”的文法如下:

```
〈句子〉::=〈主语〉〈谓语〉〈宾语〉
〈主语〉::=〈代词〉|〈名词〉
〈谓语〉::=〈动词〉
〈宾语〉::=〈代词〉|〈名词〉
〈代词〉你|我|他
〈名词〉7大学生I筱霞I英语
〈动词〉::=是|学习
```


注：这里的符号“::=”表示“定义为”的意思，用“〈”和“〉”括住的是非终结符，没有括住的是终结符。

* 句子

句子是语言的基本单位，是语言集中的一个元素，它由终结符构成，能由“文法”推导出。例如，上述文法可以推出“我是大学生”，所以它是句子。

* 语法树

语法树是句子结构的一种树型表示，它代表了句子的推导结果，它有利于理解句子语法结构的层次。下图 所示是“我是大学生”的语法树:



![句子“我是大学生”的语法树](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260358239.gif)

有了以上基础知识，现在来介绍解释器模式的结构就简单了。解释器模式的结构与组合模式相似，不过其包含的组成元素比组合模式多，而且组合模式是对象结构型模式，而解释器模式是类行为型模式。

#### 11.2.1. 模式的结构

解释器模式包含以下主要角色:

1. 抽象表达式（Abstract Expression）角色：定义解释器的接口，约定解释器的解释操作，主要包含解释方法 interpret()。
2. 终结符表达式（Terminal Expression）角色：是抽象表达式的子类，用来实现文法中与终结符相关的操作，文法中的每一个终结符都有一个具体终结表达式与之相对应。
3. 非终结符表达式（Nonterminal Expression）角色：也是抽象表达式的子类，用来实现文法中与非终结符相关的操作，文法中的每条规则都对应于一个非终结符表达式。
4. 环境（Context）角色：通常包含各个解释器需要的数据或是公共的功能，一般用来传递被所有解释器共享的数据，后面的解释器可以从这里获取这些值。
5. 客户端（Client）：主要任务是将需要分析的句子或表达式转换成使用解释器对象描述的抽象语法树，然后调用解释器的解释方法，当然也可以通过环境角色间接访问解释器的解释方法。


解释器模式的结构图如下图所示:



![解释器模式的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260255520.gif)


#### 11.2.2. 模式的实现

解释器模式实现的关键是定义文法规则、设计终结符类与非终结符类、画出结构图，必要时构建语法树，其代码结构如下：

```java
package net.biancheng.c.interpreter;

//抽象表达式类
interface AbstractExpression {
    public void interpret(String info);
    //解释方法
}

//终结符表达式类
class TerminalExpression implements AbstractExpression {
    public void interpret(String info) {
        //对终结符表达式的处理   
    }
}

//非终结符表达式类
class NonterminalExpression implements AbstractExpression {
    private AbstractExpression exp1;
    private AbstractExpression exp2;

    public void interpret(String info) {
        //非对终结符表达式的处理 
    }
}

//环境类
class Context {
    private AbstractExpression exp;

    public Context() {
        //数据初始化   
    }

    public void operation(String info) {
        //调用相关表达式类的解释方法   
    }
}
```

## 11.3 模式的应用实例

**【例】用解释器模式设计一个“韶粵通”公交车卡的读卡器程序**

说明：假如“韶粵通”公交车读卡器可以判断乘客的身份，如果是“韶关”或者“广州”的“老人” “妇女”“儿童”就可以免费乘车，其他人员乘车一次扣 2 元。

分析：本实例用“解释器模式”设计比较适合，首先设计其文法规则如下:

```
<expression> ::= <city>的<person>
<city> ::= 韶关|广州
<person> ::= 老人|妇女|儿童
```


然后，根据文法规则按以下步骤设计公交车卡的读卡器程序的类图:

- 定义一个抽象表达式（Expression）接口，它包含了解释方法 interpret(String info)。
- 定义一个终结符表达式（Terminal Expression）类，它用集合（Set）类来保存满足条件的城市或人，并实现抽象表达式接口中的解释方法 interpret(Stringinfo)，用来判断被分析的字符串是否是集合中的终结符。
- 定义一个非终结符表达式（AndExpressicm）类，它也是抽象表达式的子类，它包含满足条件的城市的终结符表达式对象和满足条件的人员的终结符表达式对象，并实现 interpret(String info) 方法，用来判断被分析的字符串是否是满足条件的城市中的满足条件的人员。
- 最后，定义一个环境（Context）类，它包含解释器需要的数据，完成对终结符表达式的初始化，并定义一个方法 freeRide(String info) 调用表达式对象的解释方法来对被分析的字符串进行解释。其结构图如下图所示:



![“韶粵通”公交车读卡器程序的结构图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108260401696.gif)

程序代码如下：

```java
package net.biancheng.c.interpreter;

import java.util.*;/*文法规则  <expression> ::= <city>的<person>  <city> ::= 韶关|广州  <person> ::= 老人|妇女|儿童*/

public class InterpreterPatternDemo {
    public static void main(String[] args) {
        Context bus = new Context();
        bus.freeRide("韶关的老人");
        bus.freeRide("韶关的年轻人");
        bus.freeRide("广州的妇女");
        bus.freeRide("广州的儿童");
        bus.freeRide("山东的儿童");
    }
}

//抽象表达式类
interface Expression {
    public boolean interpret(String info);
}

//终结符表达式类
class TerminalExpression implements Expression {
    private Set<String> set = new HashSet<String>();

    public TerminalExpression(String[] data) {
        for (int i = 0; i < data.length; i++) set.add(data[i]);
    }

    public boolean interpret(String info) {
        if (set.contains(info)) {
            return true;
        }
        return false;
    }
}

//非终结符表达式类
class AndExpression implements Expression {
    private Expression city = null;
    private Expression person = null;

    public AndExpression(Expression city, Expression person) {
        this.city = city;
        this.person = person;
    }

    public boolean interpret(String info) {
        String s[] = info.split("的");
        return city.interpret(s[0]) && person.interpret(s[1]);
    }
}

//环境类
class Context {
    private String[] citys = {"韶关", "广州"};
    private String[] persons = {"老人", "妇女", "儿童"};
    private Expression cityPerson;

    public Context() {
        Expression city = new TerminalExpression(citys);
        Expression person = new TerminalExpression(persons);
        cityPerson = new AndExpression(city, person);
    }

    public void freeRide(String info) {
        boolean ok = cityPerson.interpret(info);
        if (ok) System.out.println("您是" + info + "，您本次乘车免费！");
        else System.out.println(info + "，您不是免费人员，本次乘车扣费2元！");
    }
}
```

程序运行结果如下：

```
您是韶关的老人，您本次乘车免费！
韶关的年轻人，您不是免费人员，本次乘车扣费2元！
您是广州的妇女，您本次乘车免费！
您是广州的儿童，您本次乘车免费！
山东的儿童，您不是免费人员，本次乘车扣费2元！
```

## 11.4 模式的应用场景

前面介绍了解释器模式的结构与特点，下面分析它的应用场景:

1. 当语言的文法较为简单，且执行效率不是关键问题时。
2. 当问题重复出现，且可以用一种简单的语言来进行表达时。
3. 当一个语言需要解释执行，并且语言中的句子可以表示为一个抽象语法树的时候，如 XML 文档解释。


注意：解释器模式在实际的软件开发中使用比较少，因为它会引起效率、性能以及维护等问题。如果碰到对表达式的解释，在Java中可以用 Expression4J 或 Jep 等来设计。

## 11.5 模式的扩展

在项目开发中，如果要对数据表达式进行分析与计算，无须再用解释器模式进行设计了，Java 提供了以下强大的数学公式解析器：Expression4J、MESP(Math Expression String Parser) 和 Jep 等，它们可以解释一些复杂的文法，功能强大，使用简单。

现在以 Jep 为例来介绍该工具包的使用方法。Jep 是 Java expression parser 的简称，即 Java 表达式分析器，它是一个用来转换和计算数学表达式的 Java 库。通过这个程序库，用户可以以字符串的形式输入一个任意的公式，然后快速地计算出其结果。而且 Jep 支持用户自定义变量、常量和函数，它包括许多常用的数学函数和常量。

使用前先下载 Jep 压缩包，解压后，将 jep-x.x.x.jar 文件移到选择的目录中，在 Eclipse 的“Java 构建路径”对话框的“库”选项卡中选择“添加外部 JAR(X)...”，将该 Jep 包添加项目中后即可使用其中的类库。

下面以计算存款利息为例来介绍。存款利息的计算公式是：本金x利率x时间=利息，其相关代码如下：

```java
package net.biancheng.c.interpreter;

import com.singularsys.jep.*;

public class JepDemo {
    public static void main(String[] args) throws JepException {
        Jep jep = new Jep();
//定义要计算的数据表达式        
        String 存款利息 = "本金*利率*时间";
//给相关变量赋值    
        jep.addVariable("本金", 10000);
        jep.addVariable("利率", 0.038);
        jep.addVariable("时间", 2);
        jep.parse(存款利息);   //解析表达式        
        Object accrual = jep.evaluate();    //计算   
        System.out.println("存款利息：" + accrual);
    }
}
```

程序运行结果如下：

```
存款利息：760.0
```

