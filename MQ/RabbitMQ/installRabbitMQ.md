# 单机版安装步骤
## 参考文章
* [单机版RAbbitMQ安装](https://blog.csdn.net/super_rd/article/details/70241007?utm_source=itdadao&utm_medium=referral)

## 安装erl
```
yum update -y //更新yum 
reboot 
yum -y install gcc glibc-devel make ncurses-devel openssl-devel xmlto perl wget //安装依赖
cd /home 
wget http://www.erlang.org/download/otp_src_18.3.tar.gz  //下载erlang包
tar -xzvf otp_src_18.3.tar.gz  //解压
cd otp_src_18.3/ //切换到安装路径
./configure --prefix=/usr/local/erlang  //生产安装配置
make && make install  //编译安装


```
## 设置环境变量
```
 vi /etc/profile  //在底部添加以下内容
 
      #set erlang environment
      ERL_HOME=/usr/local/erlang
      PATH=$ERL_HOME/bin:$PATH
      export ERL_HOME PATH   
 
 source /etc/profile  //生效
 erl  //如果进入erlang的shell则证明安装成功，退出即可。
```

## 安装MQ
```
cd /home 
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.1/rabbitmq-server-generic-unix-3.6.1.tar.xz  //下载RabbitMQ安装包
xz -d rabbitmq-server-generic-unix-3.6.1.tar.xz
tar -xvf rabbitmq-server-generic-unix-3.6.1.tar
mv rabbitmq_server-3.6.1/ rabbitmq //重命名
```

## 配置MQ环境变量
```$xslt
vi /etc/profile
    #set rabbitmq environment
    export PATH=$PATH:/home/rabbitmq/sbin
source /etc/profile
```
## 启动MQ服务
```$xslt
rabbitmq-server -detached //启动rabbitmq，-detached代表后台守护进程方式启动。
rabbitmqctl status //查看状态
```
### 其他命令
```$xslt
启动服务：rabbitmq-server -detached【 /usr/local/rabbitmq/sbin/rabbitmq-server  -detached 】
查看状态：rabbitmqctl status【 /usr/local/rabbitmq/sbin/rabbitmqctl status  】
关闭服务：rabbitmqctl stop【 /usr/local/rabbitmq/sbin/rabbitmqctl stop  】
列出角色：rabbitmqctl list_users
```

## 安装监控网页插件
```$xslt
mkdir /etc/rabbitmq  //建立目录
rabbitmq-plugins enable rabbitmq_management //启用插件
```
## 配置linux端口15672网页 管理5672的AMQP端口防火墙
```$xslt
firewall-cmd --permanent --add-port=15672/tcp
firewall-cmd --permanent --add-port=5672/tcp
systemctl restart firewalld.service
```
## 进入管理页面
```$xslt
  服务器IP:15672
```
## 配置页面用户和密码
```$xslt
rabbitmqctl add_user admin admin  //添加用户，后面两个参数分别是用户名和密码,都是admin了。
rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"  //添加权限
rabbitmqctl set_user_tags superrd administrator  //修改用户角色
```