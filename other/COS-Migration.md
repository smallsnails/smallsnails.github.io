## 1、安装jdk

### 1.1、进入[Java Downloads | Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)官网下载jdk(当前下载的版本为：jdk-8u411-linux-x64.tar.gz)

### 1.2、将下载的jdk-8u411-linux-x64.tar.gz文件使用ftp或者rz命令上传到服务器

### 1.3、解压文件，命令：tar -xf jdk-8u411-linux-x64.tar.gz

### 1.4、移动文件，命令：mv jdk1.8.0_411 /usr/local/java/

### 1.5、设置环境变量

在/etc/profile文件末尾追加如下内容

```bash
# set java environment
JAVA_HOME=/usr/local/java/jdk1.8.0_411        
JRE_HOME=/usr/local/java/jdk1.8.0_411/jre     
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
```

使用source /etc/profile命令使用配置生效

详见：[对象存储 Java 安装与配置-工具指南-文档中心-腾讯云](https://cloud.tencent.com/document/product/436/10865)

## 2、安装cos migrate

下载地址：[Releases · tencentyun/cos_migrate_tool_v5 · GitHub](https://github.com/tencentyun/cos_migrate_tool_v5/releases)


```bash
# 下载页面：https://github.com/tencentyun/cos_migrate_tool_v5/releases
wget https://github.com/tencentyun/cos_migrate_tool_v5/archive/refs/tags/v1.4.14.zip
 
# 解压
unzip v1.4.14.zip
 
# 进入目录
cd cos_migrate_tool_v5-1.4.14
```

## 3、配置conf/config.ini文件

需要配置secretId、secretKey、bucketName、region、cosPath以及localPath

原配置文件如下：

```bash
# 配置迁移类型
# 目前支持四大类, 这里的存储类型和之后的分节名称一致
# 1 从本地迁移, migrateLocal(本地迁移工具, 同之前的本地同步工具)
# 2 从友商迁移, migrateAws(从aws迁移), migrateAli(从阿里迁移), migrateQiniu(从七牛迁移), migrateUpyun(从又拍云迁移)
# 3 从url列表迁移, migrateUrl(这些url都是可以直接下载的，将要迁移的url放到一个文件或者多个文件里)
# 4 COS的bucket复制. migrateBucketCopy(将COS一个bucket下的数据复制到另外一个bucket, 支持跨账号跨地域，前提是账户需要对源bucket源bucket有可读权限，对目的bucket有putObjectCopy权限)
[migrateType]
type=migrateLocal
 
# 迁移工具的公共配置分节，包含了要迁移到得目的COS的账户信息 
[common]
# 用户的秘钥 secret_id (可在 https://console.qcloud.com/capi 查看)
secretId=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
# 用户的秘钥 secret_key  (可在 https://console.qcloud.com/capi 查看)
secretKey=yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
# 如果使用临时密钥访问存储桶，此处配置临时密钥的Token,该密钥需要有目的桶的PutObject权限(如果迁移类型是migrateBucketCopy，则该密钥需要有源桶的GetObject权限以及目的桶的PutObject权限)
# token=zzzzzzzzzzzzzzzzz
# 目的Bucket的名称, 命名规则为{name}-{appid}，即bucket名必须包含appid, 例如movie-1251000000
bucketName=mybucket-1251668577
# 目的bucket的region信息. COS地域的简称请参照 https://www.qcloud.com/document/product/436/6224
region=ap-guangzhou
# 存储类型, 标准(Standard), 多AZ标准(Maz_Standard), 低频(Standard_IA), 多AZ低频(Maz_Standard_IA),
# 智能分层(Intelligent_Tiering), 多AZ智能分层(Maz_Intelligent_Tiering), 归档(Archive), 深度归档(Deep_Archive)
storageClass=Standard
# 要迁移到的cos路径, /表示迁移到bucket的根路径下, /aaa/bbb/表示要迁移到bucket的/aaa/bbb/下面, 如果/aaa/bbb/不存在,则会自动建立
cosPath=/
# 是否使用HTTPS传输(传输速度较慢，适用于对传输安全要求高的场景), on开启, off关闭
https=off
# 是否使用HTTP短链接，true使用短链接, false或者不填使用默认使用长链接
# shortConnection=false
# 临时目录,用于运行过程中,临时文件的存储, 主要用于友商数据迁移到COS, 因为迁移会现将数据下载到临时目录，再进行上传后删除.对于linux绝对路径, 如/a/b/c, 对于windows绝对路径，注意分隔符为两个反斜杠，如E:\\a\\b\\c
# 默认存储在工具下的tmp目录, 请确保磁盘空间充足，取决于要迁移的文件的大小与并发度。
tmpFolder=./tmp
# 小文件阈值的字节，大于等于这个阈值使用分块上传，否则使用简单上传, 默认5MB
# 注意：最大能上传5GB的小文件
smallFileThreshold=5242880
# 小文件(文件小于smallFileThreshold)的并发度，使用简单上传，此值对于带宽充足或小文件过多时，可以适当增大调整为128或者256等。
# 同时如果从友商或者URL迁移，下载是使用的线程池并发度也由smallFileExecutorNum来决定，因此可以通过增大下载速度。
smallFileExecutorNum=64
# 大文件(文件大于等于smallFileThreshold)的并发度，使用分块上传,此值不宜过大，建议不大于32
bigFileExecutorNum=8
# 用来指定分块上传时单个分块的大小, 单位字节，默认分块大小是5MB
# 由于分块上传对单个文件块的数目有最大限制（10000块），所以对于超出5MB*10000大小的文件，需要根据具体情况调整该参数 
bigFileUploadPartSize=5242880
# 用来指定上传到 COS 时单个线程的最高带宽，单位 bit/s，默认不做限制
# 可以配合并发度控制迁移时的上传带宽，例如指定为 83886080 (83886080 = 80 * 1024 * 1024)，则限制单线程 80Mbps 上传带宽
# 注意限速范围为819200 - 838860800，即800Kbps - 800Mbps
threadTrafficLimit=
# 表示迁移工具将全文的MD5计算后，存入文件的自定义头部x-cos-meta-md5中, 用于后续的校验，因为COS的分块上传的大文件的etag不是全文的md5
# on 打开, off关闭
entireFileMd5Attached=off
# 表示是否启用damon模式，damon表示程序会循环不停的去执行同步，每一轮同步的间隔由damonModeInterVal参数设置
# 如果启用damon模式, 则设置为on, 否则为off
daemonMode=off
# 表示每一轮同步结束后，多久进行下一轮同步，单位为秒
daemonModeInterVal=60
# 表示任务执行的时间窗口, 满足部分客户要求在指定时间段内执行，比如03:30,21:00, 表示在凌晨03:30到晚上21:00之间执行任务。
# 如果当前时间不在时间窗口内，则会进入睡眠状态,暂停迁移，直到下一个时间窗口内自动再继续执行。
# 但每一个任务都是 先判断时间是否在迁移窗口，然后开始迁移，有可能判断的时候 在时间窗口，但是迁移过程中有可能跨过时间窗口, 即存在少量的迁移在时间窗口外执行。
executeTimeWindow=00:00,24:00
 
# 迁移成功的结果，按日期归档此目录，为空即不输出。格式每一行为：绝对路径\t文件大小\t最后修改时间，该目录需要存在。
outputFinishedFileFolder=./result
 
# 是否接着最后一次运行的结果，继续往下遍历源的文件列表
resume=false
 
# 如果 COS 已经有相同的文件，是否直接跳过。默认不跳过，即覆盖原有文件。
skipSamePath=false
 
# 客户端加密配置 on/off
clientEncryption=off
# 加密算法，目前支持 AES-CTR (代表 AES/CTR/NOPadding 加密)
encryptionAlgo=AES-CTR
# 存放密钥的路径，一个绝对路径
keyPath=/
# 固定 16 byte，不填的话随机生成
encryptIV=
# 迁移完成后，校验客户端加密文件的 length, on/off
check=off
 
# rocks db配置，如果持续迁移中打开的sst文件过多导致占用的内存过多，可以尝试此值，比如调成100，注意比较性能
rocksMaxOpenFile=
 
# 是否使用带超时监控的请求，on/off,默认不启用;超时时间单位是毫秒;
# requestTimeoutEnable = off
# requestTimeoutMs = 500000
 
# 请求失败后默认总的尝试次数，必须大于0
# requestTryCount = 5
 
# 从本地迁移到COS配置分节
[migrateLocal]
# 本地目录, 表示将该路径下的数据都迁移到COS, 对于linux绝对路径, 如/a/b/c, 对于windows绝对路径，注意分隔符为两个反斜杠，如E:\\a\\b\\c
localPath=E:\\code\\java\\workspace\\cos_migrate_tool\\test_data
# 要排除的目录或者文件的绝对路径, 表示将localPath下面某些目录或者文件不进行迁移，多个绝对路径之前用分号分割，不填表示localpath下面的全部迁移
excludes=
# 排除更新时间与当前时间相差不足一定时间段的文件，单位为秒
# 默认不设置, 表示不根据lastmodified时间进行筛选
# 适用于客户在更新文件的同时又在运行迁移工具, 不准备把正在更新的文件迁移上传到COS, 比如设置为300, 表示只上传更新了5分钟以上的文件
ignoreModifiedTimeLessThanSeconds=
# 多个后缀用;隔开，例如:.txt;.tmp;
ignoreSuffix=
# on：忽略空文件，off：不忽略。默认不忽略
ignoreEmptyFile=
 
# 设定要迁移的文件后缀，不符合后缀的文件会被排除
# 多个后缀用;隔开，例如:.txt;.tmp;
includeSuffix=
# on: 使用列表文件指定所有待迁移文件的相对路径; off: migration递归遍历localPath, 添加待迁移文件; 默认off
# 可使用excludes, ignoreModifiedTimeLessThanSeconds, ignoreSuffix, ignoreEmptyFile忽略列表中的文件
fileListMode=off
# 当fileListMode=on的时候，fileListPath参数为localPath下待迁移文件的相对路径列表
fileListPath=/data/config/myFileList.txt
 
# 是否检查本地db里有记录. 如果本地db里有记录, 则不再上传此对象. 建议保持默认为 true, 避免迁移时重复上传.
# 如果置为 false, 则不再检查本地db里是否有记录, 即使任务已经执行, 也会尝试重复上传, 甚至可能覆盖服务端数据, 一般不建议使用, 只用于极特殊场景.
# 上传前是否检查服务端有数据，由开关 skipSamePath 控制. 执行顺序是先判断 checkLocalRecord, 再判断 skipSamePath, 其中任何一个判断结果跳过，再跳过不执行上传
# 这个开关不会控制上传后是否记录数据库, 一定会记录的.
checkLocalRecord=true
```

详见：[对象存储 COS Migration 工具-工具指南-文档中心-腾讯云](https://cloud.tencent.com/document/product/436/15392)

## 4、执行sh start_migrate.sh命令进行文件上传

