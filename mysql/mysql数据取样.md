假设表中有1万条数据，我们需要随机抽取1000条：

1. select * from table_name order by rand() limit 100;

   原理：rand()函数会产生0-1之间的随机数，order by rand()

   根据产生的随机数进行排序。limit 100截取前100行数据。从而达到随机抽样100条数据的目的。当然如果需要抽样出N条数据就使用limit N

2. select * from table_name where rand()<0.015 limit 100;

   我们先计算我们需要抽样数据与总体的占比

   100/7142=0.014

   ,然后通过查询语句随机抽样出上述比例的数据：

   select * from table_name where rand()<0.014

   解释：对于每一行数据，都会执行判断条件

   where rand()<0.014

   ，而

   rand()

   是产生0-1的随机函数，那么每条数据都有0.014的机率被筛选出来，最终会筛选出

   总体×0.014

   条数据。当然实际筛选出的数据条数不是固定的，它随着数据量越大越接近0.014这个比例。我们为了抽取出刚好100条数据，我们可以稍微提高抽取的比例，然后使用limit 100

   

3. 本方法需要表中有一列是连续编号的数字，一般的表中id或序号都是连续编号的，我们可以直接使用。如果没有连续编号的列，那么我们需要人为的创建一列序号数据。在table_name 中无论是id还是序号都是不连续的，所以我们首先新增一列，列名为‘num’。

   1）alter table 表名 add column 列名 数据类型;

   2）set @rn=0;update table_name set num=(@rn:=@rn+1);

   3）set @max=7142;set @min=1;select *from table_name a join(select floor(@min+(@max-@min+1)*rand()) as num from table_name limit 100) b on a.num=b.num limit 100;

   需要注意的是在使用update语句里，可能会报错：Error Code 1175 You are using safe update mode。也就是如果没有加where限制条件更新值是不允许的。这里我们关闭安全更新模式：

   SET SQL_SAFE_UPDATES = 0;

