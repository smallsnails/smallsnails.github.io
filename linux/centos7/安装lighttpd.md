1、安装编译工具
```bash
yum -y install gcc gcc-c++ gamin gamin-devel
```

2、安装lighttpd依赖
```bash
yum -y install glib2-devel openssl-devel pcre-devel bzip2-devel
```

3、安装lighttpd（官网下载http://www.lighttpd.net/）
```bash
1、下载
wget https://download.lighttpd.net/lighttpd/releases-1.4.x/lighttpd-1.4.55.tar.gz
 
2、解压
tar -xzf lighttpd-1.4.55.tar.gz
 
3、进入lighttpd目录
cd lighttpd-1.4.55
 
4、查看配置帮助信息
./configure --help
 
5、安装
./configure --prefix=/usr/local/lighttpd
make && make install
 
6、复制文件
cp -r doc/config/ /usr/local/lighttpd/
 
```

4、查看lighttpd配置信息
```bash
shell命令：
cat lighttpd.conf |grep -v '#' | grep -v '^$'
 
lighttpd.conf的部分配置信息：
var.log_root    = "/var/log/lighttpd"
var.server_root = "/srv/www"
var.state_dir   = "/var/run"
var.home_dir    = "/var/lib/lighttpd"
var.conf_dir    = "/etc/lighttpd"
var.vhosts_dir  = server_root + "/vhosts"
var.cache_dir   = "/var/cache/lighttpd"
var.socket_dir  = home_dir + "/sockets"
include "modules.conf"
server.port = 80
server.use-ipv6 = "enable"
server.username  = "lighttpd"
server.groupname = "lighttpd"
server.document-root = server_root + "/htdocs"
server.pid-file = state_dir + "/lighttpd.pid"
server.errorlog             = log_root + "/error.log"
include "conf.d/access_log.conf"
include "conf.d/debug.conf"
server.event-handler = "linux-sysepoll"
server.network-backend = "sendfile"
server.max-fds = 2048
server.stat-cache-engine = "simple"
server.max-connections = 1024
index-file.names += (
  "index.xhtml", "index.html", "index.htm", "default.htm", "index.php"
)
url.access-deny             = ( "~", ".inc" )
$HTTP["url"] =~ "\.pdf$" {
  server.range-requests = "disable"
}
static-file.exclude-extensions = ( ".php", ".pl", ".fcgi", ".scgi" )
include "conf.d/mime.conf"
include "conf.d/dirlisting.conf"
server.follow-symlink = "enable"
server.upload-dirs = ( "/var/tmp" )
```

5、配置lighttpd
```bash
mkdir /etc/lighttpd
cp lighttpd.conf /etc/lighttpd/lighttpd.conf
cp modules.conf /etc/lighttpd
cp -r conf.d/ /etc/lighttpd
 
vi /etc/lighttpd/lighttpd.conf
修改server.bind的信息
server.bind = "0.0.0.0"
 
mkdir -p  /srv/www/htdocs
往htdocs中添加测试文件
echo "hello world" > index.html
 
 
【说明】
其中：
var.log_root    = "/var/log/lighttpd"
var.server_root = "/srv/www"
var.state_dir   = "/var/run"
var.home_dir    = "/var/lib/lighttpd"
var.conf_dir    = "/etc/lighttpd"
你可以对这些参数进行修改，设置指定目录，复制相关配置文件和创建目录...
 
官方文档说明（详见https://redmine.lighttpd.net/projects/lighttpd/wiki/GetLighttpd），
还需要做以下配置，
mkdir /var/log/lighttpd
touch /var/log/lighttpd/lighttpd.error.log
touch /var/log/lighttpd/lighttpd.access.log
chown www:www /var/log/lighttpd
chown www:www /var/log/lighttpd/lighttpd.error.log
chown www:www /var/log/lighttpd/lighttpd.access.log
根据情况，做相应修改吧。
```

6、创建软连接
```bash
ln -s /usr/local/lighttpd/sbin/lighttpd /usr/sbin/lighttpd
```

7、为lighttpd创建用户组
```bash
groupadd lighttpd
adduser -m -g lighttpd -d /srv/www -s /sbin/nologin lighttpd
 
 
以下是adduser命令说明：
adduser --help
-m, --create-home             create the user's home directory
-g, --gid GROUP               name or ID of the primary group of the new account
-d, --home-dir HOME_DIR       home directory of the new account
-s, --shell SHELL             login shell of the new account
```

8、为lighttpd创建文件/设置权限
```bash
mkdir /var/log/lighttpd
chown lighttpd:lighttpd /var/log/lighttpd
```

9、启动服务
```bash
lighttpd -f /etc/lighttpd/lighttpd.conf
或者
/usr/sbin/lighttpd -D -f /etc/lighttpd/lighttpd.conf
```

10、编写shell脚本，方便启动/停止lighttpd服务
```bash
# 编写lighttpd.sh脚本，内容如下（如需使用，请做相应修改）：
 
touch lighttpd.sh
chmod 755 lighttpd.sh
```

```bash 
#!/bin/bash
cmd=$1
 
start(){
 echo "start lighttpd ..."
 pid=`ps -ef|grep -v grep|grep -v 'lighttpd.sh'|grep /usr/sbin/lighttpd|sed -n '1P'|awk '{print $2}'`
 if [ -z $pid ] ;then
  /usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf
 else
  echo "lighttpd is running!"
 fi
}
 
stop(){
 echo "killing lighttpd..."
 pid=`ps -ef|grep -v grep|grep -v 'lighttpd.sh'|grep /usr/sbin/lighttpd|sed -n '1P'|awk '{print $2}'`
 if [ -z $pid ] ;then
  echo "lighttpd is killing"
 else
  killall lighttpd
 fi
}
 
restart(){
 stop
 start
}
 
status(){
 pid=`ps -ef|grep -v grep|grep -v 'lighttpd.sh'|grep /usr/sbin/lighttpd|sed -n '1P'|awk '{print $2}'`
 if [ -z $pid ] ;then
  echo "lighttpd is not running"
 else
  echo "lihgttpd is running"
 fi
}
 
help(){
 echo "use:$0 {start|stop|restart|status}"
}
 
case "$cmd" in 
 [Ss][Tt][Aa][Rr][Tt])
  start;;
 [Ss][Tt][Oo][Pp])
  stop;;
 [Rr][Ee][Ss][Tt][Aa][Rr][Tt])
  restart;;
 [Ss][Tt][Aa][Tt][Uu][Ss])
  status;;
 *)
  help;;
esac
```

`ps -ef|grep -v grep|grep -v 'lighttpd.sh'|grep /usr/sbin/lighttpd|sed -n '1P'|awk '{print $2}'`

对上述这条命令进行说明：
```bash
①ps -ef：用标准的格式显示进程；ps aux：是用BSD的格式来显示

②grep -v grep ：为了去除包含grep的进程行 ，避免影响最终数据的正确性 。

③sed -n '1P'：显示第一行

④awk '{pring $2}'：打印第二个字段

启动lighttpd服务：./lighttpd.sh start

停止lighttpd服务：./lighttpd.sh stop
```

11、sed命令说明（主要是以 “行” 为单位，对数据行进行替换、删除、新增、选取等处理）
linux的sed命令详见：  
1、https://www.cnblogs.com/tureno/articles/6677942.html  
2、https://www.cnblogs.com/dong008259/archive/2011/12/07/2279897.html  

参考：  
https://blog.csdn.net/robertzhouxh/article/details/8174096  
https://blog.csdn.net/wangguorui89/article/details/83649258  
https://blog.csdn.net/u014745198/article/details/53671817  
https://blog.csdn.net/tengzhaorong/article/details/6955748  
