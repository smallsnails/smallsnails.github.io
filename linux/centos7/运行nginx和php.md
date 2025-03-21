1、安装php-fpm,
```bash
yum install php-fpm
service php-fpm start    #启动 php-fpm
```
或者是在安装php的时候带上--enable-fpm，不清楚可以看看php-fpm安装以及配置  

2、修改nginx配置文件nginx.conf 
```bash
 vi /usr/local/nginx/conf/nginx.conf，
```

如下把之前的#给去掉就可以了，顺手改一下fastcgi_param
```bash
location ~ \.php$ {
    root           html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME $document_root/$fastcgi_script_name;
    include        fastcgi_params;
}
```

3、重启nginx和php-fpm。
```bash
service nginx restart
service php-fpm restart
```

4、php-fpm说明
```bash
#到php的配置目录
       cd /usr/local/php/etc

#有一个php-fpm.conf.default的文件，cp复制
       cp php-fpm.conf.default php-fpm.conf

cd /usr/local/php/etc/php-fpm.d/

cp www.conf.default www.conf

#编辑 php-fpm.conf
       #找到以下配置项， 配置如下
       pid = run/php-fpm.pid
```

参考：  
1、[nginx无法解析php文件]()  
2、[编译安装php-fpm5.6 (centos 7)]()  
