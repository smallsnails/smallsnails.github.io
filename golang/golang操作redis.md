1、使用go get github.com/garyburd/redigo/redis下载redis

2、连接redis
```go
package main
 
import (
	"fmt"
	"github.com/garyburd/redigo/redis"
)
 
func main() {
	conn, err := redis.Dial("tcp", "127.0.0.1:6379")
	if err != nil {
		fmt.Println("redis.Dial err: ", err)
		return
	}
 
	fmt.Println("连接成功")
	defer conn.Close()
}
```

3、string类型的set、get操作
```go
package main
 
import (
	"fmt"
	"github.com/garyburd/redigo/redis"
)
 
func main() {
	conn, err := redis.Dial("tcp", "127.0.0.1:6379")
	if err != nil {
		fmt.Println("redis.Dial err: ", err)
		return
	}
 
	//fmt.Println("连接成功")
	defer conn.Close()
 
	// set操作
	_, err = conn.Do("set", "abc", 100)
	if err != nil {
		fmt.Println("set err: ", err)
		return
	}
 
	// get操作
	reply, err := redis.Int(conn.Do("get", "abc"))
	if err != nil {
		fmt.Println("get err: ", err)
		return
	}
 
	fmt.Println(reply) // 100
}
```

4、string类型批量操作
```go
package main
 
import (
	"fmt"
	"github.com/garyburd/redigo/redis"
)
 
func main() {
	conn, err := redis.Dial("tcp", "127.0.0.1:6379")
	if err != nil {
		fmt.Println("redis.Dial err: ", err)
		return
	}
 
	//fmt.Println("连接成功")
	defer conn.Close()
 
	// mset操作
	_, err = conn.Do("mset", "abc", 100, "efg", 300)
	if err != nil {
		fmt.Println("mset err: ", err)
		return
	}
 
	// mget操作
	reply, err := redis.Ints(conn.Do("mget", "abc", "efg"))
	if err != nil {
		fmt.Println("mget err: ", err)
		return
	}
 
	fmt.Println(reply) // [100 300]
}
```

5、设置过期时间
```go
package main
 
import (
	"fmt"
	"github.com/garyburd/redigo/redis"
	"time"
)
 
func main() {
	//main1()
	conn, err := redis.Dial("tcp", "127.0.0.1:6379")
	if err != nil {
		fmt.Println("redis.Dial err: ", err)
		return
	}
 
	//fmt.Println("连接成功")
	defer conn.Close()
 
	// expire操作
	//_, err = conn.Do("expire", "abc", 3)
	//if err != nil {
	//	fmt.Println("expire err: ", err)
	//	return
	//}
 
	time.Sleep(3 * time.Duration(time.Second))
 
	// get操作
	reply, err := redis.Int(conn.Do("get", "abc"))
	if err != nil {
		fmt.Println("get err: ", err) // 100 // get err:  redigo: nil returned
		return
	}
	fmt.Println(reply)
}
```

6、list队列操作
```go
package main
 
import (
	"fmt"
	"github.com/garyburd/redigo/redis"
)
 
func main() {
	conn, err := redis.Dial("tcp", "127.0.0.1:6379")
	if err != nil {
		fmt.Println("redis.Dial err: ", err)
		return
	}
 
	//fmt.Println("连接成功")
	defer conn.Close()
 
	// lpush操作
	//_, err = conn.Do("lpush", "book_list", "abc", "ceg", 300)
	//if err != nil {
	//	fmt.Println("lpush err: ", err)
	//	return
	//}
 
	// lpop操作
	reply, err := redis.String(conn.Do("lpop", "book_list"))
	if err != nil {
		fmt.Println("lpop err: ", err)
		return
	}
 
	fmt.Println(reply)
}
```

7、hash表
```go
package main
 
import (
	"fmt"
	"github.com/garyburd/redigo/redis"
)
 
func main() {
	conn, err := redis.Dial("tcp", "127.0.0.1:6379")
	if err != nil {
		fmt.Println("redis.Dial err: ", err)
		return
	}
 
	//fmt.Println("连接成功")
	defer conn.Close()
 
	// hset操作
	_, err = conn.Do("hset", "test", "abc", 100)
	if err != nil {
		fmt.Println("hset err: ", err)
		return
	}
 
	// hget操作
	reply, err := redis.Int(conn.Do("hget", "test", "abc"))
	if err != nil {
		fmt.Println("hget err: ", err)
		return
	}
 
	fmt.Println(reply)
}
```

8、redis连接池
```go
package main
 
import (
	"fmt"
	"github.com/garyburd/redigo/redis"
	"time"
)
 
var pool *redis.Pool
 
func init() {
	pool = &redis.Pool{ //实例化一个连接池
		MaxIdle:     50,                //最初的连接数量
		MaxActive:   30,                //连接池最大连接数量,不确定可以用0（0表示自动定义），按需分配
		IdleTimeout: 300 * time.Second, //连接关闭时间 300秒 （300秒不使用自动关闭
		Dial: func() (redis.Conn, error) {
			// 1. 打开连接
			c, err := redis.Dial("tcp", "127.0.0.1:6379")
			if err != nil {
				fmt.Println(err)
				return nil, err
			}
 
			// 2. 访问认证
			//if _, err = c.Do("AUTH", ""); err != nil {
			//	c.Close()
			//	return nil, err
			//}
			return c, nil
		},
		TestOnBorrow: func(conn redis.Conn, t time.Time) error {
			if time.Since(t) < time.Minute {
				return nil
			}
			_, err := conn.Do("PING")
			return err
		},
	}
}
 
func main() {
	conn := pool.Get()
	defer conn.Close()
 
	// set操作
	_, err := conn.Do("set", "abc", 100)
	if err != nil {
		fmt.Println("set err: ", err)
		return
	}
 
	// get操作
	reply, err := redis.Int(conn.Do("get", "abc"))
	if err != nil {
		fmt.Println("get err: ", err)
		return
	}
 
	fmt.Println(reply) // 100
	defer pool.Close()
}
```

参考：https://www.kancloud.cn/uvohp5na133/golang/934139
