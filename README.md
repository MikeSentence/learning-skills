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

## 迭代约定

1. 改动前先确认当前分支状态：`git status`
2. 一次提交只做一件事，提交信息写清"改了什么、为什么"
3. `SKILL.md` 顶部的 frontmatter（`name` / `description`）决定技能能否被触发，改这两个字段要格外小心
4. 新增技能时同步更新本 README 的「已有技能」段
