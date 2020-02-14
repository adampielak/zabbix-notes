# simple check监控方式

simple check是Zabbix-Server主动发起的对外请求，在不安装任何Agent的情况下对一些常见的服务协议进行监控。

+ simple check进程获取数据
  + icmp方式
    + ping存活  icmpping
    + ping响应  icmppingsec
    + ping丢包  icmppingloss
  + 远程服务检测
    + SSH、POP、HTTPS    net.tcp.service
    + LDAP、NNTP、FTP    net.tcp.service.perf
    + SMTP、IMAP、TELNET    net.udp.service
    + HTTP、TCP、NTP    net.udp.service.perf
  + Vmware检测
    + Vcenter
    + ESXI

## 支持的key

```
# packets: ping多少个数据包
# interval: 隔多久ping一次, 单位为ms
# size: 包的大小, 单位为bytes
# timeout: 超时时间, 单位为ms
# mode: 对ping响应时间的计算, 支持avg、min、max
# service: 可以为ssh、ldap、smtp、ftp、http、pop、nntp、imap、tcp、https、telnet
icmpping[<target>,<packets>,<interval>,<size>,<timeout>]
icmppingloss[<target>,<packets>,<interval>,<size>,<timeout>]
icmppingsec[<target>,<packets>,<interval>,<size>,<timeout>,<mode>]
net.tcp.service[service,<ip>,<port>]
net.tcp.service.perf[service,<ip>,<port>]
net.udp.service[service,<ip>,<port>]
net.udp.service.perf[service,<ip>,<port>]
```

zabbix用fping处理ICMP ping请求，需要安装fping程序。在zabbix_server.conf中，FpingLocation参数用于配置fping程序路径。

fping默认以root权限工作，而Zabbix-Server是zabbix用户运行的，需要对fping程序设置setuid：
```
chown root:zabbix /usr/sbin/fping
chmod 4710 /usr/sbin/fping
```
