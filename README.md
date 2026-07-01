# 🦖 Hexo

基于 [Hexo](https://hexo.io/) 的静态博客仓库，通过 GitHub Actions 实现博客的初始化、主题安装与自动部署，无需在本地安装 Node.js / Hexo 环境。

## 📁 目录结构

```
.
├── .github/workflows/
│   ├── InitHexo.yml           # 🚀 一次性初始化 Hexo 脚手架
│   ├── InstallHexoTheme.yml   # 🎨 手动安装并应用主题
│   └── Deploy.yml             # 📦 构建并部署到 GitHub Pages
├── .gitignore
└── LICENSE
```

> 💡 注：Hexo 生成的博客源码（`package.json`、`_config.yml`、`source/` 等）会在运行 `InitHexo` 后自动提交到仓库，首次使用时这些文件还不存在。

## 🚀 快速开始

### 1️⃣ 初始化博客（只需运行一次）

在 GitHub 仓库的 **Actions** 页面找到 **Init Hexo Blog**，点击 **Run workflow** 手动触发。

它会自动完成：
- 📦 安装 `hexo-cli`
- 🏗️ 在空目录中运行 `hexo init`，生成博客脚手架
- 📥 安装依赖
- ✅ 将生成的文件提交并推送回仓库

### 2️⃣ 启用 GitHub Pages

进入仓库 **Settings → Pages**，将 **Build and deployment → Source** 设置为 **GitHub Actions**（而不是 "Deploy from a branch"，避免触发 GitHub 自带的 Jekyll 构建导致冲突）。

### 3️⃣ 安装主题（可选）

在 **Actions** 页面找到 **Install Hexo Theme**，点击 **Run workflow**，输入主题的 npm 包名，例如：

- `hexo-theme-butterfly`
- `hexo-theme-anzhiyu`
- `hexo-theme-shoka`

它会自动完成：
- 📦 安装主题 npm 包
- 🖌️ 安装 `pug` / `stylus` 渲染器（部分主题模板依赖它们）
- ⚙️ 修改 `_config.yml` 中的 `theme` 字段
- 📝 若主题自带默认配置，则生成对应的 `_config.<主题名>.yml` 独立配置文件（用于自定义导航栏、头像、颜色等，避免主题更新时被覆盖）
- ✅ 提交并推送改动

### 4️⃣ 部署

`Deploy.yml` 监听 `main` 分支的 push 事件，也支持手动触发（**Run workflow**）。每次触发都会：

1. 📥 安装依赖
2. 🏗️ 执行 `hexo generate` 构建静态站点
3. 🚀 将构建产物部署到 GitHub Pages

## ❓ 常见问题

**🎨 页面样式没有加载 / 显示裸文字？**
检查 `_config.yml` 中的 `url` 和 `root` 字段是否与实际的 GitHub Pages 地址一致：

- 用户主页仓库（`用户名.github.io`）：`root: /`
- 项目仓库（如本仓库）：`root: /仓库名/`，且 `url` 需带上仓库名路径，例如 `https://用户名.github.io/仓库名`

**📂 主题目录（`themes/`）里看不到装好的主题？**
通过 npm 安装的主题实际存放在 `node_modules/` 中，而不是 `themes/` 文件夹，这是正常现象。`node_modules/` 已被 `.gitignore` 排除，因此不会出现在仓库里。

**🔒 Push 失败，提示 403 Permission denied？**
前往 **Settings → Actions → General → Workflow permissions**，选择 **Read and write permissions**，让 workflow 使用的默认 `GITHUB_TOKEN` 拥有推送权限。

## 📄 License

本仓库使用 [MIT License](./LICENSE)。
