# BatchSSH（线程一对一）

一款SSH批量操作的程序，可以按运行模式实现SSH批量命令执行、权限维持、批量改密、C段监控批量命令执行、C段监控批量改密、C段字典生成
```
-i(指定他的IP，可以是多个IP,也可以用*来标识0~255的IP段，必填)
-d(指定VPS的默认密码，必填)
-c(运行模式，必填)
-u(指定SSH的用户名)
-p(要修改的密码，默认为：byechopwdsec)
-o(指定端口，默认22)

运行模式：
 执行改密操作      循环执行        命令执行
      1             1             1       =>无限执行并改密加命令执行
      0             1             1       =>批量重复执行某个命令
      1             1             0       =>批量改密
      0             0             1       =>只执行一次命令(这里执行命令后，程序才会自动结束)
      1             0             0       =>只执行一次改密操作

flagVPS.txt ----->目标服务器，生成后可以更改
common.txt ----->每次改密后执行的操作，一行一行执行
commonResult.txt ----->common执行操作的输出结果
result.txt ----->改密的机器

```
部署：
1.建议使用Venv环境进行部署，需要的包在requirements.txt中(不知道Venv部署可以看下这个师傅的文章[Venv部署](https://blog.csdn.net/m0_61155226/article/details/131670779))
```
pip install -r requirements.txt
```
2.直接使用Python部署，安装包之后直接Usage
```
pip install -r requirements.txt
```

**Usage** 

```
python .\SSHSUOHA.py -i targetIP -d OldPassword -c XXX
```

**Example**

```
python .\SSHSUOHA.py -i 192.168.213.132 -d adminadmin -c 111
python .\SSHSUOHA.py -i 192.168.213.132 192.168.212.* -d adminadmin -u usertest -c 111
python .\SSHSUOHA.py -i 192.168.213.* -d adminadmin -c 111
```

tips:

​	1.当网络连接比较慢的时候可以适当增加timeout的大小

  2.当有确定的SSH地址的时候，可以生成后再flagVPS.txt中进行CRUD，来提高效率

​	3.由于paramiko超时的原因，有时候因为莫名奇妙的问题会出现except拦截错误(这里如果有师傅知道可以告知一下)，不用管就好，看result.txt里面的就行

​	4.在某些比赛也可以添加一个自己的IP来避免重置的时候被他人改密

​	5.在某些比赛可以通过循环改密自己的ssh密码来避免重置的时候被别人改密码

进阶技巧：

程序生成flagVPS后，可以在监控之前数据清洗：

![image](https://github.com/PlusTop/SSHsuoha/assets/105430146/cf5466f7-9563-493e-bfe7-36dc5b85d9bb)

运行过程中可以更改common.txt的命令，会在下一次ssh连接成功执行（命令是一行一行调用）:
![image](https://github.com/PlusTop/SSHsuoha/assets/105430146/95f7bf0d-2895-4eac-8668-8645e2a2916b)
运行结果：
![image](https://github.com/PlusTop/SSHsuoha/assets/105430146/7dda52ba-d178-41cd-83e2-0f1eac3a4f04)

运行模式111(批量改密+批量命令执行):![111](https://github.com/PlusTop/SSHsuoha/assets/105430146/6a11e7c2-54d2-4059-9051-051bf6556441)
运行模式011(批量命令执行):![011](https://github.com/PlusTop/SSHsuoha/assets/105430146/d88f4cfd-947d-4bcd-a321-3aca1f38d552)


