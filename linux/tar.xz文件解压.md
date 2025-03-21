1、压缩命令:xz -z  文件

      如果要保留被压缩的文件加上参数 -k ，如果要设置压缩率加入参数 -0 到 -9调节压缩率。

      如果不设置，默认压缩等级是6.

2、解压命令:xz -d 文件

      同样使用 -k 参数来保留被解压缩的文件。

3、创建或解压tar.xz文件

     【创建tar.xz文件步骤如下】

       ① tar cvf xxx.tar xxx/

       ② xz -z xxx.tar

        说明：①是先创建xxx.tar文件；②是将 xxx.tar文件压缩成xxx.tar.xz文件。

    【解压tar.xz文件步骤如下】

       ① xz -d xxx.tar.xz

       ② tar xvf xxx.tar

        说明：①是将 xxx.tar.xz文件解压成 xxx.tar文件；②是解压xxx.tar文件。

详见原文：https://www.cnblogs.com/wenxingxu/p/9603654.html
