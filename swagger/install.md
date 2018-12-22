## 安装
```
yum -y install gcc gcc-c++ openssl-devel
wget https://npm.taobao.org/mirrors/node/v10.13.0/node-v10.13.0.tar.gz
tar xvf node-v10.13.0.tar.gz
cd node-v10.13.0
./configure --prefix=/usr/local/node
make && make install
```
## 匹配环境
```
vim /etc/profile
export NODE_HOME=/usr/local/node
export PATH=$NODE_HOME/bin:$PATH
xport NODE_PATH=$NODE_HOME/lib/node_modules:$PATH
source /etc/profile
node -v
```
