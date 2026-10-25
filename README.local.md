# 本地开发

这是若依 Vue 3 / Element Plus / Pinia 前端，使用 Vite 6。配套后端为 JDK 8 / Spring Boot 2 版本，具体依赖以 `package.json` 为准。

## 环境要求

- 准备 Node.js 与一个包管理器。[Vite 6 官方说明](https://v6.vite.dev/guide/)列出 Node.js 18+ 或 20+ 要求；还需满足本项目全部依赖的 `engines` 限制，并记录实际安装和构建通过的版本。
- 按项目选定的包管理器安装依赖；已有对应锁文件时使用冻结锁文件的安装方式，不混用锁文件。本仓库保留 npm 的 `package-lock.json`，正常复现使用 `npm ci`；仅在明确更新依赖时重新生成锁文件。
- 准备可访问的配套本地后端，并使用专用开发库中配置的账户进行联调。

## 本机配置

公开配置可以保存在仓库外的本机环境文件，由自己的启动器导出为进程环境变量；Vite 不会自动寻找仓库外文件。也可使用未提交的 `.env.development.local`；提交前确认该文件被 Git 忽略。

请求前缀由 `VITE_APP_BASE_API` 读取，例如本地代理使用 `/dev-api`。已有进程环境变量优先于 `.env` 文件；改动后需重启开发服务。[Vite 环境变量说明](https://v6.vite.dev/guide/env-and-mode)

当前 `vite.config.js` 的开发代理通过 `baseUrl` 指向 `http://localhost:8080`，并转发 `/dev-api`。它尚未提供代理目标环境变量；只设置一个自定义变量不会自动改变代理目标。后端端口不同时，需明确调整代理配置或另行实现外置配置读取。

`VITE_*` 会暴露给浏览器，禁止放入密码、令牌或模型密钥；账户凭据由后端管理，示例仅使用环境变量占位。

## 启动与构建

在仓库根目录执行。下面使用仅监听本机的开发地址和普通端口；端口可按实际情况调整。

```sh
npm run dev -- --host 127.0.0.1 --port 5173 --strictPort
npm run build:prod
```

暂存环境构建使用 `npm run build:stage`，本地查看构建结果使用 `npm run preview -- --host 127.0.0.1`。以终端实际输出地址为准；前台服务可用 `Ctrl+C` 停止。

当前脚本未提供 `test` 或 `lint`。交付时分别记录构建结果和登录、菜单、权限、相关列表或表单的联调结果；本文中的命令示例不代表已经执行通过。
