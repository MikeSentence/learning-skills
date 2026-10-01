# learning-skills

Personal learning skills for WorkBuddy. Learn whatever you want, one level at a time.

## 仓库说明

这个仓库是**学习类 skill 的唯一真源**。本机 `~/.workbuddy/skills/` 下的技能目录通过
**目录联接（junction / symlink）**指向本仓库，因此：

- 在仓库里改代码 = 技能立刻生效，**不需要手动同步**
- 改动的历史由 git 记录，可以随时回滚
- 换电脑时 `git clone` + 建联接即可完整恢复

## 目录结构

```
learning-skills/
├── README.md
└── learning-coach/          # 闯关式一对一学习教练
    ├── SKILL.md             # 技能主文件（触发条件 + 执行流程 + 硬性规则）
    └── references/
        ├── question-types.md    # 六种题型的出题模板
        └── example-session.md   # 一次完整闯关的示范
```

每个 skill 一个顶级目录，目录名即技能名（需与 `SKILL.md` 中 `name` 字段一致）。

## 安装 / 关联到本机

### Windows（目录联接，无需管理员）

```powershell
$repo = "D:\code\learning-skills"
$skill = "learning-coach"
New-Item -ItemType Junction -Path "$env:USERPROFILE\.workbuddy\skills\$skill" -Target "$repo\$skill"
```

### macOS / Linux

```bash
ln -s /path/to/learning-skills/learning-coach ~/.workbuddy/skills/learning-coach
```

关联后，WorkBuddy 会在会话启动时扫描 `~/.workbuddy/skills/`，自动加载该技能。

## 已有技能

### learning-coach — 闯关式一对一学习教练

**触发**：说「我想学习 / 学一下 / 带我学 / 教我 / 我想搞懂 / 复习一下 / 准备面试 XX」等。

**核心机制**：

| 机制 | 说明 |
|---|---|
| 关卡地图 | 先把主题拆成 4-6 关，每关写明"学完你能做到什么" |
| 逐关四拍 | 铺垫 → 展示 → 检验 → 结算，一次只问一题 |
| 题型混搭 | 选择 / 判断 / 填空 / 场景 / 反问 / 手撕 |
| 难度自适应 | 连对两题升级，答错立刻降档补基础 |
| 到期复习 | 薄弱点按 1天→3天→7天→15天→30天 递增间隔复习，答错回到 1 天 |
| 进度条 | 每轮回复带一行紧凑进度条，随时知道自己在哪一关 |
| 术语规范 | 缩写首现给「中文全称 + 英文全称（缩写）」，隔段复述 |
| 双写归档 | 每次会话结束写 markdown 档案 + 结构化 JSON |

**学习数据不放在本仓库**，存在本地文档库：

- 人读档案：`D:\文档\学习记录\<主题>-<日期>.md`（头尾各一份进度总览）
- 跨主题总览：`D:\文档\学习记录\index.md`
- 结构化主库：`D:\文档\学习记录\learning-data.json`（预留给未来做 app 直接 import）

## 公开内容边界（重要）

本仓库是**公开仓库**，只放"可公开、可给别人复用"的东西。

| ✅ 可以进仓库 | ❌ 绝不进仓库 |
|---|---|
| 技能逻辑（SKILL.md、references） | 学习档案（`D:\文档\学习记录\` 全部内容） |
| 通用题型模板、示例 | 答题对错记录、正确率、薄弱点清单 |
| 目录结构、安装说明 | 个人的面试短板、职业规划、薪资等 |
| 与具体个人无关的通用知识 | 任何内部/非公开资料 |

**执行方式**：学习数据一律存在本地文档库，**物理上不在仓库目录内**，因此不可能被误提交。
`learning-coach` 技能里也写明了这一点：写数据时必须写到 `D:\文档\学习记录\`，不得写进技能目录。

> 如果以后想跨设备同步学习数据，**另开一个 private 仓库**，不要混进本仓库。

## 迭代约定

### 提交前自检

1. 改动前先确认当前分支状态：`git status`
2. 一次提交只做一件事，提交信息写清"改了什么、为什么"
3. `SKILL.md` 顶部的 frontmatter（`name` / `description`）决定技能能否被触发，改这两个字段要格外小心
4. 新增技能时同步更新本 README 的「已有技能」段
5. 提交前扫一眼 `git status` 的待提交清单，确认**没有学习数据 / 个人资料混进来**

### 分支流程

`main` 保持随时可用（本机通过联接直接加载它，坏了会立刻影响使用）。因此：

```
main                      ← 稳定分支，只接受 review 过的合入
  ↑ 合并（PR / --no-ff）
feat/xxx  fix/xxx  docs/xxx   ← 开发分支，一个改动一个分支
```

**步骤**：

```bash
# 1. 从最新 main 拉开发分支
git checkout main && git pull
git checkout -b feat/your-change

# 2. 在分支上改 + 提交（可以多次提交）
git add -A && git commit -m "..."

# 3. 推到远端
git push -u origin feat/your-change

# 4. 开 PR，review 通过后合入 main
gh pr create --fill        # 或到 GitHub 网页上开

# 5. 合入后清理
git checkout main && git pull
git branch -d feat/your-change
```

**首次搭建（本次）属于例外**：仓库初始结构是直接从零建立的，已直接提交到 `main` 并推送。此后一律走分支流程。
