# Marp 生成的 HTML 文件

此目录用于托管 Marp 生成的 HTML 演示文稿。

## 用途

- 存储从 `presentation*.md` 生成的 HTML 文件
- 独立版本控制，便于管理和分享演示文稿

## 使用方法

### 自动生成（推荐）

GitHub Actions 会自动处理：
- 当 `docs/presentation*.md` 或 `docs/assets/**` 文件更新时
- 自动复制 assets 到 submodule
- 自动生成所有 HTML 文件
- 自动提交并推送到 submodule
- 自动部署到 GitHub Pages

### 手动生成

1. 使用 Marp 生成 HTML：
   ```bash
   marp docs/presentation4.md -o docs/pre/presentation4.html
   ```

2. 复制 assets（如果需要）：
   ```bash
   mkdir -p docs/pre/assets/pictures
   cp -r docs/assets/pictures/* docs/pre/assets/pictures/
   ```

3. 提交到本 submodule：
   ```bash
   cd docs/pre
   git add -A
   git commit -m "Update presentation HTML and assets"
   git push
   ```

