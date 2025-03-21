```go
package main
 
import (
	"fmt"
	"strconv"
	"strings"
)
 
func main() {
	s := "255.255.255.255"
	fmt.Println(ip2iong(s)) // 4294967295
	l := 4294967295
	fmt.Println(long2ip(uint32(l))) // 255.255.255.255
}
 
// 字符串转整型
func ip2iong(value string) uint32 {
	list := strings.Split(value, ".")
	result := uint32(0)
	for i, v := range list {
		temp, _ := strconv.ParseUint(v, 10, 8)
		result += uint32(temp) << uint8(24-i*8)
	}
	return result
}
 
// 整型转ip
func long2ip(value uint32) string {
	return fmt.Sprintf("%d.%d.%d.%d", byte(value>>24), byte(value>>16), byte(value>>8), byte(value))
}
```
