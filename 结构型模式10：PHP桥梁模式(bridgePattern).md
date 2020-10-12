PHP设计模式笔记：使用PHP实现桥梁模式

【意图】
将抽象部分与它的实现部分分享，使它们都可以独立的变化【GOF95】

【桥梁模式结构图】

桥梁模式

【桥梁模式中主要角色】
抽象化(Abstraction)角色：定义抽象类的接口并保存一个对实现化对象的引用。
修正抽象化(Refined Abstraction)角色：扩展抽象化角色，改变和修正父类对抽象化的定义。
实现化(Implementor)角色：定义实现类的接口，不给出具体的实现。此接口不一定和抽象化角色的接口定义相同，实际上，这两个接口可以完全不同。实现化角色应当只给出底层操作，而抽象化角色应当只给出基于底层操作的更高一层的操作。
具体实现化(Concrete Implementor)角色：实现实现化角色接口并定义它的具体实现。

【桥梁模式的优点】
1、分离接口及其实现部分
将Abstraction与Implementor分享有助于降低对实现部分编译时刻的依赖性
接口与实现分享有助于分层，从而产生更好的结构化系统

2、提高可扩充性

3、实现细节对客户透明。

【桥梁模式适用场景】
1、如果一个系统需要在构件的抽象化和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的联系。
2、设计要求实现化角色的任何改变不应当影响客户端，或者说实现化角色的改变对客户端是完全透明的。
3、一个构件有多于一个的抽象化角色和实现化角色，并且系统需要它们之间进行动态的耦合。
4、虽然在系统中使用继承是没有问题的，但是由于抽象化角色和具体化角色需要独立变化，设计要求需要独立管理这两者。

【桥梁模式与其它模式】
抽象工厂模式(abstract factory模式)：抽象工厂模式可以用来创建和配置一个特定的桥梁模式。
适配器模式(adapter模式)：适配器模式用来帮助无关的类协同工作。它通常是在系统设计完成之后才会被使用。然而，桥梁模式是在系统开始时就被使用，它使得抽象接口和实现部分可以独立进行改变。
状态模式(state模式)：桥梁模式描述两个等级结构之间的关系，状态模式则是描述一个对象与状态对象之间的关系。状态模式是桥梁模式的一个退化的特殊情况。

【桥梁模式PHP示例】
<?php
/**
 * 桥梁模式 2015-10-09
 * 1.目的
 * 定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。策略模式可以使算法可独立于使用它的客户而变化
 * 2.使用场景
 * 2.1 如果一个系统需要在构件的抽象化和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的联系。
 * 2.2 设计要求实现化角色的任何改变不应当影响客户端，或者说实现化角色的改变对客户端是完全透明的。
 * 2.3 一个构件有多于一个的抽象化角色和实现化角色，并且系统需要它们之间进行动态的耦合。
 * 2.4 虽然在系统中使用继承是没有问题的，但是由于抽象化角色和具体化角色需要独立变化，设计要求需要独立管理这两者。
 */

/**
 * 抽象化角色
 * 抽象化给出的定义，并保存一个对实现化对象的引用。
 */
abstract class Abstraction 
{ 
    /* 对实现化对象的引用 */
    protected $imp;
 
    /**
     * 某操作方法
     */
    public function operation() 
    {
        $this->imp->operationImp();
    }
}
 
/**
 * 修正抽象化角色
 * 扩展抽象化角色，改变和修正父类对抽象化的定义。
 */
class RefinedAbstraction extends Abstraction 
{ 
    public function __construct(Implementor $imp) 
    {
        $this->imp = $imp;
    }
 
    /**
     * 操作方法在修正抽象化角色中的实现
     */
    public function operation() 
    {
        echo 'RefinedAbstraction operation  ';
        $this->imp->operationImp();
    }
}
 
/**
 * 实现化角色
 * 给出实现化角色的接口，但不给出具体的实现。
 */
abstract class Implementor 
{ 
    /**
     * 操作方法的实现化声明
     */
    abstract public function operationImp();
}
 
/**
 * 具体化角色A
 * 给出实现化角色接口的具体实现
 */
class ConcreteImplementorA extends Implementor 
{ 
    /**
     * 操作方法的实现化实现
     */
    public function operationImp() 
    {
        echo 'Concrete implementor A operation <br />';
    }
}
 
/**
 * 具体化角色B
 * 给出实现化角色接口的具体实现
 */
class ConcreteImplementorB extends Implementor 
{ 
    /**
     * 操作方法的实现化实现
     */
    public function operationImp() 
    {
        echo 'Concrete implementor B operation <br />';
    }
}
 
/**
 * 客户端
 */
class Client 
{ 
     /**
     * Main program.
     */
    public static function main() 
    {
        $abstraction = new RefinedAbstraction(new ConcreteImplementorA());
        $abstraction->operation();
 
        $abstraction = new RefinedAbstraction(new ConcreteImplementorB());
        $abstraction->operation();
    }
}
 
Client::main();
?>
在写上面的代码时，就构造方法是否可以放在抽象化角色中纠结了许久！