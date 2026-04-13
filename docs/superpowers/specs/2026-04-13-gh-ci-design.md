# gh-ci 项目设计规范

## 一、项目概述

**项目名称**: gh-ci  
**项目类型**: Tauri 桌面应用程序  
**核心目标**: 学习 GitHub Actions CI/CD 流程 + Tauri 自动更新机制

---

## 二、技术栈

| 类别 | 技术 | 版本 |
|------|------|------|
| 前端框架 | React | 19.x |
| 语言 | TypeScript | 5.8.x |
| 构建工具 | Vite | 7.x |
| 桌面框架 | Tauri | 2.x |
| 包管理器 | pnpm | 10.x |
| Node.js | Node.js | 24.14.0 |
| Rust | Rust | 1.94.0 |

---

## 三、CI/CD 流程设计

### 触发条件
```
push tags (v*.*.*)
```

### 工作流程

```yaml
1. 构建 (Build)
   - Checkout code
   - Setup Node.js 24
   - Install pnpm
   - Build Tauri (Windows x64)
   - 输出: .exe, .msi 安装包

2. 测试 (Test)
   - 运行 TypeScript 类型检查
   - (后续添加前端单元测试)

3. 发布 (Release)
   - 创建 GitHub Release
   - 上传安装包 (exe, msi)
   - 自动生成 CHANGELOG.md
   - 生成 latest.json (用于 updater)
```

### 产物目录
```
gh-ci_0.1.0_x64-setup.exe  (NSIS 安装包)
gh-ci_0.1.0_x64_en-US.msi  (MSI 安装包)
latest.json                (Tauri updater 配置)
CHANGELOG.md               (变更日志)
```

---

## 四、版本管理方案

### 版本号规则
- 遵循语义化版本 (SemVer): `v0.1.0`
- 使用 Git tag 管理版本号
- 版本号从 tag 中提取

### 发布流程
```bash
# 1. 本地修改代码
# 2. 提交代码
git add . && git commit -m "feat: xxx"

# 3. 创建 tag 并推送
git tag v0.1.0
git push origin v0.1.0

# 4. GitHub Actions 自动构建并发布
```

---

## 五、CHANGELOG 自动生成

### 工具
使用 [git-chglog](https://github.com/git-chglog/git-chglog) 生成 CHANGELOG

### 配置
- 输出文件: `CHANGELOG.md`
- 基于 commit message 生成
- 格式: `<type>: <message>`

### Commit Message 规范
```
feat:     新功能
fix:      Bug 修复
chore:    维护性改动
docs:     文档更新
style:    代码格式
refactor: 重构
test:     测试
```

---

## 六、Tauri Updater 配置

### 配置项 (tauri.conf.json)
```json
{
  "bundle": {
    "createUpdaterArtifacts": true
  },
  "plugins": {
    "updater": {
      "pubkey": "<需要生成>",
      "endpoints": [
        "https://github.com/NimbleWing/gh-ci/releases/latest/download/latest.json"
      ],
      "windows": {
        "installMode": "passive"
      }
    }
  }
}
```

### 更新流程
1. 应用启动时检查 `latest.json`
2. 如果有新版本，提示用户更新
3. 用户确认后下载并安装

---

## 七、实施计划

| 步骤 | 内容 | 状态 |
|------|------|------|
| 1 | 初始化 Tauri 项目 ✅ | 完成 |
| 2 | 本地构建成功 ✅ | 完成 |
| 3 | 初始化 Git 仓库 ✅ | 完成 |
| 4 | 编写 GitHub Actions workflow | 待完成 |
| 5 | 配置自动生成 CHANGELOG | 待完成 |
| 6 | 配置 Tauri Updater | 待完成 |
| 7 | 推送并测试 CI/CD 流程 | 待完成 |

---

## 八、验收标准

- [x] 项目可以本地构建成功
- [ ] GitHub Actions 自动构建成功
- [ ] 发布产物包含 .exe 和 .msi
- [ ] CHANGELOG.md 自动生成
- [ ] Tauri Updater 可以检测更新

---

## 九、仓库信息

- **GitHub 仓库**: https://github.com/NimbleWing/gh-ci
- **本地路径**: D:\NimbleWing\gh-ci
