# tomcat安装

## 安装

* 安装好jdk,本文不重复介绍

* 选择对应版本的tomcat下载并安装
```
wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-7/v7.0.82/bin/apache-tomcat-7.0.82.tar.gz
tar -xzvf apache-tomcat-7.0.82.tar.gz
mv apache-tomcat-7.0.82 tomcat
```

```
wget http://mirrors.shuosc.org/apache/tomcat/tomcat-9/v9.0.10/bin/apache-tomcat-9.0.10.tar.gz
tar -xzvf apache-tomcat-9.0.10.tar.gz
mv apache-tomcat-9.0.10 tomcat
```

```
wget http://mirrors.shuosc.org/apache/tomcat/tomcat-8/v8.5.32/bin/apache-tomcat-8.5.32.tar.gz
tar -xzvf apache-tomcat-8.5.32.tar.gz
mv apache-tomcat-8.5.32 tomcat
```

更多版本： http://mirrors.shuosc.org/apache/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz


* [配置防火墙](/Linux/content/iptables.md),打开80端口

* 配置tomcat端口
    * 进入tomcat目录``cd {tomcat dir}/``
    * ``vim conf/server.xml``
    * 在文件内搜索http协议的``Connector``，修改端口``<Connector port="80" protocol="HTTP/1.1" connectionTimeout="20000 redirectPort="8443"/>``
    * 其他Connector的端口保持与其他应用不重复即可；

* 清空{tomcat dir}/webapps目录,``rm -rf webapps/*``
* 上传工程war包到{tomcat dir}/webapps目录

* 测试启动tomcat
```
[linux]# cd {tomcat dir}
[linux]# ./bin/catalina.sh run
```

* 正常启动tomcat 

```
[linux]# cd {tomcat dir}
[linux]# ./bin/startup.sh 
```

注意： {tomcat dir}/webapps目录下的默认工程，在实际使用中需要删除；

## Trouble shooting

* 端口冲突，``conf/server.xml``里面出来有配置http端口以外，还有配置另外2个端口，在该文件中搜索port可以找到这2个端口，如果对应端口冲突，可以通过修改这个文件处理；
* [jvm优化](/tools/tomcat/jvm_config.md)
