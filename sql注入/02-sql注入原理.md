## sql注入的原因

语言分类：解释器语言和编译型语言。解释器语言是变解析边执行，编译型语言是一次编译后不用重复执行。

在解释型语言中，如果程序与用户进行交互。用户就可以构造特殊字符的输入来拼接到程序中执行，从而使得程序依据用户的执行有可能存在恶意行为的代码。

例如：在与用户交互的程序中，用户的输入拼接到SQL语句中，执行了与原定计划不同的行为，从而产生的SQL注入漏洞。

## 登陆案例讲解

登陆的SQL语句：

```sql
select * from admin where username="用户名" and password="用户输入的密码"。
```

用户输入的内容可以由用户自行控制，例如何以输入' or 1=1 -- 。（--后面有一个空格）

拼接成的SQL语句：

```sql
select * from admin where username='' or 1=1 -- 'and password ='用户输入的密码'。
```

其中 or 1=1永远为真，--注释后面的内容不再执行，因此sql语句会返回admin表中的所有内容。

## CMS SQL注入讲解

CMS逻辑：index.php首页展示内容，具有文章列表（链接具有文章id），articles.php文章详情页，URL中article.php?id=文章id读取id文章。

SQL注入验证：

1. '(单引号)
2. and 1=1
3. and 1=2

如果页面中Mysql报错，证明该页面存在SQL注入漏洞。

## SQL注入普遍的思路

- 发现SQL注入位置
- 判断后台数据库类型
- 确定XP_CMDSHELL可执行情况
- 发现WEB虚拟目录
- 上传ASP脚本
- 得到管理员权限

## sqlmap基本使用

sqlmap是检测和利用sql注入漏洞的一款强大的工具。几乎有了sqlmap不需要任何软件来探测和利用sql漏洞。

### sqlmap的下载

链接：https://pan.baidu.com/s/1q3pqjZo-9fE9Xvmgt2mT2g 
提取码：3xwr 
复制这段内容后打开百度网盘手机App，操作更方便哦

然后下载之后解压，进入相关目录运行：

```shell
python3 sqlmap.py
```

即可。
