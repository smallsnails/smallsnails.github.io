> PHP官方手册警告：数组最后一个元素的 $value 引用在 foreach 循环之后仍会保留。建议使用 unset() 来将其销毁。

```php
// 示例代码
$data = [1,2,3];
foreach($data as &$d){}
var_dump($data);
//循环完后，最后一个元素$d = &$data[2],指向的是同一个地址
 
 
foreach($data as $d){}
var_dump($data);
 
// 执行结果
array(3) {
  [0]=>int(1)
  [1]=>int(2)
  [2]=>&int(3)
}
array(3) {
  [0]=>int(1)
  [1]=>int(2)
  [2]=>&int(2)
}
```

是不是很奇怪，执行的结果不是1，2，3？

来看看官方给的示例：

```php
// 警告：
// 数组最后一个元素的 $value 引用在 foreach 循环之后仍会保留。
// 建议使用 unset() 来将其销毁。 否则你会遇到下面的情况：
 
$arr = array(1, 2, 3, 4);
foreach ($arr as &$value) {
    $value = $value * 2;
}
// 现在 $arr 是 array(2, 4, 6, 8)
 
// 未使用 unset($value) 时，$value 仍然引用到最后一项 $arr[3]
 
foreach ($arr as $key => $value) {
    // $arr[3] 会被 $arr 的每一项值更新掉…
    echo "{$key} => {$value} ";
    print_r($arr);
}
// ...until ultimately the second-to-last value is copied onto the last value
 
// output:
// 0 => 2 Array ( [0] => 2, [1] => 4, [2] => 6, [3] => 2 )
// 1 => 4 Array ( [0] => 2, [1] => 4, [2] => 6, [3] => 4 )
// 2 => 6 Array ( [0] => 2, [1] => 4, [2] => 6, [3] => 6 )
// 3 => 6 Array ( [0] => 2, [1] => 4, [2] => 6, [3] => 6 )
```

参考：  
https://www.php.net/manual/zh/control-structures.foreach.php  
https://blog.csdn.net/echojson/article/details/106113967  
https://www.laruence.com/2008/11/20/630.html  
https://www.cnblogs.com/catcrazy/p/6369629.html  
