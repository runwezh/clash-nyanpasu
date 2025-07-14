# Clash Nyanpasu 项目分析

## 1. 项目概述

Clash Nyanpasu 是一个基于 [Tauri](https://tauri.app/) 构建的 [Clash](https://github.com/Dreamacro/clash) GUI 客户端。它提供了一个图形用户界面来管理 Clash 核心，支持 Clash Premium、Mihomo (Clash.Meta) 和 Clash.rs。该项目旨在提供一个功能丰富、易于使用的 Clash 客户端，并支持通过 YAML、JavaScript 和 Lua 对配置文件进行增强。

**核心功能:**

- **多核心支持**: 内置支持 Clash Premium、Mihomo 和 Clash.rs。
- **配置文件管理**: 提供强大的配置文件管理和增强功能。
- **Provider 管理**: 支持管理 Clash 的 Provider。
- **现代化 UI**: 采用 Google Material You 设计风格，并支持动画效果。

## 2. 技术栈

该项目是一个典型的 Monorepo 项目，使用 pnpm workspaces 进行管理，结合了 Rust 后端和基于 React 的前端。

- **后端 (Backend)**:
    - **框架**: [Tauri](https://tauri.app/) (使用 Rust)
    - **语言**: [Rust](https://www.rust-lang.org/)
    - **核心模块**:
        - `tauri`: Tauri 应用主模块。
        - `nyanpasu-egui`: 一个 egui 集成模块，可能用于某些原生UI部分。
        - `boa_utils`: 集成��� [Boa](https://boa-dev.github.io/) JavaScript 引擎，用于执行 JS 脚本。
        - `nyanpasu-macro`: 提供过程宏。
- **前端 (Frontend)**:
    - **框架**: [React](https://react.dev/) (v19)
    - **构建工具**: [Vite](https://vitejs.dev/)
    - **UI 库**: [MUI (Material-UI)](https://mui.com/)
    - **路由**: [TanStack Router](https://tanstack.com/router)
    - **状态管理**: [Jotai](https://jotai.org/)
    - **样式**: [Tailwind CSS](https://tailwindcss.com/) 和 [Sass](https://sass-lang.com/)
    - **语言**: [TypeScript](https://www.typescriptlang.org/)
- **包管理器**: [pnpm](https://pnpm.io/)
- **脚本与工具**:
    - 使用 `tsx` 来运行 TypeScript 脚本。
    - 代码格式化与检查: Prettier, ESLint, Stylelint, `cargo fmt`, `cargo clippy`。
    - 提交规范: Commitlint, Husky, lint-staged。
    - 自动化: GitHub Actions 用于 CI/CD。

## 3. 项目结构

项目采用 Monorepo 结构，主要分为 `backend`、`frontend` 和 `scripts` 三个部分。

```
.
├── backend/         # Rust 后端 (Tauri)
│   ├── tauri/       # Tauri 应用核心
│   ├── nyanpasu-egui/ # Egui 相关模块
│   └── ...
├── frontend/        # 前端代码
│   ├── nyanpasu/    # 主应用 (React + Vite)
│   ├── ui/          # 共享 UI 组件
│   └── interface/   # 类型定义和接口
├── scripts/         # 项目脚本 (构建、发布等)
├── locales/         # 国际化语言文件
├── manifest/        # 应用清单和更新信息
└── ...
```

- **`backend/`**: 包含所有的 Rust 代码。`backend/tauri` 是 Tauri 应用的根目录，`tauri.conf.json` 定义了应用的窗口、插件、构建配置等。
- **`frontend/`**: 包含前端工作区。
    - `frontend/nyanpasu`: 是主应用，包含了所有的页面、组件和业务逻辑。
    - `frontend/ui`: 可能包含一些可以被其他前端应用复用的基础 UI 组件。
    - `frontend/interface`: 定义了前后端通信或项目内部使用的 TypeScript 类型和接口。
- **`scripts/`**: 存放用于自动化任务的脚本，例如检查依赖、生成更新日志、发布应用等。
- **`pnpm-workspace.yaml`**: 定义了 pnpm 的工作区，将 `frontend/*` 和 `scripts` 目录下的包纳入管理。

## 4. 开发与构建

根据 `package.json` 和 `README.md`，以下是主要的开发和构建流程。

### 4.1. 环境配置

1.  **安装 Rust 和 Node.js**: 参考 [Tauri 环境要求](https://v2.tauri.app/start/prerequisites/)。
2.  **安装 pnpm**:
    ```shell
    npm install -g pnpm
    ```
3.  **安装项目依赖**:
    ```shell
    pnpm i
    ```
4.  **下载 Clash 核心等依赖**:
    ```shell
    pnpm check
    ```

### 4.2. 开发模式

要同时启动前端开发服务器和 Tauri 应用，可以运行：

```shell
pnpm dev
```

此命令会：
1.  启动 Vite 开发服务器 (`web:dev`)。
2.  启动 Tauri 应用并连接到 Vite 服务器 (`tauri:dev`)。

### 4.3. 构建应用

要构建生产环境的桌面应用，可以运行：

```shell
pnpm build
```

此命令会：
1.  构建前端代码 (`web:build`)。
2.  运行 `tauri build`，将前端代码和 Rust 后端打包成适用于当前操作系统的可执行文件。

### 4.4. Linting 和格式化

项目配置了完整的 Linting 和格式化工具，可以通过以下命令运行检查：

- **格式化代码**: `pnpm fmt`
- **检查代码风格**: `pnpm lint`

## 5. 总结

Clash Nyanpasu 是一个结构清晰、技术栈现代化的桌面应用项目。它通过 Tauri 将 Rust 的高性能和安全性与 React 生态的丰富 UI 能力结合起来。项目工程化程度很高，拥有完善的开发、构建和质量保障流程。对于想要学习 Tauri、Rust 和 React 结合开发的开发者来说，这是一个很好的学习案例。
