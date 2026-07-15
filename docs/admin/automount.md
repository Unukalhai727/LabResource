## NAS自动挂载

### Step 1

编写`/etc/systemd/system/mnt-nas.mount`

其中`10.31.112.30`为NAS的IP地址，`/volume1/data`为NAS的挂载点，`/mnt/nas`为系统的挂载位置

参数`Options=soft,timeo=5,nofail`可在挂载失败时避免卡启动

```
[Unit]
Description=Mount lab-nas data for /mnt/nas with NFS

[Mount]
What=10.31.112.30:/volume1/data
Where=/mnt/nas
Type=nfs
Options=soft,timeo=5,nofail
```

### Step 2

编写`/etc/systemd/system/mnt-nas.automount`

```latex
[Unit]
Description=Automount NFS Share for /mnt/nas
Requires=network-online.target
After=network-online.target

[Automount]
Where=/mnt/nas
TimeoutIdleSec=600

[Install]
WantedBy=multi-user.target
```

### Step 3

启用服务，启动开机自动挂载

```shell
sudo systemctl start mnt-nas.automount
sudo systemctl enable mnt-nas.automount
```
