```javascript
//格式化时间戳
var format = function(timestamp)
{
	//timestamp是整数，否则要parseInt转换
	var time = new Date(timestamp);
	var y = time.getFullYear();
	var m = time.getMonth()+1;
	var d = time.getDate();
	var h = time.getHours();
	var mm = time.getMinutes();
	var s = time.getSeconds();
	return y+'-'+add0(m)+'-'+add0(d)+' '+add0(h)+':'+add0(mm)+':'+add0(s);
}
 
//补零
var add0 = function(m){
	return m<10?'0'+m:m 
}
```
