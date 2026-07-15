## 系统重置流程

1. 备份数据
2. 安装Ubuntu Server 24.04 LTS
    1. 在版本选择页勾选 third-party driver
    2. 在镜像源配置页填入BFSU的URL
    3. 在磁盘配置页反选 LVM
    4. 在附加设置页勾选 openssh server
3. 安装必要包
    - build-essential: C/C++编译套件
    - nvidia-headless-<version>-server-open: nvidia驱动，如果在上一步已经安装可跳过
    - nvidia-utils-<version>-server: `nvidia-smi`管理工具
4. （optional）调整Local TTY分辨率


```bash
# 编辑`/etc/default/grub`
GRUB_GFXMODE=1920x1080
GRUB_GFXPAYLOAD_LINUX=keep
```

```bash
# 执行
sudo update-grub
sudo reboot
```

5. 配置防火墙

```bash
# 1) 对实验室内网 10.31.112.0/24：开放全部入站端口
sudo ufw allow from 10.31.112.0/24

# 2) 对校园网 10.0.0.0/8：仅开放 22 入站端口（SSH）
sudo ufw allow from 10.0.0.0/8 to any port 22 proto tcp

# 4) 启用防火墙
sudo ufw enable
```

6. 配置SSH
    1. 上传公钥
    2. 设置为仅允许密钥登录
7. （optional）编译并安装btop