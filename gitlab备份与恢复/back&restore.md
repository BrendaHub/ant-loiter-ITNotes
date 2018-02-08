# gitLab备份与恢复

## 备份命令

```shell
/usr/bin/gitlab-rake gitlab:backup:create
```
以上命令就会在/var/opt/gitlab/backup目录下生成一个备份的压缩包；

第一个下划线前面是当前备份文件的最一ID， 恢复时可以通这个ID进行定位备份包；

## GitLab恢复命令

```shell
# 停止相关数据连接服务
gitlab-ctl stop unicorn
gitlab-ctl top sidekiq

# 从一个备份包上恢复，找到备份包的ID
gitlab-rake gitlab:backup:restore BACKUP=12312312312(ID)

# 启动GitLab
sudo gitlab-ctl start
```

# GitLab迁移

迁移如同备份与恢复步骤一样， 只需要将老服务器/var/opt/gitlab/backup目录下的备份文件拷贝到新服务器上的/var/opt/gitlab/backup即可。 
但是需要注 意的是，新服务器上的gitlab的版本必须与创建备份时的gitlab版本号相同， 比如新服务器安装的是最新的7.6版本的gitlab，那么迁移之前， 最好将老服务器的gitlab升级为7.6在进行备份；

