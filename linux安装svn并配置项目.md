1、安装svn
```
[root@localhost ~]# yum -y install subversion
```

2、创建svn版本库
```
[root@localhost ~]# mkdir /home/svn

#配置
[root@localhost ~]# svnadmin create  /home/svn
```

3、创建用户设置密码
```
[root@localhost ~]# vi /home/svn/conf/passwd
#在user下面添加新用户和密码
[users]
# harry = harryssecret
# sally = sallyssecret
svn_test1=Benben520.
```

4、权限控制authz配置
```
[root@localhost ~]# vi /home/svn/conf/authz
#在最底下追加,标识用户拥有读写功能
[/]
svn_test1=rw
```

5、服务svnserve.conf配置
```
[root@localhost ~]# vi /home/svn/conf/svnserve.conf
找到[general]并在其下面添加
#匿名访问的权限，可以是read,write,none,默认为read
anon-access=none
#使授权用户有写权限
auth-access=write
#密码数据库的路径
password-db=passwd
#访问控制文件
authz-db=authz
#认证命名空间
realm=/home/svn
```

6、启动svn服务
```
[root@localhost ~]# svnserve -d -r /home/svn  --listen-port=3690
#防火墙开放3690端口

```

7、修改配置后重新启动
```
#查看进程号
[root@localhost ~]# ps -aux|grep svnserve
root       1758  0.0  0.1 180836  1056 ?        Ss   23:11   0:00 svnserve -d -r /home/svn/test1 --listen-port=3690
root       1770  0.0  0.0 112808   968 pts/0    R+   23:14   0:00 grep --color=auto svnserve
#清除进程号
[root@localhost ~]# kill -9 1758
#重新启动svn
[root@localhost ~]# svnserve -d -r /home/svn  --listen-port=3690
```

8、设置svn开机启动，新建sh执行
```
[root@localhost ~]#  vi /etc/init.d/svnsh
#加入内容
#!/bin/sh
# chkconfig: 3 88 88
/usr/bin/svnserve -d -r /home/svn --listen-port=3690

#给执行权限
[root@localhost ~]# chmod 755 /etc/init.d/svnsh

#开机运行svnsh
[root@localhost ~]# chkconfig svnsh on
```

用户检出时的操作地址为：svn://ip地址










