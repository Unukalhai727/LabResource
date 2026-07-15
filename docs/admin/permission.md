## 用户、管理员及权限操作

- 添加用户

```bash
sudo adduser <username>
```

- 删除用户

```bash
sudo deluser <username>
```

- 注册管理员

```bash
sudo usermod -aG sudo <username>
```

- 注销管理员

```bash
sudo deluser <username> sudo
```

- 赋予普通用户对特定目录的读/写权限（其他用户创建的文件夹不受影响）

```bash
sudo setfacl -m u:<username>:rwX <path>
sudo setfacl -d -m u:<username>:rwX <path>
```

- 撤销普通用户对特定目录的权限

```bash
sudo setfacl -x u:<username> <path>
sudo setfacl -x d:u:<username> <path>
```

- 重置特定目录的权限

```bash
sudo setfacl -b -k <path>
```