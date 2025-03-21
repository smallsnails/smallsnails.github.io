## 1、安装依赖

```bash
# centos7系统安装依赖包
yum install -y gcc gcc-c++ clang libgcc libstdc++
yum install -y wget ncurses-devel geoip-devel libmaxminddb-devel openssl-devel
```

## 2、安装GoAccess

```bash
# 官方下载页面：https://goaccess.io/download
# 下载
wget https://tar.goaccess.io/goaccess-1.9.3.tar.gz
 
# 解压
tar -xzvf goaccess-1.9.3.tar.gz
 
# 切换目录
cd goaccess-1.9.3/
 
# 配置
./configure --enable-utf8 --enable-geoip=mmdb
 
# 编译安装
make
make install
```

## 3、 指定nginx日志输出html文件

```bash
goaccess access.log -o report.html --log-format=COMBINED
```

参考：

[GoAccess - Get Started](https://goaccess.io/get-started)

[https://zhuanlan.zhihu.com/p/672768030](https://zhuanlan.zhihu.com/p/672768030)

[nginx日志分析，实时可视化工具goaccess_nginx日志分析工具-CSDN博客](https://blog.csdn.net/ly1358152944/article/details/131699608)
