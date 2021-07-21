\#查看所有分区
SELECT
　　database,
　　table,
　　partition,
　　name,
　　active
FROM system.parts
WHERE table = 'table_name'

Clickhouse删除分区命令: 分区name
alter table sip.ngfw_access_tuple_all_20y DROP PARTITION '2020-05-01';

Clickhouse统计当日数据：
SELECT count() FROM log.netflow WHERE toDate(record_time) = '{}';


\#查看库表容量，压缩率等
select
　　sum(rows) as row,--总行数
　　formatReadableSize(sum(data_uncompressed_bytes)) as ysq,--原始大小
　　formatReadableSize(sum(data_compressed_bytes)) as ysh,--压缩大小
　　round(sum(data_compressed_bytes) / sum(data_uncompressed_bytes) * 100, 0) ys_rate--压缩率
from system.parts

\#查看各库表指标(字节显示)：大小，行数，日期，落盘数据大小，压缩前，压缩后大小
select database,
　　table,
　　sum(bytes) as size,
　　sum(rows) as rows,
　　min(min_date) as min_date,
　　max(max_date) as max_date,
　　sum(bytes_on_disk) as bytes_on_disk,
　　sum(data_uncompressed_bytes) as data_uncompressed_bytes,
　　sum(data_compressed_bytes) as data_compressed_bytes,
　　(data_compressed_bytes / data_uncompressed_bytes) * 100 as compress_rate,
　　max_date - min_date as days,
　　size / (max_date - min_date) as avgDaySize
from system.parts
where active
　　and database = 'db_name'
　　and table = 'table_name'
　　group by database, table

\#查看各库表指标(GB显示)：大小，行数，日期，落盘数据大小，压缩前，压缩后大小
select
　　database,
　　table,
　　formatReadableSize(size) as size,
　　formatReadableSize(bytes_on_disk) as bytes_on_disk,
　　formatReadableSize(data_uncompressed_bytes) as data_uncompressed_bytes,
　　formatReadableSize(data_compressed_bytes) as data_compressed_bytes,
　　compress_rate,
　　rows,
　　days,
　　formatReadableSize(avgDaySize) as avgDaySize
from
　(
　　　select
　　　　　　database,
　　　　　　table,
　　　　　　sum(bytes) as size,
　　　　　　sum(rows) as rows,
　　　　　　min(min_date) as min_date,
　　　　　　max(max_date) as max_date,
　　　　　　sum(bytes_on_disk) as bytes_on_disk,
　　　　　　sum(data_uncompressed_bytes) as data_uncompressed_bytes,
　　　　　　sum(data_compressed_bytes) as data_compressed_bytes,
　　　　　　(data_compressed_bytes / data_uncompressed_bytes) * 100 as compress_rate,
　　　　　　max_date - min_date as days,
　　　　　　size / (max_date - min_date) as avgDaySize
　　　　from system.parts
　　　　where active
　　　　　　and database = 'db_name'
　　　　　　and table = 'tb_name'
　　　　group by
　　　　　　database,
　　　　　　table
)


\#查看表中数据大小：

SELECT column,
　　any(type),
　　sum(column_data_compressed_bytes) AS compressed,
　　sum(column_data_uncompressed_bytes) AS uncompressed,
　　sum(rows)
FROM system.parts_columns
WHERE database = 'db_name'
　　and table = 'table_name'
　　AND active
GROUP BY column
ORDER BY column ASC


\#python 模块地址
/usr/lib64/python2.7/site-packages/clickhouse

\#删除表
DROP table db.tb

\#全流量元数据建表
"CREATE TABLE IF NOT EXISTS slb.netflow_25E_io_test (src_ip IPv6, src_port UInt16, dst_ip IPv6, dst_port UInt16, app_crc UInt32, request_flow Int64, response_flow Int64, record_time DateTime) ENGINE = MergeTree() PARTITION BY toDate(record_time) ORDER BY record_time SETTINGS index_granularity = 8192"

\#批处理 SQL 语句执行，文件插入
\#cat 读取文件流，作为 INSERT 数据输入
cat /data/test_fetch.tsv | clickhouse-client --query "INSERT INTO test_fetch FORMAT TSV"

\#重定向输出
clickhouse-client --query="SELECT * FROM test_fetch" > /data/test_fetch.tsv"

\#多条SQL语句,分号间隔，依次输出
clickhouse-client -h 127.0.0.1 --multiquery --query="SELECT 1;SELECT 2;SELECT 3;"

--host -h 地址
--port 端口
--user -u
--password
--database -d
--query
--multiquery -n
--time -t 打印每条sql执行时间

\#建库
CREATE DATABASE IF NOT EXISTS db_name [ENGINE = engine]

\#数据库支持的五种引擎
Ordinary 默认
Dictionary 字典引擎
Memory 内存引擎,存放临时数据，此库下的数据表只停留在内存中，不涉及磁盘操作，重启数据消失
Lazy 日志引擎，该数据库下只能使用Log 系列的表引擎
MySQL mysql引擎，该数据库会自动拉取远端MySQL中的数据，并为他们创建MySQL的表引擎的数据表

CREATE DATABASE DB_TEST;
默认数据库实质是磁盘的一个文件目录，建库语句执行后 ck 会在安装路径下创建 DB_TEST 数据库的文件目录
\#pwd
/chbase/data
\#ls
DB_TEST default system

\#删库
DROP DATABASE [IF EXISTS] db_name;

\#建表
CREATE TABLE [IF NOT EXISTS] [db_name.]table_name (
name1 [type] [DEFAULT | MATERIALIZED | ALIAS expr],
name2....
.....
) ENGINE = engine;

\#复制其他表结构
CREATE TABLE [IF NOT EXISTS] [db_name.]new_tb AS [db_name2.]old_tb [ENGINE = engine]
\#eg:
create table if not exists new_tb as default.hits_v1 engine = TinyLog;

\#SELECT 语句复制表，并 copy 数据
CREATE TABLE IF NOT EXISTS [db_name.]new_tb ENGINE = engine AS SELECT .....
\#eg:
create table if not exists new_tb engine=Memory as select * from default.hits_v1

\#删除表
DROP TABLE [IF EXISTS] [db_name.]tb_name;

\#按照分区表查询，提高查询速度
SELECT * FROM partition_name WHERE record_time = '2020-06-17';

\#删除字段
ALTER TABLE tb_name DROP COLUMN [IF EXISTS] name
alter table test_v1 drop column URL;

\#移动表/重命名表 - 类 Linux mv 命令
RENAME TABLE [db_name11.]tb_name11 TO [db_name12.]tb_name12, [db_name21.]tb_name21 TO [db_name22.]tb_name22,.....
\#eg:
rename table default.test_v1 to db_test.test_v2;

\#清空数据表
TRUNCATE TABLE [IF EXISTS] [db_name.]tb_name
\#eg:
truncate table db_test.test_v2

\#查询分区信息
SELECT partition_id,name,table,database FROM system.parts where table = 'partition_name';

\#删除分区
ALTER TABLE tb_name DROP PARTITION partition_expr

\#卸载分区 DETACH 语句
ALTER TABLE tb_name DETACH PARTITION partition_expr;
\#eg: 如下语句将卸载整个2020年6月的数据
alter table tb_nama detach partition '202006';
\#被卸载的数据移动到
\#pwd
/chbase/data/data/default/partition_v2/detached 目录下
分区一旦移动到 detached 子目录，代表它脱离了 Clickhouse 的管理，clickhouse 并不会主动清理这些文件，只能自己删除，除非重新装载它们

\#重新装载分区
ALTER TABLE partition_v2 ATTACH PARTITION '202006';

\#分布式DDL执行 只需要加上 ON CLUSTER cluster_name 即可：
一条普通DDL语句转换分布式执行,如下语句将会对 ch_cluster 集群内的所有节点广播这条 DDL 语句：
CREATE TABLE partition_v3 ON CLUSTER ch_cluster(
　　ID String,
　　URL String,
　　EventTime Data
) ENGINE = MergeTree()
PARTITION BY toYYYYMM(EventTime)
ORDER BY ID

\#数据写入 INSERT 语句，三种方式
1.常规,多行数据后面逗号依次展开
INSERT INTO [db.]table [(c1,c2,c3...)] values (v11,v12,v13...),(v21,v22,v23...),.....
同时支持表达式及函数
insert into partition_v2 values ('a0014',toString(1+2),now());

2.使用指定格式的语法
INSERT INTO [db.]table [(c1,c2,c3...)] FORMAT format_name data_set;
\#eg CSV 格式为例：
INSERT INTO partition_v2 FORMAT CSV \
'A0017','url1','2020-06-01'\
'A0018','url2','2020-06-03'\

3.使用 SELECT 子句
INSERT INTO [db.]table [(c1,c2,c3...)] SELECT * FROM partition_v1

人生还有意义。那一定是还在找存在的理由