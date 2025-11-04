# Marp 生成的 HTML 文件

此目录用于托管 Marp 生成的 HTML 演示文稿。

## 用途

- 存储从 `presentation*.md` 生成的 HTML 文件
- 独立版本控制，便于管理和分享演示文稿

## 使用方法

1. 使用 Marp 生成 HTML：
   ```bash
   marp docs/presentation4.md -o docs/pre/presentation4.html
   ```

2. 提交到本 submodule：
   ```bash
   cd docs/pre
   git add *.html
   git commit -m "Update presentation HTML"
   ```

