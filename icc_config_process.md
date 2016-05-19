# 目录
* [20160519重新安装icc](#reinstall_icc)
# 网络信息
* 4LMINI 密码: 1234567890
* wifi818 密码: 1234567890
* Landsea4L   密码: asd456789
* 内网    192.168.103.119,    192.168.103.254  DNS 218.2.135.1
* 安装电脑    192.168.1.188   Administrator   landsea
# 基础安装
1. 安装 .net Framkework 4 (done)
2. 安装 .net API   (网址都是 http://192.168.1.60:9081) (done)
3. 安装 outlook    (没有输入密钥) (done)
4. 安装 CC_SVR  
    * 集群名称    icc_cluster  
    * 默认    端口11443  
    * 需要 filenet client  
5. 安装 filenet client (done)  
6. 安装 CC_SVR (done)
    资源值单元  RVU
    CN=filenet,CN=Users,DC=m,DC=landsea,DC=cn 密码: filenet123 (验证失败)
    filenet@m.landsea.cn 密码: filenet123
    CN=filenet,OU=landsea,DC=m,DC=landsea,DC=cn 密码: filenet123 (验证成功)
    FileNet P8 服务器配置
        http://192.168.1.60:9081/wsi/FNCEWS40MTOM
        p8admin
        password
    FileNet P8 任务用户
        p8admin
        password
    FileNet P8 域和对象库
        FNOS
        p8domain
        无
    启动配置
        创建 FileNet P8 文档类失败 见日志 AfuInitialConfigTrace_01.txt (继续操作)
    新建任务
        P8_CSS_EX_2.1 - Space Saving (all email) .ctms
        修改 EC 收集30天之前的电子邮件为 
            活动
            收集源添加: 服务器上的邮箱:https://m.landsea.cn
            删除源: 服务器上的邮箱:server.company.com
            过滤: 不选择 按时效过滤电子邮件
            无法选择filenet p8文档类

# 其他信息
1. 远程链接: https://apps.na.collabserv.com/meetings/join?wrapped=true&launchpt=stmeet_url&id=7636-1754
2. 用的是http，所以需要安装微软软件。 链接 https://support.microsoft.com/en-us/kb/943508
3. 访问链接获取下载地址: http://hotfixv4.microsoft.com/Web%20Services%20Enhancements%203.0%20for%20.NET/latest/WSE3QFE1/3.0.6282.0/free/383444_intl_i386_zip.exe

# 按照IBM陈文源要求重新安装filenet client [安装记录](./20160328重新安装Client)
然而还是只有https， 重新安装filenet icc :   ./20160328重新安装icc
    创建P8文档类仍然失败，失败的日志见 AfuInitialConfigTrace_328_01.txt
    按照要求，重新打开icc set-up 配置界面，这次可以创见filenet 文档类了 ./20160328重新配置icc

<p id="20160329"></p>
# 配置ICC [安装记录](./20160329配置ICC)
1. 连接器-电子邮件-链接-自动配置
    * 没有输入账号密码的窗口    01.png
    * 而安装文档上有输入帐号密码的地方 02.png
    * 使用文档上的配置参数无法进行下一步操作，只能默认    03.png
    * 设置IBM Filenet P8连接器    04.png
2. 配置任务系列
    * P8_CSS_EX_1.1 - Default Archiving (automatic).ctms  05.png
    * EC 收集一个月之前的电子邮件
        * 常规    选择活动    06.png
        * 收集源添加  服务器上的所有邮箱
            * 删除 server.company.cn
            * 邮件服务器: 192.168.1.105   07.png
    * 缺省归档    设置为活动  08.png
    * P8查找重复邮件
        * 文档类  ICCMail 2   09.png
    * P8归档电子邮件
        * 公共设置
            * 安装文档要选 ICCMail 3  11.png
            * 但是只有文档类  ICCMail 2   10.png
        * 实例设置
            * 属性映射 默认没选，无法进行下一步 12.png
            * 选择了 ICCMailInstance 2
    * 点击保存
3. 启动 IBM Content Collector Task Routing Engine(默认没启动) 13.png
4. 点击工具的控制面板  查看状态    14.png
5. 更改第一次启动收集时间为 12:00 并保存   15.png
6. 重动 IBM Content Collector Task Routing Engine
7. 然而系统里面没有日志    16ICC没有系统日志.png, 系统日志 16ICC日志log.zip
8. 按照IBM人员要求修改配置，但是无法修改。LDAP认证失败 17AD验证失败.png
9. 操作AD域以后记录    ./20160329AD域修改配置
    * 可能需要加入AD域，按IBM人员要求加入AD域 01.png
    * 之后关闭，保存，回退都会报错    02.png
    * 重启服务后  0.3png  重启计算机
    * 然而还是没有系统日志 audit 没有文件， 日志  03加入AD域后的日志.log.zip
    * IBM人说可能要修改邮件，修改配置，无法链接到exchange。准备明天联系   04.png  05.png  06.png
10. 设置outlook以后，链接到了ad域，重启服务 
    * 仍然不行。用ad域打开outlook打不开，如果是
11. 重新安装 outlook    ./20160330重新设置ad邮箱
    * 但是用flenet登陆域以后无法登陆exchange服务器    失败    01.png
        * filenet@m.landsea.cn    filenet123
    * 使用管理员登陆以后  也不行  失败    02.png
    * 域登陆  filenet@landsea.cn  filenet123  失败    03.png  04.png
    * 管理元登陆  filenet@landsea.cn  filenet123  成功    05.png
        * 但是需要修改账户设置,无法发送邮件   06.png
尝试域内链接 exchange 服务  ./20160331域内链接exchange
    怀疑是否需要POP 和 IMAP SMTP服务
        检查账户状态    01.png  没有开启POP， IMAP， SMTP服务
        和陈文源确认，他说没有需要打开POP3, SMTP的说明， ICC 使用MAPI形式访问exchange的服务的。 
        怀疑是版本问题，查看 outlook 2007 百度发现需要 server 2003 及以上。系统是满足要求的 。 01.png
更改host能够链接到邮件服务器了. ./20160401连接exchange后配置
    已经能够用outlook发送电子邮件了 01.png
IBM人说outlook需要安装server pack补丁包,安装补丁包  ./0406升级outlook
    检查outlook原始版本 professional plus 2010 14.0.4760.1000(32位) 01.png
    下载32补丁包进行安装，提示安装失败，原因不明    02.png
        下载地址: http://download.microsoft.com/download/B/B/D/BBD98807-801E-4286-9D90-DBEBFA6809B9/officesuite2010sp1-kb2460049-x86-fullfile-zh-cn.exe
    尝试64位补丁包进行安装，未能找到期望的产品版本  03.png
        下载地址: http://download.microsoft.com/download/F/3/6/F3675AD9-0CD9-444C-9FDC-C85ECDD49E13/officesuite2010sp1-kb2460049-x64-fullfile-zh-cn.exe
    尝试使用windows的自带更新:  04.png
    询问朗诗是否有64位office, 有。安装office 2010 64位版本重新尝试
        结果陈佳佳给我的是2016的, 仍然无法运行。
准备安装2013的版本 ./20160407安装office2013
    安装后事件里面仍然没有记录  日志文件 office2013_64位的日志.zip
    需要修改注册表  查看陈工说的注册表  01.png,
    陈工说不行，原来outlook2013需要用32位的客户端
    安装了32位客户端以后，还是不行  02.png  日志文件 office2013_32位的日志.zip
# 重新按照IBM人员的要求重新配置Outlook    [安装记录](./20160408重新配置outlook)
    使用和outlook同样的账户登陆 ICC server  filnet登陆icc server ./01.png
    删除原来创建的outlook账户   02.png
    创建新的outlook账户     03.png, 04.png
    禁用Exchange cached mode.   05.png, 06.png
    检查Outlook 是否能链接上去,可以的  07.png
    重启服务,清空日志 IBM Content Collector web application 和 task routing engine
    结果:
        windows事件记录还是没有 08.png
        系统仪表板 已访问的文档数还是 '~' 09.png
        日志:   20160408_no_cached_log.zip
    把filenet用户加入组 Organization Management后重启
# 增加了filenet的权限以后重启ICC  [安装记录](./20160408增加filenet权限)
1. 增加了filenet用户的权限 01.png
2. 仍然没有事件记录， 系统仪表板还是 ~ 02.png
3. 日志    log.zip
# 重新设置 outlook    [安装记录](./20160412重新配置outlook)
    删除配置文件    
    提示使用的配置文件  01.png
    新建配置文件    AFU_M.LANDSEA.CN
        02,03,04
    只能自动配置
    重启 ICC 服务
        停止服务，删除日志
        启动 web application
        然后启动 task route engine
        然而归档的文件数还是 ~  05.png  因为没归档，但是audit里面有文件了
    使用自定义的归档功能启动
        有事件  06.png
        audit 文件夹没有文件
        log 日志见 20160412自定义归档任务的日志 20160412_服务器上的邮箱.log.zip
            收集源: 服务器上的邮箱: m.landsea.cn
    修改收集源后启动ICC 收集源: 邮箱 filenet@landsea.cn
        系统仪表板出现的错误    07.png
        log 日志    20160412_收集源_邮箱.log.zip
    陈文源说需要查看邮箱的位置。
        输入 get-mailbox 以后的结果 get-mailbox.txt
        修改收集源 mail01 的所有邮箱 和 mail02 的所有邮箱 08.png
        设置文档类为 ICCmail 2
        ICCAttachmentCorrelationKeys 设置无效,部分邮件无法归档
        查看配置和日志，报错的属性是 ICCMail 3 的属性。

# 准备安装 ICCMail 3
    询问康赛是否安装了 css 
    询问陈文源关于外网访问内网的问题。他说ICC只是一个中转，附件下载是用ICC web application的
            IBM的陈文源说他这个问题要联系北美的工程师解决，所以在联系，
                明天才能回复。如果设置不好的话就准备安装ICCMail 3 
                需要在filenet上安装搜索的功能。
# 新建一个对象存储库
## 重新给icc建立一个对象存储库 ./20160414新建ICC对象存储库
    但是帐号密码登陆不进去  因为ldap坏了，方京平正在找原因
    新建对象存储库  FnosForIcc  01.png
    选择数据库链接  FNOS_dbconnect  02.png
    存储区域类型    归档    03.png
    存储在 /image/fnosforicc    大  04.png
    添加用户    05.png 06.png
    配置点击 Workspace/Workspace XT 配置    07.png
    最终配置    08.png, 09.png, 10.png
    创建失败    11.png
    把符号名称改成 OBJECTSTOREICC 仍然报错    12.png
    新建失败，更改模式名称来启动
        新建    OSFORICC
        模式名称    OSFORICC_dbconnect
        结果仍然报错 insufficient privileges 13.png
## 上午打电话给IBM的客服下了新的工单:
    工单号: 15661999672, 客服说工程师会联系我。暂时还没有来电话。
## 新建对象存储库 ./20160420新建对象存储库
    安装数据库,以此链接oracle  01.png - 13.png
    无法使用管理员链接 oracle   13.png
    让方京平帮我创建了用户,表空间 14.png 15.png
    新建对象存储库  16.png
    尝试不改变链接去创建存储库  16.png - 25.png 失败
        虽然报错是创建表的权限不够 24.png
        但是手动操作的画是有权限的. 25.png
    尝试增加数据源，新建数据库链接  26.png - 49.png
## 检测数据源  ./20160421新建对象存储库-2
    测试新建的数据源的链接 01.png - 04.png
## 使用部署工具创建对象存储库  ./20160512使用filenet部署工具创建对象存储库
1. 清除已经建立的数据源 01.png, 02.png, 03.png
2. 打开filenet配置管理工具  04.png
3. 新建对象存储库任务   09.png
    * 里面出现点击测试链接的时候报错，无法链接。
    * 测试gcd账号和fnos账号都无法链接   09 - 11.png
    * 但是任务还是可以运行的。并且
4. 运行任务 13.png
    * 提示输出was的账号密码 输入 p8admin, password  14.png
    * 任务完成  15.png, 16.png
5. 完成数据源的创建, was的页面上出现了数据源 FNOSICC 和 FNOSICCXA 17.png
## 利用已经创建好的数据源创建对象存储库
### 尝试新建数据库链接  --- 失败
1. 新建对象存储库   18.png
2. 显示名称设置为 FNOSICC   19.png
3. 数据库链接选择新建   20.png
4. 数据库链接名称 FNOSICC_dbconnect 21.png
5. 设置JNDI数据源 FNOSICC FNOSICCXA 22.png
6. 完成， 创建失败  23.png 24.png
### 利用原有的数据库链接创建
1. 根据提示，使用原有的数据库链接需要修改模式名称   25.png
2. 设置对象存储库  
    * 选择归档 26.png
    * 设置路径和目录结构大小 27.png
    * 设置管理员   28.png
    * 设置用户 29.png
    * 设置附加组件（点击Workplace/Workplace XT配置按钮) 30-33.png
    * 完成，创建失败   34-38.png
## 重新设置fnosicc的用户权限
1. 设置用户权限 01.png
2. 创建表空间，表空间已存在 02.png
## 根据邮件同步节点 ./20160513同步后新建对象存储库
1. 停止服务
2. 同步节点
3. 启动服务
## 创建数据库连接和对象存储库   ./20160513创建ICCmail3类
1. 创建成功
## 创建ICCMail3
1. 启动失败     09.png 10.png
## 重新配置 ICC [安装记录](./20160517尝试初始化配置ICC)
1. 初始配置必须用admin用户登录才能链接服务器，否则无法验证ldap  03.png, 04.png
2. 初始配置 05.png - 06.png 有警告 06.log
3. 配置完成后仍然无法配置存储库链接 07.png
## 重现ICC无法启动的情况 [安装记录](./20160518重启ICC查看日志)
<p id="20160518reset_password"></p>
## 重设ICC服务的密码 [安装记录](./20160518重设服务的密码)
1. 三个服务启动成功
## 新建一个任务 [安装记录](./20160518新建ICC任务)
1. 启动失败, ctms 的 audit 文件夹里面没有文件, windows没有产生事件
    * 截屏 01任务(上).png 02任务(下).png 03系统仪表板有错误.png 04电子邮件连接成功.png 05仅仅收集电子邮件的任务.png
    * 日志: log.zip
    * 查看日志是乱码    06ICC日志仍然是乱码.png
    * 日志数量就是这么多    07日志数量.png 2016-05-19 11:48打包的日志.zip
    * 检查FileNet的连接     09Filenet能够连接并检测到对象库.png

# 重新安装ICC [安装记录](./20160519重新设置ICC)
<p id="reinstall_icc"></p>
1. 卸载ICC 01卸载了ICC.png
2. 重启电脑
3. 安装ICC 
    * 集群名称 cluster519   04.png
    * 目标目录 C:\Program Files (x86)\IBM\ContentCollector 05.png
    * 选择源: `Microsoft Exchange` 存储库: `IBM FileNet P8` 06.png
    * 选择如何部署: 07.png
        * 将应用程序自动部署到嵌入式 Web 应用程序服务器
        * 不勾选 `仅部署归档功能所需的应用程序`
    * 主机名: WIN-47E9N1KOE9M.m.landsea.cn 端口: 11443  08.png
        * 之前安装的时候因为计算机还没有加入域，所以这个设置有点不同
    * 最后检查  09.png 10.png 11.png
# 重新初始配置ICC [安装记录](./20160519重新设置ICC)
#### [参考](./20160328重新安装icc)
1. 启动初始配置 12.png
2. 注意19.png 警告 未对此对象库启用 CBR
3. 配置还是有警告 22.png 日志见 AfuInitialConfigTrace.log
4. 结果无法启动 23.png  之前没有出现这个问题
5. 更改那三个服务的密码 [参考](#20160518reset_password)
6. 用filenet用户登录打开配置管理器后报错 24.png, 用Admin用户可以
# 启动配置管理器  [安装记录](./20160519重新设置ICC)
#### [参考:有所区别](#20160329)
1. 用户标识里面已经存在Exchange Server凭证  25.png
2. p8的链接也已经设置好了   26.png
3. 配置一个仅仅是读取邮件的任务 27.png
4. 添加收集源   `服务器上的邮箱` `192.168.1.105`    28.png
5. 时间表改成`始终`   29.png
6. 日志级别改成`跟踪` 30.png
7. 启动服务失败 31.png  修改用户后启动成功  32.png
