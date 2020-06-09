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
    notify:
    - restart apache
  - name: ensure apache is running
    service:
        name: httpd
        state: started
  handlers: #
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
