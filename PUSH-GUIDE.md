# GitHub 推送指南

## 当前状态：本地仓库已就绪，但**这台机器无法访问 GitHub**

仓库位置：`C:\Users\25165\Desktop\SE2026-G22-校园互助平台`

```
M1 ── 302e71f  T1.1 选题、分组与预备作业（9/16）
     340f579  T1.2 项目介绍及 PPT 与汇报（9/23）
     842ab37  T1.4 项目计划评审与修订（10/14）      ← M2、M3
     b8ad5bd  T2.2 可行性分析报告（10/21）
     39d4a82  T1.5 过程材料与团队作业（10/25）
     4e01309  T1.5 个人作业：需求变更感想
```

- 分支：`main`
- 标签：`M1`、`M2`、`M3`
- 纳入版本控制：27 个文件
- 工作区：干净（无未提交改动）

### 网络实测结果

| 目标 | 结果 |
| --- | --- |
| `github.com` | ❌ 操作超时 |
| `api.github.com` | ❌ 403 Forbidden |
| `codeload.github.com` | ❌ 操作超时 |
| 系统代理 | 未启用（`ProxyEnable=0`） |
| **`gitee.com`** | ✅ 200 |
| **`gitcode.com`** | ✅ 200 |

结论：**GitHub 在当前网络被阻断**，需要代理/VPN，或改用 Gitee。仓库本身已完全准备好，只要有能上网的通道，一条命令就能推上去。

---

## 方案 A：用代理访问 GitHub（推荐）

### A-1 先在 GitHub 上建空仓库

1. 浏览器打开 https://github.com/new
2. Repository name：`SE2026-G22-campus-mutual-aid`
3. **不要**勾选 Add README / .gitignore / license（本地已有内容）
4. 点 Create repository
5. 记下仓库地址，例如 `https://github.com/<你的用户名>/SE2026-G22-campus-mutual-aid.git`

### A-2 开启代理后配置 git

假定你的代理是 `127.0.0.1:7890`（按实际端口改，Clash 一般是 7890，V2Ray 一般是 10809）：

```bash
cd "C:\Users\25165\Desktop\SE2026-G22-校园互助平台"

git config --global http.proxy  http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

验证代理是否生效：

```bash
git ls-remote https://github.com/octocat/Hello-World.git
```

能列出 ref 就说明通了。

### A-3 关联远程仓库并推送

```bash
# 关联远程（替换成你的地址）
git remote add origin https://github.com/<用户名>/SE2026-G22-campus-mutual-aid.git

# 推送主干与标签
git push -u origin main
git push origin --tags
```

如果使用 SSH：

```bash
ssh-keygen -t ed25519 -C "你的邮箱"
# 把 ~/.ssh/id_ed25519.pub 的内容贴到 https://github.com/settings/keys
git remote add origin git@github.com:<用户名>/SE2026-G22-campus-mutual-aid.git
git push -u origin main
git push origin --tags
```

### A-4 用完关掉代理（可选）

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

## 方案 B：改用 Gitee（当前网络可直接推）

Gitee 在国内可直连，功能与 GitHub 基本对等，同样能展示提交历史与版本管理。

```bash
cd "C:\Users\25165\Desktop\SE2026-G22-校园互助平台"

# 1. 在 https://gitee.com/projects/new 新建仓库（同样不要初始化 README）
# 2. 关联并推送
git remote add origin https://gitee.com/<用户名>/se2026-g22-campus-mutual-aid.git
git push -u origin main
git push origin --tags
```

首次推送会要求输入 Gitee 用户名和密码（或私人令牌）。

---

## 方案 C：换一台能上 GitHub 的电脑

把整个 `SE2026-G22-校园互助平台` 文件夹（含隐藏的 `.git` 目录）拷到 U 盘，
在能访问 GitHub 的电脑上执行方案 A-1、A-3 即可。`.git` 目录里已包含全部提交历史与标签。

⚠️ 拷贝时务必包含 `.git` 文件夹，它是隐藏目录，Windows 资源管理器需开启"显示隐藏的项目"。

---

## 推送后的验收清单

- [ ] 网页上能看到 6 条提交记录，提交信息为 `T编号 + 简述` 格式
- [ ] 标签 `M1`、`M2`、`M3` 已显示（Releases / Tags 页签）
- [ ] 目录结构为 `assets/ design/ docs/ process/ src/ test/`
- [ ] `docs/` 下有项目计划书、可行性分析报告、演示讲稿、个人作业
- [ ] `assets/` 下有 PPT、甘特图、logo
- [ ] 仓库设为 **Public**（若需给老师查看链接）或确认老师账号有访问权限

---

## 附：常用 git 命令速查

```bash
git status                  # 查看改动
git log --oneline --graph   # 图形化历史
git tag                     # 查看标签
git push origin --tags      # 推送所有标签
git remote -v               # 查看远程地址
git remote set-url origin <新地址>   # 更换远程
```
