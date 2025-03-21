```php
<?php
header('Content-type:text/html;charset=utf-8');
//读取图片文件，转换成base64编码格式
$image_file = './4296762_165319032930_2.jpg';
$image_info = getimagesize($image_file);
$base64_image_content = "data:{$image_info['mime']};base64," . chunk_split(base64_encode(file_get_contents($image_file)));
 
//保存base64字符串为图片
//匹配出图片的格式
if (preg_match('/^(data:\s*image\/(\w+);base64,)/', $base64_image_content, $result)){
$type = $result[2];
$new_file = "./test.{$type}";
if (file_put_contents($new_file, base64_decode(str_replace($result[1], '', $base64_image_content)))){
echo '新文件保存成功：', $new_file;
}
 
}
?>
 
<img src="<?php echo $base64_image_content;?>" />
```

参考：https://www.cnblogs.com/shanmao/p/3779358.html
