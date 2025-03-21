```go
package main
 
import "fmt"
 
type Worker interface {
	say()
	show()
}
 
type A struct {
	name   string
	Worker Worker
}
 
func (a A) say() {
	fmt.Println("a: say")
}
 
type B struct {
	name string
}
 
func (b B) say() {
	fmt.Println("b: say")
}
 
func (b B) show() {
	fmt.Println("b: show")
}
 
func main() {
	b := B{}
	a := A{}
	a.Worker = b
 
	// 调用B实现的方法
	a.Worker.say()
	a.Worker.show()
 
	// 如果结构体A没有实现say方法，只能调用B中的方法
	// 如果结构体A没有实现Worker接口的方法，则都不能调用，否则报错
	a.say()
}
 
// 执行结果如下：
b: say
b: show
a: say
```

参考：  
https://studygolang.com/articles/17128  
https://www.jianshu.com/p/a5bc8add7c6e  
