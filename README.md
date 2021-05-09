## sky-redis / redis实战
### 参考文章
https://github.com/redisson/redisson

### 第三方组件
redisson:3.15.2

### 目录说明
1. doc包括redis多种模式的配置文件

### 单节点
#### 安装及启动redis
docker run -d --name redis -p 6300:6379 redis:5.0 --requirepass "123456"

### 启动工程
SkyRedisApplication，启动single环境

### 主从节点（1主3从）
#### 规划

|  服务器IP | 端口 | 备注  |
| ---- | ---- | ---- |
| 172.100.0.11    |  6379 | 主   |
| 172.100.0.12     | 6379 | 从   |
| 172.100.0.13     | 6379 | 从   |
| 172.100.0.14     | 6379 | 从   |

#### 安装及启动redis
1. 在doc目录中1master3slave中修改磁盘路径
   
2.在1master3slave修改redis-s.conf中的slaveof 改成本地地址

3.然后在1master3slave中执行docker-compose

#### 启动工程
SkyRedisApplication，启动masterslave环境

### 哨兵模式（1主3从3哨兵）
#### 规划

|  服务器IP | 端口 | 备注  |
| ---- | ---- | ---- |
| 172.100.0.11    |  6379 | 主   |
| 172.100.0.12     | 6379 | 从   |
| 172.100.0.13     | 6379 | 从   |
| 172.100.0.14     | 6379 | 从   |
| 172.100.0.15     | 26379 | 哨兵   |
| 172.100.0.16     | 26379 | 哨兵   |
| 172.100.0.17     | 26379 | 哨兵   |

#### 首次在执行docker-compose命令时需要删除配置文件中自动生成的内容

#### 安装ssh
docker run -itd --name test1 --network 1master3slave3sentinal_extnetwork --ip 172.100.0.10 centos:7 /bin/bash

#### 制作镜像
docker commit test1 centos-ssh:1

#### 运行容器
docker run -it --name centos7ssh --network 1master3slave3sentinal_extnetwork --ip 172.100.0.19 -p 6000:22 centos-ssh:1 /usr/sbin/sshd

#### 在容器中执行jar