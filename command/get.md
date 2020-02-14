# zabbix_get

Zabbix-Get是zabbix的一个程序，用于Zabbix-Server到Zabbix-Agent的数据获取，通常可以用来检测验证客户端的配置是否正确。

```
# -s, 远程Zabbix-Agent的ip地址或主机名
# -p, 远程Zabbix-Agent的端口
# -I, 本机的出口ip
# -k, 获取Zabbix-Agent数据所使用的key

zabbix_get -s 192.168.0.240 -k system.uname
zabbix_get -s 192.168.0.103 -p 10050 -I 127.0.0.1 -k system.uname
```
