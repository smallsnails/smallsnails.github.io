```bash
# 安装vsftp
yum install vsftpd -y
 
# 创建test用户，默认家目录为/home/test，并且不能通过shell进行登录
useradd -s /sbin/nologin test
 
 
# 防止出现“530 Login incorrect”问题，有两种方式进行修改
#
# 方式一：编辑/etc/pam.d/vsftpd文件
# 可以对“auth       required     pam_shells.so”这行添加“#”，进行注释
# 或者将其改为“auth       required    pam_nologin.so”
# 对下面这行添加#，进行注释
#
# 方式二：
# 编辑/etc/shells文件，末尾追加/sbin/nologin
/sbin/nologin
 
 
# 防止ftp访问其他目录，编辑/etc/vsftpd/vsftpd.conf文件
chroot_local_user=YES  ## 原本就有，取掉注释就好
allow_writeable_chroot=YES  ## 添加
 
 
# 启动
service vsftpd start
```

详见：  
[手动搭建FTP站点（CentOS 7） - 云服务器 ECS - 阿里云](https://help.aliyun.com/document_detail/92048.html)  
[如何搭建FTP服务器 - 服务器 - 亿速云](https://www.yisu.com/zixun/164857.html)  
