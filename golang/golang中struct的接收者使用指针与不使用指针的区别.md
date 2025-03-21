如果想要修改结构体中的数据，接收者应该为指针类型，否则，接收者类型就为非指针类型。具体什么类型看情况而定。

```go
package main
 
import "fmt"
 
type Inter interface {
	Say(name string)
}
 
type Cat struct {
	Name string
}
 
func (c Cat) Say(name string) {
    // 修改结构体数据无效
	c.Name = name
	fmt.Printf("cat name is : %s\n", c.Name)
}
 
type Dog struct {
	Name string
}
 
func (d *Dog) Say(name string) {
    // 可以修改结构体数据
	d.Name = name
	fmt.Printf("dog name is : %s\n", d.Name)
}
 
func main() {
	c := Cat{}
	c.Name = "zhangsan"
	c.Say("lisi")
	fmt.Println("c.Name = ", c.Name)
 
	d := new(Dog)
	d.Name = "zhangsan"
	d.Say("lisi")
	fmt.Println("d.Name = ", d.Name)
}
 
 
// 执行结果
cat name is : lisi
c.Name =  zhangsan
dog name is : lisi
d.Name =  lisi
```

参考：  
https://segmentfault.com/q/1010000012079161  
https://blog.csdn.net/weixin_41395435/article/details/109222845  
