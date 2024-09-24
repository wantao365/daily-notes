## 一、安装

## 1、下载mysql离线安装包

下载地址：https://dev.mysql.com/downloads/mysql/

## 2、上传tar包至服务器

## 3、删除原有的mariadb

先查看一下是否已经安装了，命令：```rpm -qa|grep mysql```、```rpm -qa|grep mariadb```

删除mariadb/mysql，命令：```rpm -e --nodeps mariadb-libs```、```rpm -e --nodeps mysql-libs-5.1.52-1.el6_0.1.x86_64```

## 4、解压缩mysql离线安装包

```tar -xvf mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar```

> 解压缩之后，包含以下rpm包:
>
> mysql-community-embedded-devel-5.7.29-1.el7.x86_64.rpm
> mysql-community-test-5.7.29-1.el7.x86_64.rpm
> mysql-community-embedded-5.7.29-1.el7.x86_64.rpm
> mysql-community-embedded-compat-5.7.29-1.el7.x86_64.rpm
> mysql-community-libs-5.7.29-1.el7.x86_64.rpm
> mysql-community-client-5.7.29-1.el7.x86_64.rpm
> mysql-community-server-5.7.29-1.el7.x86_64.rpm
> mysql-community-devel-5.7.29-1.el7.x86_64.rpm
> mysql-community-libs-compat-5.7.29-1.el7.x86_64.rpm
> mysql-community-common-5.7.29-1.el7.x86_64.rpm

## 5、安装rmp包
逐个安装，命令如下：

【必须安装】

> rpm -ivh mysql-community-common-5.7.29-1.el7.x86_64.rpm
>
> rpm -ivh mysql-community-libs-5.7.29-1.el7.x86_64.rpm
>
> rpm -ivh mysql-community-client-5.7.29-1.el7.x86_64.rpm
>
> rpm -ivh mysql-community-server-5.7.29-1.el7.x86_64.rpm

【非必须安装】

> rpm -ivh mysql-community-libs-compat-5.7.29-1.el7.x86_64.rpm
>
> rpm -ivh mysql-community-embedded-compat-5.7.29-1.el7.x86_64.rpm
>
> rpm -ivh mysql-community-devel-5.7.29-1.el7.x86_64.rpm
>
> rpm -ivh mysql-community-test-5.7.29-1.el7.x86_64.rpm

可能会出错！CentOS8安装MySQL报错，依赖检测失败:libncurses.so.5()(64bit)

**解决办法：**在安装命令后面加--nodeps就可以安装client客户端了；```rpm -ivh --nodeps mysql-community-libs-compat-5.7.29-1.el7.x86_64.rpm```

但是在接下来的步骤中，service mysqld start成功后，修改初始密码的时候还会报：mysql: error while loading shared libraries: libncurses.so.5: cannot open shared object file: No such file or directory

**解决办法：**mysql在启动时发现缺少 libncurses.so.5 这个依赖，并且在/usr/lib以及/lib中也无法找到该依赖。在/usr/lib、/lib、/usr/lib64中寻找一个大于或者等于该依赖版本的依赖文件，我的是在/usr/lin64中找到了一个libncurses.so.6.1，然后建立一个软链接（相当于快捷方式）：

```sudo ln -s /usr/lib64/libncurses.so.6.1 /usr/lib64/libncurses.so.5``

> 查看依赖包来源：yum provides */ibvulkan.so.1
> root下在线安装依赖：yum -y install vulkan-1.1.97.0-1.el7.x86_64
# 二、服务启停

## 1、查看服务状态

命令：```systemctl status mysqld```

##  2、停止服务

命令：```systemctl stop mysqld.service```

## 3、初始化数据库
命令：```mysqld --initialize --console```

## 4、目录授权

命令：```chown -R mysql:mysql /var/lib/mysql/```

## 5、启动mysql服务

命令：```systemctl start mysqld```或```system mysqld start```

命令：```systemctl status mysqld```或者```system mysqld status```

# 三、数据库操作

1、查看临时密码：

命令：```cat /var/log/mysqld.log```

> password is general for...................

### 2、用临时密码登录数据库

命令：```mysql -u root -p ```回车键

然后输入临时密码（输入时不会显示出来，输入完直接回车）

## 3、修改mysql密码
命令：alter USER ‘root’@‘localhost’ IDENTIFIED BY ‘Admin123.’;

## 4、授权远程连接

命令：```show databases;```

命令：```use mysql;```

命令：```select host, user, authentication_string, plugin from user;```

命令：```update user set host = “%” where user=‘root’;```

命令：```select host, user, authentication_string, plugin from user;```

命令：```flush privileges;```

> navicat连接报错1045怎么办
>
> 1、首先打开命令行：开始->运行->cmd；
> 2、先进入电脑安装的mysql的bin目录下（你自己软件的安装路径），如果是C盘，就可以直接执行命令：mysql -u root mysql；
>
> 3、进入mysql后执行命令：UPDATE user SET Password=PASSWORD('newpassword') where USER='root'；
>
> 4、如果报1820的错误，则执行命令：ALTER USER USER() IDENTIFIED BY ‘新密码’，提示Query OK，说明密码已修改成功；
>
> 5、如果如下图所示报1054的错误，则更改命令语句为：update user set authentication_string=password('root') where user='root';即可。因为mysql数据库下已经没有password这个字段了，password字段改成了authentication_string。
>
> 6、完成之后，执行命令：quit 或 exit，退出mysql。
>
> ps：如果在执行命令过程中报1064错误，则说明可能是语法错误，注意查看你的sql语句是否有错误。

