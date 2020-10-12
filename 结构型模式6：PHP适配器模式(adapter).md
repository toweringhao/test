PHP设计模式笔记：使用PHP实现适配器模式

【意图】
将一个类的接口转换成客户希望的另外一个接口。Adapter模式使得原来由于接口不兼容而不能一起工作的那此类可以一起工作
【GOF95】

【适配器模式结构图】

类适配器


对象适配器


【适配器模式中主要角色】
目标(Target)角色：定义客户端使用的与特定领域相关的接口，这也就是我们所期待得到的
源(Adaptee)角色：需要进行适配的接口
适配器(Adapter)角色：对Adaptee的接口与Target接口进行适配；适配器是本模式的核心，适配器把源接口转换成目标接口，此角色为具体类

【适配器模式适用场景】
1、你想使用一个已经存在的类，而它的接口不符合你的需求
2、你想创建一个可以复用的类，该类可以与其他不相关的类或不可预见的类协同工作
3、你想使用一个已经存在的子类，但是不可能对每一个都进行子类化以匹配它们的接口。对象适配器可以适配它的父类接口（仅限于对象适配器）

【类适配器模式与对象适配器】
类适配器：Adapter与Adaptee是继承关系
1、用一个具体的Adapter类和Target进行匹配。结果是当我们想要一个匹配一个类以及所有它的子类时，类Adapter将不能胜任工作
2、使得Adapter可以重定义Adaptee的部分行为，因为Adapter是Adaptee的一个子集
3、仅仅引入一个对象，并不需要额外的指针以间接取得adaptee

对象适配器：Adapter与Adaptee是委托关系
1、允许一个Adapter与多个Adaptee同时工作。Adapter也可以一次给所有的Adaptee添加功能
2、使用重定义Adaptee的行为比较困难

【适配器模式与其它模式】
桥梁模式(bridge模式)：桥梁模式与对象适配器类似，但是桥梁模式的出发点不同：桥梁模式目的是将接口部分和实现部分分离，从而对它们可以较为容易也相对独立的加以改变。而对象适配器模式则意味着改变一个已有对象的接口
装饰器模式(decorator模式)：装饰模式增强了其他对象的功能而同时又不改变它的接口。因此装饰模式对应用的透明性比适配器更好。

【类适配器模式PHP示例】
类适配器使用的是继承
 <?php
/**
 * 类适配器模式 2015-10-09
 * 1.目的
 * 将一个类的接口转换成客户希望的另外一个接口。Adapter模式使得原来由于接口不兼容而不能一起工作的那此类可以一起工作
 * 2.使用场景
 * 2.1 你想使用一个已经存在的类，而它的接口不符合你的需求
 * 2.2 你想创建一个可以复用的类，该类可以与其他不相关的类或不可预见的类协同工作
 * 2.3 你想使用一个已经存在的子类，但是不可能对每一个都进行子类化以匹配它们的接口。对象适配器可以适配它的父类接口（仅限于对象适配器）
 */

/**
 * 目标角色
 */
interface Target 
{ 
    /**
     * 源类也有的方法1
     */
    public function sampleMethod1();
 
    /**
     * 源类没有的方法2
     */
    public function sampleMethod2();
}
 
/**
 * 源角色
 */
class Adaptee 
{ 
    /**
     * 源类含有的方法
     */
    public function sampleMethod1() 
    {
        echo 'Adaptee sampleMethod1 <br />';
    }
}
 
/**
 * 类适配器角色
 */
class Adapter extends Adaptee implements Target 
{ 
    /**
     * 源类中没有sampleMethod2方法，在此补充
     */
    public function sampleMethod2() 
    {
        echo 'Adapter sampleMethod2 <br />';
    } 
}
 
class Client 
{ 
    /**
     * Main program.
     */
    public static function main() 
    {
        $adapter = new Adapter();
        $adapter->sampleMethod1();
        $adapter->sampleMethod2(); 
    } 
}
 
Client::main();
?>

【对象适配器模式PHP示例】
对象适配器使用的是委派

<?php
/**
 * 对象适配器模式
 */
 
/**
 * 目标角色
 */
interface Target 
{ 
    /**
     * 源类也有的方法1
     */
    public function sampleMethod1();
 
    /**
     * 源类没有的方法2
     */
    public function sampleMethod2();
}
 
/**
 * 源角色
 */
class Adaptee 
{ 
    /**
     * 源类含有的方法
     */
    public function sampleMethod1() 
    {
        echo 'Adaptee sampleMethod1 <br />';
    }
}
 
/**
 * 类适配器角色
 */
class Adapter implements Target 
{ 
    private $_adaptee;
 
    public function __construct(Adaptee $adaptee) 
    {
        $this->_adaptee = $adaptee;
    }
 
    /**
     * 委派调用Adaptee的sampleMethod1方法
     */
    public function sampleMethod1() 
    {
        $this->_adaptee->sampleMethod1();
    }
 
    /**
     * 源类中没有sampleMethod2方法，在此补充
     */
    public function sampleMethod2() 
    {
        echo 'Adapter sampleMethod2 <br />';
    } 
}
 
class Client 
{ 
    /**
     * Main program.
     */
    public static function main() 
    {
        $adaptee = new Adaptee();
        $adapter = new Adapter($adaptee);
        $adapter->sampleMethod1();
        $adapter->sampleMethod2(); 
    } 
}
 
Client::main();
?>
