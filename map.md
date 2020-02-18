# 配置值映射

值映射（Value mapping）功能是指将监控指标取到的数据替换为文本进行展示，如将数值1替换为available。

Configuration -> General -> Create value map
```
Name: Host availability
Mappings:
  0  not available
  1  available
  2  unknown
```
