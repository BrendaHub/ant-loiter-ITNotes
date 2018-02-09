# 完全卸载删除GitLab
1、停止gitLab
```linux
gitlab-ctl stop
```
2、卸载gitlab(注意这里写的是gitlab-ce)
```
rpm -e gitlab-ce
```
3、查看gitlab进程
```
ps -aux | grep gitlab
```
4、杀掉第一个进程（就是带好多....的进程， 这是一个守护进程）
```
kill -9 xxxxx
```
5、删除所有包含gitlab文件
```
find / -name gitlab | xargs rm -rf 
```

即可完成卸载！！！
