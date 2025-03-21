（注：基于上一次安装的nginx环境进行nginx升级，上次安装nginx）

以下是使用make upgrade命令来对nginx平滑升级，操作步骤及说明如下：

1、需要下载对应的需要加载的第三方的扩展，或者是需要附加设置的参数（注意：之前的配置参数要保留），--add_module=PATH添加第三方扩展

2、执行make命令（注意：不要执行make install）

3、重命名nginx旧版本二进制文件（即/usr/local/nginx/sbin/nginx）,这并不会停止服务

4、然后在源码目录中复制一份重新编译的二进制文件到安装目录（安装目录是/usr/local/nginx/sbin/）

5、在源码目录执行make upgrade开始升级

```bash
# 下载nginx
wget http://nginx.org/download/nginx-1.19.1.tar.gz
 
# 解压
tar -xf nginx-1.19.1.tar.gz
 
# 执行nginx -V查看之前配置
nginx -V
################### 信息如下 ###################
nginx version: nginx/1.14.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/usr/local/nginx --user=nginx --group=nginx --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --add-module=/usr/local/src/nginx/ngx_devel_kit-0.3.0 --add-module=/usr/local/src/nginx/lua-nginx-module-0.10.9rc7
################################################
 
# 编译（还是之前的配置，如果你添加了第三扩展，请追加--add-module=路径）
./configure --prefix=/usr/local/nginx --user=nginx --group=nginx \
--with-http_gunzip_module --with-http_gzip_static_module \
--with-http_random_index_module --with-http_realip_module \
--with-http_secure_link_module --with-http_slice_module --with-http_ssl_module \
--with-http_stub_status_module --with-http_sub_module --with-http_v2_module \
--with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module \
--with-stream_ssl_module --with-stream_ssl_preread_module \
--add-module=/usr/local/src/nginx/ngx_devel_kit-0.3.0 \
--add-module=/usr/local/src/nginx/lua-nginx-module-0.10.9rc7
 
# 重命名原来的nginx
mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.old
 
# 复制到nginx的安装命令
cp objs/nginx /usr/local/nginx/sbin/
 
# 执行make upgrade
make upgrade
 
# 报错
################### 报错信息 ###################
/usr/local/nginx/sbin/nginx -t
/usr/local/nginx/sbin/nginx: error while loading shared libraries: libluajit-5.1.so.2: cannot open shared object file: No such file or directory
make: *** [upgrade] Error 127
# 解决办法
# 查找libluajit-5.1.so.2
find / -name libluajit-5.1.so.2
# 显示/usr/local/LuaJIT/lib/libluajit-5.1.so.2
vi /etc/ld.so.conf.d/libc.conf
# 往文件/etc/ld.so.conf.d/libc.conf写入/usr/local/LuaJIT/lib
# 执行命令ldconfig
ldconfig
# 执行nginx -V命令
nginx -V
################################################
 
# 继续执行make upgrade命令
make upgrade
 
# 查看nginx信息
nginx -V
 
################### 信息如下 ###################
nginx version: nginx/1.19.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/usr/local/nginx --user=nginx --group=nginx --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --add-module=/usr/local/src/nginx/ngx_devel_kit-0.3.0 --add-module=/usr/local/src/nginx/lua-nginx-module-0.10.9rc7
################################################
```
