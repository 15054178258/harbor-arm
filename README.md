---

# Harbor ARM64 Installer Factory 🚀

这是一个基于 GitHub Actions 的自动化工具，用于构建 **Harbor 官方离线安装包的 ARM64 版本**。

### 为什么需要这个项目？

Harbor 官方发布的离线安装包（Offline Installer）通常仅包含 `x86_64` 架构的镜像。在华为鲲鹏、飞腾、飞利浦等 ARM 架构服务器（国产化环境）上部署时，需要手动拉取 ARM 镜像并重新打包，过程繁琐且容易出错。

本项目通过 GitHub Actions 自动完成：

1. 从 `ghcr.io/octohelm/harbor`（或其他 ARM 镜像源）拉取全量组件。
2. 按照官方规范自动重命名镜像标签。
3. 下载官方 Release 模板，替换镜像包，生成最终的 `.tgz` 安装包。
4. 自动发布到 GitHub Releases。

---

## 🛠 快速开始

### 1. 部署到你的仓库

1. 将本项目代码克隆或复制到你的 GitHub 仓库。
2. 确保 `.github/workflows/` 目录下存在 `harbor-arm-release.yml`。

### 2. 运行工作流 (Actions)

1. 进入你仓库的 **Actions** 选项卡。
2. 在左侧菜单选择 `Harbor ARM64 Full Release Factory`。
3. 点击 **Run workflow** 下拉按钮。
4. 输入你想要构建的 Harbor 版本号（例如 `v2.10.2` 或 `v2.14.0`）。
5. 点击 **Run workflow** 开始构建。

### 3. 下载安装包

1. 等待工作流执行完毕（约 5-10 分钟）。
2. 在仓库右侧的 **Releases** 栏目中，下载生成的 `harbor-arm64-installer-v2.x.x.tgz`。

---

## 📦 包含的组件镜像

本项目对齐了官方离线安装包的所有必要组件：

* **核心组件**: `core`, `jobservice`, `portal`, `log`
* **存储组件**: `registry`, `registryctl`, `redis`, `postgresql`
* **功能组件**: `nginx`, `exporter`, `chartmuseum`, `trivy-adapter`
* **安装必备**: `harbor-prepare` (关键：用于初始化配置)

---

## 🚀 在 ARM 服务器上安装

下载完安装包后，操作流程与官方完全一致：

```bash
# 1. 解压安装包
tar -xvf harbor-arm64-installer-v2.14.0.tgz

# 2. 进入目录
cd harbor

# 3. 准备配置文件 (根据需求修改 harbor.yml)
cp harbor.yml.tmpl harbor.yml
vi harbor.yml

# 4. 执行安装脚本
sudo ./install.sh

```

---

## ⚠️ 注意事项

* **镜像来源**: 默认使用 `ghcr.io/octohelm/harbor` 提供的 ARM 镜像。如果某些极新版本在该源中缺失，构建可能会跳过部分组件。
* **权限**: 确保你的 GitHub 仓库开启了 `Settings -> Actions -> General -> Workflow permissions -> Read and write permissions`，否则无法自动创建 Release。
* **自定义镜像**: 如果你想更换镜像源，只需修改 `.github/workflows/` 脚本中的 `SOURCE_REGISTRY` 变量即可。

---

## 📄 开源协议

[MIT License](https://www.google.com/search?q=LICENSE)

---
