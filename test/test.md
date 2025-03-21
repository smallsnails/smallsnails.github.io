# 1、下载

```bash
# 创建目录
# 进入目录
# 执行 go mod init xxx 命令（即：在当前目录初始化创建一个模块）
 
# 下载gozxing
go get github.com/makiuchi-d/gozxing
```

# 2、生成二维码
```go
package main
 
import (
	"image/png"
	"os"
 
	"github.com/makiuchi-d/gozxing"
	"github.com/makiuchi-d/gozxing/oned"
	"github.com/makiuchi-d/gozxing/qrcode"
)
 
func main() {
	// writeBarcode()
	writeQrcode()
}
 
// 条形码
func writeBarcode() {
	// Generate a barcode image (*BitMatrix)
	writer := oned.NewCode128Writer()
	img, _ := writer.Encode("content: bar code", gozxing.BarcodeFormat_CODE_128, 100, 50, nil)
 
	file, _ := os.Create("barcode.png")
	defer file.Close()
 
	png.Encode(file, img)
}
 
// 二维码
func writeQrcode() {
	// Generate a qrcode image (*BitMatrix)
	writer := qrcode.NewQRCodeWriter()
	img, _ := writer.Encode("content: qr code", gozxing.BarcodeFormat_QR_CODE, 100, 100, nil)
 
	file, _ := os.Create("qrcode.png")
	defer file.Close()
 
	png.Encode(file, img)
}
```

# 3、识别二维码
```go
package main
 
import (
	"fmt"
	"image"
	_ "image/png"
	"os"
 
	"github.com/makiuchi-d/gozxing"
	"github.com/makiuchi-d/gozxing/oned"
	"github.com/makiuchi-d/gozxing/qrcode"
)
 
func main() {
	// readBarcode()
	readQrcode()
}
 
// 条形码
func readBarcode() {
	// open and decode image file
	file, _ := os.Open("barcode.png")
	img, _, _ := image.Decode(file)
 
	// prepare BinaryBitmap
	bmp, _ := gozxing.NewBinaryBitmapFromImage(img)
 
	// decode image
	reader := oned.NewCode128Reader()
	result, _ := reader.Decode(bmp, nil)
 
	fmt.Println(result)
}
 
// 二维码
func readQrcode() {
	// open and decode image file
	file, _ := os.Open("qrcode.png")
	img, _, _ := image.Decode(file)
 
	// prepare BinaryBitmap
	bmp, _ := gozxing.NewBinaryBitmapFromImage(img)
 
	// decode image
	reader := qrcode.NewQRCodeReader()
	result, _ := reader.Decode(bmp, nil)
 
	fmt.Println(result)
}
```

详见：  
https://pkg.go.dev/search?q=qrcode  
https://github.com/makiuchi-d/gozxing   


