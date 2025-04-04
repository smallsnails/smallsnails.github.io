1、使用http.Get发送get请求
```go
package main
 
import (
	"fmt"
	"io/ioutil"
	"net/http"
)
 
func main() {
	resp, err := http.Get("https://www.baidu.com")
	if err != nil {
		panic(err)
	}
 
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		panic(err)
	}
	fmt.Println(string(body))
}
```

2、使用http.Post发送post请求
```go
package main
 
import (
	"fmt"
	"io/ioutil"
	"net/http"
	"strings"
)
 
func main() {
	resp, err := http.Post("https://www.baidu.com", "application/x-www-form-urlencoded", strings.NewReader("wd=csdn"))
	if err != nil {
		panic(err)
	}
 
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		panic(err)
	}
	fmt.Println(string(body))
}
```

3、使用http.PostForm发送post请求
```go
package main
 
import (
	"fmt"
	"io/ioutil"
	"net/http"
	"net/url"
	"strings"
)
 
func main() {
	resp, err := http.PostForm("https://www.baidu.com", url.Values{"wd": {"csdn"},})
	if err != nil {
		panic(err)
	}
 
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		panic(err)
	}
 
	fmt.Println(string(body))
}
```

4、复杂请求可以使用http.Client的Do方法(比如需要设置header、cookie等)
```go
package main
 
import (
	"fmt"
	"io/ioutil"
	"net/http"
)
 
func main() {
	// 通过get请求https://www.baidu.com/s?wd=csdn
	req, err := http.NewRequest(http.MethodGet, "https://www.baidu.com/s", nil)
	if err != nil {
		panic(err)
	}
 
	// 设置header
	req.Header.Set("User-Agent","Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36")
 
	// 请求参数
	q := req.URL.Query()
	q.Add("wd", "csdn")
	req.URL.RawQuery = q.Encode()
 
	c := http.Client{}
	resp, err := c.Do(req)
	if err != nil {
		panic(err)
	}
 
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		panic(err)
	}
	fmt.Println(string(body))
}
```

参考：https://studygolang.com/articles/2355
