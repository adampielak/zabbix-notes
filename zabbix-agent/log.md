# 日志监控方式

Zabbix-Agent支持对日志文件的监控，可以对日志的关键字进行监控，然后告警。日志监控支持普通的日志文件，也支持日志轮循、切割的文件。当日志中出现特殊的字符串（如警告、报错等字符串）时，可以发送通知给用户。

## 日志监控的监控指标

```
log[/path/to/file/file_name,<regexp>,<encoding>,<maxlines>,<mode>,<output>,<maxdelay>,<options>]
logrt[/path/to/file/regexp_decribing_filename_pattern,<regexp>,<encoding>,<maxlines>,<mode>,<output>,<maxdelay>,<options>]
log.count[/path/to/file/file_name,,<regexp>,<encoding>,<maxproclines>,<mode>,<maxdelay>,<options>]
logrt.count[/path/to/file/regexp_decribing_filename_pattern,<regexp>,<encoding>,<maxproclines>,<mode>,<maxdelay>,<options>]
```

maxlines, 每次给Zabbix-Server或Zabbix-Proxy发送的日志最大行数，此参数值会高于zabbix_agentd.conf中MaxLinesPerSecond参数值。

mode, all为默认参数，表示匹配所有的日志，包括以前存在的日志；skip表示跳过已经存在的日志数据，只有新的日志才会进行匹配。

output, 表示匹配输出的正则表达式，\1~\9表示返回匹配的第几个字符串，\0表示返回匹配的全部字符串。

maxdelay, 以秒为单位的最大延迟，用于忽略老的日志数据，及时获取当前的日志数据。当处理的日志过多，在更新周期内达到maxlines的发送上限，但还有日志无法发送时，会导致大量的堆积，在严重情况下，会造成日志处理速度跟不上，这时使用此参数将忽略过期的日志发送。输入的数值类型可以是浮点数（float），0是默认是，表示永远不会忽略日志文件行。

options, 日志轮询、切割的方式。rotate为默认值，日志轮询、切割；copytruncate，先拷贝文件然后清空日志的轮询方式，不能与maxdelay一起使用，如使用此参数，maxdelay必须是0或未指定。

```
log[/path/to/the/file,"large result buffer allocation.*Entries: ([0-9]+)",,,,\1]
```

如果Zabbix用户对日志没有读取权限，则会提示权限拒绝导致数据读取失败。对于不方便设置权限的日志文件，可以是Zabbix-Agent采用root权限运行：
```
shell# vim /etc/zabbix/zabbix_agentd.conf
AllowRoot=1
shell# systemctl restart zabbix-agent
```
