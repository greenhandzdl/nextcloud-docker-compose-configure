# Nextcloud Docker Compose 部署指南

本项目通过 Docker Compose 快速部署 Nextcloud 及其相关服务，适用于本地或服务器环境。包含 Caddy 反向代理（支持 Cloudflare DNS）、Collabora 在线文档、Elasticsearch、Imaginary、ClamAV、PostgreSQL、Redis 等组件。

---

## 目录结构

- `docker-compose.yml`：主编排文件，定义所有服务、网络和卷。
- `caddy/Dockerfile`：自定义 Caddy 镜像，集成 Cloudflare DNS 插件。
- `.env.example`：环境变量模板，复制为 `.env` 并根据实际情况填写。

---

## 快速开始

### 1. 环境准备

- 安装 [Docker](https://docs.docker.com/get-docker/) 和 [Docker Compose](https://docs.docker.com/compose/install/)
- 克隆本仓库：
  ```bash
  git clone <本仓库地址>
  cd nextcloud-docker-compose-configure
  ```
- 复制环境变量模板并编辑：
  ```bash
  cp .env.example .env
  # 编辑 .env 文件，填写域名、端口、数据库密码等
  ```

### 2. 启动服务

```bash
docker compose up -d
```
- 首次启动会自动构建 Caddy 镜像并拉取其他服务镜像。
- 访问 `https://<你的域名>` 进行 Nextcloud 初始化。

### 3. 服务管理

- 查看服务状态：
  ```bash
  docker compose ps
  ```
- 查看日志：
  ```bash
  docker compose logs -f
  ```
- 停止所有服务：
  ```bash
  docker compose down
  ```
- 停止并移除所有容器、网络、卷（慎用）：
  ```bash
  docker compose down -v
  ```

### 4. 镜像重建与更新

- 拉取最新镜像并重建（如有镜像更新）：
  ```bash
  docker compose pull
  docker compose up -d --build
  ```
- 仅重建 Caddy 镜像（如修改了 `caddy/Dockerfile`）：
  ```bash
  docker compose build caddy
  docker compose up -d caddy
  ```

### 5. 清理无用镜像

```bash
docker image prune -f
```

---

## 组件说明

- **Nextcloud (FPM)**：主服务，文件同步与协作平台。
- **Caddy**：HTTPS 反向代理，自动申请/续签证书，支持 Cloudflare DNS。
- **Collabora**：在线文档协作。
- **Elasticsearch**：全文检索。
- **Imaginary**：图片处理。
- **ClamAV**：病毒扫描。
- **PostgreSQL**：数据库。
- **Redis**：缓存与会话。

---

## 常见问题

- **端口冲突**：确保 `.env` 中端口未被占用。
- **证书申请失败**：检查域名解析与 Cloudflare API 配置。
- **数据持久化**：所有数据均挂载至本地卷，`docker-compose.yml` 中可查看具体路径。

---

## 升级与维护

- 升级 Nextcloud 或其他服务：
  ```bash
  docker compose pull
  docker compose up -d
  ```
- 升级 Caddy 插件：修改 `caddy/Dockerfile` 后重建。
- 定期备份数据卷。

---

## 参考链接

- [Nextcloud 官方文档](https://docs.nextcloud.com/)
- [Docker Compose 文档](https://docs.docker.com/compose/)
- [Caddy 官方文档](https://caddyserver.com/docs/)

---

如有问题欢迎提 Issue。