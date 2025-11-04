# 配置 Personal Access Token (PAT) 以支持自动部署

## 问题说明

GitHub Actions 的默认 `GITHUB_TOKEN` 只能访问当前仓库，无法推送到其他仓库（包括 submodule）。因此需要创建一个 Personal Access Token (PAT)。

## 解决步骤

### 1. 创建 Personal Access Token

1. 访问 GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
   - 链接：https://github.com/settings/tokens

2. 点击 "Generate new token (classic)"

3. 配置 Token：
   - **Note**: `Submodule Push Token`（或其他描述性名称）
   - **Expiration**: 选择合适的时间（建议 90 天或 1 年）
   - **Scopes**: 勾选 `repo`（完整仓库访问权限）
     - 这会允许 token 访问所有仓库

4. 点击 "Generate token"

5. **重要**：立即复制 token（只显示一次！）
   - 格式类似：`ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

### 2. 添加 Token 到仓库 Secrets

1. 访问主仓库的 Settings → Secrets and variables → Actions
   - 链接：`https://github.com/edyou25/dog/settings/secrets/actions`

2. 点击 "New repository secret"

3. 配置 Secret：
   - **Name**: `SUBMODULE_PAT`
   - **Secret**: 粘贴刚才复制的 token

4. 点击 "Add secret"

### 3. 验证配置

1. 推送 workflow 文件的更改到主仓库
2. 修改 `docs/presentation*.md` 或 `docs/assets/**` 文件
3. Push 到主仓库
4. 检查 GitHub Actions 是否成功运行
5. 检查 submodule 是否有新提交

## 安全提示

- ✅ Token 只存储在 GitHub Secrets 中，不会暴露在日志中
- ✅ 使用最小权限原则（只给 `repo` 权限）
- ✅ 定期更新 token（建议每 90 天）
- ⚠️ 不要将 token 提交到代码仓库
- ⚠️ 如果 token 泄露，立即撤销并重新创建

## 故障排除

如果仍然遇到权限问题：

1. **检查 token 权限**：确保 token 有 `repo` 权限
2. **检查 secret 名称**：确保 secret 名称是 `SUBMODULE_PAT`（区分大小写）
3. **检查 token 是否过期**：如果过期，重新创建并更新 secret
4. **检查 submodule 仓库权限**：确保 token 对应的账户有权限推送到 `edyou25/pre` 仓库

## 替代方案

如果不想使用 PAT，可以考虑：

1. **使用 GitHub App**：创建 GitHub App 并配置相应权限
2. **使用 Deploy Key**：为 submodule 仓库配置 deploy key
3. **手动触发**：不使用自动化，手动 push 到 submodule

