

说明：【hadoop fs -命令】【hdfs dfs 命令】都可以使用

1. hdfs递归创建目录

   hdfs dfs -mkdir -p /xx1/xx2/xx3 

2. hdfs删除

   删除文件：hdfs dfs -rm output2/*

   删除文件夹：hdfs dfs -rm -r output2

3. 下载文件到本地

   正常下载：hdfs dfs -get /hdfs路径 /本地路径

   合并下载：hdfs dfs -getmerge /hdfs路径文件夹 /合并后的文件

4. 复制文件

   ```shell
   hdfs dfs -cp /hdfs路径1 /hdfs路径2
   如果【hdfs路径1】是文件，就复制到【hdfs路径2】下面
   如果【hdfs路径1】是目录，就会在【hdfs路径2】下面创建【hdfs路径1】子目录
   复制目录下所有文件：hdfs dfs -cp /hdfs路径1/* /hdfs路径2
   ```

5. 统计目录下文件数

   ```shell
   hdfs dfs -count /文件夹
   ```

6. 查看hdfs空间

   ```shell
   hdfs dfs -df /
   hdfs dfs -df -h /
   ```

7. 统计文件夹大小

   ```
   只统计整个目录：hadoop fs -du -s -h /user/atguigu/test
   2.7 K  /user/atguigu/tes
   
   目录下所有文件：hadoop fs -du  -h /user/atguigu/test
   1.3 K  /user/atguigu/test/README.txt
   15     /user/atguigu/test/jinlian.txt
   1.4 K  /user/atguigu/test/zaiyiqi.txt 
   ```

8. 