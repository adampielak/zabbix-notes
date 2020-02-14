# 监控指标key

```
# 格式
key[参数]
key[参数1, 参数2]

# 例子
vfs.fs.size[/opt]
name.check["my name\"Hello\""]
```

## 用户自定义参数

用户自定义参数(UserParameter)仅支持Agent的方式。

在/etc/zabbix/zabbix_agentd.conf中配置参数: 
```
# 语法格式
# item key具有唯一性, 定义[*]可以接收参数
UserParameter=key,command
UserParameter=key[*],command $1 $2 $3 ...

# 如果UserParameter包含
# \ ' " ` * ? [ ] { } ~ $ ! & ; ( ) < > | # @
# 这些字符, 默认情况下无法正常处理, 
# 需要开启UnsafeUserParameters
UnsafeUserParameters=1
```

### 配置示例

```
UserParameter=wc[*],grep -c "$2" $1
UserParameter=get.os.type,head -1 /etc/issue

shell# zabbix_get -s 127.0.0.1 -k wc[/etc/passwd,root]
shell# zabbix_get -s 127.0.0.1 -k get.os.type
```

为了便于维护和分类管理，UserParameter的内容可以单独写一个配置文件。
```
# vim /etc/zabbix/zabbix_agentd.conf
Include=/etc/zabbix/zabbix_agentd.conf.d/

# 在上面定义的目录下创建文件, 如userparameter_mysql.conf
```
