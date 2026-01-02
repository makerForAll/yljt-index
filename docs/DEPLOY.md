# 部署到阿里云 OSS

本文档说明如何使用 GitHub Actions 自动部署 VuePress 站点到阿里云 OSS。

## 前置准备

1. **阿里云 OSS Bucket**
   - 在阿里云控制台创建 OSS Bucket
      
   - 记录 Bucket 名称和 Region（如：`oss-cn-hangzhou`）
   - 获取 Endpoint（如：`https://oss-cn-hangzhou.aliyuncs.com`）

   

2. **AccessKey**
   - 在阿里云控制台创建 AccessKey
   - 记录 `AccessKey ID` 和 `AccessKey Secret`
   - ⚠️ **重要**：建议创建子账号并授予 OSS 相关权限，不要使用主账号的 AccessKey

3. **GitHub 仓库**
   - 将代码推送到 GitHub 仓库
   - 确保仓库是公开的或你有 Actions 权限

## 配置 GitHub Secrets

在 GitHub 仓库中配置以下 Secrets：

1. 进入仓库 → **Settings** → **Secrets and variables** → **Actions**
2. 点击 **New repository secret** 添加以下密钥：

| Secret 名称 | 说明 | 示例值 |
|-----------|------|--------|
| `OSS_ENDPOINT` | OSS Endpoint | `https://oss-cn-hangzhou.aliyuncs.com` 或 `oss-cn-hangzhou.aliyuncs.com` |
| `OSS_BUCKET` | OSS Bucket 名称 | `my-bucket` |
| `OSS_AK` | AccessKey ID | `LTAI5t...` |
| `OSS_SK` | AccessKey Secret | `xxx...` |
| `OSS_PREFIX` | OSS 路径前缀（可选） | `docs/` 或留空 |
| `OSS_CLEAR_BEFORE_UPLOAD` | 上传前是否清空目录（可选） | `true` 或 `false`，默认 `false` |
| `OSS_CUSTOM_DOMAIN` | 自定义域名（可选） | `docs.example.com` |

### 必需配置

- `OSS_ENDPOINT`: OSS 的 Endpoint 地址
- `OSS_BUCKET`: OSS Bucket 名称
- `OSS_AK`: 阿里云 AccessKey ID
- `OSS_SK`: 阿里云 AccessKey Secret

### 可选配置

- `OSS_PREFIX`: 如果文件要上传到 Bucket 的子目录，设置此值（如：`docs/`）
- `OSS_CLEAR_BEFORE_UPLOAD`: 设置为 `true` 会在上传前清空目标目录
- `OSS_CUSTOM_DOMAIN`: 如果配置了自定义域名，设置此值

## 使用方法

### 自动部署

1. 将代码推送到 `main` 分支
2. GitHub Actions 会自动触发构建和部署
3. 在仓库的 **Actions** 标签页查看部署进度

### 手动部署

1. 进入仓库的 **Actions** 标签页
2. 选择 **Deploy to Aliyun OSS** 工作流
3. 点击 **Run workflow** 手动触发部署

## 配置 OSS Bucket

### 1. 设置静态网站托管

1. 进入 OSS 控制台
2. 选择你的 Bucket
3. 进入 **基础设置** → **静态网站托管**
4. 启用静态网站托管
5. 设置默认首页为 `index.html`
6. 设置默认 404 页（可选）

### 2. 设置 Bucket 权限

1. 进入 **权限管理** → **读写权限**
2. 设置为 **公共读**（如果使用自定义域名，可以保持私有）

### 3. 配置自定义域名（可选）

1. 进入 **传输管理** → **域名管理**
2. 绑定你的自定义域名
3. 配置 CNAME 解析
4. 如果使用 HTTPS，可以配置 SSL 证书

## 访问站点

部署完成后，可以通过以下方式访问：

- **OSS 默认域名**: `https://<bucket-name>.<region>.aliyuncs.com`
- **自定义域名**: 如果配置了自定义域名，使用自定义域名访问

## 故障排查

### 部署失败

1. 检查 GitHub Secrets 配置是否正确
2. 检查 OSS Bucket 名称和 Region 是否正确
3. 检查 AccessKey 是否有 OSS 相关权限
4. 查看 GitHub Actions 日志获取详细错误信息

### 文件上传失败

1. 检查 OSS Bucket 权限设置
2. 检查 AccessKey 权限是否足够
3. 确认 Endpoint 配置正确

### 访问 404

1. 确认已启用静态网站托管
2. 检查默认首页设置是否为 `index.html`
3. 检查文件路径是否正确

## 注意事项

1. ⚠️ **安全提示**：不要将 AccessKey 提交到代码仓库
2. ⚠️ **权限最小化**：建议创建子账号，只授予必要的 OSS 权限
3. 📝 **路径配置**：如果使用子目录（`OSS_PREFIX`），确保 VuePress 的 `base` 配置匹配
4. 🔄 **缓存清理**：如果遇到问题，可以设置 `OSS_CLEAR_BEFORE_UPLOAD=true` 清空后重新上传

