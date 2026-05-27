# Claude Code + DeepSeek 配置指南

本文档详细介绍如何在 Windows 系统上配置 Claude Code 使用 DeepSeek API。

## 目录

- [环境要求](#环境要求)
- [第一步：安装 Node.js](#第一步安装-nodejs)
- [第二步：安装 Claude Code](#第二步安装-claude-code)
- [第三步：配置 DeepSeek API](#第三步配置-deepseek-api)
- [第四步：启动 Claude Code](#第四步启动-claude-code)
- [第五步：上传到 GitHub](#第五步上传到-github)
- [常见问题](#常见问题)

---

## 环境要求

- Windows 10/11 系统
- PowerShell 5.0 或更高版本
- Git（用于版本控制）

---

## 第一步：安装 Node.js

### 1.1 下载 Node.js

访问 [Node.js 官网](https://nodejs.org/) 下载 LTS 版本（推荐 v18 或更高版本）。

### 1.2 安装 Node.js

1. 运行下载的安装程序
2. 勾选 "Automatically install necessary tools"
3. 完成安装向导
4. 重启终端

### 1.3 验证安装

```powershell
node --version
npm --version
```

预期输出：
```
v18.x.x
9.x.x
```

---

## 第二步：安装 Claude Code

### 2.1 设置执行策略

如果遇到脚本执行错误，先运行：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 2.2 安装 Claude Code

```powershell
npm install -g @anthropic-ai/claude-code
```

### 2.3 验证安装

```powershell
claude --version
```

预期输出：`2.1.152` 或更高版本

---

## 第三步：配置 DeepSeek API

### 3.1 获取 DeepSeek API Key

1. 访问 [DeepSeek 官网](https://platform.deepseek.com/)
2. 注册/登录账号
3. 在 API Keys 页面创建新的 API Key
4. 复制生成的 Key（格式：`sk-xxxxxxxx`）

### 3.2 设置环境变量（永久生效）

打开 PowerShell，运行以下命令（将 `YOUR_API_KEY` 替换为你的实际 API Key）：

```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", "https://api.deepseek.com/anthropic", "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", "YOUR_API_KEY", "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_MODEL", "deepseek-v4-flash", "User")
```

### 3.3 手动设置（图形界面）

1. 按 `Win + R`，输入 `sysdm.cpl`，回车
2. 点击「高级」→「环境变量」
3. 在「用户变量」中点击「新建」：
   - 变量名：`ANTHROPIC_BASE_URL`
   - 变量值：`https://api.deepseek.com/anthropic`
4. 重复创建另外两个变量：
   - `ANTHROPIC_AUTH_TOKEN` = `YOUR_API_KEY`
   - `ANTHROPIC_MODEL` = `deepseek-v4-flash`
5. 点击「确定」保存
6. **重启终端**

### 3.4 可用模型

| 模型 | 环境变量值 | 说明 |
|------|-----------|------|
| DeepSeek-V4-Flash | `deepseek-v4-flash` | 快速、经济实惠 |
| DeepSeek-V4-Pro | `deepseek-v4-pro` | 更强推理能力 |

### 3.5 验证配置

```powershell
Get-ChildItem Env:ANTHROPIC*
```

应该看到三个环境变量。

---

## 第四步：启动 Claude Code

### 4.1 基本启动

```powershell
claude
```

### 4.2 继续上次会话

```powershell
claude --continue
```

### 4.3 常用命令

| 命令 | 说明 |
|------|------|
| `claude` | 启动新会话 |
| `claude --continue` | 继续上次会话 |
| `claude --resume <session>` | 恢复指定会话 |
| `claude -n "项目名"` | 命名当前会话 |
| `/quit` 或 `Ctrl+D` | 退出 Claude Code |
| `Ctrl+C` | 强制中断当前操作 |

### 4.4 切换模型

**临时切换（当前会话）：**
```powershell
$env:ANTHROPIC_MODEL="deepseek-v4-pro"
claude
```

**永久切换：**
```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_MODEL", "deepseek-v4-pro", "User")
# 重启终端生效
```

### 5.2 本地初始化 Git

在项目文件夹中打开 PowerShell：

```powershell
# 初始化 Git 仓库
git init

# 配置用户信息（替换为你的信息）
git config user.name "Your Name"
git config user.email "your.email@example.com"
```


添加内容：
```markdown
# Claude Code + DeepSeek 配置指南

本文档介绍如何在 Windows 上配置 Claude Code 使用 DeepSeek API。

## 快速开始

1. 安装 Node.js
2. 安装 Claude Code：`npm install -g @anthropic-ai/claude-code`
3. 配置 DeepSeek API 环境变量
4. 运行 `claude` 开始使用

详见 [完整配置指南](./CLAUDE-CODE-DEEPSEEK-SETUP.md)
```



## 常见问题

### Q1: 出现 "Unable to connect to Anthropic services" 错误

**原因：** 环境变量未正确设置或未生效。

**解决：**
1. 检查环境变量是否设置正确：
   ```powershell
   $env:ANTHROPIC_BASE_URL
   $env:ANTHROPIC_AUTH_TOKEN
   $env:ANTHROPIC_MODEL
   ```
2. 如果未显示值，重启终端
3. 确认 API Key 正确且有效

### Q2: npm 安装失败

**解决：**
```powershell
# 清除 npm 缓存
npm cache clean --force

# 使用管理员权限重试
npm install -g @anthropic-ai/claude-code
```

### Q3: 如何查看会话历史？

会话保存在：`C:\Users\<用户名>\.claude\sessions`

使用以下命令恢复：
```powershell
claude --resume <session-id>
```

### Q4: 可以使用其他模型吗？

可以，只需修改 `ANTHROPIC_MODEL` 环境变量：
```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_MODEL", "deepseek-v4-pro", "User")
```

### Q5: VS Code 中如何使用？

在 VS Code 终端中运行：
```powershell
$env:ANTHROPIC_BASE_URL = "https://api.deepseek.com/anthropic"
$env:ANTHROPIC_AUTH_TOKEN = "YOUR_API_KEY"
$env:ANTHROPIC_MODEL = "deepseek-v4-flash"
claude --ide
```

---


## 参考链接

- [Claude Code 官方文档](https://docs.anthropic.com/claude-code)
- [DeepSeek API 文档](https://platform.deepseek.com/docs)
- [Node.js 官网](https://nodejs.org/)
- [Git 官网](https://git-scm.com/)
