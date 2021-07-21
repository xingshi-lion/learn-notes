1. 数据取样(tablesample()函数)

   1） tablesample(n percent) 根据hive表数据的大小按比例抽取数据，并保存到新的hive表中。如：抽取原hive表中10%的数据
   （注意：测试过程中发现，select语句不能带where条件且不支持子查询，可通过新建中间表或使用随机抽样解决）
   create table xxx_new as select * from xxx tablesample(10 percent)
   2）tablesample(n M) 指定抽样数据的大小，单位为M。
   3）tablesample(n rows) 指定抽样数据的行数，其中n代表每个map任务均取n行数据，map数量可通过hive表的简单查询语句确认（关键词：number of mappers: x)

   ```sql
   -- 按行数进行采样
   SELECT
      name
   FROM employee 
   TABLESAMPLE(1 ROWS) a;
   
   
   --按数据量的百分比进行采样
   SELECT
      name
   FROM employee
   TABLESAMPLE(50 PERCENT) a;
   
   -- 按照数据的字节数进行采样
   -- 支持 b/B, k/K, m/M, g/G
   SELECT 
       name 
   FROM employee 
   TABLESAMPLE(1B) a;
   ```

   

2. 分桶抽样

   hive中分桶其实就是根据某一个字段Hash取模，放入指定数据的桶中，比如将表table_1按照ID分成100个桶，其算法是hash(id) % 100，这样，hash(id) % 100 = 0的数据被放到第一个桶中，hash(id) % 100 = 1的记录被放到第二个桶中。创建分桶表的关键语句为：CLUSTER BY语句。
   分桶抽样语法：
   TABLESAMPLE (BUCKET x OUT OF y [ON colname])
   其中x是要抽样的桶编号，桶编号从1开始，colname表示抽样的列，y表示桶的数量。
   例如：将表随机分成10组，抽取其中的第一个桶的数据
   select * from table_01 tablesample(bucket 1 out of 10 on rand())

   ```sql
   SELECT
       name
   FROM employee
   TABLESAMPLE(BUCKET 1 OUT OF 2 ON rand()) a;
   
   
   -- 根据分桶列进行采样，效率更高
   SELECT 
       name
   FROM employee
   TABLESAMPLE(BUCKET 1 OUT OF 2 ON emp_id) a;
   ```

   

3. 随机抽样(rand()函数)

   1）使用rand()函数进行随机抽样，limit关键字限制抽样返回的数据，其中rand函数前的distribute和sort关键字可以保证数据在mapper和reducer阶段是随机分布的，案例如下：
   select * from table_name where col=xxx distribute by rand() sort by rand() limit num;
   2）使用order 关键词
   案例如下：
   select * from table_name where col=xxx order by rand() limit num;
   经测试对比，千万级数据中进行随机抽样 order by方式耗时更长

   ```sql
   
   SELECT 
       name 
   FROM employee
   DISTRIBUTE BY rand() SORT BY rand() 
   LIMIT 2;
   ```

   

4. 可能遇到的问题

   - Error: Error while compiling statement: FAILED: SemanticException 1:40 Percentage sampling is not supported in org.apache.hadoop.hive.ql.io.HiveInputFormat. Error encountered near token ‘10’ (state=42000,code=40000)
     解决办法：

     ```sql
     --是因为使用tez的优化引擎，所以不支持数据采样
     set hive.tez.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;
     ```

     