Notes-learn AD in a month of lunches
《learn Active Directory Management in a month of lunches》一书的笔记。

AD相关工具：位于start > administrative tools

ADAC, ADUC: AD administrative Center, AD users and computers，用于管理users, computer, group, OU等object，也可以查看DC（domain controller）；

ADSS ： AD sites and services，用于AD物理架构相关；

ADDT：AD Domains and Trusts，用于管理跨domain或跨forest的权限问题；

其余相关的有 ： DNS

<font color=red>2018.3.1：day 1:  chapter1-2: what is AD and how to create account?</font>

1.	AD 是 Directory services产品类中的一个部分，  
官方定义： AD provides a centralized service that links all machines in an organization and allows a user to log on and access any of them they have been granted permission to do so.
2.	AD 的核心是 authentication and authorization。
3.	Authentication is the act of proving that you are what you say you are.  
通常归为这几类：   
Something you know:  login ID and pwd   
Something you have:  smart card or soft token  
Something you are:  fingerprint or other biometrics  
在AD中要注意的有几点：  
1.你登录的是一个domain，而不是individual computer；2.AD使用的是Kerberos protocol for authentication；3.和AD的时间同步非常重要，如果你的PC time和AD的时间有五分钟之上的误差，认证就会失败。
4.	Authorization is the act of granting users appropriate access to resources。  
基本原理简化来讲就是，当你登录一个computer时，在这个机器上就会生成一个token，上面记录了你的membership, privileges and rights，在你访问一些资源时，系统就会检测你是否有权访问。当你访问远程资源时，这个token就会在你访问资源所在的远程机器上生成，并由远程机器的system进行检测是否有权限。所以，token(also known as access token) are never transmitted across the network from one machine to another。
5.	Authentication and authorization合起来就是 identity management，这种身份管理的服务就是AD提供的，AD的作用就是为organization提供身份管理服务.
6.	术语  
 forest:  the whole of AD, one or more domains arranged in trees. 同时forest is also security boundry，forest之间是不可以相互访问的，除非通过特定的配置；  

domain: a container for the objects you will work with, including users, computers, groups and so on。它规定了很多界限，包括administrative boundary, security boundary, policy application boundary。一般一个公司需要一个domain就可以了，但是比较大的公司可能会有数个。 如何创建一个domain？  

Domain的唯一标识符称为 DNS: fully qualified domain name   system name    

Organization unit : OU, a container within a domain that can be used to hold users, computer, group and other OU objects，为了方便管理。  

可以将domain想象成  我的电脑 里的各个硬盘，OU则是每个硬盘里的文件夹，硬盘的分区改变起来是非常困难的，但是文件夹的建立和管理就方便很多。  

还有一种容易和OU混淆的container的概念，经常会遇到的有：    
 Built-in, 用来存储一些默认groups，Users, 用来存储另外一些默认组，通常为各种管理员，当创建一个新的user 但是没有分配OU时也会被默认保存到这里，computers，新建computer account会被保存到这个里面。  
 虽然OU和container很相似，但是有区别的，主要有两点：group policy 不可以使用到container，container中不可以创建OU。

7.	作为AD administrator，经常打交道的是user account，虽然computer account也是一种特殊的user account，但是易变性却大大低于user account， user account是变动最频繁的。还有一种Managed Service accounts，是为了某些services such as Exchange, SQL server创建的，这些账户的变动可能会影响到服务的正常使用。

8.	创建新用户：通常使用GUI or Powershell，当需要大批量创建时可以使用bulk-creation。示例代码可参考书本。  
经常用到的工具有：    
ADAC ：AD Administrative Center，在windows server 2008R2中首次引进，是基于powershell cmdlets的；  
ADUC： AD Users and Computers是一个很古老的工具，用途和ADAC差不多；  
从windows server 2008R2开始，a powershell module(set of commands) is available for administering AD.  
在jabdw3835中可以打开ADAC，在tasks 中New是灰色的，也即没有权限创建new user。ADAC， ADUC以及其余的AD工具都可以在   start>administrative tools中找到。  
创建new user account，可以选择ADAC, ADUC, Powershell，根据所拥有的工具和个人偏好来选择使用哪一种。  
书中给出了三种方法使用最少数据创建新用户的步骤，但是实际中创建新用户时会需要输入很多其他信息，输入的信息多了之后就很容易出错，所以这个时候可以使用template，也即使用一个已经创建好的account作为模板复制来创建新account，在复制中只需要输入新的name, samAccountName即可，其他的信息就可以不用输入了。  
使用template创建时只能使用ADUC or  PowerShell，不可以用ADAC。
9.	Bulk creation:  首先创建一个 comma-separated value(CSV) file用来放置用户信息，然后使用powershell cmdlets来实现批创建。示例代码如下： 
10.	创建managed service accounts：   
managed service account是在windows server 2008R2中被引进的，used to run services such as SQL server, Exchange and so on. ADAC 没有这种账户的选项，虽然 ADUC可以选择，但是也不能够准确地创建，推荐的方式是使用  New-ADServiceAccount  Powershell cmdlet。

<font color=red>Chapter 3: how to delete and update account?</font>

11.	使用GUI 修改时，选中所要修改的account, right click > properties，找到修改即可，使用powershell 时使用 Set-ADUser 。
12.	Enable/disable an account。可以使用GUI 或power shell cmdlet， Get-ADUser –Identity NAME | Disable-ADAccount/Enable-ADAccount。
13.	当delete an account时，短期内这个account并没有真正从这里消失，只是tombstoned(windows 2008 and early) or moved to AD recycle bin。在tombstone period(60 or 180 days)内都可以被恢复，但是tombstoned account的大部分属性都被清空，还需要重新配置，AD recycle bin里面的账户属性还是完好的。如果PAD(Protect from Accidental Deletion)被开启，是不可以被删除的，需要首先 关闭这项属性才可以删除。当使用powershell 删除时，PAD开启时删除失败但不会提示原因，这是可以查看PAD的状态，并使用powershell 设置为false时再删除。

14.	根据bulk-creation，也可以进行bulk-modification，也是将新值写入到csv file，然后类似bulk-creation，只是其中的New-ADUser变成了Set-ADUser。

<font color=red>3.2 day 2: chapter 4, manage groups; chapter 5, troubleshooting users and groups; chapter 6, manage computer account; chapter 7, manage OU.</font>

15.	在AD中，一般都是通过groups来进行权限设置的，在同一个group里面最好不好混合user and computer accounts。
16.	有两大类的groups，分别为security groups，主要用来分配权限；distribution lists，主要用于Exchange中给多人发邮件。前一种多由AD admin来管理，后一种多由Exchange admin来管理。
17.	Group有三种scope，controls where the group membership can come from and where the group can be used。  
参考table 4.1。   
三种分别为： Domain Local,  Global, and Universal  
通常的使用方法是，将user accounts(include user, computer, other groups)添加到Global groups，将Global groups 添加到Domain Local groups，然后再给这个domain local group赋予相应的权限。
18.	创建group可以使用GUI or powershell，需要指明container or OU，三项必填信息为 OU, Name, samAccountName，关于分类和scope在创建时都可以选择，如果不选默认为security global group。实践中我没有权限，new 是灰色的，新建任何用户都是不可以的。
Powershell中操作group相关的命令有：  
 
New-ADGroup,  Set-ADGroup, Remove-ADGroup, Get-ADGroup  
还有一组命令用来修改group member ship:  
Add-ADGroupMember,  Remove-ADGroupMember,  Get-ADGroup?

19.	修改group category不常见，一般建议security and distribution 两种是分开的，修改group scope时有些复杂，有些转换是不被允许的，参考tabel4.3   
Universal group是可以转换为其余两种的，而global 和domain local间一般不可以直接转换的，都需要通过universal 做中转。

20.	除了对groups本身的管理之外，经常用到的就是对group membership的管理了，通常为view/add/remove membership，也是可以通过GUI or powershell来完成。
21.	AD经常遇到的问题就是用户无法登陆或者无法访问资源，大部分会是密码的问题。  
遇到账户问题时，第一步就是通过GUI or powershell 来查看这个用户信息，to check if the account is disabled/has expired/is locked out。推荐的方法是使用powershell script，可以对几种常用的情况书写script，比如搜索disabled accounts等。

22.	AD passwords need to be changed every a period of time。所以用户需要在这个期限前修改密码，如果没有修改，就会出现password expire，使用GUI是无法发现的，使用powershell可以查找password expire的用户 serach-ADAccount –passwordExpired，如果出现这种情况，需要进行password reset。当忘记密码时，也需要password seset。
23.	当用户输入错误密码过多次会导致账户lockout，不可以通过任何方式强制一个账户lockout，对于lockout的用户管理员可以unlock通过GUI or powershell。
24.	无法访问资源的原因多为该用户没有在正确的group里，可以通过GUI or powershell来查看或修改，复杂的操作应该使用powershell  script来实现。
25.	Computer account needs to join the domain。  
在control pane>system>advanced system > computer name中可以修改computer 所属的domain，当进入这个domain之后就会在找个domain中自动生成一个computer account，这个computer account默认是在computer container里面的。  
上面讲述的是其中一张，在computer join a domain的时候自动生成账号，还有一种是先创建账号再连接。可以这样来理解吧，一种是现在屋子里放一个座位（AD里创建computer account），然后把东西拿进来放上去（join computer with domain），实现方法是在创建computer account时填写名字时即填写想要join的computer name(?)；  
另一种是在把东西拿进来的同时加上一个座位（join the computer  while create the account），join computer to domain也不是每个人都可以做的，必须有此权限才可以。是没有权限创建computer account的，所以没有办法验证。
26.	AD domain里的computer通过一种secure channel与domain controller沟通，通过这种渠道进行认证authenticate。一般都不会出现问题，出问题时可以使用powershell来检测是否为secure channel出了问题，并进行修复。
27.	An OU is a container within a domain that can be used to hold user, computer, group, and other OU objects。  
使用OU的原因有二，一是为了control the delegation of administrative privileges，即某些administrators只能配置某些特定OU里的objects；二是为了control the application of Group Policy。要注意，OU是为了管理admin privilege，不是permission，所以OU是不可以被赋予permission的，这是不同于groups 的地方。可以使用OU来映射organization structure or geographic locations。

<font color=red>以上chapter1-7为第一部分的内容，主要是最基本的操作，包括create account, manage account(user, computer, group, OU) and  troubleshooting user account。

Day 3 3.7 :   
chapter8-9 create and manage GPO,   
chapter 10: fine-grained password policies(PSO: Password Settings Object)</font>

28.	Group Policy is a method of managing the configuration and security of the computers in your environment. GP has nothing to do with groups. A GPO(group policy object)用来定义一些列的配置，比如控制面板上面用户能够看到哪些内容，start menu或IE里那些功能是可用的等。都可以通过定义GPO，然后将users or computers  link to OU  or domain。GPO非常强大，可以包含上千条的设置来管理你的环境。
29.	使用不当GPO很容易出现问题，所以使用时一定要谨慎，几条原则尽量遵守，比如将users and computers settings 放在不同的GPO里，使用尽量少的GPO解决问题，尽量避免GPO之间复制配置信息。在创建AD最开始会有两个默认的GPO为 default Domain policy, sets the password policy for domain; default Domain Controller policy sets the default security on the domain controllers。
30.	创建GPO要在特定的工具GPMC: Group Policy Management Console中进行。我们是没有权限看到这个工具的，所以也就不能实践了。使用powershell create GPO 时要加载 GroupPolicy module。
31.	Starter GPO, function as template for creating new GPOs。
32.	每一个GPO里面都分别包含有针对computers AND  users 的config setting.两者是分开的，而且针对每一种又有policy and preference两种，后者比前者更灵活，可以被override，policy如果没有被applied to computer or user，会被自动删除，preferrence不会。
 
33.	GPO的使用又称为linking，你可以将GPO应用于三种类型：domain, OU, AD site(applied to all users and computers in that site)，其中用于site是不推荐的用法。可以使用GPMC or powershell两种方式进行link。你可以在GPMC工具中查看某个OU都link 了那些GPO，你也可以查看某个GPO都被Link到了哪些OU。不需要某种Link时，可以unlink。
34.	因为GPO可以在多种层次上应用于AD，所以会引起很多conflict，因为GPO有一些应用order用来调解各种conflict。另外还有各种block or override等等，详细可参考文档。
35.	如果需要模拟一些GPO配置的实验结果，可以使用Group Policy Modeling 功能。

<font color=red>以上为part 2 部分的内容，主要是讲解GPO的使用和配置。  
Part 3: 之前的内容主要讲的是AD作用的对象及应用于这些对象之上的policy，这一部分主要讲解AD service本身，包括数据存储DC，物理结构等。</font>

36.	DC(Domain controllers) are essential to your AD, they are the machines that store a copy of the AD database and handle the authentication and authorization requests from your users.
 
A DC is a server within the domain that has AD Domain Services installed. It hosts a copy of the AD database(ntds.dit) and the SYSVOL share(containing logon scripts and Group Policy data files).DC respond to authentication attempts by users and computers. They also store the data that controls access to resources like email system and file stores.  

简单说，DC的主要作用是用来控制user authentication and authorization.

37.	创建DC，也即在server manager上面安装AD domain service role。  
当一个computer成为一个DC之后，这个computer account 就被转移到了 Domain Controller OU里面。也即，DC实际上就是一个server，也即computer account，所以它的查看和管理可以在ADAC和ADUC中进行，本质上就是 一个computer account 的管理。

38.	RODC(Read-Only Domain Controllers) don’t have a copy of the database，必须依赖一个fully DC 进行认证，all copies are one-way，所以在RODC上面是不可以修改密码的，必须是在fully DC上面进行修改，然后复制到RODC。

39.	DC are domain-specific，你可以用一些方法来查看所有的DCs及他们提供的service。DC management是AD management一个很重要的组成部分。

40.	Protect you AD data,   
文中提到四种，一，PAD: protection accidental deletion，防止误删，如果真的需要删除，可以先更改这个设置然后再删除；二， snapshots，提供point-in-time copy of data in AD，可以在某一个时间节点进行snapshot ，然后和当下进行对比，以便于还原某些做过的改动；三，AD recycle Bin，这个是在windows server 2008 R2中引进的，可以实现undeleting objects；四，backup and restore。对那些数据进行backup以及如何restore比较重要。  
文中对AD security 进行了更多的介绍，主要是default groups and delegation.

41.	A DNS  zone is a partition in the DNS  namespace，one DNS zone will match the name of your AD domain.   
Forward lookup zone指的是由machine name查找IP，reverse lookup zone则是指的由IP address 查找machine name。你可以把DNS zones想象成AD里面的OU，containers for DNS records。

42.	DNS forwarders are created to enable your DNS servers to forward requests to a specific domain。  
即当下DNS解析不了的时候要交给forwarders （可以理解为上一级的DNS ）来解析。Normal forwarder will forward all requests to one or more DNS servers, a conditional forwarder will only forward requests for the one domain。  
可以将general forwarder理解为全通道，conditional forwarder理解为特定通道，只处理特定的requests 到特定的web site。

43.	DNS record: 将DNS理解为电话本，那么DNS record即为每一个条目，即具体的哪个machine name 对应哪个IP。如果是IPv4，则称为 A record；如果是IPv6，则称为AAA records。  
所以，配置DNS也只是将现有的对应关系以条目（DNS record）的形式添加到电话本（DNS系统）中，方便别人查阅。条目对应关系为machine name <> IP address；其中machine name为JABDW3835.corp.statestr，这里就用到了domain。之前开发provider-hosted app时，需要创建一个新的domain，新的domain如何创建？

44.	AD是一个IT system，like any other one,需要存储数据和物理结构。AD存储数据的地方就是DC(Domain controller, is servers)，AD的物理架构中涉及到的名词就是sites, subnets, site links等，对应AD sites and services GUI。

45.	AD的物理架构中包括三种组件： sites, subnets , site links。  
A site is a collection of machines connected at LAN speed.  
  A subnet defines a set of IP addresses.  
A site link is a logical link in AD between two sites。  
可以在ADSS GUI中查看，ADSS中显示的servers都是DC，非DC server在这里是看不到的，你也不可以通过ADSS来管理DC。详情参考chapter 16.

46.	AD objects(user, group, computer)可能位于不同的DC里面，当一个DC上面object info发生改变之后，其他DC上面对应的也要进行同步的更改，这个过程就称为 replication。详情参考chapter 17.

47.	不同的AD之间的交互涉及到AD trust。相关工具为AD  domains and trusts。在同一个forest里面，parent-child之间的domains会自动生成双向的trusts，non-parentchild的domain可以生动生成shortcut trust。更多的还是指跨forest的external trusts。详情参考chapter 18. P263
 
48.	AD maintaining and monitoring 参考part 4.
