# zabbix-agent监控方式常用key

## 网卡流量监控的key

```
# if表示网卡接口, mode表示想要取值的类型
# mode的可选参数为bytes(默认), packets, errors, 
# dropped, overruns, frame, compressed, multicast
net.if.in[if,<mode>]

# 获取网卡的流入流量, 获取到的值是实时的
shell# zabbix_get -s 127.0.0.1 -k net.if.in[eth0,bytes]
```

```
net.if.out[if,<mode>]
net.if.total[if,<mode>]
net.if.collisions[if]
net.if.discovery
```

## 与网络连接相关的key

```
net.tcp.listen[port]
net.tcp.port[<ip>,port]
net.tcp.service[service,<ip>,<port>]
net.tcp.service.perf[service,<ip>,<port>]
net.udp.listen[port]
net.udp.service[service,<ip>,<port>]
net.udp.service.perf[service,<ip>,<port>]
net.dns[<ip>,name,<type>,<timeout>,<count>,<protocol>]
net.dns.record[<ip>,name,<type>,<timeout>,<count>,<protocol>]
```

## 监控进程的key

```
kernel.maxfiles
kernel.maxproc
proc.mem[<name>,<user>,<mode>,<cmdline>]
proc.num[<name>,<user>,<state>,<cmdline>]
proc.cpu.util[<name>,<user>,<type>,<cmdline>,<mode>,<zone>]
```

## 监控cpu和内存的key

```
system.cpu.intr
system.cpu.load[<cpu>,<mode>]
system.cpu.num[<type>]
system.cpu.switches
system.cpu.util[<cpu>,<type>,<mode>]
system.cpu.discovery
vm.memory.size[<mode>]
system.swap.in[<device>,<type>]
system.swap.out[<device>,<type>]
system.swap.size[<device>,<type>]
sensor[device,sensor,<mode>]
```

## 磁盘I/O监控的key

```
vfs.dev.read[<device>,<type>,<mode>]
vfs.dev.write[<device>,<type>,<mode>]
vfs.fs.inode[fs,<mode>]
```

## 文件监控的key

```
vfs.file.cksum[file]
vfs.file.contents[file,<encoding>]
vfs.file.exists[file]
vfs.file.md5sum[file]
vfs.file.regexp[file,regexp,<encoding>,<start line>,<end line>,<output>]
vfs.file.regmatch[file,regexp,<encoding>,<start line>,<end line>]
vfs.file.size[file]
vfs.file.time[file,<mode>]
vfs.fs.discovery
vfs.fs.size[fs,<mode>]
vfs.dir.count[dir,<regex_incl>,<regex_excl>,<types_incl>,<types_excl>,<max_depth>,<min_size>,<max_size>,<min_age>,<max_age>]
```

## 与操作系统相关的key

```
system.boottime
system.hw.chassis[<info>]
system.hw.cpu[<cpu>,<info>]
system.hw.devices[<type>]
system.hw.macaddr[<interface>,<format>]
system.localtime[<type>]
system.run[command,<mode>]
system.stat[resource,<type>]
system.sw.arch
system.sw.os[<info>]
system.sw.packages[<package>,<manager>,<format>]
system.uname
system.uptime
system.users.num
```

## 与web性能相关的key

```
web.page.get[host,<path>,<port>]
web.page.perf[host,<path>,<port>]
web.page.regexp[host,<path>,<port>,<regexp>,<length>,<output>]
```

## 日志监控的key

```
log[file,<regexp>,<encoding>,<maxlines>,<mode>,<output>]
logrt[file_pattern,<regexp>,<encoding>,<maxlines>,<mode>,<output>]
```

## windows监控的key

```
eventlog[name,<regexp>,<severity>,<source>,<eventid>,<maxlines>,<mode>]
net.if.list
perf_counter[counter,<interval>]
proc_info[<process>,<attribute>,<type>]
service.discovery
service.info[service,<param>]
service_state[*]
services[<type>,<state>,<exclude>]
wmi.get[<namespace>,<query>]
```
