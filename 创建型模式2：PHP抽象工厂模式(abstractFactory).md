PHP设计模式笔记：使用PHP实现抽象工厂模式
抽象工厂模式（Abstact Factory）是一种常见的软件设计模式。该模式为一个产品族提供了统一的创建接口。当需要这个产品族的某一系列的时候，可以为此系列的产品族创建一个具体的工厂类。

【意图】
抽象工厂模式提供一个创建一系统相关或相互依赖对象的接口，而无需指定它们具体的类【GOF95】

【抽象工厂模式结构图】

抽象工厂模式

【抽象工厂模式中主要角色】
抽象工厂(Abstract Factory)角色：它声明一个创建抽象产品对象的接口。通常以接口或抽象类实现，所有的具体工厂类必须实现这个接口或继承这个类。
具体工厂(Concrete Factory)角色：实现创建产品对象的操作。客户端直接调用这个角色创建产品的实例。这个角色包含有选择合适的产品对象的逻辑。通常使用具体类实现。
抽象产品(Abstract Product)角色：声明一类产品的接口。它是工厂方法模式所创建的对象的父类，或它们共同拥有的接口。
具体产品(Concrete Product)角色：实现抽象产品角色所定义的接口，定义一个将被相应的具体工厂创建的产品对象。其内部包含了应用程序的业务逻辑。

【抽象工厂模式的优缺点】
抽象工厂模式的优点:
1、分离了具体的类
2、使增加或替换产品族变得容易
3、有利于产品的一致性
抽象工厂模式的缺点: 难以支持新种类的产品。这是因为AbstractFactory接口确定了可以被创建的产品集合。支持新各类的产品就需要扩展访工厂接口，从而导致AbstractFactory类及其所有子类的改变。
抽象工厂就是以一种倾斜的方式支持增加新的产品中，它为新产品族的增加提供了方便，而不能为新的产品等级结构的增加提供这样的方便。

【抽象工厂模式适用场景】
以下情况应当使用抽象工厂模式：
1、一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节，这对于所有形态的工厂模式都是重要的。
2、这个系统的产品有多于一个的产品族，而系统只消费其中某一族的产品。
3、 同属于同一个产品族的产品是在一起使用的，这一约束必须在系统的设计中体现出来。
4、系统提供一个产品类的库，所有的产品以同样的接口出现，从而使用客户端不依赖于实现

【Java与模式189页】

Abstract Factory模式的几个要点：
1、如果没有应对“多系列对象构建”的需求变化，则没有必要使用Abstract Factory模式。
2、“系列对象”指的是这项对象之间有相互依赖、或作用的关系。
3、Abstract Factory模式主要在于应对“新系列”的需求变动。缺点是难以应对
“新对象”的需求变动。这一点应该注意，就像前面说的，如果我们现在要在加入
其他系列的类，代码的改动会很大。
4、Abstract Factory模式经常和Factory Method模式共同组合来应对
“对象创建”的需求变化。

抽象工厂中的增加
1. 在产品等级结构的数目不变的情况下，增加新的产品族，就意味着在每一个产品等级结构中增加一个（或者多个）新的具体 （或者抽象和具体）产品角色。 由于工厂等级结构是与产品等级结构平行的登记机构，因此，当产品等级结构有所调整时， 需要将工厂等级结构做相应的调整。现在产品等级结构中出现了新的元素，因此， 需要向工厂等级结构中加入相应的新元素就可以了。 换言之，设计师只需要向系统中加入新的具体工厂类就可以了，没有必要修改已 有的工厂角色或者产品角色。因此，在系统中的产品族增加时，抽象工厂模式是支持“开-闭”原则的。

2. 在产品族的数目不变的情况下，增加新的产品等级结构。换言之，所有的产品等级结构 中的产品数目不会改变，但是现在多出一个与现有的产品等级结构平行的新的产品等级结构。 要做到这一点，就需要修改所有的工厂角色，给每一个工厂类都增加一个新的工厂方法， 而这显然是违背“开–闭”原则的。换言之，对于产品等级结构的增加，抽象工厂模式是不支持“开–闭”原则的。

综合起来，我们可以知道，在已有的抽象产品中添加其具体产品，支持“开—闭原则”， 然而在添加其抽象产品时，确不支持“开—闭”原则。抽象工厂模式以一种倾斜的 方式支持增加新的产品，它为新产品族的增加提供方便，而不能为新的产品等级 结构的增加提供这样的方便。

【抽象工厂模式与其它模式】
单例模式(singleton模式)：具体工厂类可以设计成单例类,由于工厂通常有一个就可以，因此具体工厂子类一般都实现为一个Singleton。
工厂方法模式(factory method模式)：抽象工厂创建产品的方法定义为工厂方法。
原型模式(prototype模式)：如果有多个可能的产品系列，具体的工厂也可以使用原型模式，具体工厂使用产品系列中
每一个产品的原型进行实例化并且通过复制它的原型来创建新的产品。

【抽象工厂模式PHP示例】
<?php
/**
 * 抽象工厂模式 2015-10-09
 * 1.目的
 * 抽象工厂模式提供一个创建一系统相关或相互依赖对象的接口，而无需指定它们具体的类
 * 2.使用场景
 * 2.1 一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节，这对于所有形态的工厂模式都是重要的
 * 2.2 这个系统的产品有多于一个的产品族，而系统只消费其中某一族的产品
 * 2.3 同属于同一个产品族的产品是在一起使用的，这一约束必须在系统的设计中体现出来
 * 2.4 系统提供一个产品类的库，所有的产品以同样的接口出现，从而使用客户端不依赖于实现
 */
 
/**
 * 抽象工厂
 */
interface AbstractFactory 
{
    /**
     * 创建等级结构为A的产品的工厂方法
     */
    public function createProductA();
 
     /**
     * 创建等级结构为B的产品的工厂方法
     */
    public function createProductB();
}
 
/**
 * 具体工厂1
 */
class ConcreteFactory1 implements AbstractFactory
{
 
    public function createProductA() 
    {
        return new ProductA1();
    }
 
    public function createProductB() 
    {
        return new ProductB1();
    }
} 
 
/**
 * 具体工厂2
 */
class ConcreteFactory2 implements AbstractFactory
{ 
    public function createProductA() 
    {
        return new ProductA2();
    }
 
    public function createProductB() 
    {
        return new ProductB2();
    }
}
 
/**
 * 抽象产品A
 */
interface AbstractProductA 
{ 
    /**
     * 取得产品名
     */
    public function getName();
}
 
/**
 * 抽象产品B
 */
interface AbstractProductB 
{ 
    /**
     * 取得产品名
     */
    public function getName();
}
 
/**
 * 具体产品Ａ1
 */
class ProductA1 implements AbstractProductA 
{
    private $_name;
 
    public function __construct() 
    {
        $this->_name = 'product A1';
    }
 
    public function getName() 
    {
        return $this->_name;
    }
}
 
/**
 * 具体产品Ａ2
 */
class ProductA2 implements AbstractProductA 
{
    private $_name;
 
    public function __construct() 
    {
        $this->_name = 'product A2';
    }
 
    public function getName() 
    {
        return $this->_name;
    }
}
 
/**
 * 具体产品B1
 */
class ProductB1 implements AbstractProductB 
{
    private $_name;
 
    public function __construct() 
    {
        $this->_name = 'product B1';
    }
 
    public function getName() 
    {
        return $this->_name;
    }
}
 
/**
 * 具体产品B2
 */
class ProductB2 implements AbstractProductB 
{
    private $_name;
 
    public function __construct() 
    {
        $this->_name = 'product B2';
    }
 
    public function getName() 
    {
        return $this->_name;
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
        self::run(new ConcreteFactory1());
        self::run(new ConcreteFactory2());
    }
 
    /**
     * 调用工厂实例生成产品，输出产品名
     * @param   $factory    AbstractFactory     工厂实例
     */
    public static function run(AbstractFactory $factory) 
    {
        $productA = $factory->createProductA();
        $productB = $factory->createProductB();
        echo $productA->getName(), '<br />';
        echo $productB->getName(), '<br />';
    } 
}
 
Client::main();
?>