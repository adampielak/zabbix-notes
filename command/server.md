# zabbix_server

```
# 手动执行清理器(Housekeeper), 删除过期数据
zabbix_server -R housekeeper_execute

# 查看命令执行日志
tail -f /var/log/zabbix/zabbix_server.log

# 在线执行重载配置缓存
zabbix_server -R config_cache_reload

# 降低日志运行级别, 执行一次, 降低一个级别
zabbix_server -R log_level_decrease
# 增加日志运行级别
zabbix_server -R log_level_increase
# 调整某个进程(pid为1322)的日志运行级别
zabbix_server -R log_level_increase=1322
# 调整增加poller进程的日志运行级别
zabbix_server -R log_level_increase=poller
# 调整增加poller的第三个进程的日志运行级别
zabbix_server -R log_level_increase=poller,3
```
