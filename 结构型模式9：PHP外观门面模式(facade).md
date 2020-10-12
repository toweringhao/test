PHP设计模式笔记：使用PHP实现门面模式

【意图】
为子系统中的一组接口提供一个一致的界面，Facade模式定义了一个高层次的接口，使得子系统更加容易使用【GOF95】
外部与子系统的通信是通过一个门面(Facade)对象进行。

【门面模式结构图】

门面模式

【门面模式中主要角色】
门面(Facade)角色：
此角色将被客户端调用
知道哪些子系统负责处理请求
将用户的请求指派给适当的子系统

子系统(subsystem)角色：
实现子系统的功能
处理由Facade对象指派的任务
没有Facade的相关信息，可以被客户端直接调用
可以同时有一个或多个子系统，每个子系统都不是一个单独的类，而一个类的集合。每个子系统都可以被客户端直接调用，或者被门面角色调用。子系统并知道门面模式的存在，对于子系统而言，门面仅仅是另一个客户端。

【门面模式的优点】
1、它对客户屏蔽了子系统组件，因而减少了客户处理的对象的数目并使得子系统使用起来更加方便
2、实现了子系统与客户之间的松耦合关系
3、如果应用需要，它并不限制它们使用子系统类。因此可以在系统易用性与能用性之间加以选择

【门面模式适用场景】
1、为一些复杂的子系统提供一组接口。
2、提高子系统的独立性
3、在层次化结构中，可以使用门面模式定义系统的每一层的接口

【门面模式与其它模式】
抽象工厂模式(abstract factory模式)：Abstract Factory模式可以与Facade模式一起使用以提供一个接口，这一接口可用来以一种子系统独立的方式创建子系统对象。Abstract Factory模式也可以代替Facade模式隐藏那些与平台相关的类
调停者模式：Mediator模式与Facade模式的相似之处是，它抽象了一些已有类的功能。然而，Mediator目的是对同事之间的任意通讯进行抽象，通常集中不属于任何单个对象的功能。Mediator的同事对象知道中介者并与它通信，而不是直接与其他同类对象通信。相对而言，Facade模式仅对子系统对象的接口进行抽象，从而使它们更容易使用；它并定义不功能，子系统也不知道facade的存在
单例模式(singleton模式)：一般来说，仅需要一个Facade对象，因此Facade对象通常属于Singleton对象。

【门面模式PHP示例】
<?php
/**
 * 门面模式 2015-10-09
 * 1.目的
 * 为子系统中的一组接口提供一个一致的界面，Facade模式定义了一个高层次的接口，使得子系统更加容易使用
 * 2.使用场景
 * 2.1 为一些复杂的子系统提供一组接口
 * 2.2 提高子系统的独立性
 * 2.3 在层次化结构中，可以使用门面模式定义系统的每一层的接口
 */

/**
 * 录像角色
 */
class Camera 
{ 
    /**
     * 打开录像机
     */
    public function turnOn() 
    {
        echo 'Turning on the camera.<br />';
    }
 
    /**
     * 关闭录像机
     */
    public function turnOff() 
    {
        echo 'Turning off the camera.<br />';
    }
 
    /**
     * 转到录像机
     * @param <type> $degrees
     */
    public function rotate($degrees) 
    {
        echo 'rotating the camera by ', $degrees, ' degrees.<br />';
    }
}
 
class Light 
{ 
    /**
     * 开灯
     */
    public function turnOn() 
    {
        echo 'Turning on the light.<br />';
    }
 
    /**
     * 关灯
     */
    public function turnOff() 
    {
        echo 'Turning off the light.<br />';
    }
 
    /**
     * 换灯泡
     */
    public function changeBulb() 
    {
        echo 'changing the light-bulb.<br />';
    }
}
 
class Sensor 
{ 
    /**
     * 启动感应器
     */
    public function activate() 
    {
        echo 'Activating the sensor.<br />';
    }
 
    /**
     * 关闭感应器
     */
    public function deactivate() 
    {
        echo 'Deactivating the sensor.<br />';
    }
 
    /**
     * 触发感应器
     */
    public function trigger() 
    {
        echo 'The sensor has been trigged.<br />';
    }
}
 
class Alarm 
{ 
    /**
     * 启动警报器
     */
    public function activate() 
    {
        echo 'Activating the alarm.<br />';
    }
 
    /**
     * 关闭警报器
     */
    public function deactivate() 
    {
        echo 'Deactivating the alarm.<br />';
    }
 
    /**
     * 拉响警报器
     */
    public function ring() 
    {
        echo 'Ring the alarm.<br />';k k k
    }
 
    /**
     * 停掉警报器
     */
    public function stopRing() 
    {
        echo 'Stop the alarm.<br />';
    }
}
 
/**
 * 门面类
 */
class SecurityFacade 
{ 
    /* 录像机 */
    private $_camera1, $_camera2;
 
    /* 灯 */
    private $_light1, $_light2, $_light3;
 
    /* 感应器 */
    private $_sensor;
 
    /* 警报器 */
    private $_alarm;
 
    public function __construct() 
    {
        $this->_camera1 = new Camera();
        $this->_camera2 = new Camera();
 
        $this->_light1 = new Light();
        $this->_light2 = new Light();
        $this->_light3 = new Light();
 
        $this->_sensor = new Sensor();
        $this->_alarm = new Alarm();
    }
 
    public function activate() 
    {
        $this->_camera1->turnOn();
        $this->_camera2->turnOn();
 
        $this->_light1->turnOn();
        $this->_light2->turnOn();
        $this->_light3->turnOn();
 
        $this->_sensor->activate();
        $this->_alarm->activate();
    }
 
    public  function deactivate() 
    {
        $this->_camera1->turnOff();
        $this->_camera2->turnOff();
 
        $this->_light1->turnOff();
        $this->_light2->turnOff();
        $this->_light3->turnOff();
 
        $this->_sensor->deactivate();
        $this->_alarm->deactivate();
    }
}

/**
 * 客户端
 */
class Client 
{ 
    private static $_security;
     /**
     * Main program.
     */
    public static function main() 
    {
        self::$_security = new SecurityFacade();
        self::$_security->activate();
    }
}
 
Client::main();
?>