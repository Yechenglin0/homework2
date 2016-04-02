一、工厂模式
优点：用一个工厂方法替代直接new一个类的功能 把创建对象的过程封装成一个工厂方法。而当类的一些属性改变了的话，只需要在工厂方法中修改即可，而不需要在每个使用了对象的地方修改。
例子：
<?php
	class Database {
		public function __construct() {
			echo 'success';
		}
	}
?>//一个类

<?php
	include 'db.class.php';
	
	class Factory {
		static function createDatabase() {
			$db = new Database;
			return $db;
		}
	}
?>//工厂类

<?php
	include 'factory.php';
	
	$db = Factory::createDatabase();
?>//在另一个php中用工厂方法实例化这个类；
二、单例模式
优点：单例类只能被自身实例化，而不能在其他类中实例化；
          主要应用在数据库应用中，使用单利模式可以避免大量new 操作 是对系统资源		  的节省。
例子：
<?php
	class Database {
		protected static $db;
		private   function __construct() {
			echo 'success';
		}
		static function getInstance() {
			if (self::$db) {
				return self::$db;
			} else {
				self::$db = new self();
				return self::$db;
			}
		}
	}
?>//一个单例模式的类
<?php
	include 'db.class.php';
	
	$db = Database::getInstance();
?>//调用这个单例模式的类
三、迭代器模式
优点：在不需要了解内部实现的前提下，遍历一个聚合对象内部元素
  支持对聚合对象的多种遍历
  为遍历不同的聚合结构提供一个统一的接口
网上的例子。。。。。。：
<?php  
header("Content-Type:text/html;charset=utf-8");   
//抽象迭代器  
abstract class IIterator  
{  
    public abstract function First();  
    public abstract function Next();  
    public abstract function IsDone();  
    public abstract function CurrentItem();  
}  
  
//具体迭代器  
class ConcreteIterator extends IIterator  
{  
    private $aggre;  
    private $current = 0;  
    public function __construct(array $_aggre)  
    {  
        $this->aggre = $_aggre;  
    }  
    //返回第一个  
    public function First()  
    {  
        return $this->aggre[0];  
    }  
  
    //返回下一个  
    public function  Next()  
    {  
        $this->current++;  
        if($this->current<count($this->aggre))  
        {  
            return $this->aggre[$this->current];  
        }  
        return false;  
    }  
  
    //返回是否IsDone  
    public function IsDone()  
    {  
        return $this->current>=count($this->aggre)?true:false;  
    }  
  
    //返回当前聚集对象  
    public function CurrentItem()  
    {  
        return $this->aggre[$this->current];  
    }  
}  

          

$iterator= new ConcreteIterator(array('周杰伦','王菲','周润发'));  
$item = $iterator->First();  
echo $item."<br/>";  
while(!$iterator->IsDone())  
{  
    echo "{$iterator->CurrentItem()}：请买票！<br/>";  
    $iterator->Next();  
}  ?>
四、观察者模式
例子：<?php
class Event{ 
    private $_observers = array();
 
    public function register($sub){ //  注册观察者 
        $this->_observers[] = $sub;
    }
 
     
    public function trigger(){  
        if(!empty($this->_observers)){
            foreach($this->_observers as $observer){
                $observer->update();
            }
        }
    }
}
 

interface Observerable{	
    public function update();
}
 
class Subscriber implements Observerable{
    public function update(){
        echo "逻辑1";
    }
}
 

$paper = new Event();
$paper->register(new Subscriber());
$paper->trigger();
?>
当一个对象状态发生改变时，依赖他的对象会全部收到通知，并自动更新
观察者模式实现了低耦合，非侵入式的通知与更新机制。。。。。。
五、注册树模式
优点：在需要调用某个对象的时候，直接从注册树上取一下就好，像使用全局变量一样方便实用
例子：<?php
	class 	register {
		protected static $objects;
		static function set($alias,$object) {
			self::$objects[$alias] = $object;
		}
		function _unset() {
			unset(self::$objects[$alias]);
		}
	}

?>
