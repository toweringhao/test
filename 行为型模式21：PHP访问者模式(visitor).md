PHP中的设计模式笔记：访问者模式

【Visitor模式】
访问者模式表示一个作用于某对象结构中各元素的操作。它可以在不修改各元素类的前提下定义作用于这些元素的新操作，即动态的增加具体访问者角色。
访问者模式利用了双重分派。先将访问者传入元素对象的Accept方法中，然后元素对象再将自己传入访问者，之后访问者执行元素的相应方法。
访问者模式多用在聚集类型多样的情况下。在普通的形式下必须判断每个元素是属于什么类型然后进行相应的操作，从而诞生出冗长的条件转移语句。而访问者模式则可以比较好的解决这个问题。对每个元素统一调用$element->accept($vistor)即可。
访问者模式多用于被访问的类结构比较稳定的情况下，即不会随便添加子类。访问者模式允许被访问结构添加新的方法。
Visitor模式实际上是分离了对象结构中的元素和对这些元素进行操作的行为，从而使我们在根据对象结构中的元素进行方法调用的时候，不需要使用IF语句判断
也就是封装了操作
但是，如果增加新的元素节点，则会导致包括访问者接口及其子类的改变，这就会违反了面向对象中的开闭原则
当这种情况出现时，一般表示访问者模式已经可能不再适用了，或者说设计时就有问题了！
【Visitor模式结构图】

访问者模式简略图

【Visitor模式中主要角色】
1)抽象访问者角色(Visitor)：为该对象结构(ObjectStructure)中的每一个具体元素提供一个访问操作接口。该操作接口的名字和参数标识了 要访问的具体元素角色。这样访问者就可以通过该元素角色的特定接口直接访问它。
2)具体访问者角色(ConcreteVisitor):实现抽象访问者角色接口中针对各个具体元素角色声明的操作。
3)抽象节点（Node）角色:该接口定义一个accept操作接受具体的访问者。
4)具体节点（Node）角色:实现抽象节点角色中的accept操作。
5) 对象结构角色(ObjectStructure)：这是使用访问者模式必备的角色。它要具备以下特征：能枚举它的元素；可以提供一个高层的接口以允许该访问者访问它的元素；可以是一个复合（组合模式）或是一个集合，如一个列表或一个无序集合(在PHP中我们使用数组代替，因为PHP中的数组本来就是一个可以放置任何类型数据的集合)

【Visitor模式的优缺点】
访问者模式有如下的优点：
1)访问者模式使得增加新的操作变得很容易。使用访问者模式可以在不用修改具体元素类的情况下增加新的操作。它主要是通过元素类的accept方法来接受一个新的visitor对象来实现的。如果一些操作依赖于一个复杂的结构对象的话，那么一般而言，增加新的操作会很复杂。而使用访问者模式，增加新的操作就意味着增加一个新的访问者类，因此，变得很容易。
2)访问者模式将有关的行为集中到一个访问者对象中，而不是分散到一个个的节点类中。
3)访问者模式可以跨过几个类的等级结构访问属于不同的等级结构的成员类。迭代子只能访问属于同一个类型等级结构的成员对象，而不能访问属于不同等级结构的对象。访问者模式可以做到这一点。
4)积累状态。每一个单独的访问者对象都集中了相关的行为，从而也就可以在访问的过程中将执行操作的状态积累在自己内部，而不是分散到很多的节点对象中。这是有益于系统维护的优点。

访问者模式有如下的缺点：
1)增加新的节点类变得很困难。每增加一个新的节点都意味着要在抽象访问者角色中增加一个新的抽象操作，并在每一个具体访问者类中增加相应的具体操作。
2)破坏封装。访问者模式要求访问者对象访问并调用每一个节点对象的操作，这隐含了一个对所有节点对象的要求：它们必须暴露一些自己的操作和内部状态。不然，访问者的访问就变得没有意义。由于访问者对象自己会积累访问操作所需的状态，从而使这些状态不再存储在节点对象中，这也是破坏封装的。

使用Visitor模式的前提: 对象群结构中(Collection) 中的对象类型很少改变。
在接口Visitor和Element中,确保Element很少变化,也就是说，确保不能频繁的添加新的Element元素类型加进来，可以变化的是访问者行为或操作，也就是Visitor的不同子类可以有多种,这样使用访问者模式最方便.
如果对象集合中的对象集合经常有变化, 那么不但Visitor实现要变化，ConcreteVisitor也要增加相应行为，GOF建议是,不如在这些对象类中直接逐个定义操作，无需使用访问者设计模式。

【Visitor模式与其它模式】
1、如果所浏览的结构对象是线性的，使用迭代模式而不是访问者模式也是可以的
2、访问者模式浏览合成模式的一些结构对象
以上两点来自《Java与模式》一书

【Visitor模式PHP示例】
<?php
/**
 * 访问者模式 2015-10-09
 * 1.目的
 * 访问者模式表示一个作用于某对象结构中各元素的操作。它可以在不修改各元素类的前提下定义作用于这些元素的新操作，即动态的增加具体访问者角色
 * 2.使用场景
 * 2.1 对象群结构中(Collection) 中的对象类型很少改变。
 */

/**
 * 抽象访问者
 */
interface Visitor 
{
    public function visitConcreteElementA(ConcreteElementA $elementA);
    public function visitConcreteElementB(ConcreteElementB $elementB);
}

/**
 * 抽象元素
 */
interface Element
{
    public function accept(Visitor $visitor);
}
 
/**
 * 具体的访问者1
 */
class ConcreteVisitor1 implements Visitor
{
    public function visitConcreteElementA(ConcreteElementA $elementA) 
    {
        echo $elementA->getName() . " visitd by ConcerteVisitor1 <br />";
    }
 
    public function visitConcreteElementB(ConcreteElementB $elementB) 
    {
        echo $elementB->getName() . " visited by ConcerteVisitor1 <br />";
    }
}
 
/**
 * 具体的访问者2
 */
class ConcreteVisitor2 implements Visitor
{
    public function visitConcreteElementA(ConcreteElementA $elementA) 
    {
        echo $elementA->getName() . " visitd by ConcerteVisitor2 <br />";
    }
 
    public function visitConcreteElementB(ConcreteElementB $elementB) 
    {
        echo $elementB->getName() . " visited by ConcerteVisitor2 <br />";
    }
}
 
/**
 * 具体元素A
 */
class ConcreteElementA implements Element 
{
    private $_name;
 
    public function __construct($name) 
    {
        $this->_name = $name;
    }
 
    public function getName() 
    {
        return $this->_name;
    }
 
    /**
     * 接受访问者调用它针对该元素的新方法
     * @param Visitor $visitor
     */
    public function accept(Visitor $visitor) 
    {
        $visitor->visitConcreteElementA($this);
    }
}
 
/**
 *  具体元素B
 */
class ConcreteElementB implements Element
{
    private $_name;
 
    public function __construct($name) 
    {
        $this->_name = $name;
    }
 
    public function getName() 
    {
        return $this->_name;
    }
 
    /**
     * 接受访问者调用它针对该元素的新方法
     * @param Visitor $visitor
     */
    public function accept(Visitor $visitor) 
    {
        $visitor->visitConcreteElementB($this);
    }
}
 
/**
 * 对象结构 即元素的集合
 */
class ObjectStructure
{
    private $_collection;
 
    public function __construct() 
    {
        $this->_collection = array();
    }
 
 
    public function attach(Element $element) 
    {
        return array_push($this->_collection, $element);
    }
 
    public function detach(Element $element) 
    {
        $index = array_search($element, $this->_collection);
        if ($index !== FALSE) {
            unset($this->_collection[$index]);
        }
 
        return $index;
    }
 
    public function accept(Visitor $visitor) 
    {
        foreach ($this->_collection as $element) {
            $element->accept($visitor);
        }
    }
}
 
class Client
{ 
    /**
     * Main program.
     */
    public static function main() 
    {
        $elementA = new ConcreteElementA("ElementA");
        $elementB = new ConcreteElementB("ElementB");
        $elementA2 = new ConcreteElementB("ElementA2");
        $visitor1 = new ConcreteVisitor1();
        $visitor2 = new ConcreteVisitor2();
 
        $os = new ObjectStructure();
        $os->attach($elementA);
        $os->attach($elementB);
        $os->attach($elementA2);
        $os->detach($elementA);
        $os->accept($visitor1);
        $os->accept($visitor2);
    }
}
 
Client::main();
?>