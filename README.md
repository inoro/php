
## PHP7.2 后新特性总结


#### 1. 定义类属性类型
```php
class User {

   // 如果属性类型错误则会报错
   // 报错实例返回：Fatal error: Uncaught TypeError: Typed property User::$id must be int, string used in /www/index.php:14 Stack trace: #0 /www/index.php(21): User->__construct(21, 'xingming') #1 {main} thrown in /www/index.php on line 14
   public int $id;
   public string $name;

   // 构造函数
   function __construct($name,$id)
   {  
      $this->id = $id;
      $this->name = $name;
      print_r($this);
   }

}
// 实例化 User 类
$modals = new User("xingming",21);

```
---

#### 2. null合并运算符
```php
// 说明：如果不存在则会返回后面的值
$username = $_GET['user'] ?? '暂无用户姓名';
print_r($username);

$userinfo = $_GET['user'] ?? $_GET['age'] ?? '暂无姓名或者年龄';
print_r($userinfo);
```
---

#### 3. 太空船操作符（组合比较符）

```php
// 说明：太空船操作符用于比较两个表达式。当$a小于、等于或大于$b时它分别返回-1、0或1。 
// 范式：$a <=> $b

// 相等：两个值相等返回 0
echo 1 <=> 1;
// 不等：a 小于 b 返回 -1
echo 1 <=> 2;
// 不等：a 大于 b 返回 1
echo 1 <=> -1;'
```
---

#### 4. 通过 define() 定义常量数组

```php
// 说明：在 PHP5.6 中仅能通过 const 定义。
define('ANIMALS',['dogs','cat','bird']);
// 输出调用
echo ANIMALS[0];
```
---

#### 5. 组使用声明

```php
// 说明：从同一 namespace 导入的类、函数和常量现在可以通过单个 use 语句 一次性导入了。

// PHP 7 之前的代码
use some\namespace\ClassA;
use some\namespace\ClassB;
use some\namespace\ClassC as C;

use function some\namespace\fn_a;
use function some\namespace\fn_b;
use function some\namespace\fn_c;

use const some\namespace\ConstA;
use const some\namespace\ConstB;
use const some\namespace\ConstC;

// PHP 7+ 及更高版本的代码
use some\namespace\{ClassA, ClassB, ClassC as C};
use function some\namespace\{fn_a, fn_b, fn_c};
use const some\namespace\{ConstA, ConstB, ConstC};
```

---

#### 6. 整数除法函数 intdiv()

```php
// 说明：新加的函数 intdiv() 用来进行 整数的除法运算。
var_dump(intdiv(10,3)); // 返回 int(3)
```

---

#### 7. 会话选项



```php
// session_start() 可以接受一个 array 作为参数， 用来覆盖 php.ini 文件中设置的 会话配置选项。
// 在调用 session_start() 的时候， 传入的选项参数中也支持 session.lazy_write 行为， 默认情况下这个配置项是打开的。它的作用是控制 PHP 只有在会话中的数据发生变化的时候才 写入会话存储文件，如果会话中的数据没有发生改变，那么 PHP 会在读取完会话数据之后， 立即关闭会话存储文件，不做任何修改，可以通过设置 read_and_close 来实现。
// 例如，下列代码设置 session.cache_limiter 为 private，并且在读取完毕会话数据之后马上关闭会话存储文件。
session_start([
    'cache_limiter' => 'private',
    'read_and_close' => true,
]);
```
---

#### 8. 可为空（Nullable）类型

```php
// 参数以及返回值的类型现在可以通过在类型前加上一个问号使之允许为空。 
// 当启用这个特性时，传入的参数或者函数返回的结果要么是给定的类型，要么是 null 。
function getuser():?string {
   // 可正常返回的
   // return '返回字符串string';
   return null;

   // 不可正常返回的
   // 报错实例：
   // Fatal error: Uncaught TypeError: Return value of getuser() must be of the type string or null,
   // return ["name"=>"inoro"];
}

var_dump(getuser());
```

---

#### 9. Void 函数

```php
// 说明：一个新的返回值类型void被引入。 返回值声明为 void 类型的方法要么干脆省去 return 语句，要么使用一个空的 return 语句。 对于 void 函数来说，NULL 不是一个合法的返回值。
function swap(&$left, &$right) : void
{
    if ($left === $right) {
        return;
    }

    $tmp = $left;
    $left = $right;
    $right = $tmp;
}

$a = 1;
$b = 2;
var_dump(swap($a, $b), $a, $b);
```
---

#### 10. 类常量可见性

```php
// 说明：现在起支持设置类常量的可见性。
class ConstDemo
{
    const PUBLIC_CONST_A = 1;
    public const PUBLIC_CONST_B = 2;
    protected const PROTECTED_CONST = 3;
    private const PRIVATE_CONST = 4;
}
```
---

#### 11. 多异常捕获处理

```php
// 说明：一个catch语句块现在可以通过管道字符(|)来实现多个异常的捕获。 这对于需要同时处理来自不同类的不同异常时很有用。

try {
   // some code
} catch (FirstException | SecondException $e) {
   // handle first and second exceptions
}
```

---

#### 12. list 语言机构（非新特性）

```php
// 说明：list — 把数组中的值赋给一组变量
$info = array('coffee','brown','caffeine');

// 列出所有变量
list($one,$two,$three) = $info;
// 字符串在单引号下支持多行但是不支持解析变量 解析变量可使用双引号
echo "1是 $one,2是$two,3是$three";
```

---

#### 12.1. list()现在支持键名

```php
// 说明：现在list()和它的新的[]语法支持在它内部去指定键名。这意味着它可以将任意类型的数组 都赋值给一些变量（与短数组语法类似）
$data = [
   ['id' => 1,'name'=>'inoro'],
   ['id' => 2,'name'=>'liming'],
];

// list() style
list("id" => $id1, "name" => $name1) = $data[0];
var_dump($name1);

// [] style
["id" => $id2, "name" => $name2] = $data[1];
```

---

#### 14. 支持为负的字符串偏移量 从字符串结尾开始的偏移量
```php
// 说明：现在所有支持偏移量的字符串操作函数 都支持接受负数作为偏移量，包括通过[]或{}操作字符串下标。在这种情况下，一个负数的偏移量会被理解为一个从字符串结尾开始的偏移量。
var_dump("abcdef"[-2]);
var_dump("abcdef"[-4]);
var_dump(strpos("aabbcc", "b", -3));
```

---

#### 15. 允许重写抽象方法(Abstract method)
```php
// 说明：当一个抽象类继承于另外一个抽象类的时候，继承后的抽象类可以重写被继承的抽象类的抽象方法。

abstract class A
{
    abstract function test(string $s);
}

abstract class B extends A
{

   // 重写抽象类方法
   // overridden - still maintaining contravariance for parameters and covariance for return
   abstract function test($s) : int;
}
```
---

#### 16. 箭头功能

```php
// 说明：箭头函数提供了用于使用隐式按值作用域绑定定义函数的简写语法。
$factor = 10;
// 注：array_map — 是数组的每个元素应用回调函数
// 格式为：array_map ( callable $callback , array $array1 [, array $... ] ) : array
$num = array_map(fn($n) => $n * $factor, [1,2,3,4,5,6]);
var_dump($num);
```

---

#### 17. 在数组内部解包
```php
$parts = ['apple', 'pear'];
$fruits = ['banana', 'orange', ...$parts, 'watermelon'];
```

---


#### 18. 在数组内部解包
```php
// 说明：数字文字可以在数字之间包含下划线。
echo 6.674_083e-11."\n"; // float
echo 299_792_458;   // decimal
echo 0xCAFE_F00D."\n";   // hexadecimal
echo 0b0101_1111;   // binary
```

---
@INORO
