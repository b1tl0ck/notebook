---
tags:
  - 日志分析
create_time: 2024-10-18 10:30
modified_time: 2024-10-18 10:30
status: complete
---
Grep：数据查找定位
Awk：数据切片
Sed：数据修改

# grep

```bash
常用选项:
-i：忽略大小写进行匹配。
-v：反向查找，只打印不匹配的行。
-n：显示匹配行的行号。
-r：递归查找子目录中的文件。
-l：只打印匹配的文件名。
-c：只打印匹配的行数。
```

```bash
更多参数说明：
-a 或 --text : 不要忽略二进制的数据。
-A<显示行数> 或 --after-context=<显示行数> : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。
-b 或 --byte-offset : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。
-B<显示行数> 或 --before-context=<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前的内容。
-c 或 --count : 计算符合样式的列数。
-C<显示行数> 或 --context=<显示行数>或-<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前后的内容。
-d <动作> 或 --directories=<动作> : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
-e<范本样式> 或 --regexp=<范本样式> : 指定字符串做为查找文件内容的样式。
-E 或 --extended-regexp : 将样式为延伸的正则表达式来使用。
-f<规则文件> 或 --file=<规则文件> : 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
-F 或 --fixed-regexp : 将样式视为固定字符串的列表。
-G 或 --basic-regexp : 将样式视为普通的表示法来使用。
-h 或 --no-filename : 在显示符合样式的那一行之前，不标示该行所属的文件名称。
-H 或 --with-filename : 在显示符合样式的那一行之前，表示该行所属的文件名称。
-i 或 --ignore-case : 忽略字符大小写的差别。
-l 或 --file-with-matches : 列出文件内容符合指定的样式的文件名称。
-L 或 --files-without-match : 列出文件内容不符合指定的样式的文件名称。
-n 或 --line-number : 在显示符合样式的那一行之前，标示出该行的列数编号。
-o 或 --only-matching : 只显示匹配PATTERN 部分。
-q 或 --quiet或--silent : 不显示任何信息。
-r 或 --recursive : 此参数的效果和指定"-d recurse"参数相同。
-s 或 --no-messages : 不显示错误信息。
-v 或 --invert-match : 显示不包含匹配文本的所有行。
-V 或 --version : 显示版本信息。
-w 或 --word-regexp : 只显示全字符合的列。
-x --line-regexp : 只显示全列符合的列。
-y : 此参数的效果和指定"-i"参数相同
```


# awk

`awk options 'pattern {action}' file`

选项参数说明：
options：是一些选项，用于控制 awk 的行为。
pattern：是用于匹配输入数据的模式。如果省略，则 awk 将对所有行进行操作。
{action}：是在匹配到模式的行上执行的动作。如果省略，则默认动作是打印整行。

options 参数说明：
```bash
-F <分隔符> 或 --field-separator=<分隔符>： 指定输入字段的分隔符，默认是空格。使用这个选项可以指定不同于默认分隔符的字段分隔符。
-v <变量名>=<值>： 设置 awk 内部的变量值。可以使用该选项将外部值传递给 awk 脚本中的变量。
-f <脚本文件>： 指定一个包含 awk 脚本的文件。这样可以在文件中编写较大的 awk 脚本，然后通过 -f 选项将其加载。
-V 或 --version： 显示 awk 的版本信息。
-h 或 --help： 显示 awk 的帮助信息，包括选项和用法示例。
```

以下是一些常见的 awk 命令用法：
```bash
# 打印整行
awk '{print}' file

# 打印特定列
awk '{print $1, $2}' file

# 使用分隔符指定列
awk -F',' '{print $1, $2}' file

# 打印行数
awk '{print NR, $0}' file

# 打印行数满足条件的行
awk '/pattern/ {print NR, $0}' file

# 计算列的总和
awk '{sum += $1} END {print sum}' file

# 打印最大值
awk 'max < $1 {max = $1} END {print max}' file

# 格式化输出
awk '{printf "%-10s %-10s\n", $1, $2}' file
```

# find

该命令支持的选项有：

选项
```bash
-mount, -xdev	只检查和指定目录在同一个文件系统下的文件，避免列出其它文件系统中的文件
-amin n			在过去 n 分钟内被读取过
-anewer file	比文件 file 更晚被读取过的文件
-atime n		在过去 n 天内被读取过的文件
-cmin n			在过去 n 分钟内被修改过
-cnewer file	比文件 file 更新的文件
-ctime n		在过去 n 天内创建的文件
-mtime n		在过去 n 天内修改过的文件
-empty			空的文件-gid n or -group name : gid 是 n 或是 group 名称是 name
-ipath p, -path p	路径名称符合 p 的文件，ipath 会忽略大小写
-name name, -iname name	文件名称符合 name 的文件。iname 会忽略大小写
-size n			文件大小 是 n 单位
-type f			文件类型是 c 的文件
```


# sed

`sed [option] 'sed command' filename`

option:

s	表示查找并替换
-i	表示直接修改源文件
-E	支持扩展表达式
-e  表示可以指定表达式

常用：
```bash
# 替换文本
sed 's/pattern/replacement/' file

# 全局替换文本
sed 's/pattern/replacement/g' file

# 替换匹配行中的第一个匹配项
sed '0,/pattern/s/pattern/replacement/' file

# 删除空白行
sed '/^$/d' file

# 删除行首或行尾的空格
sed 's/^ *//; s/ *$//' file

# 插入新行
sed '2i\New Line' file

# 删除指定行
sed '2d' file

# 执行多个编辑操作
sed -e 's/pattern1/replacement1/' -e 's/pattern2/replacement2/' file
```

进阶：
使用正则表达式：
   - 匹配行：`sed '/pattern/p' file`
   - 反向匹配行：`sed '/pattern/!p' file`
   - 匹配行范围：`sed '/start/,/end/p' file`
```bash
1. 替换指定行或范围内的文本：
sed '2s/pattern/replacement/' file
sed '2,4s/pattern/replacement/' file
2. 替换指定行或范围内的文本：
   sed '2s/pattern/replacement/' file
   sed '2,4s/pattern/replacement/' file
3. 保存替换结果到原文件（原地编辑）：
   sed -i 's/pattern/replacement/' file
4. 根据正则表达式进行删除操作：
   sed '/pattern/d' file
5. 根据条件进行处理：
   sed '/pattern/{command}' file
6. 多行处理：
   sed ':a;N;$!ba;s/\n/ /g' file
7. 引用变量：
   var="replacement"
   sed "s/pattern/$var/" file
8. 执行外部命令并将结果插入到文本中
   sed 's/pattern/$(command)/' file