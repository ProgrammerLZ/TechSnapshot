## MySQL支持的数据类型

### 日期类型

MySQL 5.0 支持的数据类型如下表

![image-20200729134626095](MySQL%E5%9F%BA%E7%A1%80.assets/image-20200729134626095.png)



### 字符串类型

<img src="MySQL%E5%9F%BA%E7%A1%80.assets/image-20200729151103689.png" alt="image-20200729151103689" style="zoom: 67%;" />



## MySQL中的运算符

### 算数运算符

| 运算符 | 作用 |
| ------ | ---- |
| +      | 加   |
| -      | 减   |
| *      | 成   |
| /,DIV  | 除   |
| %, MOD | 余   |



### 比较运算符

| 运算符          | 作用                      |
| --------------- | ------------------------- |
| =               | 等于                      |
| <> 或 !=        | 不等于                    |
| <=>             | NULL安全的等于            |
| <               | 小于                      |
| <=              | 小于等于                  |
| >               | 大于                      |
| >=              | 大于等于                  |
| BETWEEN         | 指定范围之间，闭区间[a,b] |
| IN              | 存在于指定集合            |
| IS NULL         | 是NULL                    |
| IS NOT NULL     | 不是NULL                  |
| LIKE            | 通配符匹配                |
| REGEXP 或 RLIKE | 正则表达式匹配            |



### 逻辑运算符

| 运算符     | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| NOT 或 ！  | 非                                                           |
| AND 或 &&  | 与                                                           |
| OR 或 \|\| | 或                                                           |
| XOR        | 异或（若任意操作数为NULL，结果为NULL；若都不为NULL，两值相异为1，否则为0） |



### 位运算符

| 运算符 | 作用   |
| ------ | ------ |
| &      | 位与   |
| \|     | 位或   |
| ^      | 位异或 |
| ~      | 位取反 |
| >>     | 位右移 |
| <<     | 位左移 |





## 常用函数

### 字符串函数

| 函数                  | 功能                                                 |
| --------------------- | ---------------------------------------------------- |
| CONCAT(S1,S2,...Sn)   | 连接S1,S2,...Sn                                      |
| INSERT(str,x,y,instr) | 将字符串str，从x位置开始长度为y的字符串，替换为instr |
| LOWER(str)            | 小写化                                               |
| UPPER(str)            | 大写化                                               |
| LEFT(str,x)           | 返回最左边x个字符                                    |
| RIGHT(str,x)          | 返回最右边x个字符                                    |
| LPAD(str,n,pad)       | 左侧用pad填充，直到填满为n的长度为止                 |
| RPAD(str,n,pad)       | 右侧用pad填充，直到填满为n的长度为止                 |
| LTRIM(str)            | 左侧去空格                                           |
| RTRIM(str)            | 右侧去空格                                           |
| REPEAT(str,x)         | 返回str重复x次的结果                                 |
| REPLACE(str,a,b)      | 用字符串b代替在str中所有的字符串a                    |
| STRCMP(s1,s2)         | 字符串比较                                           |
| TRIM(str)             | 去空格                                               |
| SUBSTRING(str,x,y)    | 返回str从位置x起长度为y的字符串                      |



### 数值函数

| 函数          | 作用                           |
| ------------- | ------------------------------ |
| ABS(x)        | 取绝对值                       |
| CEIL(x)       | 返回大于x的最小整数值          |
| FLOOR(x)      | 返回小于x的最大整数值          |
| MOD(x,y)      | 取模                           |
| RAND()        | 返回0~1内的随机数              |
| ROUND(x,y)    | 返回x的四舍五入的有y位小数的值 |
| TRUNCATE(x,y) | 返回数字阶段weiy位小数的结果   |



### 日期和时间函数

| 函数                              | 功能                           |
| --------------------------------- | ------------------------------ |
| CURDATE()                         | 返回当前日期                   |
| CURTIME()                         | 返回当前时间                   |
| NOW()                             | 返回当前日期时间               |
| UNIX_TIMESTAMP(date)              | 返回date的unix时间戳           |
| FROM_UNIXTIME(unixtimestamp)      | 返回unix时间戳的date           |
| WEEK(date)                        | 返回date是为一年中的第几周     |
| YEAR(date)                        | 返回date的年份                 |
| HOUR(time)                        | 返回time的小时                 |
| MINUTE(time)                      | 返回time的分钟                 |
| MONTHNAME(date)                   | 返回date的月份                 |
| DATE_FOREMAT(date,fmt)            | 格式化date                     |
| DATE_ADD(date,INTERVAL expr type) | 返回date加上一个时间间隔后的值 |
| DATE_DIFF(expr,expr2)             | 返回两个日期相差的天数         |

`DATE_FOREMAT`的fmt参数可参照下表：

<img src="MySQL%E5%9F%BA%E7%A1%80.assets/image-20200730112806348.png" alt="image-20200730112806348" style="zoom:50%;" />

`DATE_ADD`的第二个参数可参照下表：

<img src="MySQL%E5%9F%BA%E7%A1%80.assets/image-20200730113538874.png" alt="image-20200730113538874" style="zoom:50%;" />



### 流程函数

| 函数                                                         | 作用                                          |
| ------------------------------------------------------------ | --------------------------------------------- |
| IF(value,t,f)                                                | value为真，返回他，否则返回f                  |
| IFNULL(value1,value2)                                        | value1不为空，返回value1，否则返回value2      |
| CASE WHEN [value1] THEN [result1] ... ELSE [default] END     | value1为真，返回result1，否则返回default      |
| CASE [expr] WHEN [value1] THEN [result1] ... ELSE [default] END | 如果expr=value1，返回result1，否则返回default |



### 其他常用函数

| 函数           | 作用                     |
| -------------- | ------------------------ |
| DATABASE()     | 返回数据库名称           |
| VERSION()      | 返回数据库版本           |
| USER()         | 返回当前登录用户的用户名 |
| INET_ATON(IP)  | 返回IP地址的数字表示     |
| INET_NTOA(num) | 返回数字代表的IP地址     |
| PASSWORD(str)  | 返回字符串str的加密版本  |
| MD5()          | 返回字符串str的MD5值     |
