PHP设计模式笔记：使用PHP实现原型模式

【意图】
用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象

【原型模式结构图】

Prototype

【原型模式中主要角色】
抽象原型(Prototype)角色：声明一个克隆自身的接口

具体原型(Concrete Prototype)角色：实现一个克隆自身的操作

【原型模式的优点和缺点】
Prototype模式优点：
1、可以在运行时刻增加和删除产品
2、可以改变值以指定新对象
3、可以改变结构以指定新对象
4、减少子类的构造
5、用类动态配置应用

Prototype模式的缺点：
Prototype模式的最主要缺点就是每一个类必须配备一个克隆方法。
而且这个克隆方法需要对类的功能进行通盘考虑，这对全新的类来说不是很难，但对已有的类进行改造时，不一定是件容易的事。

【原型模式适用场景】
1、当一个系统应该独立于它的产品创建、构成和表示时，要使用Prototype模式
2、当要实例化的类是在运行时刻指定时，例如动态加载
3、为了避免创建一个与产品类层次平等的工厂类层次时；
4、当一个类的实例只能有几个不同状态组合中的一种时。建立相应数目的原型并克隆它们可能比每次用合适的状态手工实例化该类更方便一些

【原型模式与其它模式】
抽象工厂模式(abstract factory模式)：Abstract Factory模式与Prototype模式在某种方面是相互竞争的。但是也可以一起使用

【原型模式PHP示例】
<?php
/**
 * 原型模式 2015-10-09
 * 1.目的
 * 用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象
 * 2.使用场景
 * 2.1 当一个系统应该独立于它的产品创建、构成和表示时，要使用Prototype模式
 * 2.2 当要实例化的类是在运行时刻指定时，例如动态加载
 * 2.3 为了避免创建一个与产品类层次平等的工厂类层次时
 * 2.4 当一个类的实例只能有几个不同状态组合中的一种时。建立相应数目的原型并克隆它们可能比每次用合适的状态手工实例化该类更方便一些
 */

/**
 * 抽象原型角色
 */
interface Prototype 
{
    public function copy();
}
 
/**
 * 具体原型角色
 */
class ConcretePrototype implements Prototype
{ 
    private  $_name;
 
    public function __construct($name) 
    {
        $this->_name = $name;
    }
 
    public function setName($name) 
    {
        $this->_name = $name;
    }
 
    public function getName() 
    {
        return $this->_name;
    }
 
    public function copy() 
    {
       /* 深拷贝实现
        $serialize_obj = serialize($this);  //  序列化
        $clone_obj = unserialize($serialize_obj);   //  反序列化                                                     
        return $clone_obj;
        */
        return clone $this;     //  浅拷贝
    }
}
 
/**
 * 测试深拷贝用的引用类
 */
class Demo 
{
    public $array;
}
 
class Client 
{ 
    /**
     * Main program.
     */
    public static function main() 
    { 
        $demo = new Demo();
        $demo->array = array(1, 2);
        $object1 = new ConcretePrototype($demo);
        $object2 = $object1->copy();
 
        var_dump($object1->getName());
        echo '<br />';
        var_dump($object2->getName());
        echo '<br />';
 
        $demo->array = array(3, 4);
        var_dump($object1->getName());
        echo '<br />';
        var_dump($object2->getName());
        echo '<br />'; 
    } 
}
 
Client::main();
?>
【浅拷贝与深拷贝】

浅拷贝
被拷贝对象的所有变量都含有与原对象相同的值，而且对其他对象的引用仍然是指向原来的对象。
即 浅拷贝只负责当前对象实例，对引用的对象不做拷贝。

深拷贝
被拷贝对象的所有的变量都含有与原来对象相同的值，除了那些引用其他对象的变量。那些引用其他对象的变量将指向一个被拷贝的新对象，而不再是原有那些被引用对象。
即 深拷贝把要拷贝的对象所引用的对象也都拷贝了一次，而这种对被引用到的对象拷贝叫做间接拷贝。
深拷贝要深入到多少层，是一个不确定的问题。
在决定以深拷贝的方式拷贝一个对象的时候，必须决定对间接拷贝的对象是采取浅拷贝还是深拷贝还是继续采用深拷贝。
因此，在采取深拷贝时，需要决定多深才算深。此外，在深拷贝的过程中，很可能会出现循环引用的问题。

利用序列化来做深拷贝
利用序列化来做深拷贝,把对象写到流里的过程是序列化（Serilization）过程，但在业界又将串行化这一过程形象的称为“冷冻”或“腌咸菜”过程；
而把对象从流中读出来的过程则叫做反序列化（Deserialization）过程，也称为“解冻”或“回鲜”过程。
在PHP中使用serialize和unserialize函数实现序列化和反序列化

在上面的代码中的注释就是一个先序列化再反序列化实现深拷贝的过程