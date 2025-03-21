1、编译安装lighttpd（具体详见CentOS7之编译安装lighttpd）
```bash
注意：安装lighttpd之前，请安装相关编译工具和依赖
下载、解压lighttpd，并进入解压后的目录，执行下面命令
./configure --prefix=/usr/local/lighttpd --with-openssl 
make
make install
 
 
安装lighttpd之后，查看是否支持ssl
/usr/local/lighttpd/sbin/lighttpd -v
```

2、配置lighttpd.conf ，可以通过https访问网站
```bash
生成server.pem文件
penssl req -new -x509 -keyout server.pem -out server.pem -days 365 -nodes
 
 
在/etc/lighttpd/lighttpd.conf配置
$SERVER["socket"] == "0.0.0.0:443" {
    ssl.engine = "enable"
    ssl.pemfile = "/etc/lighttpd/ssl/server.pem" #server.pem文件路径
}
 
 
如果报错“please add "mod_openssl" to server.modules list in lighttpd.conf”，
请在/etc/lighttpd/modules.conf配置,增加mod_openssl
server.modules = {
    "mod_openssl"
}
```

3、配置lighttpd.conf ，可以解析php文件（注：记得开启php的openssl模块）
```bash
【注意】以下配置仅供参考，如需使用，请做相应修改
 
【方式1】（php使用的是php-fpm的工作方式）：
1）在module.conf中去掉#include "conf.d/fastcgi.conf"这一行的#号
2）然后在conf.d/fastcg.conf中配置，配置如下
fastcgi.server = (
    ".php" => ((
        "host" => "127.0.0.1",
        "port" => "9000",
        #"docroot" => "/srv/www/htdocs/"
    )))
 
 
【方式2】（php使用的是fast-cgi的工作方式）
1）在modules.conf，配置如下：
在sever.modules中增加mod_fastcgi，如下
sever.modules = {
    "mod_fastcgi"
}
如果报错，“(plugin.c.186) Cannot load plugin mod_fastcgi more than once, 
please fix your config (lighttpd may not accept such configs in future releases)”
,则不要添加。
并且去掉#include "conf.d/fastcgi.conf"这一行的#号，
 
2）然后在conf.d/fastcgi.conf中配置，配置如下
fastcgi.server = ( ".php" =>
                    ( "localhost" =>
                        "socket" => socket_dir + "/php-fastcig.socket", # 指定socket所在位置
                        "bin-path" => server_root + "/cgi-bin/php5", # php-cgi所在的路径
                        "bin-environment" => (
                            "PHP_FCGI_CHILDREN" => "16",
                            "PHP_FCGI_MAX_REQUESTS" => "10000",
                        ),
                        "max-procs" => 5,
                        "broken-scriptfilename" => "enable"
                    )
                )
```

4、配置完成，启动lighttpd、php-fpm
```bash
<?php
// 在网站根目录编写php的测试文件
// 文件名称index.php
phpinfo();
```

通过网页访问php的测试页面（https://192.168.186.157/index.php），正常显示则配置成功。


参考：  
https://blog.csdn.net/yunjingguang/article/details/39576535  
https://blog.csdn.net/tingfeng525/article/details/44459539  
