[图解ThinkPHP5框架(四)](https://www.php.cn/faq/362542.html)  
[ThinkPHP 框架执行流程分析](https://www.thinkphp.cn/topic/35803.html)  
[基于thinkphp5小功能设计与实现](https://www.kancloud.cn/zhiqiang/helper/247201)  

> TP5控制器大小写访问
``` 
URL大小写

默认情况下，URL是不区分大小写的，也就是说 URL里面的模块/控制器/操作名会自动转换为小写，控制器在最后调用的时候会转换为驼峰法处理。

例如：
http://localhost/index.php/Index/Blog/read//和下面的访问是等效的http://localhost/index.php/index/blog/read

如果访问下面的地址
http://localhost/index.php/Index/BlogTest/read//和下面的访问是等效的http://localhost/index.php/index/blogtest/read

在这种URL不区分大小写情况下，如果要访问驼峰法的控制器类，则需要使用：
http://localhost/index.php/Index/blog_test/read

模块名和操作名会直接转换为小写处理。
如果希望URL访问严格区分大小写，可以在应用配置文件中设置：
// 关闭URL中控制器和操作名的自动转换'url_convert'=>false,

一旦关闭自动转换，URL地址中的控制器名就变成大小写敏感了，例如前面的访问地址就要写成：
http://localhost/index.php/Index/BlogTest/read

但是下面的URL访问依然是有效的：
http://localhost/index.php/Index/blog_test/read

下面的URL访问则无效：
http://localhost/index.php/Index/blogtest/read

需要注意：路由规则中定义的路由地址是按照控制器名的实际名称定义（区分大小写）。

参考：https://www.jianshu.com/p/6b918e9a1135
```
