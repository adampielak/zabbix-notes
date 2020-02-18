# 宏 (Macros)

宏的作用是便于在模板、Item、Trigger中对数据进行变量引用，宏的名称为{$名称}，宏的字符范围为A-Z、0-9、_。

```
# 在Item key中使用
net.tcp.service[ssh,{$SSH_PORT}]

# 在触发器的表达式中当常数使用
{ca_001:system.cpu.load[,avg1].last()}>{$MAX_CPULOAD}

# 在触发器的表达式中当参数使用
{ca_001:system.cpu.load[,avg1].min({$CPULOAD_PERIOD})}>{$MAX_CPULOAD}
```

+ 全局宏 (Global macros)
  + 作用范围: 模板、主机
  + 配置步骤: Administration -> General -> Macros
+ 模板宏 (Template macros)
  + 作用范围: 当前模板
  + 配置步骤: Configuration -> Templates -> 模板名称 -> Macros
+ 主机宏 (Host macros)
  + 作用范围: 当前主机
  + 配置步骤: Configuration -> Hosts -> 主机名称 -> Macros
+ 监控项宏 (Item macros)

## 宏的函数运算

在Zabbix 4.0及以上版本中，支持对监控指标（包括LLD的监控指标）数值进行字符串截取匹配。

```
{{ITEM.VALUE}.regsub(pattern, output)}
{{#LLDMACRO}.regsub(pattern, output)}
{{ITEM.VALUE}.iregsub(pattern, output)}
{{#LLDMACRO}.iregsub(pattern, output)}

# {ITEM.VALUE}表示普通监控指标将要取得的数值
# {#LLDMACRO}表示自动发现的监控指标将要取得的数值
# regsub和iregsub的区别在于是否区分大小写，iregsub不区分大小写
```

### 例子

| 接收到的字符串 |        宏        |    输出    |
| :------------: | :--------------: | :--------: |
| 123Log line | {{ITEM.VALUE}.regsub(^[0-9]+,Problem)} | Problem |
| 123 Log line | {{ITEM.VALUE}.regsub("^([0-9]+)","Problem")}| Problem |
| 123 Log line | {{ITEM.VALUE}.regsub("^([0-9]+)","Problem ID: \1")} | Problem ID: 123 |
| customername_1 | {{IFALIAS}.regsub("(.*)_([0-9]+)",\1)} | customername |

## 宏的上下文

宏的上下文(User macro context)主要使用在监控项LLD功能中。在不使用宏的上下文功能时，只能配置一个固定的阈值，不能实现对不同的监控指标实现不同的阈值。

### 例子

预计实现功能：
+ / 分区，小于70%告警
+ /boot 分区，小于90%告警
+ 其余分区，小于10%告警

在Template OS Linux模板中进行配置：
```
# Trigger prototypes
Name: Free disk space is less than {$LOW_SPACE_LIMIT:"{#FSNAME}"}% on volume {#FSNAME}
Severity: Average
Expression: {Template OS Linux:vfs.fs.size[{#FSNAME},pfree].last()}<{$LOW_SPACE_LIMIT:"{#FSNAME}"}

# Macros
{$LOW_SPACE_LIMIT} 10
{$LOW_SPACE_LIMIT:/} 70
{$LOW_SPACE_LIMIT:/boot} 90
```
