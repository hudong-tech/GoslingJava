# linux搭建环境

### 配置网络

**将网络设置为192.168.100.66**

VMware的设置(1-5必填)

1. 虚拟机设置为**NAT模式**

2. 在vmware->编辑->虚拟网络编辑器->VMnet8->子网IP(这个ip不用和电脑ip一致，在192.168网段就可以)

   > 192.168.100.0

3. NAT设置->查看网关为  

   > 192.168.100.2

4. DHCP设置->起始IP地址  192.168.100.66    结束ip:192.168.100.254

5. 设置租用时间    随意

hudong_M虚拟机的设置：

1. 编辑ip

``` shell
$ cd /etc/sysconfig/network-scripts/
$ vim ifcfg-ens33 
$ service network restart
```

 ``` yml
 # ifcfg-ens33格式
 TYPE=Ethernet
 PROXY_METHOD=none
 BROWSER_ONLY=no
 #BOOTPROTO=dhcp  # 通过DHCP服务自动获取IP
 BOOTPROTO=static # 将IP设置为静态IP
 DEFROUTE=yes
 IPV4_FAILURE_FATAL=no
 IPV6INIT=yes
 IPV6_AUTOCONF=yes
 IPV6_DEFROUTE=yes
 IPV6_FAILURE_FATAL=no
 IPV6_ADDR_GEN_MODE=stable-privacy
 NAME=ens33
 UUID=cf65b480-0b3e-45a1-aca0-ac2647940cf0
 DEVICE=ens33
 #ONBOOT=no  # 不启动加载
 ONBOOT=yes  # 启动加载
 IPADDR=192.168.100.166	# 配置静态IP，符合子网的网段
 NETMASK=255.255.255.0	# 子网掩码
 GATEWAY=192.168.100.2	 # 配置网关，见上述VMware网关的配置
 DNS1=8.8.8.8	 # DNS服务器
 DNS2=114.1114.114.114	# DNS服务
 DNS3=10.111.1.2	# DNS服务器
 ```

### 设置yum源

#### 使用阿里源

> 安装wget

``` shell
$ yum -y install wget
```

``` shell
$ mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
$ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo 
$ yum clean all
$ yum makecache
```

#### 使用清华源

> [清华yum源使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/centos/)

使用下面命令替换yum源.

``` shell
$ cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
$ sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
         -e 's|^#baseurl=http://mirror.centos.org|baseurl=https://mirrors.tuna.tsinghua.edu.cn|g' \
         -i.bak \
         /etc/yum.repos.d/CentOS-*.repo
# 更新软件包缓存
$ sudo yum makecache
# 升级所有包同时也升级软件和系统内核
$ yum -y update
```

### 别名

``` shell
# 安装vim
$ yum -y install vim
$ vi ~/.bashrc
# 文件中写入
alias vi='vim'

$ source ~/.bashrc
# 查看所有别名
$ alias
```

### vim

> Vim 的全局配置一般在`/etc/vim/vimrc`或者`/etc/vimrc`，对所有用户生效。
>
> 用户个人的配置在`~/.vimrc`

``` shell
set number
syntax on
set showmode
set showcmd
set encoding=utf-8
set t_Co=256
set autoindent
set shiftwidth=4
```

### 安装基础软件

### jdk

> 存放地址：
>
> 安装目录： /opt/jdk1.8.0_241

``` shell
$ tar –xvf file.tar //解压tar包
$ tar -xvzf jdk1.8.0_241.tar.gz //解压tar.gz
```

> 添加环境变量

``` shell
# 修改配置文件
$ vi /etc/profile
# profile文件尾加
export JAVA_HOME=/opt/jdk1.8.0_241
export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin
# 重新加载配置文件
$ source /etc/profile
# 测试
$ java -version
```

#### Docker

> [官方安装文档](https://docs.docker.com/engine/install/centos/)
>
> [docker hub](https://hub.docker.com/ )

``` shell
1. 卸载旧版本
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
2. 安装yum-utils软件包(提供yum-config-manager 实用程序）并设置稳定的存储库。
$ sudo yum install -y yum-utils
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
3. 安装DOCKER引擎
$ sudo yum install docker-ce docker-ce-cli containerd.io
# 若执行报错，则切换yum源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
4. 启动Docker
$ sudo systemctl start docker
5. 查看是否安装成功
$ docker -v
$ docker images 
6. 设置开机启动
$ sudo systemctl enable docker 
```

配置镜像加速（设置国内下载地址）

![image-20210123043213615](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/05/20210606033028.png)

``` shell
$ sudo mkdir -p /etc/docker
$ sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://ykspkj3s.mirror.aliyuncs.com"]
}
EOF
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

#### docker 常用命令

![docker命令脑图](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/09/docker%E5%91%BD%E4%BB%A4%E8%84%91%E5%9B%BE.png)

``` shell
# 查看所有镜像
$ docker images
#这两个命令的差别就是后者会显示  【已创建但没有启动的容器】
$ sudo docker ps
$ sudo docker ps -a
# 自动启动
sudo docker update mysql --restart=always
# 手动启动
sudo docker start mysql
# 进入已启动的容器   /bin/bash就是进入一般的命令行，如果改成redis就是进入了redis
sudo docker exec -it mysql /bin/bash
# 查看当前容器挂载目录
$ docker inspect mysql |grep Mounts -A 20
# 挂载目录

```

> 如何进入容器

``` shell
# 查看容器的CONTAINER ID
$ docker ps
# 进入容器    /bin/bash：在容器内创建一个终端
$ docker exec -it 9c262f54dcfe /bin/bash
# 容器内退出 
$ exit
```

![image-20210915172644863](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/09/image-20210915172644863.png)

### Docker 安装软件

#### mysql

``` shell
1. 下载mysql
# docker pull mysql:5.7
2. 创建实例并启动
$ sudo docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
$ docker update mysql --restart=always
3. 查看docker中运行的容器
$ docker ps
```

![image-20210123044734985](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/05/20210606033023.png)

#### redis

> [redis官网](http://www.redis.cn/download.html)
>
> 准备redis配置文件。下载一个redis，解压后，将其中的`redis.conf`上传到linux的`/mydata/redis（需要创建）`中，并修改相应的配置

![1](https://hp-documents.oss-cn-hangzhou.aliyuncs.com/2021/09/1.png)

> bind 127.0.0.1 #注释掉这部分，使redis可以外部访问
> daemonize no#用守护线程的方式启动
> requirepass 你的密码#给redis设置密码
> appendonly yes#redis持久化　　默认是no
> tcp-keepalive 300 #防止出现远程主机强迫关闭了一个现有的连接的错误 默认是300
>
> logfile "./log/redis.log"

``` shell
$ docker pull redis
$ sudo docker run -p 6379:6379 --name redis -v /data/redis/conf/redis.conf:/etc/redis/redis.conf  -v /data/redis/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes
```

参数解释：

> -p 6379:6379:把容器内的6379端口映射到宿主机6379端口
> -v /data/redis/redis.conf:/etc/redis/redis.conf：把宿主机配置好的redis.conf放到容器内的这个位置中
> -v /data/redis/data:/data：把redis持久化的数据在宿主机内显示，做数据备份
> redis-server /etc/redis/redis.conf：这个是关键配置，让redis不是无配置启动，而是按照这个redis.conf的配置启动
> –appendonly yes：redis启动后数据持久化

``` shell
# 查看是否成功启动
$ docker ps
# 查看运行日志
$ docker logs -f redis
# 进入容器测试
$ docker exec -it redis /bin/bash
@redis $ redis-cli
```

#### ELK

> ELK版本要一致

``` shell
$ docker pull elasticsearch:7.4.2
$ docker pull kibana:7.4.2
```

