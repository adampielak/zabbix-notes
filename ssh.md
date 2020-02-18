# ssh监控方式

对于ssh监控方式，支持ssh密码访问目标主机，也支持密钥访问目标机器。通过ssh协议连接到远程主机，然后执行相关命令获取数据。

## Zabbix-Server配置密钥存储路径

默认情况下，Zabbix-Server并不知道使用哪个ssh密钥来连接服务器。

```
shell# vim /etc/zabbix/zabbix_server.conf
SSHKeyLocation=/var/lib/zabbix/.ssh
shell# systemctl restart zabbix-server
```

## 生成并分发密钥

```
# 生成ssh密钥
mkdir -p /var/lib/zabbix/.ssh
chown zabbix:zabbix -R /var/lib/zabbix/.ssh
sudo -u zabbix ssh-keygen -t rsa -b 2048

# 分发密钥到被监控机
sudo -u zabbix ssh-copy-id root@10.0.3.2
```

## 配置key获取数据

```
# 在item中，ssh的key用法如下
ssh.run[<unique short description>,<ip>,<port>]
```
