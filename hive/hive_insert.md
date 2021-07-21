insert 语法格式为：



\1. 基本的插入语法：

insert overwrite table tablename [partition(partcol1=val1,partclo2=val2)] select_statement;

insert into table tablename [partition(partcol1=val1,partclo2=val2)] select_statement;

eg：

insert overwrite table test_insert select * from test_table;
insert into table test_insert select * from test_table;

注：
overwrite重写，into追加。



\2. 对多个表进行插入操作：

from source_table
insert overwrite table tablename1 [partition (partcol1=val1,partclo2=val2)] select_statement1
insert overwrite table tablename2 [partition (partcol1=val1,partclo2=val2)] select_statement2

eg:

from test_table          
insert overwrite table test_insert1
select key
insert overwrite table test_insert2
select value;

注：hive不支持用insert语句一条一条的进行插入操作，也不支持update操作。数据是以load的方式加载到建立好的表中，数据一旦导入就不可以修改。


2.通过查询将数据保存到filesystem

insert overwrite [local] directory 'directory' select_statement;

eg:

（1）导入数据到本地目录：

insert overwrite local directory '/home/hadoop/data' select * from test_insert1;

产生的文件会覆盖指定目录中的其他文件，即将目录中已经存在的文件进行删除。

只能用overwrite，into错误！

（2）导出数据到HDFS中：

insert overwrite directory '/user/hive/warehouse/table' select value from test_table;
只能用overwrite，into错误！

（3）同一个查询结果可以同时插入到多个表或者多个目录中：

from source_table
insert overwrite local directory '/home/hadoop/data' select *
insert overwrite directory '/user/hive/warehouse/table' select value;



\3. 小结：

（1）insert命令主要用于将hive中的数据导出，导出的目的地可以是hdfs或本地filesysytem，导入什么数据在于书写的select语句。

（2）overwrite与into：

insert overwrite/intotable可以搭配；

insert overwritedirectory可以搭配；