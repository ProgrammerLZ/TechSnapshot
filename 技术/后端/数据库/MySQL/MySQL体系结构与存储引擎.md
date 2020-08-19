## 体系结构

* MySQL Server
  * 连接层
    * 通信协议
    * 线程处理
    * 用户名密码认证
  * SQL层
    * 权限判断
    * 查询缓存（Query Cache）
    * 解析器
    * 预处理
    * 查询优化器
    * 缓存
    * 执行计划
* 存储引擎

<img src="MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.assets/%E6%88%AA%E5%B1%8F2020-07-28%20%E4%B8%8B%E5%8D%881.26.20.png" alt="截屏2020-07-28 下午1.26.20" style="zoom:50%;" />

## 存储引擎

### 主要的存储引擎

* InnoDB
* MyISAM
* Memory
* blackhole
* TokuDB
* MariaDB columnstore

<img src="MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.assets/Snip20200728_2.png" alt="Snip20200728_2" style="zoom:50%;" />

<img src="MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.assets/Snip20200728_3-5916604.png" alt="Snip20200728_3" style="zoom:50%;" />

#### MyISAM和InnoDB

<img src="MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.assets/image-20200728141202852.png" alt="image-20200728141202852" style="zoom:50%;" />



## InnoDB存储引擎

### 体系结构

InooDB的体系结构由以下三层组成

* 内存结构

- 线程
- 磁盘文件

<img src="MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.assets/image-20200728141627097.png" alt="image-20200728141627097" style="zoom: 33%;" />



### 存储结构

存储单元的划分

* 表空间
* 段
* 区（64个page，1MB）
* 页

<img src="MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.assets/image-20200728142632113.png" alt="image-20200728142632113" style="zoom:50%;" />

#### 表空间

InnoDB存储引擎表中所有的数据都存储在表空间当中。表空间可分为

* 系统表空间
* 独立表空间
* 临时表空间（version > MySQL 5.7）
* 通用表空间（version > MySQL 5.7）

**系统表空间**以`ibdata1`来命名，安装数据库初始化数据时系统会创建一个名为ibdata1的表空间文件。默认大小为10MB。用于存储所有的**数据信息以及回滚段**。

**独立表空间**可通过`innodb_file_per_tale=1`进行配置。每个表都拥有自己独立的表空间。用于存储对应表的数据、索引和插入缓冲等信息。

<img src="MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.assets/image-20200728151213810.png" alt="image-20200728151213810" style="zoom:50%;" />

.frm文件用于存放表结构的相关信息。



### 内存结构

MySQL的内存组成分为：

* SGA（系统全局区）
* PGA（程序缓存区）



#### 系统全局区

















