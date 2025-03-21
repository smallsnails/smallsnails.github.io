为什么以下操作不能修改原始数据的值？

```go
package main
 
import "fmt"
 
func main() {
	i := 10
	a := &i
	fmt.Printf("原始指针的内存地址是：%p\n", &a)
	modify(a)
	fmt.Printf("现在指针的内存地址和数据是：%p %v\n", &a, i)
}
 
func modify(a *int) {
	fmt.Printf("函数里接收到的指针的内存地址是：%p\n", &a)
	// 以下操作对函数外无影响
	x := 100
	a = &x
}
 
/**
执行结果：
原始指针的内存地址是：0xc000006028
函数里接收到的指针的内存地址是：0xc000006038
现在指针的内存地址和数据是：0xc000006028 10
 */
```

```go
package main
 
import "fmt"
 
type St struct {
	data interface{}
}
 
func main() {
	s := new(St)
	fmt.Printf("原始指针的内存地址是：%p\n", &s)
	s.modify()
	fmt.Printf("现在指针的内存地址是：%p\n", &s)
}
 
func (s *St) modify() *St {
	fmt.Printf("方法里接收者指针的内存地址是：%p\n", &s)
    // 以下操作对方法外无影响
	s = new(St)
	return s
}
```

具体解释见原文：  
[Go语言参数传递是传值还是传引用 | 飞雪无情的博客](https://www.flysnow.org/2018/02/24/golang-function-parameters-passed-by-value.html)  
[Go语言 参数传递究竟是值传递还是引用传递的问题分析 - hubb - 博客园](https://www.cnblogs.com/ithubb/p/15468144.html)  
