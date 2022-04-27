## git

#### 1.git error `server certificate verification failed. CAfile: none CRLfile: none`

~~~shell
## solve：
export GIT_SSL_NO_VERIFY=1

~~~

------

#### 2.git 覆盖本地

本地不小心点了编辑，导致部分代码到了推送，但是又不想推送。

所以类似还原了本地代码的版本。：

~~~shell
git fetch --all #取回远程库的所有修改；

git reset --hard origin/master #指向远程库origin的develop（可更改成自己想要取的远程分支）

git pull #把远程库拉取到本地库
~~~

#### 3.git command

~~~shell
#查看所有分支，包括远程
git branch -a

## 新建分支并切换到指定分支
git checkout -b <本地新建分支名> <远程分支名>
~~~

------

## linux系统

#### 1.ubuntu disable cloud.init 重置netplan网络

~~~shell
touch /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg;echo "network: {config: disabled}" >> /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
~~~

------



#### 2.systemctl 如何启动、关闭、启用/禁用服务

~~~shell
启动服务：systemctl start xxx.service
关闭服务：systemctl stop xxx.service
重启服务：systemctl restart xxx.service
显示服务的状态：systemctl status xxx.service
在开机时启用服务：systemctl enable xxx.service
在开机时禁用服务：systemctl disable xxx.service
查看服务是否开机启动：systemctl is-enabled xxx.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed
~~~

相关链接：

> [systemctl (www.freedesktop.org)](https://www.freedesktop.org/software/systemd/man/systemctl.html#)  

vendor preset参数：

> https://unix.stackexchange.com/questions/468058/systemctl-status-shows-vendor-preset-disabled
>
> If you see a Vendor preset: Disabled, it means when the service first installs it will be disabled on start up and will have to be manually started. If you want the service to start up automatically with boot up, all it takes is to change it’s start up setting with `systemctl enable <service>` , example: `systemctl enable httpd` .

------

#### 3.挂载 移除挂载命令

mount -o rw /dev/vdc /var/lib/mysql

sudo umount /dev/vdc

fsck /dev/vdc

dmesg

------



#### 4.systemctl start 与 systemctl restart的区别

~~~shell
when you restarting the service , it should stop the service and then try start the service,during this time interval stoped service and all service relealted dependencies try start. but while starting, only the core service are started. bcoz all the dependencies are running state(while is not restarted) , if not dependencies also started.
~~~

> [What is the difference between “systemctl restart” and “systemctl start”? - Red Hat Customer Portal](https://access.redhat.com/discussions/1415903)

------



## shell脚本

#### 1.shell判断数组是否存在某个变量

~~~shell
function contains() {
    local n=$#
    local value=${!n}
    for ((i=1;i < $#;i++)) {
        if [ "${!i}" == "${value}" ]; then
            echo "y"
            return 0
        fi
    }
    echo "n"
    return 1
}
 
A=("one" "two" "three four")
if [ $(contains "${A[@]}" "one") == "y" ]; then
    echo "contains one"
fi
if [ $(contains "${A[@]}" "three") == "y" ]; then
    echo "contains three"
fi
~~~



------

#### 2.Shell脚本中$0、$?、$!、$$、$*、$#、$@

~~~shell
1. $$
Shell本身的PID（ProcessID）

2. $!
Shell最后运行的后台Process的PID

3. $?
最后运行的命令的结束代码（返回值）

4. $-
使用Set命令设定的Flag一览

5. $*
所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。

6. $@
所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。

7. $#
添加到Shell的参数个数

8. $0
Shell本身的文件名

9.$1～$n
添加到Shell的各参数值。$1是第1参数、$2是第2参数…。
~~~

------

#### 3.判断表达式

~~~shell
一、文件比较运算符 
1. e filename 如果 filename存在，则为真 如： [ -e /var/log/syslog ] 
2. -d filename 如果 filename为目录，则为真 如： [ -d /tmp/mydir ] 
3. -f filename 如果 filename为常规文件，则为真 如： [ -f /usr/bin/grep ] 
4. -L filename 如果 filename为符号链接，则为真 如： [ -L /usr/bin/grep ] 
5. -r filename 如果 filename可读，则为真 如： [ -r /var/log/syslog ] 
6. -w filename 如果 filename可写，则为真 如： [ -w /var/mytmp.txt ] 
7. -x filename 如果 filename可执行，则为真 如： [ -L /usr/bin/grep ] 
8. filename1-nt filename2 如果 filename1比 filename2新，则为真 如： [ 
/tmp/install/etc/services -nt /etc/services ] 
9. filename1-ot filename2 如果 filename1比 filename2旧，则为真 如： [ 
/boot/bzImage -ot arch/i386/boot/bzImage ]

二、字符串比较运算符（请注意引号的使用，这是防止空格扰乱代码的好方法） 
 1. -z string  如果 string长度为零，则为真 如：  [ -z "$myvar" ]
 2. -n string  如果 string长度非零，则为真  如： [ -n "$myvar" ]
 3. string1= string2  如果 string1与 string2相同，则为真 如：  ["$myvar" = "one two three"]
 4. string1!= string2  如果 string1与 string2不同，则为真 如：  ["$myvar" != "one two three"]

三、算术比较运算符 
 1. num1-eq num2  等于 如： [ 3 -eq $mynum ]
 2. num1-ne num2  不等于 如： [ 3 -ne $mynum ]
 3. num1-lt num2  小于 如： [ 3 -lt $mynum ]
 4. num1-le num2  小于或等于  如：[ 3 -le $mynum ]
 5. num1-gt num2  大于  如：[ 3 -gt $mynum ]
 6. num1-ge num2  大于或等于 如： [ 3 -ge $mynum ]
~~~

------

## Mysql or Mariadb

#### 1.mysqlbinlog

~~~shell
mysqlbinlog -v
添加-v参数可以展现伪SQL语句

mysqlbinlog -vv
可以显示每列的数据类型和一些元数据
~~~

出现mysqlbinlog: unknown variable 'default-character-set=utf8mb4' :

mysqlbinlog --no-defaults -v
more info:

> [MySQL :: MySQL 5.7 Reference Manual :: 4.6.7 mysqlbinlog — Utility for Processing Binary Log Files](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html)
