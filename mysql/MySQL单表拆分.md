# MySQL单表拆分

## 1. 横向拆分

```sql
create table 新表的名称 select * from 被拆分的表 order by id  limit int1,int2

int1为其实位置,int2为条数

create table xuesi1 select * from xuesi order by kid limit 0,1000;
create table xuesi2 select * from xuesi order by kid limit 1000,1000;
create table xuesi3 select * from xuesi order by kid limit 2000,1000;
create table xuesi4 select * from xuesi order by kid limit 3000,1000;
create table xuesi4 select * from xuesi order by kid limit 4000,1000;
————————————————
版权声明：本文为CSDN博主「老朱佩琦」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_42821003/article/details/81263630
```

**注意：这样拆分会导致主键失效，所以需要手动让其主键生效**

```sql
alter table 新表的名称 modify 主键字段 int primary key auto_increment
```



## 2. 纵向拆分

```sql
create table 新表的名称 select 需保留的字段 from 被拆分的表
```

### 3. 拆分表的核心思想

**主要是把经常查的数据放在一个表里,不经常查的数据不做处理**