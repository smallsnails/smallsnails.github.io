1、系统环境变量

对所有用户有效，如：PATH、PATH、HOME、SHELL、SHELL、PWD等等。

查看PATH信息，命令为：echo $PATH，即变量前面加$符号

2、shell脚本

①脚本名称：xx.sh

②脚本第一行内容为：#!/bin/bash，表示使用shell解释器

③注释符号# 放在需注释内容的前面

④脚本权限，通过ll xx.sh查看脚本权限，如果没有可执行权限，chmod +x xx.sh添加脚本可执行权限

3、shell脚本执行

①bash xx.sh或./xx.sh或sh xx.sh或bash xx.sh

说明：会新开一个bash，不同bash中的变量无法共享。

②source xx.sh或. xx.sh

说明：在同一个shell里面执行的。

4、用户自定义变量

①列出所有变量：set

②删除变量：unset 变量名

③提升为系统变量：export 变量名=变量值 或者 export 声明的变量

④位置参数变量：

$n：n为数字，0代表命令本身，1之后表示参数，如$1、$9、${10}

$*：将命令中参数当成一个整体输出

$@：输出每个参数

$#：参数的个数

$?： 上个命令的退出状态，或函数的返回值

5、read命令
```bash
来源：https://www.runoob.com/linux/linux-comm-read.html
read命令用于从标准输入读取数值。
read 内部命令被用来从标准输入读取单行数据。这个命令可以用来读取键盘输入，
当使用重定向的时候，可以读取文件中的一行数据。
 
语法：
read [-ers] [-a aname] [-d delim] [-i text] [-n nchars] [-N nchars] [-p prompt] [-t timeout] [-u fd] [name ...]
 
参数说明:
-a 后跟一个变量，该变量会被认为是个数组，然后给其赋值，默认是以空格为分割符。
-d 后面跟一个标志符，其实只有其后的第一个字符有用，作为结束的标志。
-p 后面跟提示信息，即在输入前打印提示信息。
-e 在输入的时候可以使用命令补全功能。
-n 后跟一个数字，定义输入文本的长度，很实用。
-r 屏蔽\，如果没有该选项，则\作为一个转义字符，有的话 \就是个正常的字符了。
-s 安静模式，在输入字符时不再屏幕上显示，例如login时输入密码。
-t 后面跟秒数，定义输入字符的等待时间。
-u 后面跟fd，从文件描述符中读入，该文件描述符可以是exec新开启的。
```

6、sed命令
```bash
来源：https://www.cnblogs.com/dong008259/archive/2011/12/07/2279897.html
     https://www.cnblogs.com/tureno/articles/6677942.html
 
sed是一个很好的文件处理工具，本身是一个管道命令，主要是以行为单位进行处理，
可以将数据行进行替换、删除、新增、选取等特定工作，下面先了解一下sed的用法
sed命令行格式为：
sed [-nefri] ‘command’ 输入文本        
 
常用选项：
-n ∶使用安静(silent)模式。在一般 sed 的用法中，所有来自 STDIN的资料一般都会被列出到萤幕上。
    但如果加上 -n 参数后，则只有经过sed 特殊处理的那一行(或者动作)才会被列出来。
-e ∶直接在指令列模式上进行 sed 的动作编辑；
-f ∶直接将 sed 的动作写在一个档案内， -f filename 则可以执行 filename 内的sed 动作；
-r ∶sed 的动作支援的是延伸型正规表示法的语法。(预设是基础正规表示法语法)
-i ∶直接修改读取的档案内容，而不是由萤幕输出。       
 
常用命令：
a ∶新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
c ∶取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
d ∶删除，因为是删除啊，所以 d 后面通常不接任何咚咚；
i ∶插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
p ∶列印，亦即将某个选择的资料印出。通常 p 会与参数 sed -n 一起运作～
s ∶取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！
```

7、判断参数

1）文件测试语句

【参数说明】

-d：测试文件是否为目录类型

-e：测试文件是否存在

 -f：判断是否为一般文件

 -r：测试当前用户是否有权限读取

-w：测试当前用户是否有权限写入

-x：测试当前用户是否有权限执行

【示例】

[ -d /etc/fstab ]

[ -f /etc/fstab ]

2）逻辑测试语句:&& || !

【示例】

[ $USER != root ] && echo "user" || echo "root"

3）整数值语句

【参数说明】

-eq:是否等于

-ne:是否不等于

 -gt:是否大于

-lt:是否小于

-le:是否等于或小于

-ge:是否大于或等于

【示例】

[ 10 -gt 10 ]

4）字符串比较语句

= :比较字符串内容是否相同

!=:比较字符串内容是否不同

-z:判断字符串内容是否为空

【示例】

[ $LANG != "en.US" ] && echo "not en.US"

8、循环控制语句
```bash
// 提醒：[]符号记得空格
 
// if格式
if [ 条件 ] ;then
  // 业务处理
 elif [ 条件 ] ;then
  // 业务处理
 else
  // 业务处理
fi
 
 
// for格式
for 变量名 in 取值列表
do
 // 业务处理
done
 
 
// while格式
while 条件
do
 // 业务处理
done
 
 
// case格式
case 变量值 in
模式1)
 // 业务处理
 ;;
模式2)
 // 业务处理
 ;;
模式n)
 // 业务处理
 ;;
*)
 // 业务处理
esac
```

9、shell脚本分析（以下是nginx的shell脚本）
```bash
#!/bin/bash
# nginx - this script starts and stops the nginx daemon
# chkconfig: - 85 15
# description: Nginx is an HTTP(S) server, HTTP(S) reverse \
# proxy and IMAP/POP3 proxy server
# processname: nginx
# config: /etc/nginx/nginx.conf
# config: /usr/local/nginx/conf/nginx.conf
# pidfile: /usr/local/nginx/logs/nginx.pid
# Source function library.
. /etc/rc.d/init.d/functions
# Source networking configuration.
. /etc/sysconfig/network
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
nginx="/usr/local/nginx/sbin/nginx"
prog=$(basename $nginx)
NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
lockfile=/var/lock/subsys/nginx
 
make_dirs() {
# make required directories
user=`$nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
        if [ -z "`grep $user /etc/passwd`" ]; then
                useradd -M -s /bin/nologin $user
        fi
options=`$nginx -V 2>&1 | grep 'configure arguments:'`
for opt in $options; do
        if [ `echo $opt | grep '.*-temp-path'` ]; then
                value=`echo $opt | cut -d "=" -f 2`
                if [ ! -d "$value" ]; then
                        # echo "creating" $value
                        mkdir -p $value && chown -R $user $value
                fi
        fi
done
}
 
start() {
[ -x $nginx ] || exit 5
[ -f $NGINX_CONF_FILE ] || exit 6
make_dirs
echo -n $"Starting $prog: "
daemon $nginx -c $NGINX_CONF_FILE
retval=$?
echo
[ $retval -eq 0 ] && touch $lockfile
return $retval
}
 
stop() {
echo -n $"Stopping $prog: "
killproc $prog -QUIT
retval=$?
echo
[ $retval -eq 0 ] && rm -f $lockfile
return $retval
}
 
restart() {
#configtest || return $?
stop
sleep 1
start
}
 
reload() {
#configtest || return $?
echo -n $"Reloading $prog: "
killproc $nginx -HUP
RETVAL=$?
echo
}
 
force_reload() {
restart
}
 
configtest() {
$nginx -t -c $NGINX_CONF_FILE
}
 
rh_status() {
status $prog
}
 
rh_status_q() {
rh_status >/dev/null 2>&1
}
 
case "$1" in
start)
        rh_status_q && exit 0
        $1
        ;;
stop)
        rh_status_q || exit 0
        $1
        ;;
restart|configtest)
$1
;;
reload)
        rh_status_q || exit 7
        $1
        ;;
force-reload)
        force_reload
        ;;
status)
        rh_status
        ;;
condrestart|try-restart)
        rh_status_q || exit 0
        ;;
*)
echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
exit 2
esac
```

```bash
【脚本说明】
第11、13行：执行shell脚本
第15行：判断$NETWORKING的值，如果是no，则退出
第19行：首先判断是否是文件，如果是执行后面的脚本文件
第22~38行：make_dirs函数
|--第24行：获取用户
|--第25行：判断是否为空
|--第26行：添加不能登陆的用户 且 不要自动建立用户的登陆目录
|--第29~37行：for语句
  |--第31行：用=号作为分隔符，选取其第2个字段
  |--第32行：判断是否为文件夹
  |--第34行：批量创建文件 并且 给文件添加用户
第40~50行：start函数
|--第41行：当前用户是否有权限执行，否则，退出
|--第42行：判断是否为文件，否则，退出
|--第43行：调用make_dirs函数
|--第46行：判断上一条命令是否执行成功
|--第48行：判断是否等于0 且 创建文件
第52~59行：stop函数
第61~66行：restart函数
第68~74行：reload函数
第76~78行：force_reload函数
第80~82行：configtest函数
|--第81行：检查配置文件是否正确
第84~86行：rh_status函数
第88~90行：rh_status_q函数
|--第89行：标准输出和错误输出都进了“黑洞”
第92~120行：case语句,根据条件判断，调用不同的函数
 
参考：
https://blog.csdn.net/helloxiaozhe/article/details/80596138
https://blog.csdn.net/longgeaisisi/article/details/90519690
https://www.linuxprobe.com/chapter-20.html#2022_Nginx
https://www.cnblogs.com/irisrain/p/4324593.html
```

参考：  
https://www.cnblogs.com/zhangchao162/p/9614145.html  
https://blog.csdn.net/zhoulv2000/article/details/14160935  
https://www.linuxprobe.com/chapter-04.html#42_Shell  
https://blog.csdn.net/u010622613/article/details/80706119  
https://blog.csdn.net/xdmaidou/article/details/80670965  
https://blog.csdn.net/qq_35744460/article/details/89702566  
