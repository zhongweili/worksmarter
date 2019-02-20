# 填坑
没有什么学习是一片坦途的，如果有，那就不值得学习了。

都说PHP容易，上手就用，可正因为此，很多知识我都一知半解或者索性跳过不解。

从新学起，一个一个坑慢慢填，

## PHP魔术方法是什么
http://php.net/manual/zh/language.oop5.magic.php

## Thrift是什么东西

## Nginx重定向

## PHP命名空间


***
为什么要用PHP框架？

使用框架可以省去很多重复性工作，更容易上手开发，因为框架已经给解决了许多通用的问题，例如：日志处理、类加载机制、路由规则、数据库驱动、常用类库……

为什么有那么多PHP框架？

现在的PHP开发基本上必用框架，框架有「轻重之分」，有的框架偏重，规则明确、源码复杂、实现的功能全面，适合开发大型项目，有的框架则偏轻量，只实现最重要的功能，适合开发灵活的小型项目。

***
接下来看一看phputils是怎样实现这些基础功能的。

### 日志处理
入口文件 index.php 中定义了一个全局变量 global $logConfig:
```
$logConfig = array(
    'intLevel' => 0xff,
    'strLogFile' => APPPATH . '/log/didi.log',
    'intMaxFileSize' => 0, //0为不限制
);
```
用于声明日志的级别、路径、文件大小。详细的日志规则在 /src/helper/logger.php 中定义了，例如 0xff 就是 LOG_LEVEL_ALL。

### 类加载机制
PHP中有 require 和include 可以导入其他文件，但这样一个个加载太麻烦（相关联的类库文件太多，且在不断更新），于是PHP引入了__autoload($class_name)方法，可以实现自动加载其他类，例如这样：
```
$db = new DB();
function __autoload($className)
{
    require $className . '.php';
}
```
系统找不到DB类，就会自动调用__autoload()方法，从而引入DB类所在文件。但autoload并不够灵活，还有更强大的 sql_autoload_resister() 函数可以使用：
```
sql_autoload_resister('load_function'); //函数名
sql_autoload_resister(array('load_object', 'load_function')); //类和静态方法
sql_autoload_resister('load_object::load_function'); //类和方法的静态调用
```

好了，下面来看phputils中怎样自动加载。

先引入 autoloader.php，在里面用sql_autoload_register的方法加载ClassLoader，
```
spl_autoload_register(array('\Xiaoju\Beatles\Framework\Autoload\Autoloader', 'loadClassLoader'), true, true);
        self::$loader = $loader = new \Xiaoju\Beatles\Framework\Autoload\ClassLoader();
        spl_autoload_unregister(array('\Xiaoju\Beatles\Framework\Autoload\Autoloader', 'loadClassLoader'));

$loader->addPsr4('', __DIR__);
      
$loader->register(true);
```
        
ClassLoader里面实现addPsr和register的方法，完成路径转换和自动加载队列中加载的工作。
```
$loader->addpsr4('Xiaoju\Beatles\Mis\\', APPPATH);
$loader->addPsr4('Xiaoju\Beatles\Utils\\', FRAMEPATH . 'helper');
$loader->addPsr4('Xiaoju\Beatles\Utils\\', FRAMEPATH . 'libraries');
$loader->addPsr4('Xiaoju\Beatles\Phpsdk\\', SDKPATH);

public function register($prepend = false)
{
        spl_autoload_register(array($this, 'loadClass'), true, $prepend);
}
```
这样一来，可以用引用路径的方式将需要的类都自动加载，路径前缀为命名空间，与相应文件也建立了对应关系。

### 路由规则

绝大多数框架的路由规则如下：
将 URIPATH为 /operator/object/operation 的请求路由到 /controller/operator/object/operation.php的class operation的index方法

phputils的路由规则也不例外，实现方法在router.php中，
```
public function setRoute()
{
    $segments = $this->getSegments();
    if (empty($segments)) {
        $this->returnErrorPage();
        throw new \BadMethodCallException('wrong route path', -1);
    }
    foreach ($segments as $key => $value) {
        $segments[$key] = ucwords($value);
    }

    $className = sprintf('%s\\Controller\\%s',
        $GLOBALS['appNameSpace'],
        implode('\\', $segments));
    if (class_exists($className) && method_exists($className, self::FUNCTIONNAME)) {
        $method = new \ReflectionMethod($className, self::FUNCTIONNAME);
        if ($method->isPublic() && $method->getName() === self::FUNCTIONNAME) {
            $this->controller = new $className;
            return true;
        }
    }
    if (is_null($this->controller)) {
        $this->returnErrorPage();
        throw new \BadMethodCallException('wrong route path', -1);
    }
}
```
分片段配以正则进行解析。

### 数据库驱动

Web开发离不开数据库，在PHP中一般是用PDO（PHP Data Object）简化对数据库的访问和操作，因为PDO扩展定义了一个轻量级的一致接口，对于任一种数据库的操作都有相关的函数可以方便地进行数据查询和获取。

在使用时，从配置文件读出数据库配置，直接初始化一个PDO对象，即可进行数据操作。例如：
```
try {
    $dbh = new PDO($dsn, $user, $pass); //初始化一个PDO对象
    echo "连接成功<br/>";

    foreach ($dbh->query('SELECT * from FOO') as $row) {
        print_r($row); //你可以用 echo($GLOBAL); 来看到这些值
    }
    $dbh = null;
} catch (PDOException $e) {
    die ("Error!: " . $e->getMessage() . "<br/>");
}
```

在phputils中更进一步，还使用了基于PDO的高效轻量级PHP数据库框架Medoo？

### 其他类库

## 空
isset empty null

## bcdiv

