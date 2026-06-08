---
title: Docker Rclone 挂载 Webdav
published: 2026-05-10
image: api
category: 教程
tags: [alist, Webdav, rclone, Docker]
---

# Docker Rclone 挂载 Webdav

## Docker-compose 部署

创建挂载目录

```bash
sudo mkdir -p /data/ali_webdav
sudo mount --bind /data/ali_webdav /data/ali_webdav
sudo mount --make-shared /data/ali_webdav
```

### 创建 `docker-compose.yml`

```
services:
  rclone:
    image: rclone/rclone:latest
    container_name: rclone
    restart: unless-stopped
    volumes:
      - /data/ali_webdav:/mnt:rshared
      - ./config:/config/rclone
    devices:
      - /dev/fuse
    cap_add:
      - SYS_ADMIN
    security_opt:
      - apparmor:unconfined
    command: >
      mount alist:/ /mnt
      --allow-other
      --vfs-cache-mode writes
      --allow-non-empty
      --log-level DEBUG
```

### 使用说明

1. **准备目录结构**：

   ```bash
   mkdir -p ./config && sudo mkdir -p /data/ali_webdav
   ```

2. **设置共享挂载**（只需执行一次）：

   ```bash
   sudo mount --bind /data/ali_webdav /data/ali_webdav && sudo mount --make-shared /data/ali_webdav
   ```

3. **生成`./config/rclone.conf`**：

   ```bash
   # 推荐使用容器生成配置文件
   docker run -it --rm \
     -v ./config:/config/rclone \
     --cap-add SYS_ADMIN \
     --device /dev/fuse \
     --security-opt apparmor:unconfined \
     rclone/rclone:latest config


   [alist]
   type = webdav
   url = http://192.168.1.100:5244/dav
   vendor = other
   user = admin
   pass = $(alist rclone encryption password)  # 必须使用rclone加密后的密码
   ```

4. **启动服务**：

   ```bash
   docker compose up -d
   ```

## 常见问题处理

1. **挂载目录为空**：

   - 确认已执行 `mount --make-shared`。
   - 检查 rclone.conf 中的密码是否正确。
   - 增加 `--log-level DEBUG` 查看详细错误。

2. **权限问题**：

   ```bash
   sudo chmod -R 777 /data/ali_webdav  # 临时调试用
   ```

3. **更新配置**：

   ```bash
   docker compose restart rclone
   ```

4. **开机自动创建挂载点(可选)**

5. 系统服务文件`/etc/systemd/system/cloud-mount.service`

```bash
[Unit]
Description=Persistent mount binding

[Service]
Type=oneshot
ExecStart=/bin/mount --bind /data/ali_webdav /data/ali_webdav
ExecStart=/bin/mount --make-shared /data/ali_webdav
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target

```

2. 服务自启

```bash
sudo systemctl daemon-reload && sudo systemctl enable --now cloud-mount
```

---

通过本方案可实现：
✅ 容器化隔离部署
✅ 配置版本化管理
✅ 快速水平扩展
✅ 挂载状态监控

建议搭配 `systemd` 或 `supervisord` 实现服务保活，生产环境建议启用 TLS 加密 WebDAV 连接。
