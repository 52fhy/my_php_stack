# PHP标准库 (SPL)

标签： PHP

[TOC]

---



## 简介
SPL是Standard PHP Library（PHP标准库）的缩写。

>The Standard PHP Library (SPL) is a collection of interfaces and classes that are meant to solve common problems.

官网说，SPL是用来解决典型问题(common problems)的一组接口与类的集合。

那么，什么是`common problems`呢？

```
- 数据结构
    解决数据怎么存储问题
- 元素遍历
    数据怎么查看
- 常用方法的统一调用
    数组、集合大小
    自定义遍历
- 类自动加载
    spl_autoload_register
```

包含哪些内容？

* 数据结构
* 基础接口
* 基础函数
* 迭代器
* 异常
* 其它

## SPL接口

### [Iterator](http://php.net/manual/zh/class.iterator.php) 迭代器接口
SPL规定，所有实现了Iterator接口的class，都可以用在foreach Loop中。Iterator接口中包含5个必须实现的方法：
``` php
interface Iterator extends Traversable{
    //返回当前元素
    public mixed current ( void );
    
    //返回当前元素的键
    public scalar key ( void );
    
    //向前移动到下一个元素
    public void next ( void );
    
    //返回到迭代器的第一个元素
    public void rewind ( void );
    
    //检查当前位置是否有效
    public boolean valid ( void );
}
```

### [ArrayAccess](http://php.net/manual/zh/class.arrayaccess.php) 数组式访问接口
实现ArrayAccess接口，可以使得object像array那样操作。ArrayAccess接口包含四个必须实现的方法：
``` php
interface ArrayAccess {
    //检查一个偏移位置是否存在 
    public mixed offsetExists ( mixed $offset  );
    
    //获取一个偏移位置的值 
    public mixed offsetGet( mixed $offset  );
    
    //设置一个偏移位置的值 
    public mixed offsetSet ( mixed $offset  );
    
    //复位一个偏移位置的值 
    public mixed offsetUnset  ( mixed $offset  );
}
```

### [IteratorAggregate](http://php.net/manual/zh/class.iteratoraggregate.php) 聚合式迭代器接口
假设对象A实现了上面的ArrayAccess接口，这时候虽然可以像数组那样操作，却无法使用foreach遍历，除非实现了前面提到的Iterator接口。

另一个解决方法是，有时会需要将数据和遍历部分分开，这时就可以实现IteratorAggregate接口。它规定了一个`getIterator()`方法，返回一个使用Iterator接口的object。

```
IteratorAggregate extends Traversable {
    /* 获取一个外部迭代器 */
    abstract public Traversable getIterator ( void )
}
```

示例：
``` php
<?php
class myData implements IteratorAggregate {
    public $property1 = "Public property one";
    public $property2 = "Public property two";
    public $property3 = "Public property three";

    public function __construct() {
        $this->property4 = "last property";
    }

    public function getIterator() {
        return new ArrayIterator($this);
    }
}

$obj = new myData;

foreach($obj as $key => $value) {
    var_dump($key, $value);
    echo "\n";
}
?>
```

>注意：
虽然都继承自`Traversable`，但这是一个无法在 PHP 脚本中实现的内部引擎接口。我们直接使用`IteratorAggregate` 或 `Iterator` 接口来代替它。

### [RecursiveIterator](http://php.net/manual/zh/class.recursiveiterator.php)
这个接口用于遍历多层数据，它继承了Iterator接口，因而也具有标准的current()、key()、next()、 rewind()和valid()方法。同时，它自己还规定了`getChildren()`和`hasChildren()`方法。The `getChildren()` method must return an object that implements RecursiveIterator。

### [SeekableIterator](http://php.net/manual/zh/class.seekableiterator.php)
SeekableIterator接口也是Iterator接口的延伸，除了Iterator的5个方法以外，还规定了`seek()`方法，参数是元素的位置，返回该元素。如果该位置不存在，则抛出OutOfBoundsException。

### [Countable](http://php.net/manual/zh/class.countable.php)
这个接口规定了一个`count()`方法，返回结果集的数量。

## SPL数据结构

数据结构是计算机存储、组织数据的方式。

SPL提供了双向链表、堆栈、队列、堆、降序堆、升序堆、优先级队列、定长数组、对象容器。

![](http://images2015.cnblogs.com/blog/663847/201606/663847-20160610154917886-1414429759.jpg)



**基本概念**
Bottom：节点，第一个节点称Bottom；
Top：最后添加的链表的节点称Top；
当前节点（Current）：链表指针指向的节点称为当前节点；

### [SplDoublyLinkedList](http://php.net/manual/zh/class.spldoublylinkedlist.php) 双向链表

SplDoublyLinkedList 实现了`Iterator` , `ArrayAccess` , `Countable`接口。

类摘要
``` php
SplDoublyLinkedList implements Iterator , ArrayAccess , Countable {
/* 方法 */
public __construct ( void )
public void add ( mixed $index , mixed $newval )
public mixed bottom ( void )
public int count ( void )
public mixed current ( void )
public int getIteratorMode ( void )
public bool isEmpty ( void )
public mixed key ( void )
public void next ( void )
public bool offsetExists ( mixed $index )
public mixed offsetGet ( mixed $index )
public void offsetSet ( mixed $index , mixed $newval )
public void offsetUnset ( mixed $index )
public mixed pop ( void )
public void prev ( void )
public void push ( mixed $value )
public void rewind ( void )
public string serialize ( void )
public void setIteratorMode ( int $mode )
public mixed shift ( void )
public mixed top ( void )
public void unserialize ( string $serialized )
public void unshift ( mixed $value )
public bool valid ( void )
}
```

**注意：**
`SplDoublyLinkedList::setIteratorMode`用来设置链表模式：

迭代方向:
```
SplDoublyLinkedList::IT_MODE_LIFO (Stack style)
SplDoublyLinkedList::IT_MODE_FIFO (Queue style)
```
迭代器行为:
```
SplDoublyLinkedList::IT_MODE_DELETE (Elements are deleted by the iterator)
SplDoublyLinkedList::IT_MODE_KEEP (Elements are traversed by the iterator)
```
默认模式: `SplDoublyLinkedList::IT_MODE_FIFO | SplDoublyLinkedList::IT_MODE_KEEP`

**当前节点操作：**
rewind:将链表的当前指针指向第一个元素
current：链表当前指针，当节点被删除后，会指向空节点
prev：上一个
next：下一个

**增加节点操作：**
push 在双向链表的结尾处将元素压入
unshift 前置双链表元素,预备值在双链表的开始

**删除节点操作：**
pop 从双向链表的结尾弹出一个节点，不会改变指针位置
shift从双向链表的开头弹出一个节点，不会改变指针位置

**定位操作：**
bottom 返回当前双向链表的第一个节点的值，当前指针不变
top返回当前双向链表的最后一个节点的值，当前指针不变

**特定节点操作：**
offsetExists 理解为key是否存在
offsetGet将key节点拿出来
offsetSet把数据刷新
offsetUnset删除

示例：SplDoublyLinkedList.php
``` php
<?php
/**
*SplDoublyLinkedList 类学习
*/
$obj = new SplDoublyLinkedList();
$obj -> push(1);//把新的节点添加到链表的顶部top
$obj -> push(2);
$obj -> push(3);
$obj -> unshift(10);//把新节点添加到链表底部bottom
print_r($obj);
$obj ->rewind();//rewind操作用于把节点指针指向Bottom所在节点

$obj -> prev();//使指针指向上一个节点，靠近Bottom方向
echo 'next node :'.$obj->current().PHP_EOL;
$obj -> next();
$obj -> next();
echo 'next node :'.$obj->current().PHP_EOL;

$obj -> next();
if($obj -> current())
        echo 'current node valid'.PHP_EOL;
else
        echo 'current node invalid'.PHP_EOL;
$obj ->rewind();
//如果当前节点是有效节点，valid返回true
if($obj->valid())
        echo 'valid list'.PHP_EOL;
else
        echo 'invalid list'.PHP_EOL;
print_r($obj);
echo 'pop value :'.$obj -> pop().PHP_EOL;
print_r($obj);
echo 'next node :'.$obj ->current().PHP_EOL;
$obj ->next();//1
$obj ->next();//2
$obj -> pop();//把top位置的节点从链表中删除，并返回，如果current正好指>向top位置，那么调用pop之后current()会失效
echo 'next node:'.$obj -> current().PHP_EOL;
print_r($obj);

$obj ->shift();//把bottom位置的节点从链表中删除，并返回
print_r($obj);
```

### [SplStack](http://php.net/manual/zh/class.splstack.php) 栈
栈(Stack)是一种特殊的线性表，因为它只能在线性表的一端进行插入或删除元素(即进栈和出栈)。

栈是一种**后进先出(LIFO)**的数据结构。

SplStack 继承自 **[双向链表 SplDoublyLinkedList](http://php.net/manual/zh/class.spldoublylinkedlist.php)**。

示例：
``` php
<?php

$stack = new SplStack();
$stack->push(1);
$stack->push(2);
$stack->push(3);

echo 'bottom:'.$stack -> bottom().PHP_EOL;
echo "top:".$stack->top().PHP_EOL;
//堆栈的offset=0,是top所在位置（即栈的末尾）
$stack -> offsetSet(0, 10);
echo "top:".$stack->top().'<br/>';

//堆栈的rewind和双向链表的rewind相反，堆栈的rewind使得当前指针指向top所在位置，而双向链表调用之后指向bottom所在位置
$stack -> rewind();
echo 'current:'.$stack->current().'<br/>';

$stack ->next();//堆栈的next操作使指针指向靠近bottom位置的下一个节点，而双向链表是靠近top的下一个节点
echo 'current:'.$stack ->current().'<br/>';

//遍历堆栈
$stack -> rewind();
while ($stack->valid()) {
    echo $stack->key().'=>'.$stack->current().PHP_EOL;
    $stack->next();//不从链表中删除元素
}
echo '<br/>';

echo $stack->pop() .'--';
echo $stack->pop() .'--';
echo $stack->pop() .'--';
```
输出：
```
bottom:1 top:3 top:10
current:10
current:2
2=>10 1=>2 0=>1 
10--2--1--
```

### [SplQueue](http://php.net/manual/zh/class.splqueue.php) 队列
队列是一种**先进先出(FIFO)**的数据结构。使用队列时插入在一端进行而删除在另一端进行。

![](http://images2015.cnblogs.com/blog/663847/201606/663847-20160610154955558-577008856.png)



SplQueue 也是继承自 **[双向链表 SplDoublyLinkedList](http://php.net/manual/zh/class.spldoublylinkedlist.php)**，并有自己的方法：
``` php
/* 方法 */
__construct ( void )
mixed dequeue ( void )
void enqueue ( mixed $value )
void setIteratorMode ( int $mode )
```

示例1：
``` php
<?php

$queue = new SplQueue();
$queue->enqueue(1);
$queue->enqueue(2);

echo $queue->dequeue() .'--';
echo $queue->dequeue() .'--';

//1--2--
```

示例2：
``` php
<?php

$obj = new SplQueue();

$obj -> enqueue('a');
$obj -> enqueue('b');
$obj -> enqueue('c');

echo 'bottom:'.$obj -> bottom().PHP_EOL;
echo 'top:'.$obj -> top();
echo '<br/>';

//队列里的offset=0是指向bottom位置
$obj -> offsetSet(0,'A');
echo 'bottom:'.$obj -> bottom();
echo '<br/>';

//队列里的rewind使得指针指向bottom所在位置的节点
$obj -> rewind();
echo 'current:'.$obj->current();
echo '<br/>';

while ($obj ->valid()) {
    echo $obj ->key().'=>'.$obj->current().PHP_EOL;
    $obj->next();//
}
echo '<br/>';

//dequeue操作从队列中提取bottom位置的节点，并返回，同时从队列里面删除该元素
echo 'dequeue obj:'.$obj->dequeue();
echo '<br/>';
echo 'bottom:'.$obj -> bottom().PHP_EOL;
```
输出：
```
bottom:a top:c
bottom:A
current:A
0=>A 1=>b 2=>c 
dequeue obj:A
bottom:b
```

### [SplHeap](http://php.net/manual/zh/class.splheap.php) 堆
**堆(Heap)**就是为了实现优先队列而设计的一种数据结构，它是通过构造二叉堆(二叉树的一种)实现。

根节点最大的堆叫做**最大堆**或大根堆，根节点最小的堆叫做**最小堆**或小根堆。二叉堆还常用于排序(堆排序)。

SplHeap 是一个抽象类，实现了`Iterator` , `Countable`接口。最大堆(SplMaxHeap)和最小堆(SplMinHeap)就是继承它实现的。最大堆和最小堆并没有额外的方法。

如皋要使用SplHeap类，需要实现其抽象方法`int compare ( mixed $value1 , mixed $value2 )`。

类摘要:
```php
abstract SplHeap implements Iterator , Countable {
    /* 方法 */
    public __construct ( void )
    abstract protected int compare ( mixed $value1 , mixed $value2 )
    public int count ( void )
    public mixed current ( void )
    public mixed extract ( void )
    public void insert ( mixed $value )
    public bool isEmpty ( void )
    public mixed key ( void )
    public void next ( void )
    public void recoverFromCorruption ( void )
    public void rewind ( void )
    public mixed top ( void )
    public bool valid ( void )
}
```

示例：
``` php
<?php

class MySimpleHeap extends SplHeap
{
    //compare()方法用来比较两个元素的大小，绝对他们在堆中的位置
    public function  compare( $value1, $value2 ) {
        return ( $value1 - $value2 );
    }
}
 
$obj = new MySimpleHeap();
$obj->insert( 4 );
$obj->insert( 8 );
$obj->insert( 1 );
$obj->insert( 0 );
 
echo $obj->top();  //8
echo $obj->count(); //4
echo '<br/>';
 
foreach( $obj as $number ) {
    echo $number.PHP_EOL;
}
```
输出：
```
84
8 4 1 0
```

### [SplMaxHeap](http://php.net/manual/zh/class.splmaxheap.php) 最大堆

最大堆(SplMaxHeap)继承自抽象类SplHeap实现的。最大堆并没有额外的方法。

### [SplMinHeap ](http://php.net/manual/zh/class.splminheap.php) 最小堆

最小堆(SplMinxHeap)继承自抽象类SplHeap实现的。最小堆并没有额外的方法。

如下：最小堆（任意节点的优先级不小于它的子节点）

![](http://images2015.cnblogs.com/blog/663847/201606/663847-20160610155025511-30020944.png)


示例：
``` php
<?php

$obj = new SplMinHeap();
$obj->insert(4);
$obj->insert(8);

//提取
echo $obj->extract(). PHP_EOL;
echo $obj->extract();

//4 8
```


### [SplPriorityQueue](http://php.net/manual/zh/class.splpriorityqueue.php) 优先级队列

优先级队列SplPriorityQueue是基于堆实现的。和堆一样，也有`int compare ( mixed $priority1 , mixed $priority2 )`方法。

SplPriorityQueue 实现了`Iterator` , `Countable` 接口。

示例：
``` php
$pq = new SplPriorityQueue();
 
$pq->insert('a', 10);
$pq->insert('b', 1);
$pq->insert('c', 8);
 
echo $pq->count() .PHP_EOL; //3
echo $pq->current() . PHP_EOL; //a
 
/**
 * 设置元素出队模式
 * SplPriorityQueue::EXTR_DATA 仅提取值
 * SplPriorityQueue::EXTR_PRIORITY 仅提取优先级
 * SplPriorityQueue::EXTR_BOTH 提取数组包含值和优先级
 */
$pq->setExtractFlags(SplPriorityQueue::EXTR_DATA);
 
while($pq->valid()) {
    print_r($pq->current());  //a  c  b
    $pq->next();
}
```

### [SplFixedArray](http://php.net/manual/zh/class.splfixedarray.php) 定长数组

SplFixedArray 实现了`Iterator` , `ArrayAccess` , `Countable` 接口。

和普通数组不一样，定长数组规定了数组的长度。优势就是比普通的数组处理更快。

``` php
<?php

$arr = new SplFixedArray(5);
$arr[0] = 1;
$arr[1] = 2;
$arr[2] = 3;

print_r($arr);

//SplFixedArray Object ( [0] => 1 [1] => 2 [2] => 3 [3] => [4] => )
```

### [SplObjectStorage ](http://php.net/manual/zh/class.splobjectstorage.php) 对象容器

SplObjectStorage是用来存储一组对象的，特别是当你需要唯一标识对象的时候。该类实现了`Countable` ,`Iterator` ,`Serializable` ,`ArrayAccess`四个接口。可实现统计、迭代、序列化、数组式访问等功能。

示例：
``` php
class A {
    public $i;
    public function __construct($i) {
        $this->i = $i;
    }
}
 
$a1 = new A(1);
$a2 = new A(2);
$a3 = new A(3);
$a4 = new A(4);
 
$container = new SplObjectStorage();
 
//SplObjectStorage::attach 添加对象到Storage中
$container->attach($a1);
$container->attach($a2);
$container->attach($a3);
 
//SplObjectStorage::detach 将对象从Storage中移除
$container->detach($a2);
 
//SplObjectStorage::contains用于检查对象是否存在Storage中
var_dump($container->contains($a1)); //true
var_dump($container->contains($a4)); //false
 
//遍历
$container->rewind();
while($container->valid()) {
    var_dump($container->current());
    $container->next();
}
```



## SPL类

### [SPL的内置类](http://php.net/manual/zh/function.spl-classes.php)
SPL除了定义一系列Interfaces以外，还提供一系列的内置类，它们对应不同的任务，大大简化了编程。
查看所有的内置类，可以使用下面的代码：
``` php
<?php
// a simple foreach() to traverse the SPL class names
foreach(spl_classes() as $key=>$value)
        {
        echo $key.' -&gt; '.$value.'<br />';
        }
?>
```

### [SplFileInfo](http://php.net/manual/zh/class.splfileinfo.php) 
PHP SPL中提供了SplFileInfo和SplFileObject两个类来处理文件操作。

SplFileInfo用来获取文件详细信息：

``` php
$file = new SplFileInfo('foo-bar.txt');
 
print_r(array(
    'getATime' => $file->getATime(), //最后访问时间
    'getBasename' => $file->getBasename(), //获取无路径的basename
    'getCTime' => $file->getCTime(), //获取inode修改时间
    'getExtension' => $file->getExtension(), //文件扩展名
    'getFilename' => $file->getFilename(), //获取文件名
    'getGroup' => $file->getGroup(), //获取文件组
    'getInode' => $file->getInode(), //获取文件inode
    'getLinkTarget' => $file->getLinkTarget(), //获取文件链接目标文件
    'getMTime' => $file->getMTime(), //获取最后修改时间
    'getOwner' => $file->getOwner(), //文件拥有者
    'getPath' => $file->getPath(), //不带文件名的文件路径
    'getPathInfo' => $file->getPathInfo(), //上级路径的SplFileInfo对象
    'getPathname' => $file->getPathname(), //全路径
    'getPerms' => $file->getPerms(), //文件权限
    'getRealPath' => $file->getRealPath(), //文件绝对路径
    'getSize' => $file->getSize(),//文件大小，单位字节
    'getType' => $file->getType(),//文件类型 file  dir  link
    'isDir' => $file->isDir(), //是否是目录
    'isFile' => $file->isFile(), //是否是文件
    'isLink' => $file->isLink(), //是否是快捷链接
    'isExecutable' => $file->isExecutable(), //是否可执行
    'isReadable' => $file->isReadable(), //是否可读
    'isWritable' => $file->isWritable(), //是否可写
));
```

### [SplFileObject](http://php.net/manual/zh/class.splfileobject.php)
SplFileObject继承`SplFileInfo`并实现`RecursiveIterator`、 `SeekableIterator`接口 ，用于对文件遍历、查找、操作遍历：

``` php
try {
    foreach(new SplFileObject('foo-bar.txt') as $line) {
        echo $line;
    }
} catch (Exception $e) {
    echo $e->getMessage();
}
```

查找指定行：
```
try {
    $file = new SplFileObject('foo-bar.txt');
    $file->seek(2);
    echo $file->current();
} catch (Exception $e) {
    echo $e->getMessage();
}
```

写入csv文件：
```
$list  = array (
    array( 'aaa' ,  'bbb' ,  'ccc' ,  'dddd' ),
    array( '123' ,  '456' ,  '7891' )
);
 
$file  = new  SplFileObject ( 'file.csv' ,  'w' );
 
foreach ( $list  as  $fields ) {
    $file -> fputcsv ( $fields );
}
```


### [DirectoryIterator](http://php.net/manual/zh/class.directoryiterator.php)
该类继承自`SplFileInfo `并实现`SeekableIterator`接口。

这个类用来查看一个目录中的所有文件和子目录：
```
<?php

try{
  /*** class create new DirectoryIterator Object ***/
    foreach ( new DirectoryIterator('./') as $Item )
        {
        echo $Item.'<br />';
        }
    }
/*** if an exception is thrown, catch it here ***/
catch(Exception $e){
    echo 'No files Found!<br />';
}
?>
```

### [ArrayObject](http://php.net/manual/zh/class.arrayobject.php)
该类实现了`ArrayAccess` ,`Countable`, `IteratorAggregate`, `Serializable`接口。

这个类可以将Array转化为object。
``` php
<?php

/*** a simple array ***/
$array = array('koala', 'kangaroo', 'wombat', 'wallaby', 'emu', 'kiwi', 'kookaburra', 'platypus');

/*** create the array object ***/
$arrayObj = new ArrayObject($array);

/*** iterate over the array ***/
for($iterator = $arrayObj->getIterator();
   /*** check if valid ***/
   $iterator->valid();
   /*** move to the next array member ***/
   $iterator->next())
    {
    /*** output the key and current array value ***/
    echo $iterator->key() . ' => ' . $iterator->current() . '<br />';
    }
?>
```

### [ArrayIterator](http://php.net/manual/zh/class.arrayiterator.php)
该类实现了`ArrayAccess`, `Countable` , `SeekableIterator`  , `Serializable` 接口。

这个类实际上是对ArrayObject类的补充，为后者提供遍历功能。
``` php
<?php
/*** a simple array ***/
$array = array('koala', 'kangaroo', 'wombat', 'wallaby', 'emu', 'kiwi', 'kookaburra', 'platypus');

try {
    $object = new ArrayIterator($array);
    foreach($object as $key=>$value)
        {
        echo $key.' => '.$value.'<br />';
        }
    }
catch (Exception $e)
    {
    echo $e->getMessage();
    }
?>
```

>参考
1、PHP: SPL - Manual
http://php.net/manual/zh/book.spl.php
2、PHP: 预定义接口 - Manual
http://php.net/manual/zh/reserved.interfaces.php
3、PHP SPL笔记 - 阮一峰的网络日志
http://www.ruanyifeng.com/blog/2008/07/php_spl_notes.html
4、PHP SPL标准库之文件操作(SplFileInfo和SplFileObject) - PHP点点通
http://www.phpddt.com/php/SplFileObject.html







