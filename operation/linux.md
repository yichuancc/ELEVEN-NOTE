# 目录介绍

* **/bin**： bin是Binary的缩写, 这个目录存放着最经常使用的命令。

* **/boot**： 这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。

* **/dev** **：** dev是Device(设备)的缩写, 存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。

* **/etc**： 这个目录用来存放所有的系统管理所需要的配置文件和子目录。

* **/home**：用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

* **/lib**： 这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。

* **/lost+found**： 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

* **/media**：linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

* **/mnt**：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

* **/opt**：这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

* **/proc**： 这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。

* **/root**：该目录为系统管理员，也称作超级权限者的用户主目录。

* **/sbin**：s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。

* **/srv**：该目录存放一些服务启动之后需要提取的数据。

* **/sys**：这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统sysfs 。

* **/tmp**：这个目录是用来存放一些临时文件的。

* **/usr**：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。

* **/usr/bin**：系统用户使用的应用程序。

* **/usr/sbin**：超级用户使用的比较高级的管理程序和系统守护程序。

* **/usr/src**：内核源代码默认的放置目录。

* **/var**：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

* **/run**：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。

# 常用的基本命令

## 目录管理

> **ls （列出目录）**

```powershell
[root@centos081 ~]# ll 目录名称
```

选项与参数：

* -a ：全部的文件，连同隐藏文件( 开头为 . 的文件) 一起列出来(常用)

* -l ：长数据串列出，包含文件的属性与权限等等数据；(常用)

> **cd （切换目录）**

```powershell
# 切换到用户目录下
[root@centos081 /]# cd home

# 使用 mkdir 命令创建 study 目录
[root@centos081 home]# mkdir study

# 进入 study 目录
[root@centos081 home]# cd study

# 回到上一级
[root@centos081 study]# cd ..

# 回到根目录
[root@centos081 study]# cd /

# 表示回到自己的家目录，亦即是 /root 这个目录
[root@kuangshen study]# cd ~
```

> **pwd ( 显示目前所在的目录 )**

选项与参数： 

* -P ：显示出确实的路径，而非使用连结 (link) 路径

> **mkdir （创建新目录）**

选项与参数：

* -m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～

* -p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！

```shell
# 进入我们用户目录下
[root@centos081 /]# cd /home

# 创建一个 test 文件夹
[root@centos081 home]# mkdir test

# 创建多层级目录
[root@centos081 home]# mkdir test1/test2/test3/test4
mkdir: cannot create directory ‘test1/test2/test3/test4’:
No such file or directory # <== 没办法直接创建此目录啊！

# 加了这个 -p 的选项，可以自行帮你创建多层目录！
[root@centos081 home]# mkdir -p test1/test2/test3/test4

# 创建权限为 rwx--x--x 的目录。
[root@centos081 home]# mkdir -m 711 test2
[root@centos081 home]# ls -l
drwxr-xr-x 2 root root 4096 Mar 12 21:55 test
drwxr-xr-x 3 root root 4096 Mar 12 21:56 test1
drwx--x--x 2 root root 4096 Mar 12 21:58 test2
```

> **rmdir ( 删除空的目录 )**

> **cp ( 复制文件或目录 )**

```shell
[root@www ~]# cp [-adfilprsu] 来源档(source) 目标档(destination)
[root@www ~]# cp [options] source1 source2 source3 .... directory
```

选项与参数：

* **-a**：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)

* **-p**：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；

* **-d**：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；

* **-r**：递归持续复制，用于目录的复制行为；(常用)

* **-f**：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；

* **-i**：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)

* **-l**：进行硬式连结(hard link)的连结档创建，而非复制文件本身。

* **-s**：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；

* **-u**：若 destination 比 source 旧才升级 destination ！

```shell
# 找一个有文件的目录，我这里找到 root目录
[root@centos081 home]# cd /root
[root@centos081 ~]# ls
install.sh

[root@centos081 ~]# cd /home

# 复制 root目录下的install.sh 到 home目录下
[root@centos081 home]# cp /root/install.sh /home
[root@centos081 home]# ls
install.sh

# 再次复制，加上-i参数，增加覆盖询问？
[root@centos081 home]# cp -i /root/install.sh /home
cp: overwrite ‘/home/install.sh’? y # n不覆盖，y为覆盖
```

> **rm [-fir] 文件或目录** 

选项与参数：

* -f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；

* -i ：互动模式，在删除前会询问使用者是否动作

* -r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！

```shell
# 将刚刚在 cp 的实例中创建的 install.sh删除掉！
[root@centos081 home]# rm -i install.sh
rm: remove regular file ‘install.sh’? y

# 如果加上 -i 的选项就会主动询问喔，避免你删除到错误的档名！

# 普通删除文件
rm -rf 文件夹/目录


# 尽量不要在服务器上使用 rm -rf /
```

> **mv ( 移动文件与目录，或修改名称 )**

选项与参数：

* -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；

* -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！

* -u ：若目标文件已经存在，且 source 比较新，才会升级 (update)

```shell
# 复制一个文件到当前目录
[root@centos081 home]# cp /root/install.sh /home

# 创建一个文件夹 test
[root@centos081 home]# mkdir test

# 将复制过来的文件移动到我们创建的目录，并查看
[root@centos081 home]# mv install.sh test
[root@centos081 home]# ls
test
[root@centos081 home]# cd test
[root@centos081 test]# ls
install.sh

# 将文件夹重命名，然后再次查看！
[root@centos081 test]# cd ..
[root@centos081 home]# mv test mvtest
[root@centos081 home]# ls
mvtest
```

## 基本属性

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

在Linux中我们可以使用 **ll** 或者 **ls –l** 命令来显示一个文件的属性以及文件所属的用户和组。

![image-20240618145024014](images/image-20240618145024014.png)

在Linux中**第一个字符代表这个文件是目录、文件或链接文件**等等：

* 当为[ **d** ]则是目录

* 当为[ **-** ]则是文件；

* 若是[ **l** ]则表示为链接文档 ( link file )；

* 若是[ **b** ]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 )；

* 若是[ **c** ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 )。

接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。其中，[ r ]代表可读(read)、[ w ]代表可写(write)、[ x ]代表可执行(execute)。要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已。每个文件的属性由左边第一部分的10个字符来确定

![image-20240618145921456](images/image-20240618145921456.png)

从左至右用0-9这些数字来表示。**第0位确定文件类型，第1-3位确定属主（该文件的所有者）拥有该文件的权限。第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限**。

其中：

* 第1、4、7位表示读权限，如果用"r"字符表示，则有读权限，如果用"-"字符表示，则没有读权限；

* 第2、5、8位表示写权限，如果用"w"字符表示，则有写权限，如果用"-"字符表示没有写权限；

* 第3、6、9位表示可执行权限，如果用"x"字符表示，则有执行权限，如果用"-"字符表示，则没有执行权限。

对于文件来说，它都有一个特定的所有者，也就是对该文件具有所有权的用户。

同时，在Linux系统中，用户是按组分类的，一个用户属于一个或多个组。文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。因此，Linux系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限。

>  **chgrp：更改文件属组**

```shell
chgrp [-R] 属组名 文件名
```

选项与参数：

* -R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。

> **chown：更改文件属主，也可以同时更改文件属组**

```shell
chown [–R] 属主名 文件名

chown [-R] 属主名：属组名 文件名
```

> **chmod：更改文件9个属性**

```shell
chmod [-R] xyz 文件或目录
```

Linux文件属性有两种设置方法，一种是数字，一种是符号。

Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。

## 文件内容查看

Linux系统中使用以下命令来查看文件的内容：

* cat由第一行开始显示文件内容

* tac 从最后一行开始显示，可以看出 tac 是 cat 的倒著写！

* nl 显示的时候，顺道输出行号！

* more 一页一页的显示文件内容

* less 与 more 类似，但是比 more 更好的是，他可以往前翻页！

* head 只看头几行

* tail 只看尾巴几行

> tail 取出文件后面几行

```shell
tail [-n number] 文件
```

选项与参数：

* -n ：后面接数字，代表显示几行的意思

## Vim编辑器

![image-20240618213104147](images/image-20240618213104147.png)

## 进程管理

> **PS指令**

使用ps指令即可查看当前系统中正在执行的进程的各种进程信息

**选项说明：**

* -a：显示当前终端的所有进程信息

* -u：以用户的形式显示进程信息

* -x：显示后台进程运行的参数

```shell
ps -aux|grep xxx ，查看某个服务的进程 如，ps -aux|grep mysql
```

说明：

1、grep 命令用于查找文件里符合条件的字符串。

2、命令格式：**命令A|命令B**，即命令A的正确输出作为命令B的操作对象

> **终止进程kill或killall**

kill指令就像是Windows系统中的任务管理->结束任务一样

```shell
kill -9 PID   -9 :表示强迫进程立即停止
```

# 常用操作

## 防火墙

### 端口

1、查看已经开放的端口

```shell
firewall-cmd --list-ports 
```

2、开启端口

```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent  
 
 命令含义:
 –zone #作用域
 –add-port=80/tcp #添加端口，格式为：端口/通讯协议
 –permanent #永久生效，没有此参数重启后失效
 
 
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.16.0.230" port protocol="tcp" port="8877" accept"

该规则的作用是允许来自IP地址为172.16.0.230的主机通过TCP协议访问端口号为8877的服务。 
1. 使用firewall-cmd命令调用防火墙管理工具。 
2. 使用--permanent参数表示将修改应用到永久配置中。 
3. 使用--add-rich-rule参数添加一个富规则。 
4. 富规则的内容是一个字符串，其中包含了规则的详细配置。 
5. 在规则中指定了IP地址为172.16.0.230。 
6. 指定了协议为TCP。 
7. 指定了端口号为
```

3、重启

```shell
firewall-cmd --reload
```

4、关闭某个端口

```shell
sudo firewall-cmd --zone=public --remove-port=8080/tcp --permanent  
sudo firewall-cmd --reload
```

### 状态

>  **查看和关闭防火墙**CentOS 7.0默认使用的是firewall作为防火墙

1、查看防火墙状态

```
firewall-cmd --state
```

2、停止firewall

```
systemctl stop firewalld.service
```

3、禁止firewall开机启动

```shell
systemctl disable firewalld.service
```

## 文件授权

> 部署服务的时候，没有 .sh 的执行权限，用 chmod 授权。chmod是 Linux 中权限管理命令change the permissions mode of a file的缩写。

### **所有 .sh 脚本添加执行权限**

**chmod u+x \*.sh**，表示对当前目录下的file.sh文件的所有者增加可执行权限。

u 代表所有者；x 代表执行权限；+ 表示增加权限。

### **指定 .sh 脚本添加执行权限**

**chmod u+x file1.sh**，表示对当前目录下的 file1.sh 文件的所有者增加可执行权限。

u 代表所有者；x 代表执行权限；+ 表示增加权限。

### 其他权限

1、当对应的文件夹没有操作权限是，用**sudo su root** 命令切换到root用户权限操作

2、创建文件夹后，对文件夹授权，例如

```shell
chown -R szxy:szxy /data/szxy

将/data/szxy目录下的所有文件和子目录的所有者和所属组都设置为szxy。

1. chown是一个命令，用于修改文件或目录的所有者和所属组。
2. -R选项表示递归地修改目录下的所有文件和子目录。
3. szxy:szxy表示将所有者和所属组都设置为szxy。
4. /data/szxy是要修改的目录路径。
```



## 查看服务

### 服务进程

**ps -ef | grep tomcat**是一个在 Unix 和 Linux 系统中常用的命令组合，用于查找与 **tomcat** 相关的进程。

- ps 是一个显示当前进程的快照的工具。
  - -e：选择所有进程。
  - -f：全格式输出，包括完整的命令行。
- |：管道符号，将前一个命令的输出作为后一个命令的输入。
- grep tomcat：使用 grep命令搜索包含“tomcat”的行。

因此，ps -ef | grep tomcat命令将显示所有与 tomcat相关的进程及其详细信息。

### 端口进程

netstat -apn | grep 2351 是一个在 Unix 和 Linux 系统上常用的命令组合，用于查找与特定端口（在这个例子中是 2351）相关的网络连接或监听的服务。

- netstat 是一个命令行工具，用于显示网络连接、路由表、接口统计等网络相关信息。
  - -a：显示所有活动的网络连接以及服务器套接字。
  - -p：显示哪个进程在使用套接字。
  - -n：以数字形式显示地址和端口号，不进行 DNS 解析。
- |：这是一个管道符号，用于将一个命令的输出作为另一个命令的输入。
- grep 2351：这是一个文本搜索命令，用于搜索包含“2351”的行。

因此，netstat -apn | grep 2351 命令将显示所有与端口 2351 相关的网络连接或监听的服务，并显示哪个进程正在使用这些套接字。

## 服务启动

### Java服务

```shell
-  nohup : nohup命令用于在后台运行进程，即使终端关闭也不会中断该进程。 
-  java : 启动Java虚拟机。 
-  -Xms1024m : 设置Java虚拟机的初始堆内存大小为1024m。 
-  -Xmx1024m : 设置Java虚拟机的最大堆内存大小为1024m。 
-  -jar resource-center-server.jar : 运行名为resource-center-server.jar的Java可执行文件。 
-  --spring.profiles.active="prod" : 设置Spring应用程序的活动配置文件为prod。 
-  2>log_err : 将标准错误输出重定向到tt_err文件。 
-  1>log_out : 将标准输出重定向到tt_log文件。 
-  & : 在后台运行进程，允许终端继续输入其他命令。 

nohup java -Xms1024m -Xmx1024m -jar resource-center-server.jar --spring.profiles.active="prod" 2>tt_err 1>tt_log &


# jar名称，不带.jar后缀
SERVICE_NAME=resource-center-server
# 打包的jar包名称
APP_NAME=$SERVICE_NAME\.jar
# 服务进程PID文件
PID=service.pid

# 使用说明，用来提示输入参数
usage() {
    echo "输入对应参数，执行脚本 ./service.sh [start|stop|restart|status]"
    exit 1
}

# 检查程序是否在运行
is_exist() {
    pid=`ps -ef|grep $APP_NAME|grep -v grep|awk '{print $2}' `
    # 如果不存在返回1，存在返回0
    if [ -z "${pid}" ]; then
        return 1
    else
        return 0
    fi
}

# 启动方法
start() {
    is_exist
    if [ $? -eq "0" ]; then
        echo "${SERVICE_NAME} 应用已启动，进程ID为 ${pid} ."
    else
        nohup java -Xms1024m -Xmx1024m -jar $APP_NAME --spring.profiles.active="prod" 2>log_err 1>log_out &
        echo $! > $PID
        echo "${SERVICE_NAME} 应用启动中，进程ID为 $! "
    fi
}

# 停止方法
stop() {
    is_exist
    if [ $? -eq "0" ]; then
        echo "${SERVICE_NAME} 应用运行中，开始 kill ${pid}"
        kill $pid
        rm -rf $PID
        # 等待最大 60 秒，直到关闭完成。
        for ((i = 0; i < 60; i++))
            do  
                sleep 1
                pid=`ps -ef|grep $APP_NAME|grep -v grep|awk '{print $2}' `
                #  -n 用于判断字符串是否非空
                if [ -n "$pid" ]; then
                    echo -e ".\c"
                else
                    echo "${SERVICE_NAME} 应用已停止..."
                    break
                fi
        done
        # 如果正常关闭失败，那么进行强制 kill -9 进行关闭
        if [ -n "$pid" ]; then
            echo "${SERVICE_NAME}停止失败，强制 kill -9 ${pid}"
            kill -9 $pid
        fi
    else
        echo "${SERVICE_NAME} 应用未运行..."
    fi
}

# 输出运行状态
status() {
    is_exist
    if [ $? -eq "0" ]; then
        echo "${SERVICE_NAME} 应用正在运行，进程ID为 ${pid}"
    else
        echo "${SERVICE_NAME} 应用未运行..."
    fi
}

# 重启
restart() {
    stop
    start
}

# 根据输入参数，选择执行对应方法，不输入则执行使用说明
case "$1" in
    "start")
        start
        ;;
    "stop")
        stop
        ;;
    "status")
        status
        ;;
    "restart")
        restart
        ;;
    *)
        usage
        ;;
esac
```



# 常见问题

## **脚本执行异常**

异常信息：**/bin/sh^M: bad interpreter: No such file or directory**

异常原因：是我们在 windows 下编写的脚本文件，直接放到 Linux 默认的是 dos 模式的文本，不被识别，需要处理下。

解决办法：

1、用 vim 打开脚本文件，在命令模式下输入：**set ff=unix**, 保存就可以了。

2、在 windows下转换脚本格式，用 Notepad 改变文件格式即可。File-->Conversions-->DOS->UNIX。

3、在 Linux 下新建一个 .sh 文件，然后复制粘贴过去也是可以的。

# 软件安装

## JDK安装

在/usr/local目录下进行解压JDK压缩包

```shell
tar -zxvf jdk-8u321-linux-x64.tar.gz
```

root用户登录，在**/etc/profile**添加环境变量

```shell
#configuration java development enviroument
export JAVA_HOME=/usr/local/jdk1.8.0_321
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar 
export PATH=$JAVA_HOME/bin:$PATH:
```

让新增的环境变量生效

```shell
source /etc/profile
```

测试安装结果

```shell
java -version
```

## Tomcat日志切割

> 参考链接
>
> * [cronolog分割tomcat的catalina.out文件](https://www.cnblogs.com/shidian/p/11396065.html)
> * https://blog.csdn.net/Hi_Red_Beetle/article/details/103384587



