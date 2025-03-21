TypeOf
```go
// TypeOf
 
package main
 
import (
	"fmt"
	"reflect"
)
 
type UserInterface interface {
	GetUserNameById(id int64) string
}
 
type User struct {
	Id   int32
	Name string
}
 
func (u User) GetUserNameById(id int32) string {
	data := ""
	if id == 1 {
		data = "zhangsan"
	}
	return data
}
 
func main() {
	u := User{}
	t := reflect.TypeOf(u)
	fmt.Println(t.Name())    // 类型 User
	fmt.Println(t.Kind())    // 类型名 struct
	fmt.Println(t.PkgPath()) // 反射对象所在的短包名 main
	fmt.Println(t.String())  // 包名.类型 main.User
 
	fmt.Println(t.Size())                           // 保存该类型要多少字节 24
	fmt.Println(t.Align())                          // 8
	fmt.Println(t.FieldAlign())                     // 8
	fmt.Println(t.AssignableTo(reflect.TypeOf(u)))  // true
	fmt.Println(t.ConvertibleTo(reflect.TypeOf(u))) // true
 
	fmt.Println(t.NumField())             // 字段数 2
	fmt.Println(t.Field(0).Name)          // 第0个字段名称
	fmt.Println(t.FieldByName("Id"))      // Id字段类型名 {Id  int32  0 [0] false} true
	fmt.Println(t.FieldByIndex([]int{0})) // {Id  int32  0 [0] false}
 
	// 方法数 1
	fmt.Println(t.NumMethod())
	// {GetUserNameById  func(main.User, int32) string <func(main.User, int32) string Value> 0} true
	fmt.Println(t.MethodByName("GetUserNameById"))
	// {GetUserNameById  func(main.User, int32) string <func(main.User, int32) string Value> 0}
	fmt.Println(t.Method(0))
}
 
 
// 返回结果
User
struct
main
main.User
24
8
8
true
true
2
Id
{Id  int32  0 [0] false} true
{Id  int32  0 [0] false}
1
{GetUserNameById  func(main.User, int32) string <func(main.User, int32) string Value> 0} true
{GetUserNameById  func(main.User, int32) string <func(main.User, int32) string Value> 0}
```

ValueOf
```go
// ValueOf
 
package main
 
import (
	"fmt"
	"reflect"
)
 
type UserInterface interface {
	GetUserNameById(id int64) string
	DoSomeThing(s string)
	Show() User
}
 
type User struct {
	Id    int32
	Name  string
	Other *string
}
 
func (u User) GetUserNameById(id int32) string {
	data := ""
	if id == 1 {
		data = "zhangsan"
	}
	return data
}
 
func (u *User) DoSomeThing(s string) {
	fmt.Println(u.Name + " do " + s)
}
 
func (u *User) Show() User {
	fmt.Println(u.Id, u.Name)
	return *u
}
 
func main() {
	u := User{}
	u.Id = 1000
	u.Name = "zhangsan"
	other := "other info"
	u.Other = &other
 
	v := reflect.ValueOf(u)
	fmt.Println(v.Type()) // 包名.类型 main.User
	fmt.Println(v.Kind()) // 类型名 struct
 
	fmt.Println(v.NumField())                   // 字段数 2
	fmt.Println(v.Field(0))                     // 1000
	fmt.Println(v.FieldByName("Id"))            // 1000
	fmt.Println(v.FieldByName("Name").String()) // zhangsan
	fmt.Println(v.FieldByIndex([]int{0}))       // 1000
 
	fmt.Println(v.NumMethod()) // 方法数
	fmt.Println(v.Method(0))
	fmt.Println(v.MethodByName("GetUserNameById"))
 
	// 调用GetUserNameById方法
	res := v.MethodByName("GetUserNameById").Call([]reflect.Value{reflect.ValueOf(int32(1))})
	fmt.Println(res) // [zhangsan]
 
	// 接收者为指针类型，以下调用报错
	// v.MethodByName("DoSomeThing").Call([]reflect.Value{reflect.ValueOf("some thing")})
	v2 := reflect.ValueOf(&u)
	fmt.Println(v2.Elem().Field(0)) // 1000
	v2.Elem().Field(0).Set(reflect.ValueOf(int32(2)))
	fmt.Println(v2.Elem().Field(0)) // 2
	// zhangsan do some thing
	v2.MethodByName("DoSomeThing").Call([]reflect.Value{reflect.ValueOf("some thing")})
 
	// 1000 zhangsan
	res = reflect.ValueOf(&u).MethodByName("Show").Call(nil)
	// [<main.User Value>] {2 zhangsan 0xc0000321f0}
	fmt.Println(res, res[0].Interface())
	// zhangsan
	fmt.Println(res[0].FieldByName("Name"))
 
	// other info
	fmt.Println(reflect.ValueOf(&u).Elem().FieldByName("Other").Elem())
}
 
// 执行结果
main.User
struct
3
1000
1000
zhangsan
1000
1
0xc13600
0xc13600
[zhangsan]
1000
2
zhangsan do some thing
2 zhangsan
[<main.User Value>] {2 zhangsan 0xc0000881e0}
zhangsan
other info
```

参考：https://blog.csdn.net/u013474436/article/details/84553546
