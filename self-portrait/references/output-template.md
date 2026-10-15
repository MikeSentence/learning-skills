# 输出模板

配套 `SKILL.md` Step 5。两个文件都写在 `D:\文档\自我画像\`。

---

## 一、`self-portrait.md`

```markdown
# 自我画像

> 最后更新：YYYY-MM-DD ｜ 已确认 X 条 · 待验证 Y 条 · 存疑 Z 条 · 证据 N 条 ｜ 覆盖度：学识 40% / 能力 60% / 思维 30% / 态度 50% / 人格 20% / 理想 40%

## 一句话定位

（30 字以内。格式：是什么样的人 + 靠什么立身 + 往哪去。
 例：靠把复杂系统拆明白吃饭的人，想做自己能说了算的事。）

## 一、学识

### 已确认
- **标签** —— 证据：事件A、事件B

### 待验证
- **标签** —— 单一证据：事件C（再有一件类似的事就能确认）

## 二、能力

### 可迁移（换个环境依然值钱）
- **标签** —— 证据：事件A、事件D

### 岗位绑定（离开当前位置即失效）
- **标签** —— 证据：事件B

### 明显缺口
- **标签** —— 证据：事件E

> 若"岗位绑定"占比明显高于"可迁移"，这是一条需要当面说出口的风险提示：
> 当前的收入买的更多是位置，而不是人。

## 三、思维

### 已确认
- **标签** —— 证据：事件A

### 待验证
- **标签** —— 证据：事件F

## 四、态度

（同上学识结构：已确认 / 待验证 / 存疑）

## 五、人格

（同上）

## 六、理想

### 真正想要的
- **标签** —— 证据：事件G、事件H

### 愿意付的代价
- **具体愿意放弃的东西** —— 证据：事件H

### 底线（不肯妥协的）
- **标签** —— 证据：事件I

### 幻想待验证
- **标签** —— 说得出想要什么，但答不出愿意放弃什么

## 矛盾与盲区

> 这一节是本画像最值钱的部分，不要为了好看而省略。

- **自述 vs 行为**：说自己重视 X，但举的例子里 X 从没被优先过（证据：事件J）
- **理想 vs 代价**：想要 A，但不肯放弃与 A 冲突的 B（证据：事件K）
- **标签 vs 反例**：标签 M 在另一件事里完全相反（证据：事件L）

## 下一步

（基于画像的 2-3 个具体动作，要能在一个月内开始，不要写"持续提升"这种空话）

1. …
2. …
3. …

## 更新日志

- YYYY-MM-DD：新增 X 条标签，修正 Y 条，新增证据 N 条。聚焦维度：能力 / 理想。
- YYYY-MM-DD：首次建立。
```

---

## 二、`profile-data.json`

结构化状态，供下次会话增量更新，也给其他工具（简历包装、职业规划）读取。

```json
{
  "version": 1,
  "updatedAt": "YYYY-MM-DD",
  "mode": "full | focus | refresh",
  "events": [
    {
      "id": "e001",
      "kind": "failure | spontaneous | choice | success",
      "when": "大致时间，允许模糊如 2026 上半年",
      "summary": "一句话概括发生了什么",
      "rawQuote": "用户原话的关键片段，尽量保留"
    }
  ],
  "tags": [
    {
      "id": "t001",
      "dimension": "knowledge | skill | thinking | attitude | personality | aspiration",
      "label": "标签名",
      "confidence": "confirmed | pending | disputed",
      "portable": true,
      "evidence": ["e001", "e007"],
      "counterEvidence": ["e012"],
      "note": "存疑时写明反例是什么"
    }
  ],
  "contradictions": [
    {
      "claim": "自述的说法",
      "against": "与之冲突的行为或事件",
      "evidenceId": "e015",
      "resolved": false
    }
  ],
  "askedQuestions": ["K1", "S5", "V1"],
  "coverage": {
    "knowledge": 0.4,
    "skill": 0.6,
    "thinking": 0.3,
    "attitude": 0.5,
    "personality": 0.2,
    "aspiration": 0.4
  }
}
```

### 字段说明

| 字段 | 说明 |
|---|---|
| `events[].kind` | `failure` 搞砸的事 ／ `spontaneous` 没人要求主动做的 ／ `choice` 放弃或拒绝的 ／ `success` 做成的 |
| `tags[].confidence` | `confirmed` ≥2 个独立证据且无反例 ／ `pending` 仅 1 个证据 ／ `disputed` 有明确反例 |
| `tags[].portable` | 仅 `skill` 维度的必填：`true` 可迁移 ／ `false` 岗位绑定。其他维度留空 |
| `coverage` | 0~1，按"已问到的题数 / 该维度总题数"粗估。下次会话优先补低分维度 |
| `askedQuestions` | 题库编号（见 `question-bank.md`），避免重复提问 |

---

## 三、首次运行时的建档顺序

1. 建目录 `D:\文档\自我画像\`
2. 先写 `profile-data.json`（事件先记，标签后补）
3. 攒够一个维度的证据后，再往 `self-portrait.md` 里落
4. **不要在只有一两个事件时就急着写"一句话定位"** —— 那句话是全篇最需要证据的东西，放到最后写
