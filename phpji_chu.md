# PHP基础


## PHP是什么
* PHP（全称：PHP：Hypertext Preprocessor，即"PHP：超文本预处理器"）是一种通用开源脚本语言。
* PHP 脚本在服务器上执行。
* PHP 可免费下载使用。


## 基本语法
PHP 脚本可以放在文档中的任何位置。
PHP 脚本以 `<?php` 开始，以 `?>` 结束：
``` php
<?php
// PHP 代码
echo "Hello World!"; 
?>
```
PHP 文件的默认文件扩展名是 ".php"。
PHP 文件通常包含 HTML 标签和一些 PHP 脚本代码。
PHP 中的每个代码行都必须以分号结束。分号是一种分隔符，用于把指令集区分开来。
通过 PHP，有两种在浏览器输出文本的基础指令：`echo` 和 `print`。

## 注释
支持单行和多行注释。
``` php
<?php
// 这是 PHP 单行注释

/*
这是 
PHP 多行
注释
*/
?>
```

## 变量
变量可以是很短的名称（如 x 和 y）或者更具描述性的名称（如 age、carname、totalvolume）。

``` php
<?php
  $x=5;
  $y=6;
  $z=$x+$y;
  echo $z;
?>
```

PHP 变量规则：
* 变量以 $ 符号开始，后面跟着变量的名称
* 变量名必须以字母或者下划线字符开始
* 变量名只能包含字母数字字符以及下划线（A-z、0-9 和 _ ）
* 变量名不能包含空格
* 变量名是区分大小写的（$y 和 $Y 是两个不同的变量）
* PHP 语句和 PHP 变量都是区分大小写的。

PHP 没有声明变量的命令。
变量在您第一次赋值给它的时候被创建：
``` php
<?php
  $txt="Hello world!";
  $x=5;
  $y=10.5;
?>
```

### PHP 是一门弱类型语言
在上面的实例中，我们注意到，不必向 PHP 声明该变量的数据类型。
PHP 会根据变量的值，自动把变量转换为正确的数据类型。
在强类型的编程语言中，我们必须在使用变量前先声明（定义）变量的类型和名称。

### 局部和全局作用域
在所有函数外部定义的变量，拥有全局作用域。除了函数外，全局变量可以被脚本中的任何部分访问，要在一个函数中访问一个全局变量，需要使用 global 关键字。

在 PHP 函数内部声明的变量是局部变量，仅能在函数内部访问：
``` php
<?php
$x=5; // 全局变量

function myTest()
{
    $y=10; // 局部变量
    echo "<p>测试函数内变量:<p>";
    echo "变量 x 为: $x";
    echo "<br>";
    echo "变量 y 为: $y";
} 

myTest();

echo "<p>测试函数外变量:<p>";
echo "变量 x 为: $x";
echo "<br>";
echo "变量 y 为: $y";
?>
```

### PHP global 关键字
global 关键字用于函数内访问全局变量。
在函数内调用函数外定义的全局变量，我们需要在函数中的变量前加上 global 关键字：
``` php
<?php
$x=5;
$y=10;

function myTest()
{
global $x,$y;
$y=$x+$y;
}

myTest();
echo $y; // 输出 15
?>
```

PHP 将所有全局变量存储在一个名为 `$GLOBALS[index]` 的数组中。 `index` 保存变量的名称。这个数组可以在函数内部访问，也可以直接用来更新全局变量。

### Static 作用域
当一个函数完成时，它的所有变量通常都会被删除。然而，有时候您希望某个局部变量不要被删除。

要做到这一点，请在您第一次声明变量时使用 static 关键字：
``` php
<?php

function myTest()
{
static $x=0;
echo $x;
$x++;
}

myTest();
myTest();
myTest();

?>
```

然后，每次调用该函数时，该变量将会保留着函数前一次被调用时的值。

注释：该变量仍然是函数的局部变量。

## echo 和 print 语句
echo 和 print 区别:
* echo - 可以输出一个或多个字符串
* print - 只允许输出一个字符串，返回值总为 1
提示：echo 输出的速度比 print 快， echo 没有返回值，print有返回值1。

## 数据类型
String（字符串）, Integer（整型）, Float（浮点型）, Boolean（布尔型）, Array（数组）, Object（对象）, NULL（空值）。

## 常量
常量是一个简单值的标识符。该值在脚本中不能改变。

一个常量由英文字母、下划线、和数字组成,但数字不能作为首字母出现。 (常量名不需要加 $ 修饰符)。

注意： 常量在整个脚本中都可以使用。

设置常量，使用 `define()` 函数，函数语法如下：
``` php
bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
```
该函数有三个参数:
* name：必选参数，常量名称，即标志符。
* value：必选参数，常量的值。
* case_insensitive ：可选参数，如果设置为 TRUE，该常量则大小写不敏感。默认是大小写敏感的。

``` php
<?php
// 区分大小写的常量名
define("GREETING", "欢迎访问 Runoob.com");
echo GREETING;    // 输出 "欢迎访问 Runoob.com"
echo '<br>';
echo greeting;   // 输出 "greeting"
?>
```

常量在定义后，默认是全局变量，可以在整个运行的脚本的任何地方使用。

## 字符串变量
字符串变量用于存储并处理文本。

当赋一个文本值给变量时，请记得给文本值加上单引号或者双引号。
* 单引号内可以有双引号，但单引号里面的变量不能被解析；
* 双引号里面可以有单引号，双引号里面的变量可以被解析
* 
``` php
<?php 
$txt1="Hello world!"; 
$txt2="What a nice day!"; 
echo $txt1 . " " . $txt2; 
?>
```
php使用`.`号连接字符串：并置运算符 `.` 用于把两个字符串值连接起来。

常用函数：
``` php
substr($str, $start, $len)截取字符串，长度为$len
strstr($str, $needle, $before_needle =false) 在字符串找到$needle，找到后从$needle首次出现位置截取到末尾。$before_needle决定返回前一部分还是后一部分
strchr($str, $needle) strstr的别名
stristr($str, $needle) strstr的不区分大小写版本
strrchr($str,$needle) 在字符串找到$needle，找到后从$needle最后一次出现位置截取到末尾

strpos($str, $needle) 返回字符串中$needle首次出现的位置
stripos($str, $needle) 不区分大小写的strpos
strrpos($str, $needle) 返回字符串中$needle最后一次出现的位置
strripos($str, $needle) 不区分大小写的strrpos

strtouper($str)转为大写
strtolower($str)转为小写
ucfirst($str)首字母大写

strlen($str) 获取字符串长度
mb_strlen($str, $encoding) 获取字符串的长度 

mb_substr($str, $start, $len, $encoding)
mb_strstr($str, $needle, $before_needle =false ,$encoding) 
mb_strpos($str, $needle, $offset, $encoding)

implode($glue, $arr)将数组使用$glup连接成字符串
explode($glue, $str)将字符串使用$glue分割成数组

str_replace($from, $to, $str)字符串替换，从$from到$to

trim($str, $needle) 去除字符串中的$needle
ltrim,rtrim与trim类似，只是分别去除左、右的$needle

strrev($str)字符串反转

chr()   从指定的 ASCII 值返回字符
ord()   返回字符串第一个字符的 ASCII 值

str_pad($str,$len, $needle = '')    把字符串填充为指定的长度
str_repeat($str, $count)    重复$count次指定字符串
str_split($str, $len = 1)   把字符串以每段$len分割到数组中

ip2long()   ip转整形数字
long2ip()   整形数字转ip
```

## 运算符


## 条件语句

## 循环语句

## 数组

## 超级全局变量

PHP中预定义了几个超级全局变量（superglobals） ，这意味着它们在一个脚本的全部作用域中都可用。 你不需要特别说明，就可以在函数及类中使用。

PHP 超级全局变量列表:
``` php
$GLOBALS 包含了全部变量的全局组合数组
$_SERVER 包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组
$_REQUEST 包含$_POST和$_GET
$_POST POST参数数组
$_GET GET参数数组
$_FILES 文件数组
$_ENV 环境变量数组
$_COOKIE COOKIE数组
$_SESSION SESSION数组
```

## 函数

## 魔术变量

## 面向对象