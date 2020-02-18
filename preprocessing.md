# 监控项指标数据的预处理

预处理（Preprocessing），指在Zabbix-Server接收到监控项数据之后，在入库之前再次处理的过程。这意味着可以从客户端采集任意格式的数据，大大提高了数据处理的灵活性。

在Zabbix-Server中，预处理进程默认有3个，可根据实际情况进行调整，如果进程过少，会导致处理速度跟不上而影响数据入库速度。
```
shell# vim /etc/zabbix/zabbix_server.conf
StartPreprocessors=3
```

## 预处理的运行流程

![预处理的运行流程](/images/preprocessing.png)

采集原始数据后，通过数据处理进程，首先将其存储到数据缓存区域，这样确保了数据采集不被阻塞。当数据缓存达到最大值时，将会给出缓存太小的警告信息。然后预处理进程开始工作，从数据缓存中获取将要处理的数据，再将处理后的结果放在放到历史数据缓存中。在预处理进程工作时，其处理队列采用先进先出（FIFO）的策略，确保了处理队列的顺序。接下来，数据同步进程会不断地对历史数据缓存中的数据进行处理，当达到触发阈值的条件时，会触发告警动作。最后，将数据存储到数据库中。

## 预处理的数据类型

+ 字符类型 - 正则表达式 (Regular expression)
  + pattern, 匹配的正则表达式
  + output, 匹配输出的正则表达式，\1-\9表示返回匹配的第几个字符串，\0表示返回匹配的全部字符串
+ 字符删除 - 全部匹配 (Trim)
+ 字符删除 - 从左往右 (Left trim)
+ 字符删除 - 从右往左 (Right trim)
+ 数据类型 - JSON (JSON Path)
  + JSON数据类型的取值，和标准的JSON解析类似，用"$"开头，以"."为分隔符
    + $.document.item.value - {"document":{"item":{"value":10}}} - 10
    + $.document.items[1].value - {"document":{"items":[{"value":10},{"vaule":20}]}} - 20
    + $['a document'].item.value - {"a document":{"item":{"value":10}}} - 10
+ 数据类型 - XML (XML XPath)
  + 此功能需要Zabbix-Server在编译时开启libxml参数，rpm安装默认已经带此参数
    + number(/document/item/value) - <document><item><value>10</value></item></document> - 10
    + number(/document/item/@attribute) - <document><item attribute="10"></item></document> - 10
+ 乘法运算 - 倍数 (Custom multiplier)
+ 数值变化 - 差值 (Simple change)
+ 数值变化 - 速率 (Change per second)
+ 数值转换 - 布尔到十进制 (Boolean to decimal)
+ 数值转换 - 八进制到十进制 (Octal to decimal)
+ 数值转换 - 十六进制到十进制 (Hexadecimal to decimal)
