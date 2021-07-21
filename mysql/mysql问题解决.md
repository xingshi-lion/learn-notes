1. navicat链接MySQL8.0出现2059问题

   在navicat链接mysql8以后的版本时，会出现2059的错误，这个错误出现的原因是在mysql8之前的版本中加密规则为mysql_native_password，而在mysql8以后的加密规则为caching_sha2_password。

   解决此问题有两种方法:

   一种是更新navicat驱动来解决此问题

   一种是将mysql用户登录的加密规则修改为mysql_native_password
   这里采用第二种：

   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #修改加密规则
    
    
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';  #更新一下用户的密码
   FLUSH PRIVILEGES; #刷新权限
   ```

   

2.  忘记保存初始密码

   1）window

   在mysql安装目录生成的data文件下，查找xxx.err的文件，里面记录了安装所有日志，包含了初始密码，可以搜索“password”快速定位

   2）Linux

   find / -name *.err

   3)另一种解决办法

   ```sql
   使用mysql的安全模式启动且启动过程中跳过权限表，不加载它，这样进入mysql的是就不需要密码了  
         mysqld_safe --skip-grant-tables &
   进入mysql：
        mysql -u root
   修改密码：
        update mysql.user set password=password('you password') where user='root';
        flush privileges;//刷新权限
   -----------------------------------------
   
   ```

   