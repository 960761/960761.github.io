
About linux
参考连接：http://www.runoob.com/linux/linux-tutorial.html

## 1.	Windows连接linux  
windows连接windows通过remote desktop即可，但是要连接linux，不可以使用远程连接，要么使用ssh命令，要么使用特定工具，比如putty就是很常用的一种windows连接linux的工具，其基本原理也是ssh，也可以http。

## 2. linux之间互传文件
命令cp 用来实现服务器内部的复制，不能跨服务器。  
跨服务器下载上传文件使用scp , or sftp，这两种分别对应两种不同的连接方式：  
**SSH 方式**： scp 命令    
示例：将3810上面tmp上的check.sh文件复制到3811的tmp里面  
  scp /tmp/check.sh e610374@jabdl3811:/tmp  
注意格式。

**ftp方式**：sftp命令  
首先使用ftp链接上目标服务器，比如3811  
  sftp e610374@jabdl3811
根据提示输入密码，  
然后使用get进行文件下载，使用put进行文件上传

示例： 由3810 tmp 复制到 3811 tmp  
命令： 先在3810上面连接3811  sftp e610374@jabdl3811  
然后：put /tmp/check.sh /tmp/  
使用 bye or exit退出 ftp链接  

如果想要**复制文件目录**：  
Scp中添加 –r 参数即可，但是使用sftp时要先创建目录才可

正常情况下，这两种方式在连接时都需要输入密码，想要不输入密码使用scp，参考下面方法：  
首先，在自己的Linux系统上生成一对SSH Key：SSH密钥和SSH公钥．密钥保存在自己的Linux系统上。  
然后，公钥上传到Linux服务器．之后我们就能无密码SSH登录了．SSH密钥就好比是你的身份证明．

**无密scp**具体步骤：

首先，在3810使用 ssh-keygen，  
全部enter，默认的key 两个文件就保存在了自己目录下.ssh folder中，这里是 auto/users-2/e610374/.ssh中，会有 id_rsa和id_rsa.pub 两个文件，分别为私钥和公钥；   

然后，将id_rsa.pub复制到另一个linux上，这里就是3811  
使用ssh-copy-id e610374@jabdl3811 （ssh-copy-id是一个命令）  
这样，就将id_rsa.pub复制到了jabdl311上面对应的xx/e610374/.ssh/中，

最后，登陆到3811上面，或者通过3810远程连接3811，登陆后，将id_rsa.pub  check in 进入authorized_keys文件中，使用cat ~ /.ssh/id_rsa.pub >> ~/.ssh/authorized_keys，这样就将pub文件中的内容整合进了authorized_keys中。


## 3.	windows和linux互传文件  
a. 使用psftp.exe  
Open server Name( 比如jabdl3810)  
Input login name and pwd  
 get  /xx/xx/xx.out 将linux制定路径文件复制到psftp.exe 所在位置  
 cd /etc/init.d/  切换到xx目录（cd 后面一定要有个空格键）  
ls 查看目录下面的内容列表  

b. 使用一些图形化工具，比如winscp  

winscp可以选择方式，通常为scp, ftp, sftp等，其中scp要注意，运行scp命令的一个前提是安装ssh，windows系统默认是没有安装ssh的，如果要使用scp或者基于scp的工具，那么必须在windows上安装ssh，否则会出现connection refused。

## 4.常用命令

使用cd空格 路径，导航到指定路径；  
Pwd 指示当前工作路径   
ls显示当前路径文件  

输入输出重定向：  
http://webcache.googleusercontent.com/search?q=cache:mNT79oAbqRUJ:www.runoob.com/linux/linux-shell-io-redirections.html+&cd=1&hl=en&ct=clnk&gl=us

## 5.	Init.d目录  
The init.d directory contains a number of start/stop scripts for various services on your system.  
要使用格式  /etc/init.d/command OPTION来启动一项服务，需要有root 权限，  
比如： /etc/init.d/http start 来启动http服务

## 6.	Etc/Init.d 和etc/init的区别：  
Linux中服务启动方式有三种，分别为：  
1.只从/etc/init.d/文件夹启动：  
/etc/init.d/mysql start  
2.只从/etc/init/文件夹启动(Ubuntu提倡)：  
sudo start mysql  
3.从两个文件夹中启动：  
service start mysql  
简而言之，/etc/init.d/就是旧时代liunx的用法，/etc/init/是现在Ubuntu的提倡并一步步转型的用法。为了平缓过渡，便让service命令可以同时寻找到两个文件夹。

## 7.	/etc/passwd 存放账户信息：  
passwd文件是以行为单位的配置文件，每行定义系统上的一个用户，行内分为字段，字段之间由一个冒号隔开。这些字段依次为：  
用户名：密码：用户ID：主要组ID：GECOS：主目录：登录shell  
username:password:uid:gid:allname:homedir:shell  例如：
root:x:0:0:root:/root:/bin/bash   
apache:x:48:48:Apache:/var/www:/sbin/nologin  

  字段解释：  
  用户名：就是一个用户名，登录时候用的  
  密码：在旧的UNIX系统上，这个字段含有用户的加密密码，为了安全性，现在的linux均显示为x或*号  
  用户ID：linux内核用于识别用户的一个整数ID  
  主要组ID：linux内核用于识别用户主要组的一个整数ID  
  GECOS：用户全名，安装linux时如果不输入全名，则显示为跟用户名一样，如果输入，则显示为全名（不可用于登录）  
  主目录：用户登录时，他的登录Shell将使用这个目录作为当前工作目录  
  登录Shell：用户登录时的默认Shell，在redhat 企业版中，登录shell通常是/bin/bash  

## 8.	用户类型  
3种类型的用户    
普通用户：普通用户是使用系统真实用户人群。普通用户通常把/bin/bash作为登录Shell和/home的子目录作为主目录。一般情况下，普通用户只在自己的主目录和系统范围内的临时目录里（如/tmp和/var/tmp）创建文件。在redhat企业版linux中，普通用户的用户ID数通常大于500.

root用户：用户ID为0的用户，也被称为超级用户，root用户在系统上拥有完全权限，可以修改和删除任何文件，可以运行任何命令，可以取消任何进程。root用户负责增加和保留其他用户、配置硬件、添加系统软件。虽然root用户可以在系统上的任何地方创建文件，但它也通常使用/root作为主目录    

系统用户：大多数linux系统保留一系列低UID值用户作为系统用户，系统用户不代表人，而代表系统的组成部分。例如，运行Apache网络服务器的进程经常作为用户apache（见上面的passwd文件中apache用户信息）来运行。系统用户一般没有登录Shell，因为它不代表实际登录的用户。同样，系统用户的主目录很少在/home中，而通常在属于相关应用的系统目录中。例如，用户apache的主目录是/var/www。在redhat企业版linux中，系统用户的UID值范围在1-499之间。  

## 9. ls -l 命令详解    
ls -l（这个参数是字母L的小写，不是数字1）  
这个命令使用长格式显示文件内容，如果需要察看更详细的文件资料，就要这个    
例如我在某个目录下键入ls -l可能会显示如下信息（最上面两行是我自己加的）：  
位置1 2 3 4 5 6 7    
文件属性 文件数 拥有者 所属的group 文件大小 建档日期 文件名     
drwx------ 2 Guest users 1024 Nov 21 21:05 Mail     
-rwx--x--x 1 root root 89080 Nov 7 22:41 tar*    
-rwxr-xr-x 1 root bin 5013 Aug 15 9:32 uname*   

第一列为文件种类及权限。此列共有10个字符，其中第一个字符表示文件的种类。即，-表示是普通文件，d表示为目录，c表示为字符设备，b表示为块设备。而紧跟其后的10个字符，可以分为3块，每3个字符为一块，表示了此文件（目录）的属主、属组及others的权限。其中，r表示read，w表示write，x表示execute，-表示无权限。　    

第二个栏位，表示文件个数。如果是文件的话，那这个数目自然是1了，如果是目录的话，那它的数目就是该目录中的文件个数了。

第三个栏位，表示该文件或目录的拥有者。若使用者目前处于自己的Home,那这一栏大概都是它的账号名称。 

第四个栏位，表示所属的组（group）。每一个使用者都可以拥有一个以上的组，不过大部分的使用者应该都只属于一个组，只有当系统管理员希望给予某使用者特殊权限时，才可能会给他另一个组。　　    

第五栏位，表示文件大小。文件大小用byte来表示，而空目录一般都是1024byte，你当然可以用其它参数使文件显示的单位不同，如使用ls –k就是用kb莱显示一个文件的大小单位，不过一般我们还是以byte为主。　　  

第六个栏位，表示创建日期。以“月，日，时间”的格式表示，如Aug 15 5:46表示8月15日早上5:46分。

第七个栏位，表示文件名。我们可以用ls –a显示隐藏的文件名。  

一个文件被创建之初的默认权限为：创建者 本身可读可写不可执行，group ,others只可读， 不可写也不可 执行。 

## 10. 文件权限
一个文件被创建之初的默认权限为：  
创建者 本身可读可写不可执行，group ,others只可读， 不可写也不可执行。

修改权限方法如下：  
chmod u+x hello.sh  
chmod :命令名称  
u+x:  role name + permission name  
     u: owner 第二列  
     g: group 第三列  
     o: others 第四列  
hello.sh 所要修改的文件名称   

## 11. root权限

在linux操作系统下，root就是超级管理员或最高权限，类似windows操作系统的administrator。所以获取了root权限，就等于获取了linux操作系统的控制权。

获取root权限并非字面意思赋予一个用户root级别的权限，而是让这个用户能够以root的身份登录。可以理解为不是将权利下放给你，而是让你代替root行驶权利。

在Linux下对很多文件进行修改都需要有root（管理员）权限，比如对/ect/profile等文件的修改。很多情况下，我们在进行开发的时候都是使用普通用户进行登录的，尤其在进行一些环境变量的配置工作时，常常需要对一些文件进行修改。

那么我们如何获取管理员权限呢？

一般来说，有两种方法。  
一是：利用su命令切换到root用户，在root用户下对那些文件进行修改，完成相关配置工作。  
二是：利用su命令切换到root用户，修改/etc/sudoers文件，让普通用户具有sudo权限，然后利用su命令切换回普通用户，在执行相关命令前加上sudo。

详细获取方式可参考  
https://webcache.googleusercontent.com/search?q=cache:rEk94dZsP4wJ:https://blog.csdn.net/guoweimelon/article/details/50471561+&cd=1&hl=en&ct=clnk&gl=us


## 12.	linux运行脚本.sh方法：  
a. 直接./加上文件名.sh，如运行hello.sh为./hello.sh【hello.sh必须有x权限】  
b. 直接sh 加上文件名.sh，如运行hello.sh为sh hello.sh【hello.sh可以没有x权限】  

## 13. 查看是否开启某项服务

ps –ef | grep ftp     查看是否有ftp进程在运行  
chkconfig --list       # 列出所有系统服务  
chkconfig --list | grep on    # 列出所有启动的系统服务  
rpm  -qa           查看所安装的软件包  
rpm –qa | grep ssh  查看特定的软件包安装，比如ssh  
ps –e | grep ftp    查看ssh是否运行


## 14. 关于RPM
在Linux 操作系统中，有一个系统软件包，它的功能类似于Windows里面的“添加/删除程序”，但是功能又比“添加/删除程序”强很多，它就是 Red Hat Package Manager(简称RPM)。此工具包最先是由Red Hat公司推出的，后来被其他Linux开发商所借用。由于它为Linux使用者省去了很多时间，所以被广泛应用于在Linux下安装、删除软件。

1.我们得到一个新软件，在安装之前，一般都要先查看一下这个软件包里有什么内容，假设这个文件是：Linux-1.4-6.i368.rpm，我们可以用这条命令查看：  
rpm -qpi Linux-1.4-6.i368.rpm
系统将会列出这个软件包的详细资料，包括含有多少个文件、各文件名称、文件大小、创建时间、编译日期等信息。  
2.上面列出的所有文件在安装时不一定全部安装，就像Windows下程序的安装方式分为典型、完全、自定义一样，Linux也会让你选择安装方式，此时我们可以用下面这条命令查看软件包将会在系统里安装哪些部分，以方便我们的选择：  
rpm -qpl Linux-1.4-6.i368.rpm  
1. 选择安装方式后，开始安装。我们可以用rpm-ivh Linux-1.4-6.i368.rpm命令安装此软件。在安装过程中，若系统提示此软件已安装过或因其他原因无法继续安装，但若我们确实想执行安装命 令，可以在 -ivh后加一参数“-replacepkgs”：
rpm -ivh -replacepkgs Linux-1.4-6.i368.rpm    
4.有时我们卸载某个安装过的软件，只需执行rpm-e <文件名>;命令即可。  
5.对低版本软件进行升级是提高其功能的好办法，这样可以省去我们卸载后再安装新软件的麻烦，要升级某个软件，只须执行如下命令：rpm -uvh <文件名>;，注意：此时的文件名必须是要升级软件的升级补丁  
6. 另外一个安装软件的方法可谓是Linux的独到之处，同时也是RMP强大功能的一个表现：通过FTP站点直接在线安装软件。当找到含有你所需软件的站点并 与此网站连接后，执行下面的命令即可实现在线安装，譬如在线安装Linux-1.4-6.i368.rpm，可以用命令：  
rpm -i ftp://ftp.pht.com/pub/linux/redhat/...-1.4-6.i368.rpm   
7. 在我们使用电脑过程中，难免会有误操作，若我们误删了几个文件而影响了系统的性能时，怎样查找到底少了哪些文件呢?RPM软件包提供了一个查找损坏文件的 功能，执行此命令：rpm -Va即可，Linux将为你列出所有损坏的文件。你可以通过Linux的安装光盘进行修复。  
8.Linux系统中文件繁多，在使用过程中，难免会碰到我们不认识的文件，在Windows下我们可以用“开始/查找”菜单快速判断某个文件属于哪个文件夹，在Linux中，下面这条命令行可以帮助我们快速判定某个文件属于哪个软件包：
rpm -qf <文件名>;  
9.当每个软件包安装在Linux系统后，安装文件都会到RPM数据库中“报到”，所以，我们要查询某个已安装软件的属性时，只需到此数据库中查找即可。注意：此时的查询命令不同于1和8介绍的查询，这种方法只适用于已安装过的软件包！命令格式：
rpm -参数　<文件名>;  

## 15. linux中的日志
linux系统日志文件位于/var/log目录下  
/etc/rsyslog.conf文件决定了哪些内容会被写入到对应的日志文件中

## 16. linux 脚本自动交互

Linux shell自动交互三种方法：  
http://webcache.googleusercontent.com/search?q=cache:ZZHhkNZH1SYJ:os.51cto.com/art/200912/167898.htm+&cd=1&hl=en&ct=clnk&gl=us

expect，可以理解为带有参数的命令行，用于自动化地执行linux环境下的命令行交互任务，例如scp、ssh之类需要用户手动输入密码然后确认的任务。可以用它来实现linux脚本自动输入密码，expect是一种工具，使用前需要下载安装。具体的实现可以google。
