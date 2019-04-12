## MySQL单表多表查询

### SQL通配符

通配符可用于替代字符串中的任何其他字符。

在 SQL 中，通配符与 SQL LIKE 操作符一起使用。

SQL 通配符用于搜索表中的数据。

在 SQL 中，可使用以下通配符：

| 通配符                       | 描述                       |
| ---------------------------- | -------------------------- |
| %                            | 替代 0 个或多个字符        |
| _                            | 替代一个字符               |
| [*charlist*]                 | 字符列中的任何单一字符     |
| [^*charlist*]或[!*charlist*] | 不在字符列中的任何单一字符 |



## MySQL 约束

SQL 约束用于规定表中的数据规则。

如果存在违反约束的数据行为，行为会被约束终止。

约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。

#### SQL CREATE TABLE + CONSTRAINT 语法

CREATE TABLE *table_name*
(
*column_name1 data_type*(*size*) *constraint_name*,
*column_name2 data_type*(*size*) *constraint_name*,
*column_name3 data_type*(*size*) *constraint_name*,
....
);

在 SQL 中，我们有如下约束：

- **NOT NULL** - 指示某列不能存储 NULL 值。
- **UNIQUE** - 保证某列的每行必须有唯一的值。
- **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- **CHECK** - 保证列中的值符合指定的条件。
- **DEFAULT** - 规定没有给列赋值时的默认值。



## SQL 通用数据类型

数据库表中的每个列都要求有名称和数据类型。Each column in a database table is required to have a name and a data type.

SQL 开发人员必须在创建 SQL 表时决定表中的每个列将要存储的数据的类型。数据类型是一个标签，是便于 SQL 了解每个列期望存储什么类型的数据的指南，它也标识了 SQL 如何与存储的数据进行交互。

下面的表格列出了 SQL 中通用的数据类型：

| 数据类型                          | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| CHARACTER(n)                      | 字符/字符串。固定长度 n。                                    |
| VARCHAR(n) 或CHARACTER VARYING(n) | 字符/字符串。可变长度。最大长度 n。                          |
| BINARY(n)                         | 二进制串。固定长度 n。                                       |
| BOOLEAN                           | 存储 TRUE 或 FALSE 值                                        |
| VARBINARY(n) 或BINARY VARYING(n)  | 二进制串。可变长度。最大长度 n。                             |
| INTEGER(p)                        | 整数值（没有小数点）。精度 p。                               |
| SMALLINT                          | 整数值（没有小数点）。精度 5。                               |
| INTEGER                           | 整数值（没有小数点）。精度 10。                              |
| BIGINT                            | 整数值（没有小数点）。精度 19。                              |
| DECIMAL(p,s)                      | 精确数值，精度 p，小数点后位数 s。例如：decimal(5,2) 是一个小数点前有 3 位数小数点后有 2 位数的数字。 |
| NUMERIC(p,s)                      | 精确数值，精度 p，小数点后位数 s。（与 DECIMAL 相同）        |
| FLOAT(p)                          | 近似数值，尾数精度 p。一个采用以 10 为基数的指数计数法的浮点数。该类型的 size 参数由一个指定最小精度的单一数字组成。 |
| REAL                              | 近似数值，尾数精度 7。                                       |
| FLOAT                             | 近似数值，尾数精度 16。                                      |
| DOUBLE PRECISION                  | 近似数值，尾数精度 16。                                      |
| DATE                              | 存储年、月、日的值。                                         |
| TIME                              | 存储小时、分、秒的值。                                       |
| TIMESTAMP                         | 存储年、月、日、小时、分、秒的值。                           |
| INTERVAL                          | 由一些整数字段组成，代表一段时间，取决于区间的类型。         |
| ARRAY                             | 元素的固定长度的有序集合                                     |
| MULTISET                          | 元素的可变长度的无序集合                                     |
| XML                               | 存储 XML 数据                                                |



## MySQL 数据类型

在 MySQL 中，有三种主要的类型：Text（文本）、Number（数字）和 Date/Time（日期/时间）类型。

**Text 类型：**

| 数据类型         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| CHAR(size)       | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。 |
| VARCHAR(size)    | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。**注释：**如果值的长度大于 255，则被转换为 TEXT 类型。 |
| TINYTEXT         | 存放最大长度为 255 个字符的字符串。                          |
| TEXT             | 存放最大长度为 65,535 个字符的字符串。                       |
| BLOB             | 用于 BLOBs（Binary Large OBjects）。存放最多 65,535 字节的数据。 |
| MEDIUMTEXT       | 存放最大长度为 16,777,215 个字符的字符串。                   |
| MEDIUMBLOB       | 用于 BLOBs（Binary Large OBjects）。存放最多 16,777,215 字节的数据。 |
| LONGTEXT         | 存放最大长度为 4,294,967,295 个字符的字符串。                |
| LONGBLOB         | 用于 BLOBs (Binary Large OBjects)。存放最多 4,294,967,295 字节的数据。 |
| ENUM(x,y,z,etc.) | 允许您输入可能值的列表。可以在 ENUM 列表中列出最大 65535 个值。如果列表中不存在插入的值，则插入空值。**注释：**这些值是按照您输入的顺序排序的。可以按照此格式输入可能的值： ENUM('X','Y','Z') |
| SET              | 与 ENUM 类似，不同的是，SET 最多只能包含 64 个列表项且 SET 可存储一个以上的选择。 |

**Number 类型：**

| 数据类型        | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| TINYINT(size)   | 带符号-128到127 ，无符号0到255。                             |
| SMALLINT(size)  | 带符号范围-32768到32767，无符号0到65535, size 默认为 6。     |
| MEDIUMINT(size) | 带符号范围-8388608到8388607，无符号的范围是0到16777215。 size 默认为9 |
| INT(size)       | 带符号范围-2147483648到2147483647，无符号的范围是0到4294967295。 size 默认为 11 |
| BIGINT(size)    | 带符号的范围是-9223372036854775808到9223372036854775807，无符号的范围是0到18446744073709551615。size 默认为 20 |
| FLOAT(size,d)   | 带有浮动小数点的小数字。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DOUBLE(size,d)  | 带有浮动小数点的大数字。在 size 参数中规显示定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DECIMAL(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |

> **注意：**以上的 size 代表的并不是存储在数据库中的具体的长度，如 int(4) 并不是只能存储4个长度的数字。
>
> 实际上int(size)所占多少存储空间并无任何关系。int(3)、int(4)、int(8) 在磁盘上都是占用 4 btyes 的存储空间。就是在显示给用户的方式有点不同外，int(M) 跟 int 数据类型是相同的。
>
> 例如：
>
> 1、int的值为10 （指定zerofill）
>
> ```
> int（9）显示结果为000000010
> int（3）显示结果为010
> ```
>
> 就是显示的长度不一样而已 都是占用四个字节的空间

**Date 类型：**

| 数据类型    | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| DATE()      | 日期。格式：YYYY-MM-DD**注释：**支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()  | *日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP() | *时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的秒数来存储。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS**注释：**支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()      | 2 位或 4 位格式的年。**注释：**4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

*即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。



## SQL 快速参考

| SQL 语句        | 语法                                                         |
| --------------- | ------------------------------------------------------------ |
| AND / OR        | SELECT column_name(s)FROM table_nameWHERE conditionAND\|OR condition |
| ALTER TABLE     | ALTER TABLE table_name ADD column_name datatypeorALTER TABLE table_name DROP COLUMN column_name |
| AS (alias)      | SELECT column_name AS column_aliasFROM table_nameorSELECT column_nameFROM table_name AS table_alias |
| BETWEEN         | SELECT column_name(s)FROM table_nameWHERE column_nameBETWEEN value1 AND value2 |
| CREATE DATABASE | CREATE DATABASE database_name                                |
| CREATE TABLE    | CREATE TABLE table_name(column_name1 data_type,column_name2 data_type,column_name2 data_type,...) |
| CREATE INDEX    | CREATE INDEX index_nameON table_name (column_name)orCREATE UNIQUE INDEX index_nameON table_name (column_name) |
| CREATE VIEW     | CREATE VIEW view_name ASSELECT column_name(s)FROM table_nameWHERE condition |
| DELETE          | DELETE FROM table_nameWHERE some_column=some_valueorDELETE FROM table_name (**Note:** Deletes the entire table!!)DELETE * FROM table_name (**Note:** Deletes the entire table!!) |
| DROP DATABASE   | DROP DATABASE database_name                                  |
| DROP INDEX      | DROP INDEX table_name.index_name (SQL Server)DROP INDEX index_name ON table_name (MS Access)DROP INDEX index_name (DB2/Oracle)ALTER TABLE table_nameDROP INDEX index_name (MySQL) |
| DROP TABLE      | DROP TABLE table_name                                        |
| GROUP BY        | SELECT column_name, aggregate_function(column_name)FROM table_nameWHERE column_name operator valueGROUP BY column_name |
| HAVING          | SELECT column_name, aggregate_function(column_name)FROM table_nameWHERE column_name operator valueGROUP BY column_nameHAVING aggregate_function(column_name) operator value |
| IN              | SELECT column_name(s)FROM table_nameWHERE column_nameIN (value1,value2,..) |
| INSERT INTO     | INSERT INTO table_nameVALUES (value1, value2, value3,....)*or*INSERT INTO table_name(column1, column2, column3,...)VALUES (value1, value2, value3,....) |
| INNER JOIN      | SELECT column_name(s)FROM table_name1INNER JOIN table_name2 ON table_name1.column_name=table_name2.column_name |
| LEFT JOIN       | SELECT column_name(s)FROM table_name1LEFT JOIN table_name2 ON table_name1.column_name=table_name2.column_name |
| RIGHT JOIN      | SELECT column_name(s)FROM table_name1RIGHT JOIN table_name2 ON table_name1.column_name=table_name2.column_name |
| FULL JOIN       | SELECT column_name(s)FROM table_name1FULL JOIN table_name2 ON table_name1.column_name=table_name2.column_name |
| LIKE            | SELECT column_name(s)FROM table_nameWHERE column_name LIKE pattern |
| ORDER BY        | SELECT column_name(s)FROM table_nameORDER BY column_name [ASC\|DESC] |
| SELECT          | SELECT column_name(s)FROM table_name                         |
| SELECT *        | SELECT *FROM table_name                                      |
| SELECT DISTINCT | SELECT DISTINCT column_name(s)FROM table_name                |
| SELECT INTO     | SELECT *INTO new_table_name [IN externaldatabase]FROM old_table_name*or*SELECT column_name(s)INTO new_table_name [IN externaldatabase]FROM old_table_name |
| SELECT TOP      | SELECT TOP number\|percent column_name(s)FROM table_name     |
| TRUNCATE TABLE  | TRUNCATE TABLE table_name                                    |
| UNION           | SELECT column_name(s) FROM table_name1UNIONSELECT column_name(s) FROM table_name2 |
| UNION ALL       | SELECT column_name(s) FROM table_name1UNION ALLSELECT column_name(s) FROM table_name2 |
| UPDATE          | UPDATE table_nameSET column1=value, column2=value,...WHERE some_column=some_value |
| WHERE           | SELECT column_name(s)FROM table_nameWHERE column_name operator value |



## MySQL导入导出

### MySQL 导出数据

MySQL中你可以使用**SELECT...INTO OUTFILE**语句来简单的导出数据到文本文件上。

#### 使用 SELECT ... INTO OUTFILE 语句导出数据

以下实例中我们将数据表 runoob_tbl 数据导出到 /tmp/tutorials.txt 文件中:

```sql
mysql> SELECT * FROM runoob_tbl 
    -> INTO OUTFILE '/tmp/tutorials.txt';
```

你可以通过命令选项来设置数据输出的指定格式，以下实例为导出 CSV 格式：

```sql
mysql> SELECT * FROM passwd INTO OUTFILE '/tmp/tutorials.txt'
    -> FIELDS TERMINATED BY ',' ENCLOSED BY '"'
    -> LINES TERMINATED BY '\r\n';
```

在下面的例子中，生成一个文件，各值用逗号隔开。这种格式可以被许多程序使用。

```sql
SELECT a,b,a+b INTO OUTFILE '/tmp/result.text'
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
FROM test_table;
```

#### SELECT ... INTO OUTFILE 语句有以下属性:

- LOAD DATA INFILE是SELECT ... INTO OUTFILE的逆操作，SELECT句法。为了将一个数据库的数据写入一个文件，使用SELECT ... INTO OUTFILE，为了将文件读回数据库，使用LOAD DATA INFILE。
- SELECT...INTO OUTFILE 'file_name'形式的SELECT可以把被选择的行写入一个文件中。该文件被创建到服务器主机上，因此您必须拥有FILE权限，才能使用此语法。
- 输出不能是一个已存在的文件。防止文件数据被篡改。
- 你需要有一个登陆服务器的账号来检索文件。否则 SELECT ... INTO OUTFILE 不会起任何作用。
- 在UNIX中，该文件被创建后是可读的，权限由MySQL服务器所拥有。这意味着，虽然你就可以读取该文件，但可能无法将其删除。

#### 导出表作为原始数据

mysqldump是mysql用于转存储数据库的实用程序。它主要产生一个SQL脚本，其中包含从头重新创建数据库所必需的命令CREATE TABLE INSERT等。

使用mysqldump导出数据需要使用 --tab 选项来指定导出文件指定的目录，该目标必须是可写的。

以下实例将数据表 runoob_tbl 导出到 /tmp 目录中：

```shell
$ mysqldump -u root -p --no-create-info \
            --tab=/tmp RUNOOB runoob_tbl
password ******
```

#### 导出SQL格式的数据

导出SQL格式的数据到指定文件，如下所示：

```shell
$ mysqldump -u root -p RUNOOB runoob_tbl > dump.txt
password ******
```

以上命令创建的文件内容如下：

```sql
-- MySQL dump 8.23
--
-- Host: localhost    Database: RUNOOB
---------------------------------------------------------
-- Server version       3.23.58

--
-- Table structure for table `runoob_tbl`
--

CREATE TABLE runoob_tbl (
  runoob_id int(11) NOT NULL auto_increment,
  runoob_title varchar(100) NOT NULL default '',
  runoob_author varchar(40) NOT NULL default '',
  submission_date date default NULL,
  PRIMARY KEY  (runoob_id),
  UNIQUE KEY AUTHOR_INDEX (runoob_author)
) TYPE=MyISAM;

--
-- Dumping data for table `runoob_tbl`
--

INSERT INTO runoob_tbl 
       VALUES (1,'Learn PHP','John Poul','2007-05-24');
INSERT INTO runoob_tbl 
       VALUES (2,'Learn MySQL','Abdul S','2007-05-24');
INSERT INTO runoob_tbl 
       VALUES (3,'JAVA Tutorial','Sanjay','2007-05-06');
```

如果你需要导出整个数据库的数据，可以使用以下命令：

```shell
$ mysqldump -u root -p RUNOOB > database_dump.txt
password ******
```

如果需要备份所有数据库，可以使用以下命令：

```shell
$ mysqldump -u root -p --all-databases > database_dump.txt
password ******
```

--all-databases 选项在 MySQL 3.23.12 及以后版本加入。

该方法可用于实现数据库的备份策略。

#### 将数据表及数据库拷贝至其他主机

如果你需要将数据拷贝至其他的 MySQL 服务器上, 你可以在 mysqldump 命令中指定数据库名及数据表。

在源主机上执行以下命令，将数据备份至 dump.txt 文件中:

```shell
$ mysqldump -u root -p database_name table_name > dump.txt
password *****
```

如果完整备份数据库，则无需使用特定的表名称。

如果你需要将备份的数据库导入到MySQL服务器中，可以使用以下命令，使用以下命令你需要确认数据库已经创建：

```shell
$ mysql -u root -p database_name < dump.txt
password *****
你也可以使用以下命令将导出的数据直接导入到远程的服务器上，但请确保两台服务器是相通的，是可以相互访问的：</p>
$ mysqldump -u root -p database_name \
       | mysql -h other-host.com database_name
```

以上命令中使用了管道来将导出的数据导入到指定的远程主机上。.



## 问题区

- 关于LocalHost的意义是什么？
- 127.0.0.1这个IP地址的意义是什么？
- 什么是表结构、索引、属性？

