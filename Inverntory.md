# Inverntory


Ansible 默认库文件位于 /etc/ansible/hosts 可以使用-i参数指定库文件路径

支持配置文件格式 *YAML* *INI*

默认组为all和ungrouped. all组包含所有主机，ungrouped组包含所有未在其他组（除all组）中的主机。所有主机至少属于两个组。

*INI* 配置示例

```ini
mail.example.com # 定义主机名

[webservers] #定义组名
foo.example.com:5309 #指定端口号
bar.example.com http_port=80 maxRequestsPerChild=888 #设置主机变量在playbooks中使用
www[a:f].example.com #相似字母简写

[dbservers]
db-[01:50].example.com #相似名称可简写

[dbserver:vars] #定义组变量,vars
ntp_server=ntp.example.com

[examples:children] #将组作为另一个组的成员,可供playbook使用,但不能用于/usr/bin/ansible
dbservers
webservers

```

等效 *YAML* 配置示例

```yaml
---
all:
    hosts: #在yaml中，hosts下存放主机及其变量
        mail.example.com
    children: #children 存放子组
        webservers:
            hosts:
                foo.example.com:5309:
                bar.example.com:
                    http_port: 80
                    maxRequestsPerChild: 888
        dbservers:
           hosts:
               db-[01:50].example.com:
           vars:
               ntp_server: ntp.example.com
        examples:
            children:
                webservers:
                dbservers:
               
```


## 变量合并

从低到高

* all group
* parent group
* child group
* host

默认情况下，

同级group可以通过设置ansible_group_prority 影响变量，数值最大的会被继承

```yaml
a_groups:
    testvar: a
    ansible_group_prority: 10
b_groups:
    testvar: b
```



## 分文件定义变量

可分文件定以host及group 变量，文件需要符合YAML 语法格式。文件扩展名需要以‘.yml’,'.yaml','.json'结尾。

**文件结构**

```
/etc/ansible/hosts #仓库主文件
/etc/ansible/group_vars/webservers #webservers组变量
#也可以为组设置文件夹，ansible-playbook 会遍历其文件夹下的文件
/etc/ansible/group_vars/dbservers/db_settings
/etc/ansible/group_vars/dbservers/ntp_settings
/etc/asnible/host_vars/bar.example.com #bar.example.com 主机变量
```

**变量文件内容**

```yaml
---
    ntp_server: acme.example.org
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
