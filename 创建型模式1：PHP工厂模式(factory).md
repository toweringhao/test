PHP设计模式笔记：使用PHP实现工厂模式

【意图】
定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使用一个类的实例化延迟到其子类【GOF95】

【工厂模式结构图】

工厂方法模式

【工厂模式中主要角色】
抽象产品(Product)角色：具体产品对象共有的父类或接口
具体产品(Concrete Product)角色：实现抽象产品角色所定义的接口，并且工厂方法模式所创建的每一个对象都是某具体产品对象的实例
抽象工厂(Creator)角色：模式中任何创建对象的工厂类都要实现这个接口，它声明了工厂方法，该方法返回一个Product类型的对象。
Creator也可以定义一个工厂方法的缺省实现，它返回一个缺省的的ConcreteProduct对象
具体工厂(Concrete Creator)角色：实现抽象工厂接口，具体工厂角色与应用逻辑相关，由应用程序直接调用以创建产品对象。

【工厂模式的优点和缺点】
工厂模式的优点
工厂方法模式可以允许系统在不修改工厂角色的情况下引进新产品。

工厂模式的缺点
客户可能仅仅为了创建一个特定的ConcreteProduct对象，就不得不创建一个Creator子类

【工厂模式适用场景】
1、当一个类不知道它所必须创建的对象的类的时候
2、当一个类希望由它的子类来指定它所创建的对象的时候
3、当类将创建对象的职责委托给多个帮助子类的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候

【工厂模式与其它模式】
抽象工厂模式(abstract factory模式)：Abstract Factory模式经常使用工厂方法来实现
Template Method模式: 工厂方法通常在Template Methods中被调用

【工厂模式PHP示例】
 <?php
/**
 * 工厂模式 2015-10-09
 * 1.目的
 * 定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使用一个类的实例化延迟到其子类
 * 2.使用场景
 * 2.1 当一个类不知道它所必须创建的对象的类的时候
 * 2.2 当一个类希望由它的子类来指定它所创建的对象的时候
 * 2.3 当类将创建对象的职责委托给多个帮助子类的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候
 */

/**
 * 抽象工厂角色
 */
interface Creator 
{
    public function factoryMethod();
}
 
/**
 * 具体工厂角色A
 */
class ConcreteCreatorA implements Creator 
{ 
    /**
     * 工厂方法 返回具体 产品A
     * @return ConcreteProductA
     */
    public function factoryMethod() 
    {
        return new ConcreteProductA();
    }
}
 
/**
 * 具体工厂角色B
 */
class ConcreteCreatorB implements Creator 
{ 
    /**
     * 工厂方法 返回具体 产品B
     * @return ConcreteProductB
     */
    public function factoryMethod() 
    {
        return new ConcreteProductB();
    }
}
 
/**
 * 抽象产品角色
 */
interface Product 
{
    public function operation();                                                                                    
}
 
/**
 * 具体产品角色A
 */
class ConcreteProductA implements Product 
{ 
    /**
     * 接口方法实现 输出特定字符串
     */
    public function operation() 
    {
        echo 'ConcreteProductA <br />';
    }
}
 
/**
 * 具体产品角色B
 */
class ConcreteProductB implements Product 
{ 
    /**
     * 接口方法实现 输出特定字符串
     */
    public function operation() 
    {
        echo 'ConcreteProductB <br />';
    }
}
 
class Client 
{
    /**
     * Main program.
     */
    public static function main() 
    {
        $creatorA = new ConcreteCreatorA();
        $productA = $creatorA->factoryMethod();
        $productA->operation();
 
        $creatorB = new ConcreteCreatorB();
        $productB = $creatorB->factoryMethod();
        $productB->operation();
    }                                                
}
 
Client::main();
?>

【工厂方法模式与简单工厂模式】
工厂方法模式与简单工厂模式再结构上的不同不是很明显。工厂方法类的核心是一个抽象工厂类，而简单工厂模式把核心放在一个具体类上。
工厂方法模式之所以有一个别名叫多态性工厂模式是因为具体工厂类都有共同的接口，或者有共同的抽象父类。
当系统扩展需要添加新的产品对象时，仅仅需要添加一个具体对象以及一个具体工厂对象，原有工厂对象不需要进行任何修改，也不需要修改客户端，很好的符合了”开放－封闭”原则。而简单工厂模式在添加新产品对象后不得不修改工厂方法，扩展性不好。
工厂方法模式退化后可以演变成简单工厂模式。