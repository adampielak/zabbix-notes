# 计算型监控方式

计算型（Calculated）监控方式是在Zabbix-Server端对历史数据进行统计分析的一个功能。例如，可以求主机的磁盘容量、网络流量等。

在计算型的监控项（Calculated Item）配置选项中，最关键的是监控指标（key）和计算表达式（Formula），其中key在主机/模板中不能与其他key重复。

## 计算表达式

### 语法

```
func(<key>|<hostname:key>,<parameter1>,<parameter2>,...)

# func, 支持Trigger函数last、min、max、avg、count等
# key, 需要被计算的目标监控指标，形式为key或者hostname:key
## 建议将key放到双引号中，以避免不正确的解析
```

### 例子

```
# 计算剩余磁盘的百分比
100*last("vfs.fs.size[/,free]",0)/last("vfs.fs.size[/,total]",0)

# 计算10分钟内Zabbix values的可用大小
avg("Zabbix server:zabbix[wcache,values]",600)

# 统计eth0的进出流量总和
last("net.if.in[eth0,bytes]",0)+last("net.if.out[eth0,bytes]",0)

# 统计进流量占网卡总流量的百分比
100*last("net.if.in[eth0,bytes]",0)/(last("net.if.in[eth0,bytes]",0)+last("net.if.out[eth0,bytes]",0))

# 对聚合型监控方式的监控项（Aggregated Item）进行计算
last("grpsum[\"video\",\"net.if.out[eth0,bytes]\",\"last\",\"0\"]",0) / 
last("grpsum[\"video\",\"nginx_stat.sh[active]\“，\"last\",\"0\"]",0)
```
