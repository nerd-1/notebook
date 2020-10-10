## Bootstrap

关于bootstrap我想说的是，我的网站就是由bootstrap建起来的，关于为什么不用更加复杂而高级的框架来构建网站呢，因为我不会emmmm。所以对于bootstrap我总结一下。bootstrap入门十分简单，超级容易上手。bootstrap网站里面的东西就是引入相关库之后即可将包装好的东西拿来现学现用。

### bootstrap3的基本格式

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap 101 Template</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet">
  </head>
  <body>
    <h1>你好，世界！</h1>
    <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js"></script>
  </body>
</html>
```

bootstrap3需要引入这三个文件，然后就可以编写了。

栅格系统有一个要注意的是，比如说设置手机端隐藏可以设置class属性为：hidden-xs。

### bootstrap4的基本格式

需要引入这四个cdn库：

```html
<!-- 新 Bootstrap4 核心 CSS 文件 -->
<link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/4.3.1/css/bootstrap.min.css">
 
<!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
<script src="https://cdn.staticfile.org/jquery/3.2.1/jquery.min.js"></script>
 
<!-- bootstrap.bundle.min.js 用于弹窗、提示、下拉菜单，包含了 popper.min.js -->
<script src="https://cdn.staticfile.org/popper.js/1.15.0/umd/popper.min.js"></script>
 
<!-- 最新的 Bootstrap4 核心 JavaScript 文件 -->
<script src="https://cdn.staticfile.org/twitter-bootstrap/4.3.1/js/bootstrap.min.js"></script>
```

好了，大功告成！