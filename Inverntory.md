# Inverntory

Ansible 默认库文件位于 /etc/ansible/hosts 可以使用-i参数指定库文件路径

```
mail.example.com # 定义主机名

[webserver] #定义组名
foo.example.com:5309 #指定端口号
bar.example.com http_port=80 maxRequestsPerChild=888 #设置主机变量在playbooks中使用

[dbserver]
db-[01:50].example.com #相似名称可简写

[dbserver:vars] #定义组变量
ntp_server=ntp.example.com

[example:children] #将组作为另一个组的成员,可供playbook使用,但不能用于/usr/bin/ansible
dbserver
webserver

```

## 分文件定义变量

```
cat /etc/ansible/hosts

[control]
192.168.33.10

[worker]
192.168.33.11
192.168.33.12

[etcd]
192.168.33.10
192.168.33.11
192.168.33.12
```

```
tree /etc/ansible/

/etc/ansible/
├── group_vars
│   ├── contorl
│   │   ├── a_setting
│   │   └── b_setting
│   ├── etcd
│   └── worker
├── hosts
└── host_vars
    └── 192.168.98.11
```

采用yaml格式编写

```
sudo cat /etc/ansible/host_vars/192.168.33.10

ansible_ssh_pass: ops
```



## inventory参数说明

```
ansible_ssh_host
      将要连接的远程主机名.与你想要设定的主机的别名不同的话,可通过此变量设置.

ansible_ssh_port
      ssh端口号.如果不是默认的端口号,通过此变量设置.

ansible_ssh_user
      默认的 ssh 用户名

ansible_ssh_pass
      ssh 密码(这种方式并不安全,我们强烈建议使用 --ask-pass 或 SSH 密钥)

ansible_sudo_pass
      sudo 密码(这种方式并不安全,我们强烈建议使用 --ask-sudo-pass)

ansible_sudo_exe (new in version 1.8)
      sudo 命令路径(适用于1.8及以上版本)

ansible_connection
      与主机的连接类型.比如:local, ssh 或者 paramiko. Ansible 1.2 以前默认使用 paramiko.1.2 以后默认使用 'smart','smart' 方式会根据是否支持 ControlPersist, 来判断'ssh' 方式是否可行.

ansible_ssh_private_key_file
      ssh 使用的私钥文件.适用于有多个密钥,而你不想使用 SSH 代理的情况.

ansible_shell_type
      目标系统的shell类型.默认情况下,命令的执行使用 'sh' 语法,可设置为 'csh' 或 'fish'.

ansible_python_interpreter
      目标主机的 python 路径.适用于的情况: 系统中有多个 Python, 或者命令路径不是"/usr/bin/python",比如  \*BSD, 或者 /usr/bin/python
      不是 2.X 版本的 Python.我们不使用 "/usr/bin/env" 机制,因为这要求远程用户的路径设置正确,且要求 "python" 可执行程序名不可为 python以外的名字(实际有可能名为python26).

      与 ansible_python_interpreter 的工作方式相同,可设定如 ruby 或 perl 的路径....
```