# command line tools

## ansible

为指定主机运行单次任务

**可选参数**
|参数|说明|
|-|-|
|--ask-vault-pass|询问vault密码|
|--become-method|指定提权模式,默认%(defaults)s,使用ansible-doc -t become -l 检查可选项|
|--become-user <BECOME_USER>|以该用户执行操作,默认root|
|--list-hosts|输出所有匹配主机,不执行任何操作|
|--playbook-dir <BASEDIR>||
|--private-key <PRIVATE_KEY_FILE>, --key-file <PRIVATE_KEY_FILE>|指定连接使用的私钥|
|--scp-extra-args <SCP_EXTRA_ARGS>|scp 额外选项|
|--sftp-extra-args <SFTP_EXTRA_ARGS>|sftp 额外选项|
|--ssh-common-args <SSH_COMMON_ARGS>|sftp,scp,ssh共通选项|
|--ssh-extra-args <SSH_EXTRA_ARGS>|ssh 额外选项|
|--syntax-check|仅语法检查,不执行|
|--vault-id|使用的vault id|
|--vault-password-file|vault 密码文件|
|--version|显示程序版本|
|-B <SECCOUNDS>,--background <SECONDS>|异步执行,X秒后失败,默认N/A|
|-C,--check|检查会发生的改变,但不执行|
|-D,--diff|当对小文件进行变更时,检查文件的差异,比--check效果好|
|-K,--ask-become-pass|询问提权密码|
|-M,--module-path|指定module路径|
|-P <POOL_INTERVAL>,--pool <POOL_INTERVAL>|若使用-B,设置轮询间隔|
|-T <TIMEOUT>,--timeout <TIMEOUT>|修改连接超时时间,默认10秒|
|-a <MODULE_ARGS>,--args <MODULE_ARGS>|模块参数|
|-b,--become|以become模式运行,不提示输入密码|
|-c <CONNECTION>,--connection <CONNECTION>|指定连接方式,默认smart|
|-e,--extra-vars|定义额外变量,如key=value 或YAML/JSON文件|
|-f <FORKS>,--forks <FORKS>|指定并发数量,默认5|
|-h |帮助|
|-k,--ask-pass|询问连接密码|
|-l <SUBSET>,--limit <SUBSET>|进一步限制主机或组,仅执行-l后的主机|
|-m <MODULE_NAME>,--module-name <MODULE_NAME>|指定模块名|
|-o,--one-line|单行输出|
|-t <TREE>,--trss <TREE>|以主机名为文件名保存日志到指定目录,若存在文件会覆盖|
|-u <REMOTE_USER>,--user <REMOTE_USER>|指定远程连接用户|
|-v,--verbose|详情模式,-vvv显示更多信息,-vvvv 开启调试模式|

## ansible-config

```{bash}
ansible-config [-h] [--version] [-v] {list,dump,view}
```

### 共通选项

|选项|说明|
|-|-|
|--version|显示版本信息|
|-h,--help|显示帮助信息|
|-v,--verbose|显示详情|

### 动作

**list**

显示从 *lib/constants.py* 中读取的配置并展示环境变量和配置文件名称

```{bash}
-c <CONFIG_FILE>,--config <CONFIG_FILE>
```

**dump**

显示当前配置,如果定义了ansible.cfg,与其对比

```--only-changed```
仅显示从默认值变更的项目

**view**

显示当前配置文件

### 环境变量

```ANSIBLE_CONFIG```-替代默认ansible 配置文件

### 文件

```/etc/ansible/ansible.cfg```-配置文件
```~/.ansible.cfg```-用户配置文件,如果预设会替代默认配置