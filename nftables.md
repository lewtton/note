### 安装
apt-get install nftables conntrackd netfilter-persistent
apt-get purge iptables

### 地址族确定要处理的数据包的类型。在 nftables 中有六个地址族，它们是：
ip
ipv6
inet
arp
bridge
netdev

在 nftables 中，ipv4 和 ipv6 协议可以被合并为一个称为 inet 的单一地址族。

### 典型的 nftables 规则包含三个部分：表、链和规则。

表是链和规则的容器。它们由其地址族和名称来标识。链包含 inet/arp/bridge/netdev 等协议所需的规则，并具有三种类型：过滤器、NAT 和路由。nftables 规则可以从脚本加载，也可以在终端键入，然后另存为规则集。

对于家庭用户，默认链为过滤器。inet 系列包含以下钩子：
```
Input
Output
Forward
Pre-routing
Post-routing
```
nft 需要以 root 身份运行或使用 sudo 运行。使用以下命令分别列出、刷新、删除规则集和加载脚本。
```
nft list ruleset
nft flush ruleset
nft delete table inet filter
/usr/sbin/nft -f /etc/nftables.conf
```
### 防火墙包含三部分
- 输入（input）
- 转发（forward）
- 输出（output）

```
(base) david@debian:~$ cat /etc/nftables.conf 
#!/usr/sbin/nft -f
flush ruleset
table inet filter {
        chain input {
                type filter hook input priority 0;
        }
        chain forward {
                type filter hook forward priority 0;
        }
        chain output {
                type filter hook output priority 0;
        }
}
```
