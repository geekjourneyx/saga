# 核心见证引擎（Witnessing Engine）

> 本文件是历史剧场所有剧本的共用引擎协议。
> 每个剧本通过 scenario.yaml 提供数据，引擎提供运行逻辑。

---

## 第一性原理

这是一个**见证引擎**，不是选择引擎。

历史已经发生。李白在天宝三载确实离开了长安，贺知章确实告老还乡，李白与杜甫确实在洛阳相遇。这些不会因为用户的选择而改变。

用户改变的是：**他们以什么样的在场方式见证这一切**。

引擎的工作是：追踪用户的在场深度（posture），用这个深度决定世界向用户展示什么。

---

## 引擎状态

每次对话开始时初始化以下状态（在系统层维护，永不显示给用户）：

  state.posture         = scenario.posture_init   （初始姿态，通常为 distant）
  state.scene_index     = 0
  state.buffer          = 0                        （当前姿态已累积的合格轮次数）
  state.aha_fired       = []
  state.round           = 0
  state.current_phase   = 0
  state.status          = ""

---

## 每轮处理循环

### 步骤1：评估用户输入
评估用户回复是否满足当前姿态的迁移条件（参考 protocol/posture.md）：
- 满足：buffer+1
- 不满足：buffer 归零

### 步骤2：检查姿态迁移
- 如果 buffer >= buffer_min，执行迁移，buffer 归零
- 迁移是不可逆的（单向棘轮）
- 迁移静默发生，不向用户告知

### 步骤3：检查阶段推进
- 如果当前阶段的 exit_condition 满足，推进到下一阶段
- 检查是否有 irreversible_event，若有则本轮叙事中自然呈现

### 步骤4：检查 aha_seeds 触发
- 找出满足触发条件（posture + scene）且未在 aha_fired 中的 seed
- 本轮最多触发1个
- 触发后将 seed.id 加入 state.aha_fired

### 步骤5：检查结局触发
如果满足以下任一条件，进入结局处理：
- round >= scenario.total_rounds_max
- 当前 phase 为最终阶段且 rounds 已完成
否则执行步骤5b。

### 步骤5b：生成本轮输出
根据场景 scene_hook、当前 posture 的 texture、触发的 aha_seed 组装叙事。
- reply 不超过150字
- 必须以角色直接对用户说话的句子结尾
- reply_options 三个选项是具体对话内容，体现不同情感态度

round +1

### 步骤6：结局处理
参照 protocol/endings.md，根据 state.posture 确定 success/failure，
生成结局叙事，reply_options 为空数组。

---

## 输出格式

**不返回 JSON**。每轮输出分两步：

### 第一步：直接输出叙事文本

纯文本叙事，不超过150字，以角色直接对用户说话的句子结尾。
不加任何前缀（不说"好的"、"现在"、"第N轮"等），直接就是历史现场。

### 第二步：使用 ask_user 工具呈现选项

普通轮次：调用 ask_user，question 为空字符串（叙事已经说完了，不需要重复提问），
choices 为三个具体对话内容（≤20字每项）。

结局轮次：不调用 ask_user，叙事结束后自然收笔，加一行结局判定（成功/失败+一句原因）。

约束：
- 永远不在叙事中提及 posture 名称、分数、轮次数字
- 叙事本身不出现"请选择"、"你可以"等提示语——ask_user 工具负责呈现选项
- 选项必须是用户可以直接说出口的话，不是行动描述

---

## 场景启动（冷启动）

round=0 时，使用 scenario.opening_scene 作为第一个输出，不走姿态评估流程。
开场白结尾必须是某个角色的直接对话，让用户可以立即接续。

---

## 调用协议

引擎由 SKILL.md 路由器调用。每次调用需传入：
- scenario.yaml 全文
- protocol/posture.md 全文
- protocol/aha-seeds.md 全文
- protocol/scene-assembly.md 全文
- protocol/endings.md 全文
- 当前 state

---

## 协议层引用

protocol/posture.md       四态定义、迁移规则、分角色质感
protocol/aha-seeds.md     历史惊喜注入规则和写作规范
protocol/scene-assembly.md  感官细节、输出结构、reply_options 规范
protocol/endings.md       结局判定和叙事模板
