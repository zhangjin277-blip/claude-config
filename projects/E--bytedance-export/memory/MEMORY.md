# 项目关键信息

## miracle-pro3 最新代码路径
- **最新前端+后端源码**: `D:\桌面\miracle-pro3\`（不是 `C:\Users\Administrator\miracle-pro3\`）
- **NAS 部署路径**: `/volume1/docker/miracle-pro-cs/`
- **NAS 前端**: `/volume1/docker/miracle-pro-cs/dist/`（构建产物，需本地 build 后上传）
- **NAS 后端**: `/volume1/docker/miracle-pro-cs/server/`

## 部署流程
1. 前端：在 `D:\桌面\miracle-pro3\` 执行 `npm run build`，打包后上传 `dist/` 到 NAS
2. 后端：base64 编码上传 `.js` 文件到 NAS server/routes/
3. 重启：`docker restart miracle-pro-cs`

## 手动采集触发功能（2026-03-08 新增）
- **trigger-server.js**: bytedance-export 容器内 HTTP 服务，端口 9090
  - `POST /trigger` — 触发采集 `{ task: 'main'|'creator', date?, account? }`
  - `GET /status` — 查询状态
- **后端代理**: `server/routes/bytedance-export.routes.js` → 代理到 9090
- **前端按钮**: BusinessView.jsx 工具栏橙色"数据采集"按钮（仅 admin 可见）

## 时区修复（2026-03-08）
- `docker-compose.yml` 添加 `TZ=Asia/Shanghai`
- 修复了 `publish_date` 因 UTC 时区偏移 8 小时的问题

## NAS 连接信息
- 详见 CLAUDE.md

## 文件上传到 NAS 的方法
- 小文件用 base64: `B64=$(base64 -w0 file) && ssh ... "echo '$B64' | base64 -d > target"`
- 大文件用管道: `cat file | ssh ... "cat > target"`
- scp 在此 NAS 上不可用（权限问题）
