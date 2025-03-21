示例：计算192.168.1.100/28的IP范围
```php
$ip = '192.168.1.100';
$mask = 28;
 
$long = ip2long($ip);
$bit = 32- $mask;
 
 
// 计算开始IP地址
$long2 =  0xffffffff << $bit;
$val = long2ip(sprintf('%u',($long & $long2)));
var_dump($val);
 
// 计算结束IP地址
$long3 =  0xffffffff >> $mask;
$val = long2ip(sprintf('%u',($long | $long3)));
var_dump($val);
```

示例：计算192.168.1.100/255.255.255.240的IP范围
```php
$ip = '192.168.1.100';
$long = ip2long($ip);
 
$mask = '255.255.255.240';
$long2 = ip2long($mask);
 
// 计算开始IP地址
$val = long2ip(sprintf('%u',($long & $long2)));
var_dump($val);
 
// 计算结束IP地址
$long3 =  0xffffffff ^ $long2;
$val = long2ip(sprintf('%u',($long | $long3)));
var_dump($val);
```

最后根据情况来划分：网络地址、第一个可用IP地址、最后一个可用IP地址、广播地址
