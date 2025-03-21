### 安装docsify说明
```
详见：https://docsify.js.org/
```

### 1.下载
```
wget https://codeload.github.com/docsifyjs/docsify/zip/refs/heads/master -o docsify-master.zip
```

### 2.解压
```
unzip docsify-master.zip
```

### 3.移动文件到nginx所在目录[略]

### 4.配置nginx，示例如下
```
location / {
    root   'docsify所在的目录';
    index  index.html index.htm;

    #add_header Cache-Control "no-cache, no-store, must-revalidate";
    #add_header Pragma "no-cache";
    #add_header Expires 0;
}
```

### 5.重启nginx
```
nginx -s reload
```

### 6.浏览器通过ip或域名访问项目

### 7.手动修改index.html
```
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
    <meta name="viewport" content="width=device-width,initial-scale=1"/>
    <meta charset="UTF-8"/>
    <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/lib/themes/dark.css">
</head>
<body>
<div id="app">Loading...</div>
<script>
    window.$docsify = {
        name: 'snail',
        loadSidebar: 'zh-cn/_sidebar.md',
        //loadSidebar: true,
        //loadNavbar: true,
        search: {
            placeholder: '搜索',
            noData: '找不到结果',
            depth: 2, // 搜索标题的最大层级, 1 - 6
            namespace: 'docsify',
            pathNamespaces: ['/zh-cn'],
        }
    };
</script>
<script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/emoji.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/zoom-image.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/docsify-copy-code/dist/docsify-copy-code.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-bash.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-php.min.js"></script>
</body>
</html>
```

### 8.根据情况修改_sidebar.md、_navbar.md

### 9.补充：更换css、js使用国内cdn资源
```
<!-- 更换css、js使用国内cdn资源 -->
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
    <meta name="viewport" content="width=device-width,initial-scale=1"/>
    <meta charset="UTF-8"/>
    <link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/docsify/4.13.0/themes/dark.min.css">
    <title>snail</title>
</head>
<body>
<div id="app">Loading...</div>
<script>
    window.$docsify = {
        name: 'snail',
        //basePath: 'zh-cn/',
        loadSidebar: 'zh-cn/_sidebar.md',
        //loadSidebar: '_sidebar.md',
        //loadSidebar: true,
        //loadNavbar: true,
        search: {
            placeholder: '搜索',
            noData: '找不到结果',
            depth: 2, // 搜索标题的最大层级, 1 - 6
            namespace: 'docsify',
            pathNamespaces: ['/zh-cn'],
        }
    };
</script>
<script src="https://cdn.bootcdn.net/ajax/libs/docsify/4.13.0/docsify.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/docsify/4.13.0/plugins/search.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/docsify/4.13.0/plugins/emoji.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/docsify/4.13.0/plugins/zoom-image.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/docsify-copy-code/2.1.1/docsify-copy-code.min.js"></script>
</body>
</html>
```
