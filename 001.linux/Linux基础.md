# 1、Linux的概述

Linux是基于Unix的开源免费的操作系统，由于系统的稳定性和安全性几乎成为程序代码运行的最佳系统环境。Linux是由Linus Torvalds（林纳斯·托瓦兹）起初开发的，由于源代码的开放性，现在已经衍生出了成千上百种不同的Linux系统。

Linux系统的应用非常广泛，不仅可以长时间的运行我们编写的程序代码，还可以安装在各种计算机硬件设备中，比如手机、平板电脑、路由器等。尤其在这里提及一下，我们熟知是Android程序最底层就是运行在linux系统上的。

# 2、Linux的分类

(1)Linux根据市场需求不同，基本分为两个方向：

1）图形化界面版：注重用户体验，类似window操作系统，但目前成熟度不够

2）服务器版：没有好看的界面，是以在控制台窗口中输入命令操作系统的，类似 于DOS，是我们架设服务器的最佳选择

(2)Linux根据原生程度，又分为两种：

1）内核版本：在Linus领导下的内核小组开发维护的系统内核的版本号；

2）发行版本：一些组织或公司在内核版基础上进行二次开发而重新发行的版本。

(3)Linux发行版本不同，又可以分为n多种：

![](images/Linux的分类.png)

# 3、linux常用命令

> 目录介绍
>
> - bin :存放二进制可执行文件
> - sbin：存放二进制可执行文件，只有root可以访问
> - etc：存放系统配置文件
> - usr：用于存放共享的系统资源
> - home：存放用户文件的根目录
> - root：超级用户目录
> - dev：用于存放设备文件
> - lib：存放跟文件系统中的程序运行所需要的共享库及内核模块
> - mnt：系统管理员安装临时文件系统的安装点
> - boot：存放用于系统引导时使用的各种文件
> - tmp：用于存放各种零食文件
> - var：用于存放运行时需要改变的数据文件
>
> 推荐使用SSH工具（FinalShell、SecureCRT、xshell）

## 3.1、目录切换命令

```shell
# 切换到该目录下usr目录 
[root@localhost ~]# cd usr	

# 切换到上一层目录 
[root@localhost ~]# cd ../	

# 切换到系统根目录 	
[root@localhost ~]# cd /

# 切换到用户主目录 
[root@localhost ~]# cd ~	

# 切换到上一个所在目录
[root@localhost ~]# cd -		
```

## 3.2、目录的操作命令

### 3.2.1、增加目录

> 命令：mkdir 目录名称

```shell
# 示例：在根目录 / 下 mkdir test，就会在根目录 / 下产生一个test问目录
[root@localhost ~]# mkdir test
```

### 3.2.1、查看目录

> 命令：ls [-al] 父目录

```shell
# 示例：在根目录 / 下使用ls，可以看到该目录下的所有的目录和文件
[root@localhost ~]# ls

# 示例：在根目录 / 下使用ls -a，可以看到该目录下的所有文件和目录，包括隐藏的
[root@localhost ~]# ls -a

# 示例：在根目录 / 下使用ls -l，可以看到该目录下的所有目录和文件的详细信息。注意：ls -l 可以缩写成ll
[root@localhost ~]# ls -l
```

### 3.2.2、搜索目录

> 命令：find 目录 参数 文件名称

```shell
# 示例：find /root -name ‘test*’
[root@localhost ~]# find /root -name ‘test*’
```

### 3.2.3、修改目录的名称

> 命令：mv 目录名称 新目录名称
>
> 注意：mv的语法不仅可以对目录进行重命名而且也可以对各种文件，压缩包等进行重命名的操作

```shell
# 示例：test目录下有一个oldTest目录，使用mv oldTest newTest命令修改
[root@localhost ~]# mv oldTest newTest
```

### 3.2.4、移动目录的位置(剪切)

> 命令：mv 目录名称 目录的新位置
>
> 注意：mv语法不仅可以对目录进行剪切操作，对文件和压缩包等都可执行剪切操作

```shell
# 示例：在test下将newTest目录剪切到 /usr下面，使用mv newTest /usr
[root@localhost ~]# mv newTest /usr
```

### 3.2.5、拷贝目录

> 命令：cp -r 目录名称 目录拷贝的目标位置 -----r代表递归
>
> 注意：cp命令不仅可以拷贝目录还可以拷贝文件，压缩包等，拷贝文件和压缩包时不用写-r递归

```shell
# 示例：将/usr下的newTest拷贝到根目录下的test中，使用cp -r /usr/newTest /test
[root@localhost ~]# cp -r /usr/newTest /home/test
```

### 3.2.6、删除目录

> 命令：rm [-rf] 目录
>
> 注意：rm不仅可以删除目录，也可以删除其他文件或压缩包，为了方便大家的记忆，无论删除任何目录或文件，都直接使用rm -rf 目录/文件/压缩包

```shell
# 示例：删除/usr下的newTest，进入/usr下使用rm -r newTest
[root@localhost ~]# rm -r newTest

# 示例：删除/test下的newTest而不需要询问强制删除，在/test下使用rm -rf newTest
[root@localhost ~]# rm -rf newTest
```

## 3.3 、文件的操作命令

### 3.3.1、文件的创建

> 命令：touch 文件名称 ----- 空文件

```shell
# 示例：在test目录下创建一个空文件 touch aaa.txt
[root@localhost ~]#touch aaa.txt
```

### 3.3.2、文件的查看

> 命令：cat/more/less/tail 文件
>
> 注意：命令 tail -f 文件 可以对某个文件进行动态监控，例如tomcat的日志文件，会随着程序的运行，日志会变化，可以使用tail -f catalina-2016-11-11.log 监控文件的变化

```shell
# 示例：使用cat查看/etc/sudo.conf文件，只能显示最后一屏内容
[root@localhost ~]# cat /etc/sudo.conf

# 示例：使用more查看/etc/sudo.conf文件，可以显示百分比，回车可以向下一行，空格可以向下一页，q可以退出查看
[root@localhost ~]# more /etc/sudo.conf

# 示例：使用less查看/etc/sudo.conf文件，可以使用键盘上的PgUp和PgDn向上 和向下翻页，q结束查看
[root@localhost ~]# less /etc/sudo.conf

# 示例：使用tail -10 查看/etc/sudo.conf文件的后10行，Ctrl+C结束
[root@localhost ~]# tail -f ../logs/catalina.out
[root@localhost ~]# tail -n 20 ../logs/catalina.out
```

### 3.3.3、文件的内容修改

> 命令：vim 文件

```shell
# 示例：编辑/test下的aaa.txt文件，使用vim aaa.txt
[root@localhost ~]# vim aaa.txt
```

但此时并不能编辑，因为此时处于命令模式，点击键盘i/a/o进入编辑模式，可以 编辑文件编辑完成后，按下Esc，退回命令模式；此时文件虽然已经编辑完成，但是没有保存，需输入冒号：进入底行模式，在底行模 式下输入wq代表写入内容并退出，即保存；输入q!代表强制退出不保存。

> 总结：
>
> vim编辑器是Linux中的强大组件，是vi编辑器的加强版，vim编辑器的命令和快捷方式有很多，但此处不一一阐述，大家也无需研究的很透彻，使用vim编辑修改文件的方式基本会使用就可以了。

### 3.3.4、文件的删除

> 同目录删除：熟记 rm -rf 文件 即可

```shell
[root@localhost ~]# rm -rf aaa.txt
```

### 3.3.5、文件的压缩与解压

> Windows的压缩文件的扩展名 .zip/.rar
>
> linux中的打包文件：.tar
>
> linux中的压缩文件：.gz
>
> linux中打包并压缩的文件：.tar.gz
>
> ---
>
> Linux中的打包文件一般是以.tar结尾的，压缩的命令一般是以.gz结尾的。
>
> 而一般情况下打包和压缩是一起进行的，打包并压缩后的文件的后缀名一般.tar.gz。
>
> ```tex
> > -c 建立新的压缩文件> -x 从压缩的文件中提取文件> -f 指定压缩文件> -v 显示操作过程> -z 支持gzip解压文件
> ```

- 打包并压缩文件

  > 命令：tar -zcvf 打包压缩后的文件名 要打包的文件
  >
  > z：调用gzip压缩命令进行压缩
  >
  > c：打包文件
  >
  > v：显示运行过程
  >
  > f：指定文件名
  
  ```shell
  # 示例：打包并压缩/test下的所有文件 压缩后的压缩包指定名称为xxx.tar.gz  
  [root@localhost ~]# tar -zcvf xxx.tar.gz aaa.txt bbb.txt ccc.txt  
  #或：  
  [root@localhost ~]# tar -zcvf xxx.tar.gz /usr/test/*
  ```
  
- 解压压缩包

  > 命令：tar [-xvf] 压缩文件
  >
  > 其中：x：代表解压

  ```shell
  # 示例：将/test下的xxx.tar.gz解压到当前目录下
  [root@localhost ~]# tar -xvf xxx.tar.gz
  
  # 示例：将/test下的xxx.tar.gz解压到根目录/usr下
  # -C代表指定解压的位置
  [root@localhost ~]# tar -xvf xxx.tar.gz -C /usr
  ```

## 3.4 、其他命令

### 3.4.1、显示工作目录

> 命令：pwd

```shell
[root@localhost ~]# pwd
```

### 3.4.2、查看进程

> 命令：ps -ef 

```shell
# 显示所有的进程
[root@localhost ~]# ps -ef

# 管道命令搜索指定进程
[root@localhost ~]# ps -ef | grep java

# 查看端口对应的进程
[root@localhost ~]# lsof -i :8089
```

### 3.4.3、关闭进程

>  命令：kill -9 pid（pid是进程的id）

```shell
[root@localhost ~]# kill -9 pid
```

### 3.4.4、搜索

> 命令：grep 要搜索的字符串 要搜索的文件

```shell
# 示例：搜索/usr/sudu.conf文件中包含字符串to的行
[root@localhost etc]# grep to sudo.conf --color

# 示例：搜索/usr/sudu.conf文件中包含字符串to的行 to要高亮显示
[root@localhost etc]# grep to sudo.conf
```

### 3.4.5、管道命令

> 命令：| 将前一个命令的输出作为本次目录的输入

```shell
# 示例：查看当前系统中所有的进程中包括system字符串的进程
[root@localhost etc]# ps -ef | grep system
```

### 3.4.6、网络通信命令

> 命令：ifconfig、ping、netstat

```shell
# 查看当前系统的网卡信息：ifconfig
[root@localhost ~]# ifconfig

# 查看与某台机器的连接情况：ping
[root@localhost ~]# ping 192.169.0.0.1

# 查看当前系统的端口使用：netstat -an
[root@localhost ~]# netstat -an
```

### 3.4.7、关机与重启

```shell
# 重启命令：reboot
[root@localhost ~]# reboot

# 立即关机：halt
[root@localhost ~]# halt

# 注销：exit
[root@localhost ~]# exit

# 立刻重启(root用户使用)
[root@localhost ~]# shutdown -r now   

# 立刻关机(root用户使用)。
[root@localhost ~]# shutdown -h now   

#  10分钟后自动关机。   
[root@localhost ~]# shutdown -h 10   

# 在时间为20:35时候重启(root用户使用)   
[root@localhost ~]# shutdown -r 20:35     
```

### 3.4.8 网络抓包

```shell
# 网络抓包 
[root@localhost ~]# tcpdump -i enp5s0 tcp and host 192.168.1.27 -w /home/test.cap
```

# 4、Linux权限与分配

权限是Linux中的重要概念，每个文件/目录等都具有权限，通过ls -l命令我们可以 查看某个目录下的文件或目录的权限。

```shell
# 示例：在随意某个目录下ls -l
[root@localhost local]# ls -l
总用量 0
drwxr-xr-x. 2 root root 134 8月  18 10:07 
bindrwxr-xr-x. 2 root root   6 4月  11 2018 
etcdrwxr-xr-x. 2 root root   6 4月  11 2018 
gamesdrwxr-xr-x. 2 root root   6 4月  11 2018 
includedrwxr-xr-x. 2 root root  39 8月  17 18:20 
influxdbdrwxr-xr-x. 2 root root   6 4月  11 2018 
libdrwxr-xr-x. 2 root root   6 4月  11 2018 
lib64drwxr-xr-x. 2 root root   6 4月  11 2018 
libexecdrwxr-xr-x. 3 root root  51 8月  18 10:04 
redisdrwxr-xr-x. 2 root root   6 4月  11 2018 
sbindrwxr-xr-x. 5 root root  49 8月   9 22:42 
sharedrwxr-xr-x. 2 root root   6 4月  11 2018 src
```

<table>
    <tr>
    	<td colspan='3'>属主（user）</td>
        <td colspan='3'>属组（group）</td>  
        <td colspan='3'>其他用户</td>
    </tr>
    <tr>
    	<td>r</td>
        <td>w</td>
        <td>x</td>
        <td>r</td>
        <td>w</td>
        <td>x</td>
        <td>r</td>
        <td>w</td>
        <td>x</td>
    </tr>
    <tr>
    	<td>4</td>
        <td>2</td>
        <td>1</td>
        <td>4</td>
        <td>2</td>
        <td>1</td>
        <td>4</td>
        <td>2</td>
        <td>1</td>
    </tr>
</table>


**修改文件/目录的权限的命令：chmod**

```shell
# 示例：修改/test下的aaa.txt的权限为属主有全部权限，属主所在的组有读写权限，其他用户只有读的权限
[root@localhost ~]# chmod u=rwx,g=rw,o=r aaa.txt

# 上述示例还可以使用数字表示：
[root@localhost ~]# chmod 764 aaa.txt
```

# 5、用户组及用户

## 5.1、用户组

```shell
# 添加用户组
[root@localhost ~]# groupadd dev

# 修改组
[root@localhost ~] groupmod -n dev  dev1 

# 删除用户组
[root@localhost ~]# groupdel dev

# 查询组

[root@localhost ~]# cat /etc/group 
# 或者使用管道来精确查询 
[root@localhost ~]# cat /etc/group | grep dev

# 查看当前登录用户所在的组 groups
[root@localhost ~]# groups someuser
```

## 5.2、用户

```shell
# 添加用户：useradd -m -g 组 新建用户名（别忘记设置密码）  	
    # Usage: useradd [options] LOGIN    
    #    Options:    
    #     -b, --base-dir BASE_DIR       设置基本路径作为用户的登录目录    
    #     -c, --comment COMMENT         对用户的注释    
    #     -d, --home-dir HOME_DIR       设置用户的登录目录    
    #     -D, --defaults                改变设置    
    #     -e, --expiredate EXPIRE_DATE  设置用户的有效期    
    #     -f, --inactive INACTIVE       用户过期后，让密码无效    
    #     -g, --gid GROUP               使用户只属于某个组    
    #     -G, --groups GROUPS           使用户加入某个组    
    #     -h, --help                    帮助    
    #     -k, --skel SKEL_DIR           指定其他的skel目录    
    #     -K, --key KEY=VALUE           覆盖 /etc/login.defs 配置文件    
    #     -m, --create-home             自动创建登录目录    
    #     -l,                           不把用户加入到lastlog文件中    
    #     -M,                           不自动创建登录目录    
    #     -r,                           建立系统账号    
    #     -o, --non-unique              允许用户拥有相同的UID    
    #     -p, --password PASSWORD       为新用户使用加密密码    
    #     -s, --shell SHELL             登录时候的shell    
    #     -u, --uid UID                 为新用户指定一个UID    
    #     -Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping
# 注意：-m 自动建立用户家目录； -g 指定用户所在的组，否则会建立一个和用户名同名的组 
[root@localhost ~]# useradd -m -g dev test1

# 修改用户 
# 示例：修改和创建密码 passwd 用户名    如果不加用户名则默认修改当前登录者的密码
[root@localhost ~]# passwd test1   输入两次newpwd

# 示例：将test用户的登录目录改成/home/test，并加入test2组，注意这里是大G。
[root@localhost ~]# usermod -d /home/test -G test2 test 

# 示例：将用户test加入到test2组
[root@localhost ~]#  gpasswd -a test test2 

# 示例：将用户test从test2组中移出
[root@localhost ~]# gpasswd -d test test2 

# 设置用户不能修改密码
[root@localhost ~]# passwd -l test1     //在root下，禁止test1用户修改密码的权限
[root@localhost ~]# passwd -d test1    //删除test1的密码
[root@localhost ~]# passwd -S test1     //查看test1的密码

# 删除用户：
# 删除用户username
[root@localhost ~]# userdel username  

# 删除用户username所在目录
[root@localhost ~]# rm -rf username   

# 查看用户
# 示例： 查看当前登录的所有用户
[root@localhost ~]# w
[root@localhost ~]# who

# 示例：查看当前登录用户名
[root@localhost ~]# whoami
```

# 6、防火墙

## 6.1、查看firewall防火墙状态

```shell
[root@localhost ~]# firewall-cmd --state
# 或者
[root@localhost ~]# systemctl status firewalld
```

## 6.2、打开防火墙

```shell
[root@localhost ~]# systemctl start firewalld
```

## 6.3、关闭防火墙

```shell
[root@localhost ~]# systemctl stop firewalld
```

## 6.4、重启防火墙

```shell
[root@localhost ~]# firewall-cmd --relaod
# 或者
[root@localhost ~]# systemctl reload firewalld
```

## 6.5、开机自启动

```shell
# 开机自启动防火墙
[root@localhost ~]# systemctl enable firewalld

# 禁止开机启动防火墙
[root@localhost ~]# systemctl disable firewalld
```

## 6.6、查看已打开的端口

```shell
[root@localhost ~]# firewall-cmd --list-ports
```

## 6.7、打开端口

```shell
# 开放或者关闭端口都需要重启防火墙#
[root@localhost ~]# firewall-cmd --permanent --zone=public --add-port=8080/tcp
```

## 6.8、关闭端口

```shell
# 开放或者关闭端口都需要重启防火墙#
[root@localhost ~]# firewall-cmd --permanent --zone=public --remove-port=8080/tcp
```

> ```shell
> # 无法使用iptables
> [root@localhost ~]# yum install iptables-services
> [root@localhost ~]# systemctl enable iptables.service //设置开机启动
> 
> 重启防火墙：firewall-cmd –reload
> sudo systemctl stop firewalld 临时关闭
> sudo systemctl disable firewalld ，然后reboot 永久关闭
> sudo systemctl status  firewalld 查看防火墙状态。
> 开启防火墙(即时生效，重启后失效)：service iptables start
> 关闭防火墙(即时生效，重启后失效)：service iptables stop
> 重启防火墙:service iptables restartd
> ```

# 7、Linux安装远程桌面

> 需要linux安装桌面服务

- Xrdp在[EPEL软件](https://linuxize.com/post/how-to-enable-epel-repository-on-centos/)存储库中可用。如果您的系统上未启用EPEL，请输入以下命令启用它：

```shell
[root@localhost ~]# sudo dnf install epel-release
```

- 安装Xrdp软件包：

```shell
[root@localhost ~]# sudo dnf install xrdp 
```

- 安装过程完成后，启动Xrdp服务并在启动时启用它：

```shell
[root@localhost ~]# sudo systemctl enable xrdp --now
```

- 您可以通过键入以下命令来验证Xrdp是否正在运行：

```shell
[root@localhost ~]# sudo systemctl status xrdp
```

> xrdp.service - xrdp daemon Loaded: loaded (/usr/lib/systemd/system/xrdp.service; enabled; vendor preset: disabled) Active: active (running) since Sat 2020-09-05 23:00:31 EDT; 12s ago

- Xrdp配置文件位于/ etc / xrdp目录中。 对于常规Xrdp连接，只需将Xrdp设置为使用Gnome：

```shell
sudo nano /etc/xrdp/xrdp.ini
最后一行加
exec gnome-session
```

保存之后重启：

```shell
[root@localhost ~]# sudo systemctl restart xrdp
```

- 设置防火墙：

> 默认情况下，Xrdp侦听所有接口上的端口3389。 如果您在CentOS计算机上运行防火墙（应该始终这样做），则需要添加一条规则以允许Xrdp端口上的通信。通常，您只希望允许从特定IP地址或IP范围访问Xrdp服务器。 例如，要仅允许192.168.1.0/24范围内的连接，请输入以下命令：

```shell
[root@localhost ~]# sudo firewall-cmd --new-zone=xrdp --permanent
[root@localhost ~]# sudo firewall-cmd --zone=xrdp --add-port=3389/tcp --permanent
[root@localhost ~]# sudo firewall-cmd --zone=xrdp --add-source=192.168.1.0/24 --permanent
[root@localhost ~]# sudo firewall-cmd --reload
```

- 要允许从任何地方到3389端口的通信，请使用以下命令:

```shell
[root@localhost ~]# sudo firewall-cmd --add-port=3389/tcp --permanent
[root@localhost ~]# sudo firewall-cmd --reload
```

# 8、共享文件夹设置（Samba）

**安装vm-tools：**`yum install open-vm-tools`

- **安装samba**

```
[root@localhost ~]# yum install samba samba-common samba-client
```

- **添加用户** 如果想省事,直接使用root用户即可(在测试环境下建议使用root,方便开发)

```
[root@localhost ~]# groupadd samba
[root@localhost ~]# useradd -g samba samba
```

- **修改配置**

一般配置路径是：/etc/samba/smb.conf,先查找smb.conf确认路径

```
[root@localhost samba]# vim /etc/samba/smb.conf

[samba]
	comment = samba
	path = /home/samba
	browseable = yes
	writable = yes
	available = yes
	valid users = samba
	write list = samba
```

> comment：描述信息 
>
> path：共享目录 
>
> browseable：是否可浏览 
>
> writable：是否有写权限 
>
> available：可使用权限 
>
> valid：允许访问该共享的用户 
>
> write：可写入共享的用户列表

- **启动服务**

```shell
// 关闭防火墙
[root@localhost ~]# systemctl stop firewalld.service
// 开启服务
[root@localhost ~]# systemctl start smb.service
// smb密码
[root@localhost ~]# smbpasswd -a 用户名
// 关闭selinux
[root@localhost ~]# setenforce 0
```

> 假如文件创建人和所使用的用户不一样,那么就不能直接修改文件 1.执行以下命令改变用户即可。
>
> ```shell
> chown -R 用户名 文件名
> 
> chmod -R 777 /home/test/myshare
> chown -R test:test /home/test
> ```

# 9、Vim命令合集

**命令历史**

以:和/开头的命令都有历史纪录，可以首先键入:或/然后按上下箭头来选择某个历史命令。

**启动vim**

在命令行窗口中输入以下命令即可

**vim 直接启动vim**

vim filename 打开vim并创建名为filename的文件

**文件命令**

```
打开单个文件
vim file
同时打开多个文件
vim file1 file2 file3 ...
在vim窗口中打开一个新文件
:open file
在新窗口中打开文件
:split file
切换到下一个文件
:bn
切换到上一个文件
:bp
查看当前打开的文件列表，当前正在编辑的文件会用[]括起来。
:args
打开远程文件，比如ftp或者share folder
:e ftp://192.168.10.76/abc.txt
:e \\qadrive\test\1.txt
```

**vim的模式**

正常模式（按Esc或Ctrl+[进入） 左下角显示文件名或为空

插入模式（按i键进入） 左下角显示--INSERT--

可视模式（不知道如何进入） 左下角显示--VISUAL--

**导航命令**

% 括号匹配

**插入命令**

```
i 在当前位置生前插入
I 在当前行首插入
a 在当前位置后插入
A 在当前行尾插入
o 在当前行之后插入一行
O 在当前行之前插入一行
```

**查找命令**

```
/text　　查找text，按n健查找下一个，按N健查找前一个。
?text　　查找text，反向查找，按n健查找下一个，按N健查找前一个。
vim中有一些特殊字符在查找时需要转义　　.*[]^%/?~$
:set ignorecase　　忽略大小写的查找
:set noignorecase　　不忽略大小写的查找
查找很长的词，如果一个词很长，键入麻烦，可以将光标移动到该词上，按*或#键即可以该单词进行搜索，相当于/搜索。而#命令相当于?搜索。
:set hlsearch　　高亮搜索结果，所有结果都高亮显示，而不是只显示一个匹配。
:set nohlsearch　　关闭高亮搜索显示
:nohlsearch　　关闭当前的高亮显示，如果再次搜索或者按下n或N键，则会再次高亮。
:set incsearch　　逐步搜索模式，对当前键入的字符进行搜索而不必等待键入完成。
:set wrapscan　　重新搜索，在搜索到文件头或尾时，返回继续搜索，默认开启。
```

**替换命令**

```
ra 将当前字符替换为a，当期字符即光标所在字符。

s/old/new/ 用old替换new，替换当前行的第一个匹配

s/old/new/g 用old替换new，替换当前行的所有匹配

%s/old/new/ 用old替换new，替换所有行的第一个匹配

%s/old/new/g 用old替换new，替换整个文件的所有匹配

:10,20 s/^/  /g 在第10行知第20行每行前面加四个空格，用于缩进。

ddp 交换光标所在行和其下紧邻的一行。
```

**移动命令**

```
h 左移一个字符

l 右移一个字符，这个命令很少用，一般用w代替。

k 上移一个字符

j 下移一个字符

以上四个命令可以配合数字使用，比如20j就是向下移动20行，5h就是向左移动5个字符，在Vim中，很多命令都可以配合数字使用，比如删除10个字符10x，在当前位置后插入3个！，3a！<Esc>，这里的Esc是必须的，否则命令不生效。

w 向前移动一个单词（光标停在单词首部），如果已到行尾，则转至下一行行首。此命令快，可以代替l命令。

b 向后移动一个单词 2b 向后移动2个单词

e，同w，只不过是光标停在单词尾部

ge，同b，光标停在单词尾部。

^ 移动到本行第一个非空白字符上。

0（数字0）移动到本行第一个字符上，

<HOME> 移动到本行第一个字符。同0健。

$ 移动到行尾 3$ 移动到下面3行的行尾

gg 移动到文件头。 = [[

G（shift + g） 移动到文件尾。 = ]]

f（find）命令也可以用于移动，fx将找到光标后第一个为x的字符，3fd将找到第三个为d的字符。

F 同f，反向查找。

跳到指定行，冒号+行号，回车，比如跳到240行就是 :240回车。另一个方法是行号+G，比如230G跳到230行。

Ctrl + e 向下滚动一行

Ctrl + y 向上滚动一行

Ctrl + d 向下滚动半屏

Ctrl + u 向上滚动半屏

Ctrl + f 向下滚动一屏

Ctrl + b 向上滚动一屏
```

**撤销和重做**

```
u 撤销（Undo）
U 撤销对整行的操作
Ctrl + r 重做（Redo），即撤销的撤销。
```

**删除命令**

```
x 删除当前字符
3x 删除当前光标开始向后三个字符
X 删除当前字符的前一个字符。X=dh
dl 删除当前字符， dl=x
dh 删除前一个字符
dd 删除当前行
dj 删除上一行
dk 删除下一行
10d 删除当前行开始的10行。
D 删除当前字符至行尾。D=d$
d$ 删除当前字符之后的所有字符（本行）
kdgg 删除当前行之前所有行（不包括当前行）
jdG（jd shift + g）   删除当前行之后所有行（不包括当前行）
:1,10d 删除1-10行
:11,$d 删除11行及以后所有的行
:1,$d 删除所有行
J(shift + j)　　删除两行之间的空行，实际上是合并两行。
```

**拷贝和粘贴**

```
yy 拷贝当前行
nyy 拷贝当前后开始的n行，比如2yy拷贝当前行及其下一行。
p  在当前光标后粘贴,如果之前使用了yy命令来复制一行，那么就在当前行的下一行粘贴。
shift+p 在当前行前粘贴
:1,10 co 20 将1-10行插入到第20行之后。
:1,$ co $ 将整个文件复制一份并添加到文件尾部。
正常模式下按v（逐字）或V（逐行）进入可视模式，然后用jklh命令移动即可选择某些行或字符，再按y即可复制
ddp交换当前行和其下一行
xp交换当前字符和其后一个字符
```

**剪切命令**

```
正常模式下按v（逐字）或V（逐行）进入可视模式，然后用jklh命令移动即可选择某些行或字符，再按d即可剪切ndd 剪切当前行之后的n行。利用p命令可以对剪切的内容进行粘贴
:1,10d 将1-10行剪切。利用p命令可将剪切后的内容进行粘贴。
:1, 10 m 20 将第1-10行移动到第20行之后。
```

**退出命令**

```
:wq 保存并退出
ZZ 保存并退出
:q! 强制退出并忽略所有更改
:e! 放弃所有修改，并打开原来文件。
```

**窗口命令**

```
:split或new 打开一个新窗口，光标停在顶层的窗口上
:split file或:new file 用新窗口打开文件
split打开的窗口都是横向的，使用vsplit可以纵向打开窗口。
Ctrl+ww 移动到下一个窗口
Ctrl+wj 移动到下方的窗口
Ctrl+wk 移动到上方的窗口
```

**关闭窗口**

```
:close 最后一个窗口不能使用此命令，可以防止意外退出vim。
:q 如果是最后一个被关闭的窗口，那么将退出vim。
ZZ 保存并退出。
关闭所有窗口，只保留当前窗口
:only
录制宏
按q键加任意字母开始录制，再按q键结束录制（这意味着vim中的宏不可嵌套），使用的时候@加宏名，比如qa。。。q录制名为a的宏，@a使用这个宏。
执行shell命令
:!command
:!ls 列出当前目录下文件
:!perl -c script.pl 检查perl脚本语法，可以不用退出vim，非常方便。
:!perl script.pl 执行perl脚本，可以不用退出vim，非常方便。
:suspend或Ctrl - Z 挂起vim，回到shell，按fg可以返回vim。
```

**注释命令**

```
perl程序中#开始的行为注释，所以要注释某些行，只需在行首加入#
3,5 s/^/#/g 注释第3-5行
3,5 s/^#//g 解除3-5行的注释
1,$ s/^/#/g 注释整个文档。
:%s/^/#/g 注释整个文档，此法更快。
```

**帮助命令**

```
:help or F1 显示整个帮助
:help xxx 显示xxx的帮助，比如 :help i, :help CTRL-[（即Ctrl+[的帮助）。
:help 'number' Vim选项的帮助用单引号括起
:help <Esc> 特殊键的帮助用<>扩起
:help -t Vim启动参数的帮助用-
：help i_<Esc> 插入模式下Esc的帮助，某个模式下的帮助用模式_主题的模式
帮助文件中位于||之间的内容是超链接，可以用Ctrl+]进入链接，Ctrl+o（Ctrl + t）返回
```

**其他非编辑命令**

```
. 重复前一次命令
:set ruler?　　查看是否设置了ruler，在.vimrc中，使用set命令设制的选项都可以通过这个命令查看
:scriptnames　　查看vim脚本文件的位置，比如.vimrc文件，语法文件及plugin等。
:set list 显示非打印字符，如tab，空格，行尾等。如果tab无法显示，请确定用set lcs=tab:>-命令设置了.vimrc文件，并确保你的文件中的确有tab，如果开启了expendtab，那么tab将被扩展为空格。
Vim教程
在Unix系统上
$ vimtutor
在Windows系统上
:help tutor

:syntax 列出已经定义的语法项
:syntax clear 清除已定义的语法规则
:syntax case match 大小写敏感，int和Int将视为不同的语法元素
:syntax case ignore 大小写无关，int和Int将视为相同的语法元素，并使用同样的配色方案
```
