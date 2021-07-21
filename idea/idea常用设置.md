1. 显示工具栏

   - 标注1：View–>Toolbar

   - 标注2：View–>Tool Buttons

     

2. 设置鼠标悬浮提示

   File–>settings–>Editor–>General–>勾选Show quick documentation

   

3. 设置方法分割

   File–>settings–>Editor–>Appearance–>勾选
   
4. 忽略大小写提示

   File–>settings–>Editor–>General -->Code Completion -->

   

5. 自动导包

   File–>settings–>Editor–>general–>Auto Import–>

   

6. 单行显示多个Tabs

   File–>settings–>Editor–>General -->Editor Tabs–>去掉√

   

7. 代码自动提示

   File–>settings–>Keymap->Main menu->Code->Completion->Basic    设置为“alt+/”，需要先删除

   

8. 模板创建

   类和方法模板：https://blog.csdn.net/sdut406/article/details/81750858

   代码模板：https://blog.csdn.net/m0_38143867/article/details/92784862

   代码模板举例：

   1） psvm : 可生成 main 方法

   2） sout : System.out.println() 快捷输出
   类似的：
   soutp=System.out.println("方法形参名 = " + 形参名);
   soutv=System.out.println("变量名 = " + 变量);
   soutm=System.out.println(“当前类名.当前方法”);
   “abc”.sout => System.out.println(“abc”);

   3 ）fori : 可生成 for 循环
   类似的：
   iter：可生成增强 for 循环
   itar：可生成普通 for 循环

   4） list.for : 可生成集合 list 的 for 循环

   List list = new ArrayList();

   输入: list.for 即可输出
   for(String s:list){
   }
   list.fori 正序遍历
   list.forr 倒序遍历

   5） ifn：可生成 if(xxx = null) 条件判断
   类似的：
   inn：可生成 if(xxx != null) 或 xxx.nn 或 xxx.null

   6） prsf：可生成 private static final

   类似的：
   psf：可生成 public static final
   psfi：可生成 public static final int
   psfs：可生成 public static final String
   
   

9. idea配置导入导出

   导出：File->Export Settings

   ​			File->Manage idea Settings->Export Settings

   目录：C:\Users\86150\.IntelliJIdea2018.1\config\settings.jar

   导入[选择导出的文件]：File->import Settings

   ​			File->Manage idea Settings->import Settings