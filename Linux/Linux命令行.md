### Linux命令行

##### 查看Linux内核版本命令（两种方法）：

```shell
cat /proc/version

uname -a
```



##### 查看Linux系统版本的命令（3种方法）

```shell
#这个命令适用于所有的Linux发行版，包括Redhat、SuSE、Debian…等发行版
lsb_release -a

#这种方法只适合Redhat系的Linux
cat /etc/redhat-release

#此命令也适用于所有的Linux发行版
cat /etc/issue
```



参考：

https://www.qiancheng.me/post/coding/show-linux-issue-version