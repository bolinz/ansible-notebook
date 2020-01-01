# Ad-Hoc

以并行方式执行

```bash
ansbile all -a "/sbin/reboot" -f 10
```

以sudo方式执行

```{bash}
ansible all -a "/sbin/reboot" -u ops --become
#如果需要sudo密码
ansible all -a "/sbin/reboot" -u ops --become --ask-become-pass
```

默认使用command 模块,使用-m 参数切换模块

```{bash}
ansible all -m shell -a 'echo ${TERM}'
```


## 文件传输

以并行方式scp文件到多台主机

```{bash}
ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts"
```

使用file模块修改文件属组和权限

```{bash}
ansible all -m file -a "dest=/srv/foo/a.txt mode=600"
ansible all -m file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"
```

创建目录

```{bash}
ansible all -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"
```

删除目录(递归)和删除文件

```{bash}
ansible all -m file -a "dest=/path/to/c state=absent"
```


## 使用yum或apt包管理

```{bash}
#确认软件包已经安装,但不升级
ansible all -m yum -a "name=acme state=present"
#确认软件包的安装版本
ansible all -m yum -a "name=acme-1.5 state=present" 
#确认一个软件包还没有安装
ansible all -m yum -a "name=acme state=absent"
#安装最新版本的软件包
ansible all -m yum -a "name=httpd state=latest"
```

## 用户和组

使用user模块创建删除用户

```{bash}
ansible all -m user -a "name=foo password=<crypted password here>"
ansible all -m user -a "name=foo state=absent"
```

## 从版本控制部署

```{bash}
ansible webservers -m git -a "repo=git://foo.example.org/repo.git dest=/srv/myapp version=HEAD"
```

## 管理服务

```{bash}
#确认服务已经启动
ansible all -m service -a "name=httpd state=started"
#重启服务
ansible all -m service -a "name=httpd state=restarted"
#确认服务已经停止
ansible all -m service -a "name=httpd state=stop"
```

## 收集所有facts

```
ansible all -m setup
```

## 查看模块说明

https://docs.ansible.com/ansible/latest/modules/modules_by_category.html