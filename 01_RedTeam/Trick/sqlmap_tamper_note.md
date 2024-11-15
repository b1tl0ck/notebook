---
tags:
  - sql注入
create_time: 2023-12-07 23:22
modified_time: 2024-05-27 14:54
status: complete
---
### sqlmap部分脚本用法说明

### 1、apostrophemask.py

适用数据库：ALL

作用：将引号替换为utf-8，用于过滤单引号

使用脚本前：tamper("1 AND '1'='1")

使用脚本后：1 AND %EF%BC%871%EF%BC%87=%EF%BC%871

### 2、base64encode.py

适用数据库：ALL

作用：替换base64编码

使用脚本前：tamper("1' AND SLEEP(5)#")

使用脚本后：MScgQU5EIFNMRUVQKDUpIw==

### 3、multiplespaces.py

适用数据库：ALL

作用：围绕sql关键字添加多个空格

使用脚本前：tamper('1 UNION SELECT foobar')

使用脚本后：1 UNION SELECT foobar

## 4、space2plus.py

适用数据库：ALL

作用:用加号替换空格

使用脚本前：tamper('SELECT id FROM users')

使用脚本后：SELECT+id+FROM+users

## 5、space2randomblank.py

适用数据库：ALL

作用：将空格替换为其他随机有效字符

使用脚本前：tamper('SELECT id FROM users')

使用脚本后：SELECT%0Did%0CFROM%0Ausers

## 6、unionalltounion.py

适用数据库：ALL

作用：将union all select 替换为union select

使用脚本前：tamper('-1 UNION ALL SELECT')

使用脚本后：-1 UNION SELECT

## 7、space2dash.py

适用数据库：ALL

作用：将空格替换为破折号（--），并添加一个随机字符和换行符（\n）

使用脚本前：tamper('1 AND 9227=9227')

适用脚本后：1--upgPydUzKpMX%0AAND--RcDKhIr%0A9227=9227

## 8、space2mssqlblank.py

适用数据库：mssql

测试数据库版本：Microsoft SQL Server 2000 、Microsoft SQL Server 2005

作用：将空格替换为有效字符集的随机空白字符('%01', '%02', '%03', '%04', '%05', '%06', '%07', '%08', '%09', '%0B', '%0C', '%0D', '%0E', '%0F', '%0A')

使用脚本前：tamper('SELECT id FROM users')

适用脚本后：SELECT%0Did%0DFROM%04users

## 9、between.py

测试数据库：Microsoft SQL Server 2005 、MySQL 4, 5.0 and 5.5、 Oracle 10g、 PostgreSQL 8.3, 8.4, 9.0

作用：将">"替换为"NOT BETWEEN 0 AND #"，将"="替换为"BETWEEN # AND #"

使用脚本前：tamper('1 AND A > B--')，tamper('1 AND A = B--')

使用脚本后：1 AND A NOT BETWEEN 0 AND B--，1 AND A BETWEEN B AND B--

## 10、percentage.py

适用数据库：ASP

测试数据库：Microsoft SQL Server 2000, 2005 、MySQL 5.1.56, 5.5.11 、PostgreSQL 9.0

作用：在每个字符前加上一个%

使用脚本前：tamper('SELECT FIELD FROM TABLE')

使用脚本后：%S%E%L%E%C%T %F%I%E%L%D %F%R%O%M %T%A%B%L%E

## 11、sp_password.py

适用数据库：mssql

作用：将sp_password追加到有效载荷后，以便从DBMS日志中自动混淆。

使用脚本前：tamper('1 AND 9227=9227-- ')

使用脚本后：1 AND 9227=9227-- sp_password

## 12、charencode.py

测试数据库：Microsoft SQL Server 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0

作用：对指定的payload全部使用url编码（不处理已进行编码的字符）

使用脚本前：tamper('SELECT FIELD FROM%20TABLE')

使用脚本后：%53%45%4C%45%43%54%20%46%49%45%4C%44%20%46%52%4F%4D%20%54%41%42%4C%45

## 13、randomcase.py

测试数据库：Microsoft SQL Server 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0、SQLite 3

作用：将字符替换为随机大小写

使用脚本前：tamper('INSERT')

使用脚本后：InSeRt

## 14、charunicodeencode.py

适用数据库：ASP 、ASP.NET

测试数据库：Microsoft SQL Server 2000 、Microsoft SQL Server 2005、MySQL 5.1.56 、PostgreSQL 9.0.3

作用：适用字符串的Unicode编码

使用脚本前：tamper('SELECT FIELD%20FROM TABLE')

使用脚本后：%u0053%u0045%u004C%u0045%u0043%u0054%u0020%u0046%u0049%u0045%u004C%u0044%u0020%u0046%u0052%u004F%u004D%u0020%u0054%u0041%u0042%u004C%u0045

## 15、space2comment.py

测试数据库：Microsoft SQL Server 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0

作用：将空格替换为/**/

使用脚本前：tamper('SELECT id FROM users')

使用脚本后：SELECT/**/id/**/FROM/**/users