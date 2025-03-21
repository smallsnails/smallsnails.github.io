1、创建person表
```go
CREATE TABLE `person` (
  `user_id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(260) DEFAULT NULL,
  `sex` varchar(260) DEFAULT NULL,
  `email` varchar(260) DEFAULT NULL,
  PRIMARY KEY (`user_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

2、使用go get github.com/go-sql-driver/mysql来下载mysql包

3、连接mysql
```go
package main
 
import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)
 
var (
	driverName = "mysql"
	host       = "127.0.0.1"
	port       = "3306"
	username   = "root"
	password   = "root"
	charset    = "utf8"
	dbName     = "test"
)
 
func main() {
	//database, err := sql.Open("数据库类型", "用户名:密码@tcp(地址:端口)/数据库名")
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?%s", username, password, host, port, dbName, charset)
	db, err := sql.Open(driverName, dsn)
	if err != nil {
		panic(err)
	}
	if err := Db.Ping(); err != nil {
		fmt.Println("Db.Ping err: ", err)
		return
	}
	fmt.Println("连接成功")
	defer db.Close()
}
```

4、新增数据
```go
package main
 
import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)
 
var (
	driverName = "mysql"
	host       = "127.0.0.1"
	port       = "3306"
	username   = "root"
	password   = "root"
	charset    = "utf8"
	dbName     = "test"
)
 
var Db *sql.DB
 
func init() {
	//database, err := sql.Open("数据库类型", "用户名:密码@tcp(地址:端口)/数据库名")
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?%s", username, password, host, port, dbName, charset)
	database, err := sql.Open(driverName, dsn)
	if err != nil {
		fmt.Println("sql.Open err: ", err)
		return
	}
	Db = database
}
 
func main() {
	insert()
}
 
func insert() {
	sqlStr := "insert into person(username,sex,email) values(?,?,?)"
	result, err := Db.Exec(sqlStr, "stu001", "man", "stu01@qq.com")
	if err != nil {
		fmt.Println("Db.Exec err: ", err)
		return
	}
 
	insertId, err := result.LastInsertId()
	if err != nil {
		fmt.Println("result.LastInsertId err: ", err)
		return
	}
	fmt.Println("insert id: ", insertId)
	// 打印结果
    // insert id:  1
}
```

5、查询数据
```go
package main
 
import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)
 
var (
	driverName = "mysql"
	host       = "127.0.0.1"
	port       = "3306"
	username   = "root"
	password   = "root"
	charset    = "utf8"
	dbName     = "test"
)
 
var Db *sql.DB
 
func init() {
	//database, err := sql.Open("数据库类型", "用户名:密码@tcp(地址:端口)/数据库名")
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?%s", username, password, host, port, dbName, charset)
	database, err := sql.Open(driverName, dsn)
	if err != nil {
		fmt.Println("sql.Open err: ", err)
		return
	}
	Db = database
}
 
func main() {
    //findOne()
	findAll()
}
 
type Person struct {
	UserId   int
	Username string
	Sex      string
	Email    string
}
 
func findOne() {
	rows, err := Db.Query("select * from person limit 1")
	if err != nil {
		fmt.Println("Db.Query err: ", err)
		return
	}
 
	p := &Person{}
	rows.Next()
	err = rows.Scan(&p.UserId, &p.Username, &p.Sex, &p.Email)
	if err != nil {
		fmt.Println("rows.Scan err: ", err)
		return
	}
 
	fmt.Printf("%+v", p)
	// 打印结果
	// &{UserId:1 Username:stu001 Sex:man Email:stu01@qq.com}
}
 
func findAll() {
	rows, err := Db.Query("select * from person")
	if err != nil {
		fmt.Println("Db.Query err: ", err)
		return
	}
 
	data := make([]interface{}, 0)
	p := &Person{}
	for rows.Next() {
		err = rows.Scan(&p.UserId, &p.Username, &p.Sex, &p.Email)
		if err != nil {
			fmt.Println("rows.Scan err: ", err)
			return
		}
		data = append(data, p)
	}
 
	for _, item := range data {
		fmt.Printf("%+v\n", item)
		// 打印结果
		//&{UserId:2 Username:stu001 Sex:man Email:stu01@qq.com}
		//&{UserId:2 Username:stu001 Sex:man Email:stu01@qq.com}
	}
}
```

6、更新数据
```go
package main
 
import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)
 
var (
	driverName = "mysql"
	host       = "127.0.0.1"
	port       = "3306"
	username   = "root"
	password   = "root"
	charset    = "utf8"
	dbName     = "test"
)
 
var Db *sql.DB
 
func init() {
	//database, err := sql.Open("数据库类型", "用户名:密码@tcp(地址:端口)/数据库名")
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?%s", username, password, host, port, dbName, charset)
	database, err := sql.Open(driverName, dsn)
	if err != nil {
		fmt.Println("sql.Open err: ", err)
		return
	}
	Db = database
}
 
func main() {
	update()
}
 
func update() {
	result, err := Db.Exec("update person set username=? where user_id=?", "stu0003", 1)
	if err != nil {
		fmt.Println("Db.Exec err: ", err)
		return
	}
 
	rowsAffected, err := result.RowsAffected()
	if err != nil {
		fmt.Println("result.RowsAffected err: ", err)
		return
	}
	fmt.Println("rows affected: ", rowsAffected)
	// 打印结果
	//rows affected:  1
}
```

7、删除数据
```go
package main
 
import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)
 
var (
	driverName = "mysql"
	host       = "127.0.0.1"
	port       = "3306"
	username   = "root"
	password   = "root"
	charset    = "utf8"
	dbName     = "test"
)
 
var Db *sql.DB
 
func init() {
	//database, err := sql.Open("数据库类型", "用户名:密码@tcp(地址:端口)/数据库名")
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?%s", username, password, host, port, dbName, charset)
	database, err := sql.Open(driverName, dsn)
	if err != nil {
		fmt.Println("sql.Open err: ", err)
		return
	}
	Db = database
}
 
func main() {
	del()
}
 
func del() {
	result, err := Db.Exec("delete from person where user_id=?", 1)
	if err != nil {
		fmt.Println("Db.Exec err: ", err)
		return
	}
 
	rowsAffected, err := result.RowsAffected()
	if err != nil {
		fmt.Println("result.RowsAffected err: ", err)
		return
	}
	fmt.Println("rows affected: ", rowsAffected)
	// 打印结果
	//rows affected:  1
}
```

参考：https://www.kancloud.cn/uvohp5na133/golang/934132
