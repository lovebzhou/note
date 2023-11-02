- [AWK 简明教程](https://coolshell.cn/articles/9070.html)
- [awk 入门教程](https://www.ruanyifeng.com/blog/2018/11/awk.html)

## 快速入门

```sh
# 演示文件
tail 10 /etc/passwd > demo.txt
```

### 基本用法

```sh
awk 动作 文件名

# `print $0`就是把标准输入`this is a test`，重新打印了一遍。
# 依次用`$1`、`$2`、`$3`代表第一个字段、第二个字段、第三个字段等等。
echo 'this is a test' | awk '{print $0}'

# 输出：a
echo 'this is a test' | awk '{print $3}'

# `-F`参数指定分隔符为冒号
cat /etc/passwd | awk -F ':' '{print $1}'
```

### 变量


```sh
# =>>> $ + 数字：表示匹配行第几列字段
echo 'this is a test' | awk '{print $2}'

# =>>> NF：表示当前行有多少个字段

# 打印第一、和最后一个字段。","表示输出用空格分隔
echo 'this is a test' | awk '{print $1,$NF}'

# FILENAME：当前文件名
awk -F ':' '{print FILENAME " -- " $1}' demo.txt

# NR：表示当前处理的是第几行
cat /etc/passwd | awk -F ':' '{print NR ") " $1}'
```

**`awk`的其他内置变量如下：**
-  `FILENAME`：当前文件名
-   `FS`：字段分隔符，默认是空格和制表符。
-   `RS`：行分隔符，用于分割每一行，默认是换行符。
-   `OFS`：输出字段的分隔符，用于打印时分隔字段，默认为空格。
-   `ORS`：输出记录的分隔符，用于打印时分隔记录，默认为换行符。
-   `OFMT`：数字输出的格式，默认为`％.6g`。

### 函数

```sh
# toupper：将字符转为大写
echo 'this is a test' | awk '{print toupper($4)}'
```

-   `tolower()`：字符转为小写。
-   `length()`：返回字符串长度。
-   `substr()`：返回子字符串。
-   `sin()`：正弦。
-   `cos()`：余弦。
-   `sqrt()`：平方根。
-   `rand()`：随机数。

### 条件

```sh
# =>>> awk '条件 动作' 文件名

# 输出奇数行
awk -F ':' 'NR % 2 == 1 {print NR ")" $1}' demo.txt

# 输出第三行以后的行
awk -F ':' 'NR >3 {print NR ")" $1}' demo.txt

# 输出第三个字段为 280 的行
awk -F ':' '$3 == 280 { print NR ")" $0}' demo.txt
```
