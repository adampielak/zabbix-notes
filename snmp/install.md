# snmp监控配置

```
shell# yum install -y net-snmp
shell# vim /etc/snmp/snmpd.conf
com2sec mynetwork 192.168.0.240 public_monitor
com2sec mynetwork 127.0.0.1 public
group MyROGroup v2c mynetwork
access MyROGroup "" any noauth prefix all none none
view all included .l 80
shell# systemctl enable snmpd
shell# systemctl start snmpd
```
