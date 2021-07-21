在创建mysql账户时，限制连接账户远程登录。也就是说，除了当前mysql所在的安装服务器外，其他的ip（主机）都是不允许访问的，即使你的用户名和密码是正确的。这时候就要修改用户的访问权限。

```sql
首先是用root用户登录到mysql的安装主机，然后进入mysql：

mysql -u root -p

root是mysql的最高授权用户名，这时会提示你输入密码，正确输入密码后回车，进入mysql。回车

然后输入如下命令：

grant all on 数据库名.* to ‘数据库账户名’@’%’ identified by ‘密码’ with grant option;

flush privileges;
```

注意：上面的单引号不能省，数据库名.* 表示要开放的数据库下所有表，如果该连接的所有数据库都要开放，可以用 *.*代替。
‘数据库账户名’@’%’ 这里表示要开放的账户，百分号表示在任何主机都允许访问。
如果以上两步均显示 “Query OK, 0 rows affected (0.00 sec)”，那么说明命令已经成功执行，现在就可以远程连接你的mysql数据库了。

(1).如果想赋予所有操作的权限

 grant all on ... to 用户名......

(2).如果想赋予操作所有数据库的所有表的权限

 grant ... on *.* to 用户名......

(3).如果想赋予某个数据库的所有表

 grant ... on 数据库名称.'*' to 用户名...... ([ * ]两边一定要加单引号)

(4).如果想赋予某个数据库的某张表

 grant ... on 数据库名称.表名 to 用户名......

(5).如果想任何客户端都能通过该用户名远程访问

 grant ... on ... to 用户名@% ....... (要把IP地址改成[ % ])

(6).改完了以后一定要重新启动MySQL服务

用户会存到Mysql服务器上的user表中，所以下面的两种方法都可以解决这个问题：

1。 改表法。可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"

mysql -u root -p

mysql>use mysql;

mysql>update user set host = ’%’ where user = ’root’;mysql>select host, user from user; 

mysql>flush privileges;

\2. 授权法。例如，你想myuser使用mypassword从任何主机连接到mysql服务器的话。

GRANT ALL PRIVILEGES ON *.* TO ’myuser’@’%’ IDENTIFIED BY ’mypassword’ WITH GRANT OPTION; 

如果你想允许用户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用mypassword作为密码

GRANT ALL PRIVILEGES ON *.* TO ’myuser’@’192.168.1.3’ IDENTIFIED BY ’mypassword’ WITH GRANT OPTION;



如果还是无法远程我们可参考

1、Mysql的端口是否正确，通过netstat -ntlp查看端口占用情况，一般情况下端口是3306。在用工具连接MySQl是要用到端口。例如My AdminMy Query BrowserMySQl Front等。

2、检查用户权限是否正确。
  例如：用户Tester,user表里有两条记录：host分别为localhost和%(为了安全，%可以换成你需要外部连接的IP)。


3、查看/etc/my.cnf中,skip-networking 是否已被注掉,需要注掉。
 报错：ERROR 2003 (HY000): Can't connect to MySQL server on '192.168.51.112' (111)


4、查看iptables是否停掉,没关的情况下,无法连接。
通过：service iptables stop临时关闭。
报错：ERROR 2003 (HY000): Can't connect to MySQL server on '192.168.51.112' (113)

另外，我们还可以通过配置http通道来使Navicat远程连接到数据库，这样做的好处是不需要前面繁杂的配置。在主机名ＩＰ地址那里填写LocalHost

用户名与密码一栏则填写你所在的数据库用户名与密码。

这时候还不能连接数据库的，需要通过Http通道的形式进行数据库连接。

