# MySql
- MySql是一个关系型数据库管理系统。事务有原子性,一致性,隔离型,持久性,而持久性可以通过数据库得到实现。
- MySql因为其体积小,速度快,总体拥有成本低,开放源码等特点，常常会被中小型网站开发选用为网站数据库。

## MySql的安装与配置
- windows  
  1 我们可以在windows下进行mysql的安装,只需要在官网上进行下载就可以了,这里分安装版和免安装版,可以自行选择。  
  2 下载完成之后需要进行一些配置,这里我就不一一讲明了,[这里有][1]。
- 虚拟机  
  1 我们在虚拟机中进行安装时需要安装服务器和客户端两个`yum install MariaDB-server MariaDB-client -y`。  
  2 同样的,下载了之后也需要进行配置,我还是直接给个[链接][2]好了。

## Navicat  
- Navicat是mysql的可视化操作界面,可以自行去[官网][3]进行下载。
- 所有mysql的操作都可以在这个可视化操作界面中通过新建查询的方法执行。

## 实际操作
- 我们首先创建一个数据库并使用  
`create database example default charset utf8;`  
`use example;`

- 创建一个基本的学生库  
```
create table stu (
id integer primary key auto_increment,
s_name varchar(5),
s_sex bit default 1
);
```
- 再插入一些数据
```
insert into stu (s_name, s_sex) values ('屈原', 1);
insert into stu values (2,'杜甫', default);
insert into stu (s_name, s_sex) values ('李白', 1), ('貂蝉',0),('魏延',1),('谢道韫',0),('杨玉环',0);
``` 

- 一些基本的mysql操作指令  
  1. 显示数据`select * from stu;`  
![base](https://github.com/codeconveyer/mysql/raw/master/picture/base.jpg)  

  2. 条件显示`select s_name, s_sex from stu where s_sex=1;`  
![require](https://github.com/codeconveyer/mysql/raw/master/picture/1.png)  

  3. if,case根据判定条件输出对应结果  
  `select s_name 姓名, if(s_sex=1,'男','女') 性别 from stu;`  
  `select s_name 姓名, (case when s_sex=1 then '男' else '女' end) 性别 from stu;`  
  
  4. 删除,更新数据  
      - 添加数据`insert into stu values()`  
      - 删除第七行`delete from stu where id=7`  
      - 更新第一条数据的姓名`update stu set s_name='许褚' where id=1`  
      - 删除s_sex这列`alter table stu drop columu s_sex`  
      - 添加s_sex这列`alter table stu add column s_sex`  
      - 重命名列和修改列的数据类型`alter table stu change column s_name name varchar(20)`如果不希望改名,仍需要写重命名为原来名字就可,不能不写  
      - 修改列的数据类型`alter table stu modify column s_name varchar(5)`  

  5. union和union all 操作符用于连接两个以上的select语句的结果组合到一个结果集合中,不同的是union会自动去重,而union all则会全部显示  
      ```
      select s_name 姓名 from stu
      union all
      select s_sex 性别 from stu;
      ```  
        ![tyerror](https://github.com/codeconveyer/mysql/raw/master/picture/tyerror.png)  
  这样搜索的结果确实组合到了一个结果中,列的名称默认为第一个select语句的名称。但是因为定义表的时候,s_name为varchar类型,s_sex为bit类型,字段类型不同产生了乱码。  
  
  6. 就上文中的乱码问题,我们可以使用cast和convert来转换字段类型  
  `cast(s_sex as signed)` or `convert(s_sex, signed)`  
![correct](https://github.com/codeconveyer/mysql/raw/master/picture/correct.png)  
  之所以不将格式转换为varchar,是因为这种方法转换类型是有限的  
      + 字符型,可带参数 char()  
      + 日期 date  
      + 时间 time  
      + 日期时间 datetime  
      + 浮点型 deciaml  
      + 整数 signed  
      + 无符号整数 unsigned  
  
  7. concat, concat_ws, group_concat连接字符串,显示在一个结果框内  
      - concat  
      `select concat(id, s_name) 组合 from stu` concat中只要有一个值为null,则整体输出为null  
      - concat_ws  
      `select concat_ws(',', id, s_name) from stu` 第一个为指定分隔符,如果分隔符为null,则输出为null;但是如果选择的某值为null时,该值输出为空  
      - group_concat  
      该方法的完整表达是为select id, group_concat(distinct value separator) group by id; distinct和separator都是可以进行指定的,表示是否去重和分隔符  
      - `select s_sex, group_concat(concat_ws('-', id, s_name) separator ',') 例子 from stu group by s_sex;`  
![example](https://github.com/codeconveyer/mysql/raw/master/picture/example.png)
      

[1]: http://www.jb51.net/article/99626.htm "windows mysql安装配置"
[2]: https://www.linuxidc.com/Linux/2018-03/151403.htm "虚拟机 mariadb安装配置"
[3]: http://www.navicat.com.cn/ "Navicat官网"
