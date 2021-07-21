1. Unable to import maven project: See logs for details

   - 修改maven的jdk for important为jdk版本

     <img src="https://img-blog.csdnimg.cn/20191021134452681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p6MTg0MzU4NDI2NzU=,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:80%;" />

   - 删除项目的.idea文件，选择根项目，然后重新导入项目

   - 防火墙问题：关闭防火墙

   - idea2019与maven3.6版本不兼容问题，要么升级idea，要么降级maven

   - settings.xml文件有问题

   - mirror地址是否正确，格式是否正确