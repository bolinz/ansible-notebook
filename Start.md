# Start

Ansible 默认通过SFTP连接,如果不支持,需要在Ansible配置文件中切换为SCP模式

Ansible 使用SSH Key进行认证,当使用密码时,需要添加--ask-pass 选项,使用sudo时如果需要密码,添加--ask-sudo-pass选项

## 第一条命令

在/etc/ansible/hosts 中添加若干远程系统

```
192.168.33.10
192.168.33.11
192.168.33.12
```

将ssh公钥添加至这些系统 使用--private-key 选项可指定私钥文件,ping 所有节点

```{bash}
ansible all -m ping
```

默认使用当前用户名连接远程计算机,使用-u指定远程用户,--sudo 开启sudo模式 --sudo-user 指定sudo用户

```{bash}
ansible all -m ping -u ops
ansible all -m ping -u ops --sudo 
```

对所有节点运行命令

```{bash}
ansible all -a "/bin/echo hello"
```

## 公钥认证

Ansible 默认启用公钥认证,如果在known_hosts中有了不同的key,错误信息会持续到被纠正为止

可以通过编辑配置文件/etc/ansible/ansible.cfg 或 ~/.ansible.cfg 实现:

```
[defaults]
host_key_checking = False
```

或修改环境变量实现:

```{bash}
export ANSIBLE_HOST_KEY_CHECKING=False
```