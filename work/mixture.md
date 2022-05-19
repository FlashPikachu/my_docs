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

#### 4..(点命令)和source

都是用来执行脚本，区别在于：
（1）. 来执行shell是在一个子shell里运行的，所以执行后，结果并没有反应到父shell里
（2）source是在本shell中执行的

------

#### 5.测试端口是否开放

需要开放端口的机子上执行：

~~~shell
nc -l <port>
~~~

在另外的机子上执行

~~~shell
nc -vz <ip> <port>
~~~

------

#### 6.apt命令相关

~~~shell
#更新系统包缓存
sudo apt-get update
#查看jdk8存在的版本
apt-cache madison openjdk-8-jdk

root@0001:/usr/lib/jvm/java-8-openjdk-amd64# apt-cache madison openjdk-8-jdk
openjdk-8-jdk | 8u212-b03-0ubuntu1.16.04.1 | http://cn.archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages
openjdk-8-jdk | 8u212-b03-0ubuntu1.16.04.1 | http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages
openjdk-8-jdk | 8u77-b03-3ubuntu3 | http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 Packages
 
#根据版本号下载软件包
sudo apt-get install openjdk-8-jdk=8u212-b03-0ubuntu1.16.04.1
#查看apt工具已经安装的包
apt list --installed
~~~

```
#sources.list的位置
/etc/apt/sources.list

#apt 下载的deb文件位置
/var/cache/apt/archives
```

制作ubuntu 离线安装包：

- 联网环境的主机
- apt工具已经安装
- dpkg-dev工具已经安装

以制作linux-libc-dev_5.4.0-96.109的离线安装包为例

1.下载对应的包

~~~shell
root@ubuntu:~/package# apt install linux-libc-dev
~~~

2.到/var/cache/apt/archives目录下获取deb文件，我将其存放在/home/casa/package/linux-libc-dev_5.4.0-96.109目录下

~~~shell
#创建对应目录
root@ubuntu:~/package# mkdir -p /home/casa/package/linux-libc-dev_5.4.0-96.109
root@ubuntu:~/package# cd /var/cache/apt/archives
root@ubuntu:/var/cache/apt/archives# ll |grep linux-libc-dev
-rw-r--r-- 1 root root   1114192 Jan 18 18:37 linux-libc-dev_5.4.0-96.109_amd64.deb
root@ubuntu:/var/cache/apt/archives# cp linux-libc-dev_5.4.0-96.109_amd64.deb /home/casa/package/linux-libc-dev_5.4.0-96.109/
~~~

3.修改目录权限

~~~shell
root@ubuntu:/var/cache/apt/archives# chmod -R 777 /home/casa/package/linux-libc-dev_5.4.0-96.109/
~~~

4.建立deb包的依赖关系

~~~shell
root@ubuntu:/var/cache/apt/archives# dpkg-scanpackages /home/casa/package/linux-libc-dev_5.4.0-96.109/ /dev/null |gzip >/home/casa/package/linux-libc-dev_5.4.0-96.109/Packages.gz
dpkg-scanpackages: warning: Packages in archive but missing from override file:
dpkg-scanpackages: warning:   linux-libc-dev
dpkg-scanpackages: info: Wrote 1 entries to output Packages file.
~~~

5.打包成压缩包

~~~shell
root@ubuntu:/var/cache/apt/archives# cd /home/casa/package/
root@ubuntu:~/package# tar zcvf linux-libc-dev_5.4.0-96.109.tar.gz linux-libc-dev_5.4.0-96.109/
linux-libc-dev_5.4.0-96.109/
linux-libc-dev_5.4.0-96.109/linux-libc-dev_5.4.0-96.109_amd64.deb
linux-libc-dev_5.4.0-96.109/Packages.gz
~~~

到这一步，离线安装包已经制作完成。

------

#### 7.在ubuntu16中安装python3.8：

自带python2.7和python3.5，ubuntu16的源没有3.8的python

1.执行"whereis python"查看当前安装的python

~~~shell
[root@root ~]# whereis python
python: /usr/bin/python2.7 /usr/bin/python /usr/lib/python2.7 /usr/lib64/python2.7 /etc/python /usr/include/python2.7 /usr/share/man/man1/python.1.gz
~~~

2.配置依赖环境，如果不进行这步可能会出现一些问题

~~~shell
sudo apt-get install zlib1g-dev libbz2-dev libssl-dev libncurses5-dev libsqlite3-dev
~~~

3.去官网下载[Python-3.8.1.tar.xz](https://www.python.org/ftp/python/3.8.1)

~~~shell
wget https://www.python.org/ftp/python/3.8.1/Python-3.8.1.tar.xz
# 如果下载失败
　　1.将服务器DNS改成 8.8.8.8
   2.将源改为国内源
~~~

4.解压下载的包

~~~shell
tar -xvJf  Python-3.8.1.tar.xz
cd Python-3.8.1/
~~~

4.安装依赖(非必要，可跳过此步骤，如在5步出错在执行本步骤)

~~~shell
sudo apt-get install python-dev
sudo apt-get install libffi-dev
sudo apt-get install libssl-dev
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev
~~~

5.执行安装

~~~shell
./configure prefix=/usr/local/python3
make && make install
~~~

6.修改软连接（配置全局变量）

~~~shell
#将原来的链接备份
mv /usr/bin/python /usr/bin/python.bak
ln -s /usr/local/python3/bin/python3 /usr/bin/python
#测试是否安装成功了
python -V
~~~

7.安装/升级pip

到官网获取对应python版本的get-pip.py

~~~shell
python get-pip.py
~~~

建立软连接：

~~~shell
ln -s /usr/local/python3/bin/pip3.8 /usr/local/bin/pip
~~~

------

#### 4.ubuntu20 开放root用户使用密码进行ssh远程登录

1. 设置root密码

执行命令后，依次输入当前登录用户密码，要设置的root密码，确认root密码

```undefined
sudo passwd root
```

2. 修改[ssh](https://so.csdn.net/so/search?q=ssh&spm=1001.2101.3001.7020)配置文件

如果没有安装ssh-server，执行安装命令，已经安装的跳过即可

```vbscript
sudo apt install openssh-server
```

修改配置文件

```bash
sudo vim /etc/ssh/sshd_config
```

在vim中搜索定位PermitRootLogin，可直接查找：

```undefined
/PermitRootLogin
```

> 修改以下配置：
> 33 #LoginGraceTime 2m
> 34 #PermitRootLogin prohibit-password
>
> #prohibit-password 是不可以使用密码登录
>
> 35 #StrictModes yes
> 36 #MaxAuthTries 6
> 37 #MaxSessions 10

修改为：

```bash
 LoginGraceTime 2m
 PermitRootLogin yes
 StrictModes yes
 #MaxAuthTries 6
 #MaxSessions 10

```

3. 重启ssh，使配置生效

```undefined
sudo service ssh restart
```

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
