# [crontab定时器安装与用法](https://www.cnblogs.com/carsonwuu/p/11418641.html)

crontab命令常见于Unix和[Linux](http://lib.csdn.net/base/linux)的[操作系统](http://lib.csdn.net/base/operatingsystem)之中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于“crontab”文件中，以供之后读取和执行。通常，crontab储存的指令被守护进程激活。crond 常常在后台运行，每一分钟检查是否有预定的作业需要执行。这类作业一般称为cron jobs。

 

**一、安装**

**[root@CentOS ~]# yum -y install vixie-cron
[root@CentOS ~]# yum -y install crontabs**

说明：
vixie-cron 软件包是 cron 的主程序；
crontabs 软件包是用来安装、卸装、或列举用来驱动 cron 守护进程的表格的程序。

**二、配置**

cron 是 linux 的内置服务，但它不自动起来，可以用以下的方法启动、关闭这个服务：
service crond start   //启动服务
service crond stop   //关闭服务
service crond restart  //重启服务
service crond reload  //重新载入配置
service crond status  //查看crontab服务状态

在CentOS系统中加入开机自动启动: chkconfig --level 345 crond on

cron 的主配置文件是 **/etc/crontab**，它包括下面几行：

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

\# run-parts
01 * * * * root run-parts /etc/cron.hourly
02 4 * * * root run-parts /etc/cron.daily
22 4 * * 0 root run-parts /etc/cron.weekly
42 4 1 * * root run-parts /etc/cron.monthly

前四行是用来配置 cron 任务运行环境的变量。
SHELL 变量的值告诉系统要使用哪个 shell 环境（在这个例子里是 bash shell）；
PATH 变量定义用来执行命令的路径。
cron 任务的输出被邮寄给 MAILTO 变量定义的用户名。
如果 MAILTO 变量被定义为空白字符串（MAILTO=""），电子邮件就不会被寄出。
HOME 变量可以用来设置在执行命令或脚本时使用的主目录。

 

**限制对 cron 的使用：

/etc/cron.allow**和**/etc/cron.deny** 文件被用来限制对 cron 的使用。
这两个使用控制文件的格式都是每行一个用户。
两个文件都不允许空格。
如果使用控制文件被修改了，cron 守护进程（crond）不必被重启。
使用控制文件在每次用户添加或删除一项 cron 任务时都会被读取。

无论使用控制文件中的规定如何，root 都总是可以使用 cron。

如果 cron.allow 文件存在，只有其中列出的用户才被允许使用 cron，并且 cron.deny 文件会被忽略。
如果 cron.allow 文件不存在，所有在 cron.deny 中列出的用户都被禁止使用 cron。

 

**三、crontab 命令**

**功能**：设置计时器。

**语法**：**crontab**[-u <用户名称>][配置文件] 或 crontab [-u <用户名称>][-elr]
**解释**：cron 是一个常驻服务，它提供计时器的功能，让用户在特定的时间得以执行预设的指令或程序。只要用户会编辑计时器的配置文件，就可以使 用计时器的功能。其配置文件格式如下：Minute Hour Day Month DayOFWeek Command

参数：
**-e**　编辑该用户的计时器设置。
**-l**　列出该用户的计时器设置。
**-r**　删除该用户的计时器设置。
**-u**<用户名称> 　指定要设定计时器的用户名称。

 

**格式:**
\*  *　 *　 *　 *　　command
分　时　日　月　周　 命令

第1列表示分钟1～59 每分钟用*或者 */1表示
第2列表示小时1～23（0表示0点）
第3列表示日期1～31
第4列表示月份1～12
第5列标识号星期0～6（0表示星期天）
第6列要运行的命令

 

**例子：**
*/5 * * * * root ab -n 2000 http://60.217.229.252/250k.jpg

上面例子表示每5分钟模拟用户访问http://60.217.229.252/250k.jpg 2000次

 

30 21 * * * /usr/local/etc/rc.d/lighttpd restart
上面的例子表示每晚的21:30重启apache。

45 4 1,10,22 * * /usr/local/etc/rc.d/lighttpd restart
上面的例子表示每月1、10、22日的4 : 45重启apache。

10 1 * * 6,0 /usr/local/etc/rc.d/lighttpd restart
上面的例子表示每周六、周日的1 : 10重启apache。

0,30 18-23 * * * /usr/local/etc/rc.d/lighttpd restart
上面的例子表示在每天18 : 00至23 : 00之间每隔30分钟重启apache。

0 23 * * 6 /usr/local/etc/rc.d/lighttpd restart
上面的例子表示每星期六的11 : 00 pm重启apache。

\* */1 * * * /usr/local/etc/rc.d/lighttpd restart
每一小时重启apache

\* 23-7/1 * * * /usr/local/etc/rc.d/lighttpd restart
晚上11点到早上7点之间，每隔一小时重启apache

0 11 4 * mon-wed /usr/local/etc/rc.d/lighttpd restart
每月的4号与每周一到周三的11点重启apache

0 4 1 jan * /usr/local/etc/rc.d/lighttpd restart
一月一号的4点重启apache

*/30 * * * * /usr/sbin/ntpdate 210.72.145.44
每半小时同步一下时间

 

【注意】定时器调用的脚本应该在其内加上：source /etc/profile（变量问题），如果不加上 可能导致单独执行定时器内语句成功，定时器执行不成功。

 

 

转自：http://hi.baidu.com/faoduqsldrimrwe/item/a7c474d7bb8280cbcb0c393a?qq-pf-to=pcqq.temporaryc2c