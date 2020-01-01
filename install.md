# Ansible 安装

## 控制节点需求

Python2(version 2.7) or Python3(version 3.5) 不支持Windows

## 被管理节点需求

Python2(version 2.7) or Python3(version 3.5)

**注意**

* 如果远端主机开启了**SElinux**,需要安装**libselinux-python**.

* 默认**Ansible**使用的**Python**路径位于 */usr/bin/python* ,在某些发行版中,仅有Python3安装于 */usr/bin/python3* .在这些系统中会收到下面的报错:

```
"module_stdout": "/bin/sh: /usr/bin/python: No such file or directory\r\n"
```

## 安装管理节点

On RHEL and CentOS:

```{bash}
sudo yum -y install ansible
```

On Ubuntu 

```{bash}
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

Via Pip

安装pip

```{bash}
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py --user
```

安装Ansible

```{bash}
pip install --user ansible
```

为使用paramiko连接插件或modules依赖于paramiko,需要安装paramiko.

```{bash}
pip install --user paramiko
```

同时支持安装在virtualenv中

```{bash}
python -m virtualenv ansible  # Create a virtualenv if one does not already exist
source ansible/bin/activate   # Activate the virtual environment
pip install ansible
```