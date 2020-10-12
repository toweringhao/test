PHP设计模式笔记：使用PHP实现装饰模式

【意图】
动态的给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活【GOF95】
装饰模式是以对客户透明的方式动态地给一个对象附加上更多的职责。这也就是说，客户端并不会觉得对象在装饰前和装饰后有什么不同。装饰模式可以在不使用创造更多子类的情况下，将对象的功能加以扩展。

【装饰模式结构图】

装饰模式

【装饰模式中主要角色】
抽象构件(Component)角色：定义一个对象接口，以规范准备接收附加职责的对象，从而可以给这些对象动态地添加职责。
具体构件(Concrete Component)角色：定义一个将要接收附加职责的类。
装饰(Decorator)角色：持有一个指向Component对象的指针，并定义一个与Component接口一致的接口。
具体装饰(Concrete Decorator)角色：负责给构件对象增加附加的职责。

【装饰模式的优缺点】
装饰模式的优点:
1、比静态继承更灵活；
2、避免在层次结构高层的类有太多的特征
装饰模式的缺点：
1、使用装饰模式会产生比使用继承关系更多的对象。并且这些对象看上去都很想像，从而使得查错变得困难。

【装饰模式适用场景】
1、在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
2、处理那些可以撤消的职责，即需要动态的给一个对象添加功能并且这些功能是可以动态的撤消的。
3、当不能彩生成子类的方法进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是因为类定义被隐藏，或类定义不能用于生成子类。

【装饰模式与其它模式】

【装饰模式PHP示例】
<?php
/**
 * 装饰模式 2015-10-09
 * 1.目的
 * 动态的给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活
 * 2.使用场景
 * 2.1 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
 * 2.2 处理那些可以撤消的职责，即需要动态的给一个对象添加功能并且这些功能是可以动态的撤消的。
 * 2.3 当不能彩生成子类的方法进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是因为类定义被隐藏，或类定义不能用于生成子类。
 */

/**
 * 抽象构件角色
 */
interface Component 
{
    /**
     * 示例方法
     */
    public function operation();
}
 
/**
 * 装饰角色
 */
abstract class Decorator implements Component
{ 
    protected  $_component;
 
    public function __construct(Component $component) 
    {
        $this->_component = $component;
    }
 
    public function operation() 
    {
        $this->_component->operation();
    }
}
 
/**
 * 具体装饰类A
 */
class ConcreteDecoratorA extends Decorator 
{
    public function __construct(Component $component) 
    {
        parent::__construct($component);
 
    }
 
    public function operation() 
    {
        parent::operation();    //  调用装饰类的操作
        $this->addedOperationA();   //  新增加的操作
    }
 
    /**
     * 新增加的操作A，即装饰上的功能
     */
    public function addedOperationA() 
    {
        echo 'Add Operation A <br />';
    }
}
 
/**
 * 具体装饰类B
 */
class ConcreteDecoratorB extends Decorator 
{
    public function __construct(Component $component) 
    {
        parent::__construct($component); 
    }
 
    public function operation() 
    {
        parent::operation();
        $this->addedOperationB();
    }
 
    /**
     * 新增加的操作B，即装饰上的功能
     */
    public function addedOperationB() 
    {
        echo 'Add Operation B <br />';
    }
}
 
/**
 * 具体构件
 */
class ConcreteComponent implements Component
{ 
    public function operation() 
    {
        echo 'Concrete Component operation <br />';
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
        $component = new ConcreteComponent();
        $decoratorA = new ConcreteDecoratorA($component);
        $decoratorB = new ConcreteDecoratorB($decoratorA);
        $decoratorC = new ConcreteDecoratorB($component);
 
        $decoratorA->operation();
        $decoratorB->operation();
    } 
}
 
Client::main();
?>
从以上示例可以看出：
1、装饰类中有一个属性$_component，其数据类型是Component;
2、装饰类实现了Component接口;
3、接口的实现是委派给父类，但并不是单纯的委派，还有功能的增强;
4、具体装饰类实现了抽象装饰类的operation方法。