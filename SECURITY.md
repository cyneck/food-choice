# 🛡️ 环境配置安全规范

## 1️⃣ 敏感信息零存储原则
- ❌ 禁止在代码仓库中提交 `.env`、`config.json`、`secret.key` 等含密钥文件
- ✅ 使用 `.gitignore` 明确排除所有敏感配置文件
- ✅ 通用做法：提交 `config.example.json` 或 `.env.example` + 本地 `.env`

## 2️⃣ 推送前强制检查清单
```bash
# Windows PowerShell
git log --name-only --pretty=format: | Sort-Object -Unique | Select-String "(\.env|\.secret|key\.json)"

# 也可使用 VS Code 搜索排除 *.env 文件
```

## 3️⃣ 已泄露密钥应急流程
1. **立即吊销/重置**已提交的密钥（API Key、Token、密码）
2. **清理历史记录**：`git rebase -i HEAD~5` 删除敏感提交
3. **强制推送覆盖**：`git push --force-with-lease`

## 4️⃣ 项目初始化规范
- ✅ 创建仓库时**立即配置** `.gitignore`
- ✅ 模版化初始化流程：
  ```bash
  # init-project.sh / setup.ps1
  echo "#!/bin/bash" > init.sh
  echo "cp .env.example .env" >> init.sh
  echo "echo 'Please configure .env with your credentials'" >> init.sh
  ```
- ✅ 使用 `.gitignore` 模板：`node`, `python`, `windows`, `github` 等

## 5️⃣ GitHub Pages 部署规范
- ✅ 使用 `gh-pages` 分支而非 `main` 分支
- ✅ 禁止在仓库中暴露生产环境配置
- ✅ 启用 GitHub Secrets 管理部署凭据
- ✅ 定期清理 `.git` 历史，移除误提交的密钥

## 6️⃣ 代码审查要点
- ✅ PR Checklist 包含：`🚫 敏感信息检查`
- ✅ 使用 `.env` 文件的 IDE 需开启 `safe type`（如 VS Code）
- ✅ 禁止在日志中输出完整密钥（即使部分暴露也危险）

## 7️⃣ 本项目特例说明
- 当前项目为**纯前端静态应用**，无后端服务
- 所有配置均为**开发环境测试数据**
- 上线前需移除所有硬编码的 API Key
- 推荐使用公开 API 或服务端代理访问受保护资源

---

> 📌 **备忘**：每次 `git push` 前运行检查脚本  
> 📌 **备忘**：任何密钥泄露后 5 分钟内完成重置  

---

**最后更新**：2026-03-09  
**关联项目**：`cyneck/food-choice`
