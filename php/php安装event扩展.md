环境：系统：centos7，php版本php7.3

libevent安装包下载地址：https://libevent.org/

event安装包下载地址：https://pecl.php.net/package/event

libevent安装 ：
```bash
# 安装libevent
# 当前目录是在root目录
wget https://github.com/libevent/libevent/releases/download/release-2.1.12-stable/libevent-2.1.12-stable.tar.gz
 
# 解压文件
tar -xf libevent-2.1.12-stable.tar.gz
 
# 进入目录
cd libevent-2.1.12-stable
 
# 指定安装目录
./configure --prefix=/usr/local/libevent
 
 
# 安装
make && make install
```

event安装： 
```bash
# 当前所在目录是root目录
# 下载event
wget https://pecl.php.net/get/event-2.5.6.tgz
 
# 解压文件
tar -xf event-2.5.6.tgz
 
# 进入目录
cd event-2.5.6
 
# 执行phpize
/usr/local/php/bin/phpize
 
./configure --with-php-config=/usr/local/php/bin/php-config
 
# 安装
make && make install
 
```

最后在php.ini文件中添加以下代码：
```bash
extension_dir=/usr/local/php/lib/php/extensions/no-debug-zts-20180731
extension=event.so
```

使用php -m|grep event，查看是否安装成功。
