1、编写go示例
```go
package main
 
import (
	"log"
	"net/http"
)
 
func main() {
 
	http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
		log.Println("log")
		writer.Write([]byte("hello world"))
 
	})
 
	http.ListenAndServe(":1234", nil)
}
```

2、编译
```bash
# 示例代码上传到nginx指定目录/usr/local/nginx/html/test_go
go build main.go
```

3、运行
```bash
# 如果生成的main可执行文件没有权限，请执行chmod 755 main，添加权限
/usr/local/nginx/html/test_go/main
 
# 通过ps aux|grep main，查看进程是否存在
```

4、访问ip
```bash
# 使用curl访问，日志直接输出到main所在的窗口
curl http://127.0.0.1:1234
 
# 如果不想main在后台运行 并且 日志输出到文件
# /usr/local/nginx/html/test_go/main &>> log.txt &
```

5、使用nginx做反向代理

```bash
server
{
	listen 8080;
	index index.html;
	root /usr/local/nginx/html/test_go;
 
	location /  {
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://localhost:1234;
 
	}
 
	#省略其它
}
```

使用curl访问8080端口，输出和访问1234端口一样的内容

参考：  
[go web部署，后台运行go项目，go网站利用nginx代理外网访问-杂草猿工记-个人博客-韦炳生博客-技术分享](https://blog.alipay168.cn/index/detail/item/734.html)  
[liunx中“ >” 与“ &>”,"&>>"的区别_yhc166188的博客-CSDN博客_&>>](https://blog.csdn.net/yhc166188/article/details/85606684)  
