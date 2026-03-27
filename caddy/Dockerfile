# 使用官方的 Caddy builder 镜像
FROM caddy:2-builder AS builder

# 编译并加入 cloudflare 插件
RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare

# 构建最终镜像
FROM caddy:2

# 复制编译好的二进制文件
COPY --from=builder /usr/bin/caddy /usr/bin/caddy