# 扩展检测监控方式

扩展检测（External check）监控方式，让Zabbix-Server去执行一个脚本，然后将脚本执行的结果作为监控项数据的来源。例如，我们获取了一个监控数据时就立即对Zabbix库进行修改，这时，这个功能就派上用场了。

## 扩展检测配置

```
shell# vim /etc/zabbix/zabbix_server.conf
ExternalScripts=/etc/zabbix/externalscripts
shell# systemctl restart zabbix-server
shell# vim /etc/zabbix/externalscripts/echo.sh
#!/bin/bash
echo "$1" "$2"
shell# chmod 755 /etc/zabbix/externalscripts/echo.sh
shell# chown zabbix:zabbix /etc/zabbix/externalscripts/echo.sh
```

扩展检测Item示例：
```
Name: echo check
Type: External check
Key: echo.sh[100,200]
Host Interface: 127.0.0.1:10050
Type of Information: Character
Update Interval: 30s
History storage period: 90d
```

由于扩展检测监控方式是通过Zabbix-Server来执行fork进程的，过多的fork进程会带来额外的性能开销，因此尽量少用，而应该使用Zabbix-Agent来分摊监控指标取值的工作。
