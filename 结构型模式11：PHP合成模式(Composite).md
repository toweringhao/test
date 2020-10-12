PHP设计模式笔记：使用PHP实现合成模式

【意图】
将对象组合成树形结构以表示”部分-整体”的层次结构。Composite使用户对单个对象和组合对象的使用具有一致性。
Composite变化的是一个对象的结构和组成

【合成模式中主要角色】
抽象组件(Component)角色：抽象角色，给参加组合的对象规定一个接口。在适当的情况下，实现所有类共有接口的缺省行为。声明一个接口用于访问和管理Component的子组件
树叶组件(Leaf)角色：在组合中表示叶节点对象，叶节点没有子节点。在组合中定义图元对象的行为。
树枝组件(Composite)角色：存储子部件。定义有子部件的那些部件的行为。在Component接口中实现与子部件有关的操作。
客户端(Client)：通过Component接口操纵组合部件的对象

【合成模式的优点和缺点】
Composite模式的优点
1、简化客户代码
2、使得更容易增加新类型的组件

Composite模式的缺点：使你的设计变得更加一般化，容易增加组件也会产生一些问题，那就是很难限制组合中的组件

【合成模式适用场景】
1、你想表示对象的部分-整体层次结构
2、你希望用户忽略组合对象和单个对象的不同，用户将统一地使用组合结构中的所有对象。

【合成模式与其它模式】
装饰器模式：Decorator模式经常与Composite模式一起使用。当装饰与合成一起使用时，它们通常有一个公共的父类。因此装饰必须支持具有add,remove和getChild操作的Component接口
享元模式：Flyweight模式让你共享组件，但不再引用他们的父部件
迭代器模式：Itertor可用来遍历Composite
访问者模式：Visitor将本来应该分布在Composite和Leaf类中的操作和行为局部化。

【安全式的合成模式】
在Composite类里面声明所有的用来管理子类对象的方法。这样的做法是安全的。因为树叶类型的对象根本就没有管理子类的方法，因此，如果客户端对树叶类对象使用这些方法时，程序会在编译时期出错。编译通不过，就不会出现运行时期错误
这样的缺点是不够透明，因为树叶类和合成类将具有不同的接口。

【安全式的合成模式结构图】

安全式的合成模式
【安全式的合成模式PHP示例】
<?php 
/**
 * 合成模式 2015-10-09
 * 1.目的
 * 将对象组合成树形结构以表示”部分-整体”的层次结构。Composite使用户对单个对象和组合对象的使用具有一致性。
 * 2.使用场景
 * 2.1 你想表示对象的部分-整体层次结构
 * 2.2 你希望用户忽略组合对象和单个对象的不同，用户将统一地使用组合结构中的所有对象。
 */

/**
 * 安全的合成模式
 */

/**
 * 抽象组件角色
 */
interface Component
{
 
    /**
     * 返回自己的实例
     */
    public function getComposite();
 
    /**
     * 示例方法
     */
    public function operation();
}
 
/**
 * 树枝组件角色
 */
class Composite implements Component
{
    private $_composites;
 
    public function __construct()
    {
        $this->_composites = array();
    }
 
    public function getComposite()
    {
        return $this;
    }
 
    /**
     * 示例方法，调用各个子对象的operation方法
     */
    public function operation() 
    {
        echo 'Composite operation begin:<br />';
        foreach ($this->_composites as $composite)
        {
            $composite->operation();
        }
        echo 'Composite operation end:<br /><br />';
    }
 
    /**
     * 聚集管理方法 添加一个子对象
     * @param Component $component  子对象
     */
    public function add(Component $component) 
    {
        $this->_composites[] = $component;
    }
 
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  删除是否成功
     */
    public function remove(Component $component) 
    {
        foreach ($this->_composites as $key => $row) {
            if ($component == $row) {
                unset($this->_composites[$key]);
                return TRUE;
            }
        } 
        return FALSE;
    }
 
    /**
     * 聚集管理方法 返回所有的子对象
     */
    public function getChild() 
    {
       return $this->_composites;
    }
 
}
 
class Leaf implements Component 
{
    private $_name;
 
    public function __construct($name) 
    {
        $this->_name = $name;
    }
 
    public function operation() 
    {
        echo 'Leaf operation ', $this->_name, '<br />';
    }
 
    public function getComposite() 
    {
        return null;
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
        $leaf1 = new Leaf('first');
        $leaf2 = new Leaf('second');
 
        $composite = new Composite();
        $composite->add($leaf1);
        $composite->add($leaf2);
        $composite->operation();
 
        $composite->remove($leaf2);
        $composite->operation();
    }
 
} 

Client::main();
?>

【透明式的合成模式】
在Composite类里面声明所有的用来管理子类对象的方法。这样做的是好处是所有的组件类都有相同的接口。在客户端看来，树叶类和合成类对象的区别起码在接口层次上消失了，客户端可以同等的对待所有的对象。这就是透明形式的合成模式
缺点就是不够安全，因为树叶类对象和合成类对象在本质上是有区别的。树叶类对象不可能有下一个层次的对象，因此调用其添加或删除方法就没有意义了，这在编译期间是不会出错的，而只会在运行时期才会出错。

【透明式的合成模式结构图】

透明式的合成模式
【透明式的合成模式PHP示例】
<?php 
/**
 * 透明的合成模式 2010-08-10                                          
 */
  
/**
 * 抽象组件角色
 */
interface Component 
{
    /**
     * 返回自己的实例
     */
    public function getComposite();
 
    /**
     * 示例方法
     */
    public function operation();
 
    /**
     * 聚集管理方法 添加一个子对象
     * @param Component $component  子对象
     */
    public function add(Component $component);
 
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  删除是否成功
     */
    public function remove(Component $component);
 
    /**
     * 聚集管理方法 返回所有的子对象
     */
    public function getChild(); 
}
 
/**
 * 树枝组件角色
 */
class Composite implements Component 
{
    private $_composites;
 
    public function __construct() 
    {
        $this->_composites = array();
    }
 
    public function getComposite() 
    {
        return $this;
    }
 
    /**
     * 示例方法，调用各个子对象的operation方法
     */
    public function operation() 
    {
        echo 'Composite operation begin:<br />';
        foreach ($this->_composites as $composite) {
            $composite->operation();
        }
        echo 'Composite operation end:<br /><br />';
    }
 
    /**
     * 聚集管理方法 添加一个子对象
     * @param Component $component  子对象
     */
    public function add(Component $component) 
    {
        $this->_composites[] = $component;
    }
 
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  删除是否成功
     */
    public function remove(Component $component) 
    {
        foreach ($this->_composites as $key => $row) {
            if ($component == $row) {
                unset($this->_composites[$key]);
                return TRUE;
            }
        }
 
        return FALSE;
    }
 
    /**
     * 聚集管理方法 返回所有的子对象
     */
    public function getChild() 
    {
       return $this->_composites;
    }
 
}
 
class Leaf implements Component 
{
    private $_name;
 
    public function __construct($name) 
    {
        $this->_name = $name;
    }
 
    public function operation() 
    {
        echo 'Leaf operation ', $this->_name, '<br />';
    }
 
    public function getComposite() 
    {
        return null;
    }
 
    /**
     * 聚集管理方法 添加一个子对象,此处没有具体实现，仅返回一个FALSE
     * @param Component $component  子对象
     */
    public function add(Component $component) 
    {
        return FALSE;
    }
 
    /**
     * 聚集管理方法 删除一个子对象
     * @param Component $component  子对象
     * @return boolean  此处没有具体实现，仅返回一个FALSE
     */
    public function remove(Component $component) 
    {
        return FALSE;
    }
 
    /**
     * 聚集管理方法 返回所有的子对象 此处没有具体实现，返回null
     */
    public function getChild() 
    {
       return null;
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
        $leaf1 = new Leaf('first');
        $leaf2 = new Leaf('second');
 
        $composite = new Composite();
        $composite->add($leaf1);
        $composite->add($leaf2);
        $composite->operation();
 
        $composite->remove($leaf2);
        $composite->operation(); 
    } 
}

Client::main();
?>
可以看到透明式合成模式的Leaf有各聚集管理方法的平庸实现