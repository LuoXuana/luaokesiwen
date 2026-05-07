---
tags:
  - obsidian
  - git
  - setup
---

# Obsidian Git 自动备份设置

## ✅ 已完成

- [x] git 仓库已初始化（`git init`）
- [x] `.gitignore` 已配置（排除 workspace、缓存等无关文件）
- [x] 插件文件已下载到 `.obsidian/plugins/obsidian-git/`
  - `manifest.json` — v2.38.2
  - `main.js` — 主程序
  - `styles.css` — 样式
- [x] 已注册到 `community-plugins.json`
- [x] 预设自动备份配置（`data.json`）

## 📋 还需要你做

### 1️⃣ 开启 Obsidian 命令行接口

```
Obsidian → 设置 → 常规 → 高级 → 打开「命令行接口」
```

### 2️⃣ 启用插件

```
Obsidian → 设置 → 第三方插件 → 社区插件 → 刷新列表
→ 找到「Git」→ 启用
```

或者等 Obsidian 重启后，插件应该会自动加载。

### 3️⃣ 关联 GitHub 仓库

在 Obsidian 里按 `Ctrl+P` → 输入 `Git: Clone an existing remote repo`，或者手动：

```bash
# 在命令行中（在 E:\claude 目录下）
git remote add origin https://github.com/你的用户名/你的仓库名.git
git add -A
git commit -m "first backup"
git branch -M main
git push -u origin main
```

如果还没有 GitHub 仓库，先去 [github.com](https://github.com) 建一个空的。

### 4️⃣ 检查插件是否运行

Obsidian 右下角状态栏会出现 Git 图标和分支名，表示插件已激活。

---

## ⚙️ 已预设的自动备份策略

| 设置 | 值 | 说明 |
|------|-----|------|
| 自动 commit 间隔 | 10 分钟 | 每 10 分钟自动备份 |
| 自动 pull 间隔 | 10 分钟 | 同步远程变更 |
| 文件变更自动备份 | ✅ 开启 | 改完文件就触发 |
| commit 格式 | `auto: {{date}}` | 自动备份的消息格式 |

这些可以在 Obsidian 里随时改：`设置 → Git`
