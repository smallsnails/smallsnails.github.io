接口类型转换，从超集到子集的转换是可以的；从方法集的子集到超集的转换会导致编译错误

```go
package main
 
import "fmt"
 
type Worker interface {
	say()
	show()
}
 
type A struct {
 
}
 
func (a A) say() {
	fmt.Println("say")
}
 
func (a A) show() {
	fmt.Println("show")
}
 
func do() Worker {
	// 接口类型转换，从超集到子集的转换是可以的
	// 从方法集的子集到超集的转换会导致编译错误
	return A{}
}
 
func main() {
	// 通过函数返回值的类型为接口类型
	d := do()
	d.show()
 
	// 或者声明变量类型为接口类型
	var a Worker = A{}
	a.show()
}

```

参考：https://blog.csdn.net/uudou/article/details/52456133
