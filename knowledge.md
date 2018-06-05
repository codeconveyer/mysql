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
`select * from stu;`
![base](https://github.com/codeconveyer/mysql/raw/master/picture/base.jpg)  

`select s_name, s_sex from stu where s_sex=1;`

[1]: http://www.jb51.net/article/99626.htm "windows mysql安装配置"
[2]: https://www.linuxidc.com/Linux/2018-03/151403.htm "虚拟机 mariadb安装配置"
[3]: http://www.navicat.com.cn/ "Navicat官网"
