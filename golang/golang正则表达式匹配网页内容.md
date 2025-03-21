```go
// 简单爬虫：正则匹配网页内容
// 以下是复制人家的正则表达式的代码
 
var (
    //邮箱
    reQQEmail = `(\d+)@qq.com`
    reEmail   = `\w+@\w+\.\w+(\.\w+)?`
 
    //超链接
    //<a href="http://news.baidu.com/ns?cl=2&rn=20&tn=news&word=%C1%F4%CF%C2%D3%CA%CF%E4%20%B5%BA%B9%FA"
    reLinkBad = `<a[\s\S]*?href="(https?://[\s\S]+?)"`
    reLink    = `href="(https?://[\s\S]+?)"`
 
    //手机号
    //13x xxxx xxxx
    rePhone = `1[345789]\d\s?\d{4}\s?\d{4}`
 
    //身份证号
    //123456 1990 0817 123X
    reIdcard = `[123456]\d{5}((19\d{2})|(20[01]\d))((0[1-9])|(1[012]))((0[1-9])|([12]\d)|(3[01]))\d{3}[\dX]`
 
    //图片链接
    //"http://img2.imgtn.bdimg.com/it/u=2403021088,4222830812&fm=26&gp=0.jpg"
    reImg = `"(https?://[^"]+?(\.((jpg)|(jpeg)|(png)|(gif)|(bmp)|(svg)|(swf)|(ico))))"`
)
```

接下来，调用golang里面regexp包进行处理网页内容，提取数据，做相关处理。  

详见原文：  
https://studygolang.com/topics/8505  
https://studygolang.com/topics/8506  
