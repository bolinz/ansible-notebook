# Playbook

## playbook 示例

```yaml
---
- hosts: webservers #主机名
  vars: #变量
      http_port: 80
      max_client: 200
  remote_user: root #ansible 远程登录用户
  become: yes #可以使用become 使用其他用户执行任务
  tasks: #任务列表
  - name: ensure apache is at the latest version
    remote_user: root #可以针对单个task定制
    yum:
        name: httpd
        state: latest
  - name: write the apache config file
    template:
        src: /srv/httpd.j2
        dest: /etc/httpd.conf
    notify: #触发对应名称的handler
    - restart apache
  - name: ensure apache is running
    service:
        name: httpd
        state: started
  handlers: #发生变更时，执行的操作
  - name: restart apache
    service:
        name: httpd
        state: restarted
        
- hosts: databases  #一个playbook中可以包含多个主机
  remote_users: root
  
  tasks:
  - name: ensure postgresql is at the last version
  yum: 
      name: postgresql
      state: latest
  - name: ensure that postsql is started
    service:
        name: postgresql
        state: started
```


## Handlers

在每个task尾部可以使用'notify' 动作触发，且多个task仅触发一次。

当文件改变时，重启两个服务

```yaml
- name: template configuration file
  template:
      src: template.j2
      dest: /etc/foo.conf
  notify:
    - restart mecahed
    - restart apache
```

handlers 与常规 tasks 不同，具有全局唯一名，且需要备notify 触发。如果没有触发hanlder,其不会执行。多条任务触发同一个handler，其在特定play的任务完成后，仅执行一次。


handlers 中可以携带变量

```yaml
handlers:
- name: restart "{{ web_service_name }}"
```

**如果 handler name中的变量不可用，play将执行失败。play 中途改变变量不会影响到新的handler.**

作为替代，将变量放置与handler 中 task 的参数中。 可以使用 include_vars 导入值，如下：

```yaml
tasks:
 - name: Set host variables based on distribution
   include_vars: "{{ ansible_facts.distribution}}.yml"
   
handlers:
 - name: restart web service
   service:
       name: "{{ web_service_name | default('httpd') }}"
       state: restarted
```

## playbooks 复用

ansibel 使用三种方式： *include* *import* *roles*

*include* *import* 可以将单个playbook拆分为更小的子playbook,并可多处复用

*roles* 不仅将task 打包，且可以包含handlers,变量，模块及其他插件。不同于*include* 和 *import*,roles 可以通过 ansible Galaxy 上传并分享。

### 动态与静态

ansible使用*动态*和*静态*两种方式复用内容。

动态： include
静态： import

## 变量

**变量合法性**

变量可以使用 字母，数字，及下划线，且必须以字母开头。

YAML同时支持k-v结构的字典

```yaml
foo:
    field: one
    field: two
```

调用变量

```yaml
foo['field1']
foo.field2
```

### 在仓库中定义变量

### 在playbook定义变量

```yaml
- host: webservers
  vars:
      http_port: 80
```

### 在include 文件和roles中定义变量

### 在jinja2中使用变量

 ```jinja2
 My app goes tp {{ max_amp_value }}
 ```

> ansible 允许在模板中使用jinja2的循环及条件，但无法在 playbooks 中使用。

### 使用jinja2 过滤器转换变量

根据YAML语法要求， 值以 {{ foo }} 开头时需要以双引号包含，以保证是一个YAML字典。

错误的写法：

```yaml
- hosts: app_servers
  vars:
      app_path: {{ base_path }}/22
```

正确的写法：

```yaml
- hosts: app_servers
  vars:
      app_path: "{{ base_path }}/22"
```

### 系统发现变量: Facts

Facts 是从远程系统中收集的信息。使用*ansible_facts*变量可以获取完整集合。大多数变量提升到了顶级变量域中，以ansible_开头；其他的下沉以避免冲突。此行为可以通过**INJECT_FACTS_AS_VARS**设置控制。

在playbook 中加入以下内容查看哪些信息可用

```yaml
- debug: var=ansible_facts
```

也可以使用下面方法查看原始信息

```shell
ansible hostname -m setup
```

调用方法

```jinja2
{{ ansible_facts['devices']['xvda']['model'] }}
{{ ansible_facts['nodename'] }}
```



## 模板

## 条件

## 循环

## 阻断

## 高级特性

## 控制ansible执行

## 最佳实践

