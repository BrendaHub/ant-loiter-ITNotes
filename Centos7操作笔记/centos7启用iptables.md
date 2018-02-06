## 在centos7下启用iptables

```linux
systemctl stop firewalld.service
systemctl disable firewalld.service

yum install iptables-services -y
systemctl enable iptables
systemctl start iptables
```

## 编辑iptables的规则
```linux
vim /etc/sysconfig/iptables
```
# 重启iptables服务

```linux
service iptables restart
```

