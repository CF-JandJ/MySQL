[toc]

# MySQL简介

数据库：保持有组织的数据的容器

> 数据库软件称为DBMS（数据库管理系统），数据库是通过DBMS创建和操纵的容器。

表：某种特定类型数据的结构化清单

> 存储在表中的数据是一种类型的数据或一个清单。

数据库中每个表都有一个名字，称为表名。在同一数据库中，表名是唯一的，但是在不同的数据库中，可以使用相同的表名。

模式：关于数据库和表的布局及特性的信息

列：表中的一个字段，所有表都是由一个或多个列组成的。

> 分解数据：正确地将数据分解成多个列极其重要，按照其特性不同点分


数据类型：所容许的数据的类型。每个表列都有相应的数据类型，它限制该列中存储的数据。

> 数据类型可以限制存储在列中的数据种类，还可以帮助正确地排序，所以创建表的时候，必须对数据类型给予特别的关注。

行：表中的数据是按行存储的，所保存的每个记录存储在自己的行内。

> 行又可称数据库记录。

主键：每一行都应该有可以**唯一标识**自己的一列。

> 应该总是定义主键：便于以后的数据操纵和管理

主键值规则：1. 任意两行都不具有相同的主键值  2. 每个行都必须具有一个主键值（主键列不允许NULL值）

> 也可以一起使用多个列作为主键。在使用多个列作为主键时，上述条件必须应用到构成主键的所有列，所有列值的组合必须是唯一的（此时单个列的值可以不唯一）


主键的最好习惯：

1. 不更新主键列中的值
2. 不重用主键列的值
3. 不在主键列中使用可能会更改的值（例如，如果使用一个名字作为主键以标识某个供应商，当该供应商合并和更改其名字时，必须更改这个主键）


SQL：结构化查询语言，是一种专门用来与数据库通信的语言。

> SQL的目的能提供一种从数据库中读写数据的简单有效的方法

DBMS(数据库管理系统):数据的所有存储、检索、管理和处理实际上是由数据库软件-DBMS完成的

DBMS分成两类：1. 基于共享文件系统的DBMS，另一类为基于客户机-服务器的DBMS。

客户机-服务器应用分为两个不同的部分。
服务器部分：负责所有数据访问和处理的一个软件。这个软件运行在称为数据库服务器的计算器上。

与数据文件打交道的只有服务器软件。关于数据、数据添加、删除和数据更新的所有请求都由服务器软件完成。这些请求或更改来自运行客户机软件的计算机。客户机是与用户打交道的软件。

## MySQL的三层结构 - 破除神秘

![image.png](https://note.youdao.com/yws/res/14647/WEBRESOURCE3f5f4d85fd197adaf9dd7f6f2a4711b2)

## SQL语句分类

DDL：数据定义语句[create 表，库...]
DML：数据操作语句[增加 insert，修改 update,删除 delete]
DQL：数据查询语句[selet]
DCL：数据控制语句[管理数据库：比如用户权限 grand revoke]

# 使用Mysql

## 启动mysql数据库的常用方式

net stop mysql 服务器名
net start mysql 服务器名

## 连接到Mysql服务（Mysql数据库）的指令

mysql -h 主机名 -P 端口 -u 用户名 -p密码

注意-p密码，中间没有空格

没有写-h 主机，默认为本机
没有写-P 端口，默认为3306

- 命令输入在`mysql>`之后
- 命令用`;`或`\g`结束，仅按`Enter`不执行命令
- 输入`help`或`\h`获得帮助
- 输入`quit`或`exit`退出命令行实用程序

关键字：作为Mysql语言组成部分的一个保留字。绝不要用关键字命名一个表或列。

## 数据库

### 创建数据库

```
CREATE DATABASE [IF NOT EXISTS] db_name [create_specification [,create_specification] ....]

create_specification：

[DEFAULT] CHARACTER SET charset_name
[DEFAULT] COLLATE collation_name

// CHARACTER SET:指定数据库采用的字符集，如果不指定，默认为utf8
// COLLATE:指定数据库字符集的校对规则（常用的utf8_bin[不区分大小写]、utf8_general_ci[区分大小写] 注意：默认的是 utf8_general_ci）

```

在创建数据库/表的时候，为了规避关键字，可以使用反引号` `` `解决。

### 显示可用的数据库列表：
`show databases;`返回可用数据库的一个列表

![image.png](https://note.youdao.com/yws/res/13057/WEBRESOURCE3941d490268cf2cf1fb304159cfb7194)

### 显示数据库创建语句

```
SHOW CREATE DATABASE db_name
```

### 数据库删除语句

```
DROP DATABASE [IF EXISTS] db_name
```

### 使用一个数据库：
`use` 数据库名

![image.png](https://note.youdao.com/yws/res/13059/WEBRESOURCE18da2e6a7b88425f4387a2ba5dac4b8c)

为了获得一个数据库内的表的列表，使用`show tables;`
`show tables;`:返回当前选择的数据库内可用表的列表（即把数据库内所有的表都列出来）

![image.png](https://note.youdao.com/yws/res/13121/WEBRESOURCE2d121d9cbba49c1cface9ae6c4ab6754)

用来显示某个表的表列：
`show columns from xxxx;`
![image.png](https://note.youdao.com/yws/res/13124/WEBRESOURCEd071e9a16b9a04b3dcf4b5f0751ecff6)

> 注意：`describe`语句可用替代`show columns from`

![image.png](https://note.youdao.com/yws/res/13127/WEBRESOURCE74694baa6bd4e6811f457399da59be50)

自动增量：表列需要唯一值，在每个行添加到表中时，MySQL可用自动为每个行分配下一个可用编号，不用在添加一行时手动分配唯一值。如果需要它，则必须在`CREATE`语句创建表时把它作为表定义的组成部分。

其他SHOW语句：

- SHOW STATUE 用于显示广泛的服务器状态信息；
- SHOW CREATE DATABASE和SHOW CREATE TABLE，分别用来显示创建特定数据库或表的MySQL语句
- SHOW GRANTS，用来显示授予用户的安全权限
- SHOW ERRORS和SHOW WARNINGS,用来显示服务器错误或警告消息


### 备份恢复数据库

备份数据库（注意：在DOS执行）命令行

```
mysqldump -u 用户名 -p(密码) -B 数据库1 数据库2 数据库n > 文件名.sql
```

备份数据库的表

```
mysqldump -u 用户名 -p密码 数据库 表1 表2 表n > d::\\文件名.sql
```

恢复数据库(注意：进入Mysql命令行再执行)

```
Source 文件名.sql
```


## 表

### 创建表

![image.png](https://note.youdao.com/yws/res/14786/WEBRESOURCE214a6bcf98d04cbb2245e45670fc40b6)

注意如果在类型后面加了`not null`，则表明默认不为空


### Mysql 常用数据类型（列类型）

![image.png](https://note.youdao.com/yws/res/14794/WEBRESOURCEcaa417ffdb15e6943b60f219a42995db)

#### 字符串的基本使用

`char(size)`

固定长度字符串，最大255字符

`varchar(size)` 

可变长度字符串，最大65532字节 （utf8编码最大21844字符，因为utf8中3个字节等于1个字符，1-3个字节用于记录大小）


> 注意：

字符不区分是字母还是汉字

`char(4)`是定长，即使插入`aa`，也会占用分配的4个字符的空间。
`varchar(4)`是变长，如果插入了`aa`，实际占用空间的大小并不是4个字符，而是按照实际占用空间来分配。

#### 日期类型的基本使用

```mysql
create table birthday6
(t1 date,t2 datetime,t3 timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP); 

//timestamp为时间戳
//如果加上后面那段，则会自动更新时间
//timestamp在insert和update时，自动更新datetime.sql
```


![image.png](https://note.youdao.com/yws/res/14854/WEBRESOURCE5e61f4e32e94ed7b9043da19260e0348)

### 修改表

![image.png](https://note.youdao.com/yws/res/15148/WEBRESOURCE7f64722a6083f597e82f38aef51113ca)

实例：

```sysql
//1.员工表emp上增加一个image列，varchar类型，且要求在resume后面

alter table emp
            add image varchar(32) not null default ''
            after resume ;

//2. 查看表的所有字段
desc emp;

//3.修改job列，使其长度为60
alter table emp 
            modify job varchar(60) not null default '';
            
//4.删除sex列
alter table emp 
            drop sex;
            
//5.将列名name修改为user_name
alter table employee
            change `name` user_name varchar(32) not null default '';

```

### 数据库的增，删，改，查

```
insert语句;添加数据
update语句；更新数据
delete语句；删除数据
select语句；查找数据
```

#### insert语句

![image.png](https://note.youdao.com/yws/res/15179/WEBRESOURCEc61c0639f83d6d46f6d4ec5483e41959)

```
//创建一张商品表goods(id int,goods_name varchar(10),price double);并且添加1个数据

create table `goods`(id int,goods_name varchar(10),price double);

//添加数据
insert into `goods`(id,goods_name,price) values(10,'手机',2000);

```

- insert语句注意事项

![image.png](https://note.youdao.com/yws/res/15205/WEBRESOURCEd040766319ddce12d48d3ba7eded4598)


```
//第八点
//如何设置某个参数是否默认不为空

create table `goods`(id int,goods_name varchar(10),price double not null);

//如何设置一个默认值呢
create table `goods`(id int,goods_name varchar(10),price double not null default 100);
```

#### update语句修改表中的数据

![image.png](https://note.youdao.com/yws/res/15217/WEBRESOURCE1e3d1b15ee3e857c19fdd4ff54006ddc)

![image.png](https://note.youdao.com/yws/res/15220/WEBRESOURCEd6640d8dbd309c019dddfe256d1898eb)

```
//1.[没有带where条件，会修改所有的记录]

update employee set salary = 5000;

//2.

update employee set salary = 3000 
                where user_name = '小妖怪';

//3.

update employee set salary = salary + 1000 
                where user_name = '老妖怪';

```

- update语句注意事项

![image.png](https://note.youdao.com/yws/res/15235/WEBRESOURCE3af56504e48289892b02c701124ba510)


```
4.

update employee set salary = salary + 1000 , job = 'xxx'
                where user_name = '老妖怪';
```


#### delete语句删除表中的数据

![image.png](https://note.youdao.com/yws/res/15245/WEBRESOURCEcff1b0134004e39e4d5512f6c3c7b30a)

= delete语句注意事项

1. 不加where条件，直接删除表中的所有内容。
2. delete语句不能删除某一列的值（可使用update设为null或者''）
3. 使用delete语句仅删除记录，不删除表本身。如要删除表，使用drop table语句。


```
// 删除表中名称为老妖怪的记录
delete from employee
       where user_name = '老妖怪';
       
// 删除表中所有记录
delete from employee;

```

### select语句

#### 基本语法

![image.png](https://note.youdao.com/yws/res/15259/WEBRESOURCE49600a68d6aa7c1bd5a0c825e8d32ddc)

![image.png](https://note.youdao.com/yws/res/15261/WEBRESOURCE3083ce24d656704bccd161e61e723b7c)

```
//统计每个学生的总分

select `name` , (chinese + engligh + math) from student;

//使用别名表示学生分数
select `name` as `名字` ,(chinese + english + math) as total_score from student;
```

![image.png](https://note.youdao.com/yws/res/15269/WEBRESOURCE6cb33db3a26162b7246fff20adbd46e9)

```
//查询姓名为赵云的学生成绩
select * from student where `name` = `赵云`;

//查询英语成绩大于90分的同学
select * from student where english > 90;

//查询总分大于200分的所有同学
selece * from student where (chinese + english + math) > 200;

//查询math大于60并且id大于4的学生成绩
select * from student where math>60 and id >4;

//查询英语成绩大于语文成绩的同学
select * from student where english > chinese;

//查询总分大于200分并且数学成绩小于语文成绩的姓韩的学生
select * from student where (chinese + english + math) > 200
and math<chinese and `name` like '韩%';

//需要注意`name`和'韩%'符号的不同

```

![image.png](https://note.youdao.com/yws/res/15286/WEBRESOURCEca3870b184b0fd410c201eba0d5b42a1)

```
//对数学成绩排序后输出升序
select * from student order by math asc;

//对总分从高到低的顺序降序输出
select `name` , (chinese + english + math) as total_score
from student order by total_score desc;

//对姓李的学生总成绩排序升序输出 where + order by
select `name` , (chinese + english + math) as total_score from student
where `name` like '李%' order by total_score;
```

#### 合计/统计函数 -count,sum,avg,max/min

![image.png](https://note.youdao.com/yws/res/15302/WEBRESOURCEf13d0b5fe9d30bce622d9e6359808d8c)

```
//统计一个班级共有多少学生
select count(*) from student;

//统计数学成绩大于90的学生有多少个
select count(*) from student where math > 90;

//统计总分大于250的人数有多少
select count(*) from student where (math + english + chinese) > 250;

// count(*)和count(列)的区别
count(*) 返回满足条件的记录的行数
count(列) 统计满足条件的某列有多少个，但是会排除为null

```

![image.png](https://note.youdao.com/yws/res/15316/WEBRESOURCE3df572dfb510d0e4114a902eaa09351f)

```
//统计一个班级数学总成绩
select sum(math) from student;

//统计一个班级语文、英语、数学各科的总成绩
select sum(math),sum(english),sum(chinese) from student;

//统计一个班级语文、英语、数学的成绩总和
select sum(math + chinese + english) from student;

//统计一个班级语文成绩平均分
select sum(chinese)/count(chinese) from student;

```

![image.png](https://note.youdao.com/yws/res/15325/WEBRESOURCE2e933b800de01be39f7bfb2902a0a8e9)


```
//求一个班级数学平均分
select avg(math) from student;

//求一个班级总分平均分
select avg(math + chinese + english) from student;

```

![image.png](https://note.youdao.com/yws/res/15330/WEBRESOURCEd009143680c041712ec5e97a736e702b)

```
//求班级最高分和最低分
select max(math+chinese+english),min(math+chinese+english) from student;

//求出班级数学最高最低分
select max(math),min(math) from student;

```

#### 分组统计

![image.png](https://note.youdao.com/yws/res/15337/WEBRESOURCE0c48cd24bb767cf83f513fe3d45ac13b)

```
//如何显示每个部门的平均工资和最高工资
select avg(sal) , max(sal) , deptno from emp group by deptno;

//显示每个部门的每种岗位的平均工资和最低工资
select avg(sal) , max(sal) , deptno , job from emp group by deptno , job;

//显示平均工资低于2000的部门号和它的平均工资
//1.显示各个部门的平均工资和部门号
//2.在1的结果基础上，进行过滤，保留 AVG(sal) < 2000
//3.使用别名进行过滤

select avg(sal) , deptno from emp group by deptno having avg(sal)<2000;

select avg(sal) as `avg_sal`, deptno from emp group by deptno having `avg_sal`<2000;

```

#### 字符串函数

![image.png](https://note.youdao.com/yws/res/15360/WEBRESOURCE88fb92e72d6f83a22690b593f88f4de6)


### 字符串函数

![image.png](https://note.youdao.com/yws/res/15444/WEBRESOURCE9ff610a4a9b3881ccbd46374f49557b8)

```
//CHAREST(str)返回字串字符集
SELECT CHARSET(ename) FROM emp;

//CONCAT(string2 [,....])连接字串，将多个列拼接成一列
SELECT CONCAT(ename, 'job is' , job) FROM emp;

//INSTR(string , substring)返回substring在string中出现的位置，没有返回0
SELECT INSTR('aabbccdd','cc') FROM DUAL;//dual 亚元表，系统表，可以作为测试表使用

//UCASE(string2) 转换成大写
SELECT UCASE(ename) FROM emp;

//LCASE(string2) 转换成小写
SELECT LCASE(ename) FROM emp;

//LEFT(string2,length) 从string2中的左边取length个字符
SELECT LEFT(ename,2) FROM emp;//同理右边取为RIGHT(string2,length)

//LENGTH(string) string长度[按照字节]
SELECT LENGTH(ename) FROM emp;

//REPLACE(str,search_str,replace_str)在str中用replace_str替换search_str
SELECT ename , REPLACE(job,'MANAGER','经理') FROM emp;

//STRCMP(string1,string2) 逐字符比较两字符大小
SELECT STRCMP('AAA','AAA') FROM DUAL;

//SUBSTRING(str,position [,length])从str的position开始[从1开始计算]，取length个字符
SELECT SUBSTRING(ename,1,2) FROM emp;

//LTRIM(string2) 去除前端空格
//RTRIM(string2) 去除后端空格
//trim           去除两端空格
SELECT LTRIM('  aa  ') FROM DUAL;

```

### 数学相关函数


![image.png](https://note.youdao.com/yws/res/15693/WEBRESOURCEb01d12913459bc5127f325de0be652ca)

```
//CONV(number2,from_base,to_base) 进制转换
//下面含义为 8是十进制的8，转为2进制输出
SELECT CONV(8,10,2) FROM DUAL;

//RAND([seed]) 返回随机数，其范围为[0,1]，如果加了种子，则相同种子每次产生的都一样
```

### 时间日期相关函数

![image.png](https://note.youdao.com/yws/res/15735/WEBRESOURCE21816db8643823cdb18587802b5cc3ac)


```
//CURRENT_STIMESTAMP() 获取当前时间戳
INSERT INTO mes VALUES(1,'北京新闻',CURRENT_STIMESTAMP());

//显示所有新闻信息，发布日期只显示日期，不显示时间
SELECT id，content, DATE(send_time) FROM mes;

//查询在10分钟内发布的新闻
SELECT * FROM mes WHERE DATE_ADD(send_time,INTERVAL 10 MINUTE) >= NOW();

//unix_timestamp():返回的是1970-1-1到现在的秒数
SELECT unix_timestamp() FROM DUAL;

//FROM_UNIXTIME():可以把一个unix_timestamp秒数（时间戳）转成指定格式的日期
// %Y-%m-%d 表示年月日格式
// %H:%i:%s 表示时分秒

```

### 加密和系统函数

![image.png](https://note.youdao.com/yws/res/15850/WEBRESOURCE3d0bb13a10466dc5030b51f8e8fd7a6a)


### 流程控制函数

![image.png](https://note.youdao.com/yws/res/15852/WEBRESOURCEbfaa0f65f6b9bd29bc47aaa4acbda457)

```
//查询emp表，如果comm是null,则显示0.0
//判断是否为空，要使用 is null，判断不为空，使用is not
//如果是具体的值，yy = xx
SELECT ename,IF(comm IS NULL,0.0,comm) FROM emp;

```

### 加强查询

- 时间直接对比
```
//查询1992.1.1后入职的员工
SELECT * FROM emp WHERE hiredate > '1992-01-01'//注意格式需要和原表相同
```

- 如何使用like操作符（模糊）

`%` :表示0到多个任意字符
`_` :表示单个任意字符

```
//如何显示首字符为s的员工姓名和工资
SELECT ename , sal FROM emp WHERE ename LIKE 'S%';

//如何显示第三个字符为大写O的所有员工的姓名和工资
SELECT ename , sal FROM emp WHERE ename LIKE '__O%';

//如何显示没有上级雇员的情况
SELECT * FROM emp WHERE mgr IS NULL;

//查询表的结构
DESC emp;
```

- order by子句

```
//如何按照工资的从底到高的顺序，显示雇员信息
SELECT * FROM emp ORDER BY sal;

//按照部门号升序，而雇员的工资降序排列，显示雇员信息
SELECT * FROM emp ORDER BY deptno ASC,sal DESC;
```

- 分页查询

```
//按雇员的id号升序取出，每页显示3条记录，请分别显示第一页，第二页，第三页

//基本语法：select ... limit start, rows 
//表示从start+1行开始取，取出rows行，start从0开始计算

//第一页
SELECT * FROM emp ORDER BY id LIMIT 0,3;
//第二页
SELECT * FROM emp ORDER BY id LIMIT 3,3;
//第三页
SELECT * FROM emp ORDER BY id LIMIT 6,3;
//第N页
SELECT * FROM emp ORDER BY id LIMIT (每页显示数)*（N-1），每页显示数

```

- 分组增强

group by 的使用

```
//显示每种岗位的雇员总数、平均工资
SELECT COUNT(*) ,AVG(sal) , job FROM emp GROUP BY job;

//显示雇员总数，以及获得补助的雇员数
//思路：获得补助的雇员数，就是comm列为非null, == count(列)，因为当改列为空时，不统计进去
SELECT COUNT(*) ,COUNT(comm) FROM emp;

//统计没有获得补助的雇员数
SELECT COUNT(*),COUNT(IF(comm IS NULL,1,NULL)) FROM emp;

SELECT COUNT(*),COUNT(*)-COUNT(comm) FROM emp;

//显示管理者的总人数
SELECT COUNT(DISTINCT mgr) FROM emp;

//显示雇员工资的最大差额
SELECT MAX(sal) - MIN(sal) FROM emp;

```

- 总结

如果select语句同时包含有group by,having,limit,order by，那么他们的顺序是group by,having,order by,limit。

```
SELECT column1,column2,column3....FROM table
       group by column
       having condition
       order by column
       limit start,rows; 
```

```
//请统计各个部门的平均工资，并且是大于1000的，并按照平均工资从高到低排序，取出前两行纪录
//分析：各个部门-group by，平均工资-avg，大于1000-having，从高到低排序-order by，取出前面两行-limit

SELECT deptno ,AVG(sal) AS avg_sal FROM emp
        GROUP BY deptno
        HAVING avg_sal > 1000
        ORDER BY avg_sal DESC
        LIMIT 0,2;
```

### 多表查询

多表查询是指基于两个和两个以上的表查询，在实际应用中，查询单个表可能不能满足需求。

> 显示雇员名，雇员工资以及所在部门的名字；雇员名，雇员工资来自emp表，部门的名字来自dept表。

如果查询为`SELECT * FROM emp,dept;`则：

将会导致第一张表的每一行和第二张表的每一行进行匹配，列合并，比如第一张表为m*n，第二张表为a*b，那么新表为ma*(n+b);
这样的多表查询默认处理返回的结果，称为笛卡尔集。

**解决这个多表的关键就是要写出正确的过滤条件where。**
`SELECT * FROM emp,dept WHERE emp.deptno = dept.deptno;`
->
`SELECT ename,sal,dname FROM emp,dept WHERE emp.deptno = dept.deptno; ` 

注：**多表查询的条件不能少于表的个数-1，否则会出现笛卡尔集**

```
//如何显示部门号为10的部门名、员工名和工资
SELECT ename,sal,dname,emp.deptno FROM emp,dept WHERE emp.deptno = dept.deptno AND emp.deptno =10;

//显示各个员工的姓名，工资，及其工资的级别
SELECT ename,sal,grade FROM emp,salgrade WHERE sal BETWEEN losal AND hisal;

//显示雇员名，雇员工资，以及所在部门的名字，并按照部门降序，薪水降序排
SELECT ename,sal,dname,emp.deptno FROM emp,dept 
       WHERE emp.deptno = dept.deptno order by emp.deptno desc,sal.desc; 
```

#### 自连接

自连接指在同一张表的连接查询（将同一张表看做两张表）

```
//显示公司员工的名字和他的上级的名字
//员工名字在emp表，上级的名字也在emp表，员工和上级的名字通过emp表的mgr列相互关联
SELECT worker.ename AS '职员名' , boss.ename AS '上级名' 
                    FROM emp AS worker,emp AS boss
                    WHERE worker.mgr = boss.empno;                                
```

#### 子查询

子查询是指嵌入在其他sql语句中的select语句，也叫嵌套查询。
单行子查询：只返回一行数据的子查询语句
多行子查询：返回多行数据的子查询，使用关键词`in`

```
//如何显示与SMITH同一部门的所有员工？
//1.先查询到SMITH的部门号
//2.把上面的select语句当成一个子查询来使用

SELECT deptno FROM emp WHERE ename = `SMITH`;

SELECT * FROM emp WHERE deptno = (
         SELECT deptno FROM emp WHERE ename = `SMITH`);
```

```
//如何查询和部门10的工作相同的雇员的名字、岗位、工资、部门号，但是不含10部门自己的雇员
//查询到10号部门有哪些工作
//把上面查询的结果当做子查询使用

SELECT DISTINCT job FROM emp WHERE deptno = 10;

SELECT ename,job,sal,deptno FROM emp WHERE job 
                            IN (SELECT DISTINCT job FROM emp WHERE deptno = 10);
                            AND deptno != 10;
```

子查询当做临时表使用。

```
//查询ecshop各个类别中，价格最高的商品

SELECT cat_id,MAX(shop_price) AS max_price FROM ecs_goods GROUP BY cat_id;

SELECT goods_id, ecs_goods.cat_id ,goods_name ,shop_price
        FROM （SELECT cat_id, MAX(shop_price) AS max_price FROM ecs_goods GROUP BY cat_id）
        AS temp，ecs_goods
        WHERE temp.cat_id = ecs_goods.cat_id
        AND temp.max_price = ecs_goods.shop_price; 
       
```

在多行子查询中使用all操作符

```
//显示工资比部门30的所有员工的工资高的员工的姓名、工资和部门号

SELECT ename ,sal ,deptno from emp where sal>all 
                                   (select sal from emp where deptno =30);

select ename ,sal ,deptno from emp where sal > (select max(sal) from emp where deptno =30); 

```

在多行子查询中使用any操作符

```
//如何显示工资比部门30的其中一个员工的工资高的员工的姓名、工资和部门号

SELECT ename,sal,deptno FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno =30);

SELECT ename,sal,deptno FROM emp WHERE sal > (SELECT MIN(sal) FROM emp WHERE deptno =30);
```

#### 多列子查询

多列子查询则是指查询返回多个列数据的子查询语句。

```
//查询与smith的部门和岗位完全相同的所有雇员（而且不含smith本人）
//(字段1，字段2，....) = （select 字段1，字段2 from ..... ）

//1.得到smith的部门和岗位
SELECT deptno ,job FROM emp WHERE ename = 'SMITH';

//2.把上面的查询当做子查询来使用，并且使用多列子查询的语法进行匹配
SELECT * FROM emp WHERE (deptno , job) = 
        (SELECT deptno ,job FROM emp WHERE ename = 'SMITH')
         AND ename != 'SMITH'; 
```

#### 习题

```
//查询每个部门工资高于本部门平均工资的人的资料

//1.先得到每个部门的部门号和平均工资
SELECT deptno,AVG(sal) AS avg_sal FROM emp GROUP BY deptno;

//2.把上面的结果当做子查询，和emp 进行多表查询
SELECT  ename,sal,temp.avg_sal,emp.deptno
        FROM emp,(SELECT deptno,AVG(sal) AS avg_sal FROM emp GROUP BY deptno)AS temp
        WHERE emp.deptno = temp.deptno
        AND emp.sal > temp.avg_sal;
```


```
//查找每个部门工资最高的人的详细资料

//1.找到每个部门的部门号和最高工资
SELECT deptno,MAX(sal) AS avg_sal FROM emp GROUP BY deptno;

//2.把上面的结果当做子查询，和emp 进行多表查询
SELECT  ename,sal,temp.avg_sal,emp.deptno
        FROM emp,(SELECT deptno,MAX(sal) AS avg_sal FROM emp GROUP BY deptno)AS temp
        WHERE emp.deptno = temp.deptno
        AND emp.sal = temp.avg_sal;
```

```
//3.显示每个部门的信息（包括部门号，编号，地址）和人员数量
select dept.deptno,dname,loc,num 
       from dept,(select deptno,count(*)as num 
       from emp group by deptno)as temp where dept.deptno = temp.deptno;
       
//还有一种写法，表.*表示将该表所有列都显示出来
select tmp.* , dname ,loc
               form dept ,(select count(*) as per_num , deptno from emp group by deptno) tmp
               where tmp.deptno = dept.deptno;
```



### 表复制

```
//把emp表的记录复制到my_tab01中
INSERT INTO my_tab01 (id,`name`,sal,job,deptno)
       SELECT empno,ename,sal,job,deptno FROM emp;
//自我复制
INSERT INTO my_tab01 SELECT * FROM my_tab01;

//将emp表的结构复制到my_tab02上
CREARE TABLE my_tab02 LIKE emp;

//如何删除掉一张表重复记录
//思路
//1.先创建一张临时my_tmp 表，该表的结构和my_tab02一样
CREATE TABLE my_tmp LIKE my_tab02;
//2.把my_tab02 的记录通过distinct关键字处理后，把记录复制到my_tmp
INSERT INTO my_tmp SELECT DISTINCT * FROM my_tab02;
//3.清除掉my_tab02记录
DELEDT FROM my_tab02;
//4.把my_tmp表的记录复制到my_tab02
INSERT INTO my_tab02 SELECT * FROM my_tmp;
//5.drop掉临时表my_tmp
DROP TABLE my_tmp;

```

### 合并查询

```
//合并查询
SELECT ename,sal,job FROM emp WHERE sal>2500;
SELECT ename,sal,job FROM emp WHERE job = 'MANAGER';

//union all 将两个查询结果合并，不会去重
SELECT ename,sal,job FROM emp WHERE sal>2500
UNION ALL
SELECT ename,sal,job FROM emp WHERE job = 'MANAGER';

//union 将两个查询结果合并，去重
SELECT ename,sal,job FROM emp WHERE sal>2500
UNION
SELECT ename,sal,job FROM emp WHERE job = 'MANAGER';
```

### 表外连接

多表查询，如果利用where子句对两张表或者多张表，形成的笛卡尔积进行筛选，根据关联条件，显示所有匹配的记录，匹配不上的，不显示

```
//列出部门名称和这些部门的员工名称和工作，同时要求显示出那些没有员工的部门

//如果使用该方法，则无法显示emp.deptno不存在的情况
SELECT dname,ename,job FROM emp,dept 
                       WHERE emp.deptno = dept.deptno 
                       ORDER BY dname;
```

外连接：
1.左外连接（左侧的表完全显示）即：左侧的表即使没有和右侧的表完全匹配的记录，也会显示出来

```
//基本语法
SELECT ..... FROM 表1 LEFT JOIN 表2 ON 条件  表1为左表，表2为右表
```

2.右外连接（右侧的表完全显示）

```
//基本语法
SELECT ..... FROM 表1 RIGHT JOIN 表2 ON 条件  表1为左表，表2为右表
```

```
//没有用外连接
SELECT `name`,stu.id,grade
       FROM stu,exam
       WHERE stu.id = exam.id;

//左外连接
SELECT `name`,stu.id,grade
       FROM stu LEFT JOIN exam ON stu.id = exam.id;
       
//右外连接
SELECT `name`,stu.id,grade
       FROM stu RIGHT JOIN exam ON stu.id = exam.id;
       
```

### 约束

约束用于确保数据库的数据满足特定的商业规则
在mysql中，约束包括：not null、unique、primary key、foreign key、check

- 主键

```
primary key(主键)的基本使用

字段名 字段类型 primary key
用于唯一的标示表行的数据，当定义主键约束后，该列不能重复

```

```
//表示id列是主键
CREATE TABLE t17
             (id INT PRIMARY KEY,
              `name` VARCHAR(32),
              email VARCHAR(32));
```

注意事项：

1. primary key 不能重复并且不能为null;
2. 一张表最多只能有一个主键，但可以是复合主键

```
CREATE TABLE t
       (id INT,`name` VARCHAR(32),email VARCHAR(32),PRIMARY KEY(id,`name`));
``` 

3. 主键的指定方式有两种：字段名 primary key ，在表定义最后写 primary key（列名）
4. 使用desc 表名，可以看到primary key的情况


- 非空

```
not null(主键)

如果在列上定义了not null，那么当插入数据时，必须为列提供数据

字段名 字段类型 not null
```

- 唯一

```
unique(唯一)

当定义了唯一约束后，该列值是不能重复的

字段名 字段类型 unique
```

注意事项：

1. 如果没有指定not null,则unique字段可以有多个null
2. 一张表可以有多个unique字段


- foreign key (外键)

用于定义主表和从表之间的关系：外键约束要定义在从表上，主表则必须具有主键约束
或者unique约束。当定义外键约束后，要求外键列数据必须在主表的主键列存在或为null

```
FOREIGN KEY(本表字段名) REFERENCES 主表名（主键名或unique字段名）
```


```
//创建主表my_class
CREATE TABLE my_class(id INT PRIMARY KEY,`name` VARCHAR(32) NOT NULL DEFAULT '');
//创建从表my_stu
CREATE TABLE my_stu(id INT PRIMARY KEY,`name` VARCHAR(32) NOT NULL DEFAULT '',
              class_id INT,FOREIGN KEY(class_id) REFERENCES my_class(id));
```

注意事项：

1. 外键指向的表的字段，要求是primary key 或者是 unique
2. 表的类型是innodb，这样的表才支持外键
3. 外键字段的类型要和主键字段的类型一致
4. 外键字段的值，必须在主键字段中出现过，或者为null
5. 一旦建立主外键的关系，数据不能随意删除

- check

用于强制行数据必须满足的条件，假定在sal列上定义了check约束，并要求sal列值在1000~2000之间，如果不在1000~2000之间，就会提示错误。

```
基本语法：列名 类型 check (check条件)

CREATE TABLE t23
       (id INT PRIMARY KEY,`name` VARCHAR(32),sex VARCHAR(6) CHECK (sex IN('man','woman')),
       sal DOUBLE CHECK (sal>1000 AND sal<2000));
```


习题：
```
//现有一个商店的数据库shop_db,记录客户及其购物情况，由下面三个表组成：
//商品goods(商品号goods_id,商品名goods_name,单价unitprice,商品类别category,供应商provider)
//客户customer(客户号customer_id，姓名name,住址address，电邮email，性别sex,身份证card_id);
//购买purchase(购买订单号order_id,客户号customer_id，商品号goods_id，购买数量nums)；
//建表，在定义中要求声明
//1.每个表的主外键
//2.客户的姓名不能为空值
//3.电邮不能重复
//4.客户的性别男/女
//5.单价unitprice 在1-1000之间


//goods
CREATE TABLE goods(
       goods_id INT PRIMARY KEY,
       goods_name VARCHAR(64) NOT NULL DEFAULT '',
       unitprice DECIMAL(10,2) NOT NULL DEFAULT 0
                               CHECK(unitprice >= 1.0 AND unitprice <= 1000),
       category INT NOT NULL DEFAULT 0,
       provider VARCHAR(64) NOT NULL DEFAULT '');
       
//customer
CREATE TABLE customer(
       customer_id INT PRIMARY KEY,
       `name` VARCHAR(64) NOT NULL DEFAULT '',
       address VARCHAR(64) NOT NULL DEFAULT '',
       email VARCHAR(64) UNIQUE NOT NULL,
       sex VARCHAR(6) CHECK (sex IN('男','女')) NOT NULL,
       card_id CHAR(18) UNIQUE);
       
//purchase
CREATE TABLE purchase(
       order_id INT PRIMARY KEY,
       customer_id INT NOT NULL DEFAULT 0,
       goods_id INT NOT NULL DEFAULT 0,
       nums INT NOT NULL DEFAULT 0,
       FOREIGN KEY(customer_id) REFERENCES customer(customer_id),
       FOREIGN KEY(goods_id) REFERENCES goods(goods_id));

```

### 自增长

在表中，存在一个id列，在添加记录时，该列从1开始自动的增长。

```

字段名  整形  primary key auto_increment

添加自增长的字段方式
insert into xxx (字段1，字段2....) values(null,'值'....)
insert into xxx (字段2....) values ('值1'，'值2'....)
insert into xxx values (null,'值1',....)

```


```
//创建表
CREATE TABLE t24(id INT PRIMARY KEY AUTO_INCREMENT,
                 email VARCHAR(32) NOT NULL DUFAULT '',
                 `name` VARCHAR(32) NOT NULL DUFAULT '');

//测试自增长的使用
INSERT INTO t24 VALUES(NULL,'jackaa','jack');
//此时，id为1
```

- 自增长使用细节
1. 一般来说，自增长是和primary key 配合使用
2. 自增长也可以单独使用（但是需要配合一个unique）
3. 自增长修饰的字段为整数型的
4. 自增长默认从1开始，也可以通过命令修改，alter table 表名 auto_increment = xxx;
5. 如果有指定具体的值，以指定的值为准


### 索引

1. 索引本身也会占用空间
2. 创建索引后，只对创建了索引的列有效

- 使用索引的原因

当我们没有索引，`select * from emp where id=1;`会进行全表扫描。
当使用索引时，形成一个索引的数据结构，比如二叉树索引

- 代价

磁盘占用，对（update delete insert）语句，会导致索引的数据结构发生改变，需要维护，对速度有影响。

#### 索引类型

- 主键索引，主键自动的为主索引(类型Primary Key)
- 唯一索引(UNIQUE)
- 普通索引(INDEX)
- 全文索引(FULLTEXT),实际开发中，全文搜索Solr和ElasticSearch(ES)


```
//创建
CREATE TABLE t25(
       id INT,`name` VARCHAR(32));

//查询表是否有索引
//方法1
SHOW INDEX FROM t25;
//方法2
SHOW INDEXES FROM t25;
//方法3
SHOW KEYS FROM t25;
//方法4
DESC t25;

//添加索引
//添加唯一索引
CREATE UNIQUE INDEX id_index ON t25 (id);
//添加普通索引方法1
CREATE INDEX id_index ON t25 (id);
//添加普通索引方法2
ALTER TABLE t25 ADD INDEX id_index(id);
//添加主键索引方法1：建表的时候直接添加
CREATE TABLE t25(id INT PRIMARY KEY,`name` VARCHAR(32));
//添加主键索引方法2：建完表后添加
ALTER TABLE t25 ADD PRIMARY KEY(id);

//删除索引
DROP INDEX id_index ON t25;

//删除主键索引
ALTER TABLE t26 DROP PRIMARY KEY;

//修改索引：先删除后添加新的索引

```

- 哪些列适合使用索引

1. 较频繁的作为查询条件字段应该创建索引
2. 唯一性太差的字段不适合单独创建索引，即使频繁作为查询条件
3. 更新非常频繁的列不适合创建索引
4. 不会出现在WHERE字句中的字段不该创建索引

### 事务

- 什么是事务？

事务用于保证数据的一致性，它由一组相关的dml语句组成，该组的dml语句要么全部成功，要么全部失败。如：转账就要用事务来处理，用以保证数据的一致性。

> dml语句：update,insert,delete

- 事务和锁

当执行事务操作时，mysql会在表上加锁，防止其他用户该表的数据。

mysql 数据库控制台事务的几个重要操作

1. start transaction - 开始一个事务
2. savepoint 保存点名 - 设置保存点
3. rollback to 保存点名 - 回退事务
4. rollback - 回退全部事务
5. commit - 提交事务，所有的操作生效，不能回退

```
//1.创建一张测试表
CREATE TABLE t27 (id INT,`name` VARCHAR(32));
//2.开始事务
START TRANSACTION;
//3.设置保存点
SAVEPOINT a;
//4.执行dml操作
INSERT INTO t27 VALUES(100,'TOM');
//5.设置保存点
SAVEPOINT b;
//6.执行dml操作
INSERT INTO t27 VALUES(200,'JACK');
//7.回退到b
ROLLBACK TO b;

```

> 提交事务

使用commit 语句可以提交事务，当执行了commit 语句后，会确认事务的变化、结束事务、删除保存点、释放锁、数据生效。
当使用commit语句结束事务后，其他会话将可以查看到事务变化后的新数据。

- 事务细节

1. 如果不开始事务，默认情况下，dml操作是自动提交的，不能回滚
2. 如果开始一个事务，没有创建保存点，可以执行rollback，即回退到事务开始的状态
3. 可以在事务中（还没有提交时），创建多个保存点
4. 可以在事务没有提交前，选择回退到哪个保存点
5. 事务机制需要innodb的存储引擎,MyISAM不支持
6. 开始一个事务：start transaction , set autocommit = off;



#### 事务隔离级别

多个连接开启各自事务操作数据库中数据时，数据库系统要负责隔离操作，以保证各个连接在获取数据时的准确性。

如果不考虑隔离性，可能会引发如下问题：

1. 脏读：当一个事务读取另一个事务尚未提交的修改时，产生脏读。
2. 不可重复读：同一查询在同一事务中多次进行，由于其他提交事务所做的修改或删除，每次返回不同的结果集，此时发生不可重复读
3. 幻读：同一查询在同一事务中多次进行，由于其他提交事务所做的插入操作，每次返回不同的结果，此时发生幻读。

事务隔离级别：事务与事务之间的隔离程度

| 隔离级别 | 脏读 | 不可重复读 | 幻读 | 加锁读 |
| --- | --- | --- | --- | ---|
|读未提交(Read uncommitted)|√|√|√|不加锁|
|读已提交(Read committed)|×|√|√|不加锁|
|可重复读(Repeatable read)|×|×|×|不加锁|
|可串行化(Serializable)|×|×|×|加锁|

```
//查看当前会话隔离级别
SELECT @@transaction_isolation;//注意@@和后面的trans..要连在一起

//查看系统当前隔离级别
SELECT @@global.transaction_isolation;
```


```
//将控制台的隔离级别设置为Read uncommitted
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
//设置系统的隔离级别
SET GLOBAL TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```


mysql默认的事务隔离级别是repeatable read。

#### 事务ACID

1. 原子性
2. 一致性：事务必须使数据库从一个一致性状态变换到另一个一致性状态
3. 隔离性：事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离
4. 持久性：持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的

### 表类型/存储引擎

MySQL的表类型由存储引擎决定，主要包括MyISAM、InnoDB、Memory等

MySQL数据表主要支持六种类型，分别是：CSV、Memory、ARCHIVE、MRG_MYISAM、MYISAM、InnoBDB

这六种又分为两类，一类是“事务安全型”，比如InnoDB,其余都属于第二类，称为”非事务安全型“


```
//查看所有的存储引擎
SHOW ENGINES;   
```

![image.png](https://note.youdao.com/yws/res/17986/WEBRESOURCEf27536603eba311fe2f9fde786d4abd5)

- 注意事项

1. MyISAM不支持事务，也不支持外键，但其访问速度快，对事务完整性没有要求
2. InnoDB存储引擎提供了具有提交、回滚、崩溃恢复能力的安全事务。但是比起MyISAM存储引擎，InnoDB写的处理效率差一些，并且会占用更多的磁盘空间以保留数据和索引。
3. MEMORY存储引擎使用存在内存中的内容来创建表。每个MEMORY表只实际对应一个磁盘文件。MEMORY类型的表访问非常快，因为它的数据是放在内存中的，而且默认使用HASH索引。但是一旦服务关闭，表中的数据就会丢掉，表的结构还在。



- MyISAM

添加速度快，不支持外键和事务，支持表级锁

- MEMORY

数据存储在内存中，执行速度很快（没有I/O读写），默认支持索引（HASH表）

- 修改存储引擎

```
ALTER TABLE `表名` ENGINE = 存储引擎;
```

### 视图

视图是一个虚拟表，其内容由查询定义。同真实的表一样，视图包含列，其数据来自对应的真是表（基表）。

- 总结

1. 视图是根据基表来创建的，是虚拟的表
2. 视图也有列，数据来自基表
3. 通过视图可以修改基表的数据，基表的改变也会影响视图的数据
4. 视图和基表是映射关系

#### 视图的基本使用

```
CREATE VIEW 视图名 AS SELECT 语句
ALTER VIEW 视图名 AS SELECT 语句
SHOW CREATE VIEW 视图名
DROP VIEW 视图名1，视图名2
```

```
//创建视图
CREATE VIEW emp_view1 AS SELECT empno,ename,job,deptno FROM emp;

//查看视图
DESC emp_view1;

//改变视图
ALTER VIEW emp_view1 AS SELECT empno,ename,job,deptno,sal from emp;

//查看创建视图的指令
SHOW CREATE VIEW emp_view1;

//删除视图
DROP VIEW emp_view1;

```

- 注意事项

1. 创建视图后，到数据库查看，对应视图只有一个视图结构文件
2. 视图中可以再使用视图


#### 视图最佳实践

1. 安全。 一些数据表有重要的信息，有些字段是保密的，不能让用户直接看到。这时就可以创建一个视图，在这张视图中只保留一部分字段。
2. 性能。关系数据库的数据往往会分表存储，使用外键建立这些表之间的关系。这时数据库查询通常会用到JOIN，但是麻烦且效率较低，如果建立一个视图，将相关的表和字段组合在一起，就可以避免。
3. 灵活。


#### 多表联合查询

```
//针对emp,dept和salgrade三张表，创建一个视图，可以显示雇员编号，雇员名，雇员部门名称和薪水级别

CREATE VIEW emp_view03 AS
    SELECT  empno,ename,dname,grade
            FROM emp,dept,salgrade 
            WHERE emp.deptno = dept.deptno 
            AND sal BETWEEN losal AND hisal;
```


### 管理

MySQL数据库管理人员（root），根据需要创建不同的用户，赋给相应的权限，供开发人员使用。

不同的数据库用户，操作的库和表不相同。

- 用户管理

mysql中的用户，都存储在系统数据库mysql的user表中

其中user表的重要字段说明：
1. host:允许登录的"位置"，localhost 表示该用户只允许本机登录，也可以指定ip地址
2. user: 用户名：
3. authentication_string: 密码，通过password()函数加密过的

- 用户创建

```
create user '用户名' @ '允许登录位置' identified by '密码';
```

- 删除用户

```
drop user '用户名' @ '允许登录位置';
```

- 用户修改密码

```
//修改自己的密码
set password = password('密码');
//修改他人的密码
set password for '用户名' @ '登录位置' = password('密码');
```


#### 权限管理

![image.png](https://note.youdao.com/yws/res/17987/WEBRESOURCEf27b741690d051a22f807050f4197901)

- 授权

![image.png](https://note.youdao.com/yws/res/17988/WEBRESOURCEa816a7141d9abe79731520ba0f26b741)

- 回收

![image.png](https://note.youdao.com/yws/res/17989/WEBRESOURCE5ebf8bf2458c9d797be503295f920609)


#### 实例

```
//1.创建一个用户，并且只可以从本地登录，不让远程登录mysql

CREATE USER 'usename' @ 'localhost' IDENTIFIED BY '123';

//2.创建库和表 testdb 下的 news 表，要求：使用root用户创建

CREATE DATABASE testdb;
use testdb;
CREATE TABLE news(id INT,content varchar(32));
INSERT INTO news VALUES(100,'北京新闻');

//3.给用户分配查看 news 表和添加数据的权限

GRANT SELECT,INSERT ON testdb.news TO 'usename' @ 'localhost';

//4.测试看看用户是否只有这几个权限

UPDATE news SET content = '上海新闻' WHERE id = 100;//错误，对news的更新命令被拒绝

//5.修改密码，要求使用root用户完成

SET PASSWORD FOR 'usename' @ 'localhost' = PASSWORD('abc'); 

//6.回收权限

REVOKE SELECT,INSERT ON testdb.news FROM 'usename' @ 'localhost';

//7.使用root用户删除你的用户

DROP USER 'usename' @ 'localhost';

```

#### 注意事项

1. 在创建用户的时候，如果不指定Host，则为%，%表示所有IP都有连接权限

```
CREATE USER xxx;
```

2. 如果指定`CREATE USER 'xxx' @ '192.168.1.%'`,表示xxx用户在192.168.1.*的ip可以登录mysql

3. 在删除用户的时候，如果host不是%，则需要明确指定'用户'@'host值' 