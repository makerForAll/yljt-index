# 推送代码到 GitHub 步骤

## 步骤 1: 在 GitHub 创建仓库

1. 登录 [GitHub](https://github.com)
2. 点击右上角的 **+** 号，选择 **New repository**
3. 填写仓库信息：
   - **Repository name**: `yljt-index`（或你喜欢的名称）
   - **Description**: 可选，填写项目描述
   - **Visibility**: 选择 **Public**（公开）或 **Private**（私有）
   - ⚠️ **不要**勾选 "Initialize this repository with a README"（因为本地已有代码）
4. 点击 **Create repository**

## 步骤 2: 在本地提交代码

在项目根目录执行以下命令：

```bash
# 1. 添加所有文件到暂存区
git add .

# 2. 提交代码（首次提交）
git commit -m "Initial commit: VuePress site with encryption"

# 3. 将分支重命名为 main（GitHub 默认使用 main）
git branch -M main
```

## 步骤 3: 添加远程仓库并推送

```bash
# 1. 添加远程仓库（将 YOUR_USERNAME 替换为你的 GitHub 用户名）
git remote add origin https://github.com/YOUR_USERNAME/yljt-index.git

# 2. 推送代码到 GitHub
git push -u origin main
```

如果使用 SSH（需要先配置 SSH key）：
```bash
git remote add origin git@github.com:YOUR_USERNAME/yljt-index.git
git push -u origin main
```

## 步骤 4: 配置 GitHub Secrets（重要！）

推送完成后，需要配置 GitHub Secrets 才能自动部署：

1. 进入你的 GitHub 仓库
2. 点击 **Settings** → **Secrets and variables** → **Actions**
3. 点击 **New repository secret**，添加以下密钥：

### 必需配置

- **Name**: `OSS_ENDPOINT`
  - **Value**: 你的 OSS Endpoint，如 `oss-cn-hangzhou.aliyuncs.com`

- **Name**: `OSS_BUCKET`
  - **Value**: 你的 OSS Bucket 名称

- **Name**: `OSS_ACCESS_KEY_ID`
  - **Value**: 阿里云 AccessKey ID

- **Name**: `OSS_ACCESS_KEY_SECRET`
  - **Value**: 阿里云 AccessKey Secret

### 可选配置

- **Name**: `OSS_PREFIX`
  - **Value**: 上传路径前缀（如 `docs/`），留空则上传到根目录

- **Name**: `OSS_CLEAR_BEFORE_UPLOAD`
  - **Value**: `true` 或 `false`（上传前是否清空目录）

- **Name**: `OSS_CUSTOM_DOMAIN`
  - **Value**: 自定义域名（如果有）

## 步骤 5: 验证部署

1. 推送代码后，进入仓库的 **Actions** 标签页
2. 应该能看到 "Deploy to Aliyun OSS (Simple)" 工作流正在运行
3. 等待部署完成（通常需要 2-5 分钟）
4. 部署成功后，访问你的 OSS 站点地址

## 常见问题

### 问题 1: 推送时要求输入用户名密码

**解决方案**：使用 Personal Access Token
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. 生成新 token，勾选 `repo` 权限
3. 推送时，用户名输入你的 GitHub 用户名，密码输入 token

### 问题 2: 分支名称不匹配

如果本地是 `master` 分支，GitHub 是 `main` 分支：
```bash
git branch -M main
git push -u origin main
```

### 问题 3: 远程仓库已存在

如果远程仓库已存在，先删除再添加：
```bash
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/yljt-index.git
```

## 后续操作

每次修改代码后，只需：

```bash
git add .
git commit -m "你的提交信息"
git push
```

GitHub Actions 会自动触发部署！

