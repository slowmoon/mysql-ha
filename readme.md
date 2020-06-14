### mysql 主从配置方案,使用 docker swarm 集群部署#####

```shell
   docker stack deploy -c docker-compose.yml mysqls
```

#### 其他配置

当集群启动过后， 使用此配置slave 链接master

```shell
# 重新设置连接master节点
reset slave;

# 设置连接master
change master to 
    master_host='master',  # 主库的IP，因为使用了docker隔离机制，所以将其放入同一个网络内，使用服务名来当主机名
	master_user='root',    # 主库同步的用户
	master_password='123456', # 密码
	master_port=3306, # 主库的端口
	master_log_file='mysql-bin.000003', # 同步的文件 通过show master status来获取
	master_log_pos=321; # 开始从第几行同步 通过show master status来获取
	
# 启动同步
start slave;

# 查看master状态
show slave status;
————————————————
```

