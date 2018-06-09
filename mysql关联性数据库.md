# 关联性数据库
在mysql文档中,我简单了介绍了一下mysql以及它的一些基本操作,只是掌握这些是远远不够的,现代的数据都是互相关联的,所以这里我来讲一下数据库之间的关联与查询。

## 关联数据库的建立
我在这就引用上个文档中的一个数据库,同时在建立一个与它有关联关系的数据库  
```
create table stuinfo(  
id int,  
s_tel char(11),  
s_addr varchar(50),  
s_id int  
);
```  
这样我们就已经建了一个学生信息表了,但是我还没有给它添加关联信息,主键也没有进行设置,所以我们需要先为表添加一个主键  
`alter table stuinfo add constraint pk_stuinfo primary key(id);`  
这样表的主键就添加成功了,当然这里的`constraint pk_stuinfo`是可以不需要的,它的作用是成为表stuinfo的索引名。  
`alter table stuinfo add constraint fk_stu foreign key(s_id) references stu(id) on delete cascade on update cascade`
这样我们的关联关系就通过外键来实现了,有些内容我来详细说下:  

- primary key 主键  
- foreign key 外键 需要一个关联数据 references table(field)
- on delete和on update是设置在删除关联字段时的操作,有这四种
	- cascade 主表删除, 从表也删除
	- set_null 主表删除, 从表关联字段设为空
	- protect 不让删除
	- set_default 主表删除,从表关联字段设置为默认值

同样的,我们需要一些数据才可以进行之后的关联演示  
```
insert into stuinfo values(1, 11111111111, '汨罗', 1),
(2, 22222222222, '江白', 2),(3,33333333333,'铜雀',4),(4,44444444444,'榭亭',6),(5,12222222222,'修远',1);
```  
![](https://github.com/codeconveyer/mysql/blob/master/picture/stuinfo.png)  
学生表的信息如下  
![](https://github.com/codeconveyer/mysql/blob/master/picture/base.jpg)

## 关联数据的查询  
- 先来介绍一些关键字,顺序是按照执行优先级从先到后  
	- from 从哪个表取值
	- where 判断条件 where id=1
	- group by 进行分组
	- having 判断条件 having id>2
	- select 选择需要的字段
	- distinct 去重
	- order by 排序 asc正序 desc倒叙
	- limit 限制获取几条数据  

- 一些最基本的关联操作  
`select s.s_name 姓名, i.s_addr 地址 from stu s join stuinfo i on s.id=i.s_id;
`  
![](https://github.com/codeconveyer/mysql/raw/master/picture/join.png)  
mysql并没有专门建一个一对一关联的方法,但是我们可以通过将从表外键与主表主键相关联或者设置unique属性来进行一对一的关联数据库建立。因为我建的是一个一对多关联性数据库,所以这里的*屈原*的关联数据就有两条;有人会觉得这样的数据显示会有点拙劣,我们可以进行如下操作  
`select s.s_name 姓名, group_concat(i.s_addr separator ',') 地址 from stu s join stuinfo i on s.id=i.s_id group by s.s_name;
`  
![](https://github.com/codeconveyer/mysql/raw/master/picture/concat.png)  
这样通过`group_concat`将有多个数据的字段进行分组就可以达到一个不错的显示效果  
当然,我们也可以这样做  
`select s.s_name 姓名, group_concat(i.s_addr separator ',') 地址 from stu s, stuinfo i where s.id=i.s_id group by s.s_name;
`  
这样做的效果和`join on`的效果是一样的,因为我的学艺不精,所以不清楚这两种方法的区别是什么。  

- 上面这些都是通过自己建立的表获取数据的,然而实际上我们还可以通过查询语句,再从用查询语句生成的表中进行数据的获取  
`select * from (select s_addr from stuinfo) a;`  
![](https://github.com/codeconveyer/mysql/blob/master/picture/onebyone.png)  
我们可以看到,这里输出所有的字段就只有`s_addr`这一个,而这个字段就是我们括号中的查询语句生成的表的全部数据,当然,如果要这样进行查询的话,必须给这个查询语句生成的表起个别名。  

- 因为建的表之间的关系太简单了,所以很多东西不能展示  
`select s.id, s.s_name, i.s_addr, i.s_id from stu s left join (select * from stuinfo group by s_id) i on s.id=i.s_id where s.id<7 order by s.id desc; `  
![](https://github.com/codeconveyer/mysql/blob/master/picture/left.png)  
`left join`的作用是显示结果以左列表为主,右列表数据若没有匹配的则输出为null;	`right join`的作用就是以右边的表数组为主了。

- 讲到这就差不多了,还有些知识鄙人也不清楚,只能将在下了解的写在这里了;mysql的语句很简单,我写的例子也很简单,但是可以用这简单的语句来写出很复杂的内容,但最后展现出来的也必然是简洁的,这就是编程的美妙之处,各位可以自行稍后进行练习,如果错误请多指正。
