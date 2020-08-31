# Collections

collections 是包含Ansible playbook,roles,modules,plugins内容的集合。

可以通过Ansible Galaxy 安装和使用collections.

## 安装collections

* 通过ansible-galaxy 安装collection

安装在Galaxy的collection.

```bash
ansible-galaxy collection install mynamespace.my_collection
```

也可以直接使用tarball

```bash
ansible-galaxy collection install my_namespace-my_collection-1.0.0.tar.gz -p ./collections
```

## 