## warmup

#### 思路

打开后界面只有一张图片，考虑php代码注释中有提示

f12检查或利用抓包工具查看

发现如下注释

<!--source.php-->

访问后得到php代码如下  

```php
<?php
    highlight_file(__FILE__);
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {
                return true;
            }

            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }

    if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?>
```



==**当_REQUEST['file']不为空且它是字符串且checkFile函数返回true时才会包含这个文件否则继续输出图片**==

代码中还有如下提示：

```php
$whitelist = ["source"=>"source.php","hint"=>"hint.php"];
```

**继续访问hint.php**

又出现提示

flag not here, and flag in ffffllllaaaagggg

可知flag在提示的文件中

下一步：**找到该文件的路径，且使返回值为true**

利用php中函数进行绕过

#### **mb_substr（ , , ）** `用于截断字符串`

#### **mb_strpos( , , )**`用于返回指定字符在字符串中第一次出现的位置`

**==这里使用两层函数用来截断第一个问号前的字符==**

由于source.php肯定存在于白名单中于是可以构造出

==file=source.php?/../../../../ffffllllaaaagggg==

绕过判断同时包含指定文件

`文件路径是猜的由于文件名写了四次flag就猜可能需要四级../来找到该文件路径`

