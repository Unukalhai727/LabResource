# 教程：使用 SSH 密钥连接实验室服务器

本文介绍如何使用 SSH 公钥和私钥登录实验室服务器，适用于 Linux、macOS 和 Windows 10/11 自带的 OpenSSH 客户端。

关于 SSH 密钥登录的原理，可以阅读[SSH 密钥 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/SSH_密钥)获取稍深入的帮助。

该教程以 A6000 服务器为例，假设其内网 IP 为 10.31.112.238，公网转发服务器 IP 为 115.29.196.202，转发端口为 52022。

## 1. 准备并提交 SSH 公钥

SSH 密钥通常保存在当前用户的 `~/.ssh` 目录中（Windows 对应`C:\Users\<本机用户名>\.ssh`）。

其中，文件名以 `.pub` 结尾的是公钥，可以提交给管理员；不以 `.pub` 结尾的同名文件是私钥，必须妥善保管。

### 1.1 检查是否已有密钥

打开终端，查看 `.ssh` 目录：

```bash
# Linux / macOS
ls -al ~/.ssh
```

```powershell
# Windows PowerShell
Get-ChildItem $HOME\.ssh
```

常见的密钥文件包括：

- `id_ed25519` 和 `id_ed25519.pub`（现代常用的认证算法）
- `id_rsa` 和 `id_rsa.pub`（较早使用认证算法）

**如果目录中存在成对的私钥和公钥文件，通常可以直接使用，无需重复生成。**

### 1.2 生成新密钥

如果没有现存的 SSH 密钥，则需要生成新密钥。

推荐使用 Ed25519 算法。在 Linux、macOS 或 Windows PowerShell 中运行：

```bash
ssh-keygen -t ed25519
```

随后根据提示操作：

1. 提示保存位置时，直接按 Enter 使用默认路径（通常为`~/.ssh/id_ed25519`）；如果默认路径已存在，应取消操作并先确认现有密钥的用途；
2. 提示输入 passphrase 时，建议直接按 Enter 留空；
3. 生成完成后会得到私钥 `id_ed25519` 和公钥 `id_ed25519.pub`。

### 1.3 向管理员提交公钥

显示公钥：

```bash
# Linux / macOS
cat ~/.ssh/id_ed25519.pub
```

```powershell
# Windows PowerShell
Get-Content $HOME\.ssh\id_ed25519.pub
```

将公钥的完整一行发送给管理员，公钥类似下面这样：

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA...
```

管理员配置完成后，再继续下面的连接步骤。

## 2. 连接服务器

首次连接时，SSH 会询问是否信任服务器，并显示服务器的主机密钥指纹。

输入 `yes` 后，该主机的信息会保存到本地的 `~/.ssh/known_hosts` 文件中。

### 2.1 通过内网连接

连接实验室或校园内网后，使用服务器的内网 IP 登录：

```bash
ssh <username>@10.31.112.238
```

将 `<username>` 替换为管理员提供的用户名。

### 2.2 通过公网连接

不在实验室内网时，可以通过公网转发地址和端口登录：

```bash
ssh <username>@115.29.196.202 -p 52022
```

## 3. （可选）配置连接别名

为了避免每次输入地址和端口，可以编辑 SSH 客户端配置文件：

```text
# Linux / macOS: ~/.ssh/config
# Windows: C:\Users\<本机用户名>\.ssh\config
```

配置示例：

```sshconfig
Host lab-a6000
    HostName 10.31.112.238
    User <username>
    ServerAliveInterval 60

Host lab-a6000-public
    HostName 115.29.196.202
    User <username>
    Port 52022
    ServerAliveInterval 60
```

保存后可以直接连接：

```bash
ssh lab-a6000
ssh lab-a6000-public
```

## 4. 常见问题

### `Permission denied (publickey)`

表示服务器没有接受客户端提供的密钥。依次检查：

1. 用户名是否正确；
2. 目标服务器地址是否正确
3. 私钥是否发生变更

### `Connection timed out`或 `Connection refused`

通常是地址不可达、防火墙拦截或公网转发不可用。请确认当前网络、目标地址和端口；从校外连接时不能直接访问实验室内网地址。

### `REMOTE HOST IDENTIFICATION HAS CHANGED`

这表示服务器主机密钥与本地记录不一致，移除`~/.ssh/known_hosts`后重新连接即可
