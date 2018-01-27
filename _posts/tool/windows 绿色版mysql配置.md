**写在前面**：5.7增加了安全性，默认root密码不在为空，而是初始化时随机生成一个root密码，改root密码的方式也不一样了

下载地址 http://dev.mysql.com/downloads/mysql/

------

**一、下载后解压到相关目录**

比如我的解压目录是：D:\Program Files\MySQL\mysql-5.7.11-winx64



**二、创建my.ini配置文件**

![img](http://img.blog.csdn.net/20160314125011878?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

代码如下：可根据需要自行参照网上设置

------

**三、安装服务**

cmd进入到安装目录下,运行命令` mysqld install`

![img](http://img.blog.csdn.net/20160314125056391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

显示successfully成功

------

**四、初始化data**

新版mysql解压后安装目录没有data文件夹，需要初始化，注意不能手动创建，因为初始的时候还随机生成了一个初始root密码，初始化命令如下所示：

```
mysqld --initialize
```

![img](http://img.blog.csdn.net/20160314125146783?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

**注意：**要cmd到自己的安装目录下的bin目录，如上所示，没有任何提示说明初始化成功，这时候安装目录下生成了data文件夹。

重点来了：进入data文件，打开一个.err结尾的文件

![img](http://img.blog.csdn.net/20160314125218192?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

打开.err结尾的文件如下所示：

![img](http://img.blog.csdn.net/20160314125231832?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

**四、启动服务，修改密码**

![img](http://img.blog.csdn.net/20160314125300816?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

****

你可以用几种方法为root账户指定密码。以下介绍了三种方法：

  1. 使用SET PASSWORD语句
  2. 使用mysqladmin命令行客户端程序
  3. 使用UPDATE语句

要想使用SET PASSWORD指定密码，用root连接服务器并执行两个SET PASSWORD语句。一定要使用PASSWORD()函数来加密密码。

在Windows中的语句：
```
shell> mysql -u root
mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpwd');
mysql> SET PASSWORD FOR 'root'@'%' = PASSWORD('newpwd');
```
在Unix中的语句：
```
shell> mysql -u root
mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpwd');
mysql> SET PASSWORD FOR 'root'@'host_name' = PASSWORD('newpwd');
```
用服务器主机名替换第二个SET PASSWORD语句中的host_name。这是你指定匿名账户密码的主机名。

要想使用mysqladmin为root账户指定密码，执行下面的命令：
```
  shell> mysqladmin -u root password "newpwd"
  shell> mysqladmin -u root -h host_name password "newpwd"
```
上述命令适用于Windows和Unix。用服务器主机名替换第二个命令中的host_name。不一定需要将密码用双引号引起来，但是你如果密码中包含空格或专用于命令解释的其它字符，则需要用双引号引起来。
你还可以使用UPDATE直接修改user表。下面的UPDATE语句可以同时为两个root账户指定密码：
```
shell> mysql -u root
mysql> UPDATE mysql.user SET Password = PASSWORD('newpwd')
    ->     WHERE User = 'root';
mysql> FLUSH PRIVILEGES;
```
UPDATE语句适用于Windows和Unix。
设置完密码后，当你连接服务器时你必须提供相应密码。例如，如果你想要用mysqladmin 关闭服务器，可以使用下面的命令：
```
shell> mysqladmin -u root -p shutdown
Enter password: (enter root password here)
```

