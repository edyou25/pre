# 简化方案：使用 repository_dispatch 触发

## 方案概述

这个方案**不需要 PAT**，只需要：
1. 主仓库的 workflow 通过 GitHub API 触发 submodule 的 workflow
2. Submodule 的 workflow 从主仓库拉取文件并生成 HTML
3. Submodule 用自己的 `GITHUB_TOKEN` 推送（不需要额外权限）

## 配置步骤

### 1. 在 submodule 仓库添加 workflow

我已经创建了 `docs/pre/.github/workflows/generate-from-main.yml`

这个 workflow 会：
- 监听 `repository_dispatch` 事件
- 从主仓库拉取文件
- 生成 HTML
- 推送到 submodule 仓库

### 2. 配置 submodule 访问主仓库的权限

由于主仓库是私有的，需要配置一个 token 让 submodule 能够访问：

#### 选项 A：使用 GitHub App（推荐，但较复杂）

#### 选项 B：使用 Personal Access Token（简单）

1. 创建一个 PAT（只需要 `repo` 权限）
2. 在 submodule 仓库添加 Secret：
   - 访问：`https://github.com/edyou25/pre/settings/secrets/actions`
   - 名称：`MAIN_REPO_TOKEN`
   - 值：粘贴 PAT

### 3. 主仓库的 workflow

我已经创建了 `.github/workflows/trigger-presentation.yml`

这个 workflow 会：
- 检测到文件变化时
- 通过 GitHub API 触发 submodule 的 workflow

### 4. 禁用旧的 workflow（可选）

如果你想完全使用新方案，可以：
- 删除或重命名 `.github/workflows/generate-presentation.yml`
- 或者保留作为备份

## 工作流程

```
主仓库 push
    ↓
触发 trigger-presentation.yml
    ↓
通过 API 触发 submodule 的 workflow
    ↓
Submodule 从主仓库拉取文件
    ↓
生成 HTML 并推送
    ↓
GitHub Pages 自动部署
```

## 优势

✅ **不需要 PAT 来推送 submodule**（submodule 用自己的 token）  
✅ **主仓库的 workflow 很简单**（只负责触发）  
✅ **所有生成逻辑在 submodule 中**（职责清晰）  
✅ **自动触发 GitHub Pages 部署**

## 注意事项

1. **主仓库访问权限**：如果主仓库是私有的，submodule 需要 `MAIN_REPO_TOKEN` 来访问
2. **第一次设置**：需要将 submodule 的 workflow 文件推送到 `edyou25/pre` 仓库
3. **测试**：可以手动触发 workflow 来测试

## 测试

1. 推送 submodule 的 workflow 文件：
   ```bash
   cd docs/pre
   git add .github/workflows/generate-from-main.yml
   git commit -m "Add workflow to generate from main repository"
   git push
   ```

2. 配置 `MAIN_REPO_TOKEN` secret（如果主仓库是私有的）

3. 在主仓库做一个小的修改并 push，观察是否触发

