# centos虚拟机 安装vmware-tools

1. 首先启动CentOS 7,在VMware中点击上方“VM”，点击“安装VMware Tools”.

​			![](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/11/20220610010020.png)

2. 查看/dev下是否有`cdrom`

```
find /dev -name cdrom
```

![](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/11/20220610010817.png)

3. 输入“mkdir /mnt/cdrom”在/mnt目录下新建一个名为cdrom的文件夹。

``` 
mkdir /mnt/cdrom
# 查看是否成功，如果有cdrom目录说明已经创建成功！
find /mnt -name cdrom
```

4. 将光盘挂载到/mnt/cdrom目录下。

```
mount -t iso9660 /dev/cdrom /mnt/cdrom
```

![](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/11/20220610010234.png)



5. 将名为“VMwareTools-10.0.0-2977863.tar.gz”复制到/root目录下，并重新命名为vm.tar.gz。

```
ls /mnt/cdrom/
cp /mnt/cdrom/VMwareTools-10.0.0-2977863.tar.gz /root/vm.tar.gz

```

![](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/11/20220610010158.png)

6. 将文件解压，输入“ls”查看文件，可发现新增目录“vmware-tools-distrib”。

```
cd /root/
tar -xzf vm.tar.gz
ls 
```

![](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/11/20220610010212.png)

7. 进入名为“vmware-tools-distrib”的目录，尝试安装，出现错误，表明未安装编译环境。

```
cd vmware-tools-distrib/
./vmware-install.pl
# 出现错误 -bash: ./vmware-install.pl: /usr/bin/per: bad interpreter: No such file or directory
```

![](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/11/20220610010313.png)

8. 安装依赖。

``` shell
yum -y install perl gcc make kernel-headers kernel-devel
```

![img](https://img-blog.csdnimg.cn/20181114165640348.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvbmd6aG91ZmVuZw==,size_16,color_FFFFFF,t_70)

9. 在“vmware-tools-distrib”目录下重新输入“./vmware-install.pl”开始安装，基本上按回车键即可。

​	![](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/11/20220610010422.png)





原文链接：https://blog.csdn.net/longzhoufeng/article/details/84067781