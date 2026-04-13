# 历史剧场 · Theatre

> 历史已经发生。李白必然离开长安。你改变不了结局——这正是沉浸的来源。

沉浸式历史叙事体验 Skill，由**见证引擎**驱动，支持多剧本扩展。  
你不是旁观者，你是见证者——以不同深度在场，听到不同的历史。

---

## 安装

```bash
npx skills add https://github.com/geekjourneyx/saga
```

---

## 使用

```
/theatre              列出可用剧本
/theatre 盛唐          启动「盛唐气象」剧本
/theatre tang-744     直接启动（精确 ID）
```

---

## 剧本列表

| ID         | 名称                   | 时代         | 地点              | 轮次    |
|------------|------------------------|--------------|-------------------|---------|
| `tang-744` | 盛唐气象：诗意与酒     | 天宝三载 744 | 长安 → 洛阳 → 梁宋 | 20–26   |

### 盛唐气象：诗意与酒

```
你是一位来自河北道的游学士子，半旧青衫，囊中仅余十枚开元通宝。
天宝三载，你来到长安——当世最伟大的城市。

长安西市                洛阳                   梁宋
  │                      │                      │
  ▼                      ▼                      ▼
遇见李白            李白与杜甫相遇           登吹台赋诗
  │                      │                      │
告别贺知章            相约同游              邂逅高适
王维 / 岑参 / 王昌龄     │                      │
  │                      └──────────────────────┘
  └──────────────── 诗酒盛宴 · 天下散 ──────────────▶ 结局
```

**登场诗人：** 李白 · 杜甫 · 王维 · 贺知章 · 岑参 · 王昌龄 · 高适  
**历史惊喜：** 5 个真实细节藏在对话深处，等你发现

---

## 见证引擎

这不是「选择分支」的游戏。历史不因你的选择而改变。  
你能改变的只有一件事：**你以什么样的在场方式见证这一切**。

```
     你的「姿态」（Posture）

  distant ──► curious ──► warm ──► grieving
  （旁观）    （发问）   （联结）   （共鸣）

     │            │          │          │
     ▼            ▼          ▼          ▼
  表面客套    历史开口    深层独白    真实的别离
```

姿态是慢变量——不可跳跃，需要真实的情感投入才能推进。  
李白在不同姿态下，会说出完全不同的话。

**引擎流程（每轮）：**

```
用户回复
   │
   ▼
[1] 解析姿态变化
   │
   ▼
[2] 检查 aha_seed 触发条件
   │
   ├─ 触发 ──► 注入历史惊喜（角色之口，非旁白）
   │
   ▼
[3] 组装场景输出（≤150字，角色对话结尾）
   │
   ▼
[4] 生成三条推荐回复（具体对话，≤20字）
   │
   ▼
[5] 判断阶段推进 / 结局触发
```

---

## 添加新剧本

新增一个剧本只需一个文件：

```
scenarios/
  your-id/
    scenario.yaml          ← 唯一需要创建的文件
```

`scenario.yaml` 必填字段：

```yaml
scenario:       # id, title, user_role, posture_init, total_rounds_max
opening_scene:  # 开场白（感官细节 + 第一句角色对话）
posture_transitions:  # 四态转换条件
phases:         # 阶段列表（scenes, exit_condition）
aha_seeds:      # 3–5 个历史惊喜
characters:     # 角色 inner_state + posture_texture 四态声音
endings:        # witnessing_success / witnessing_failure
```

引擎（`engine.md`）和协议（`protocol/`）无需修改。  
参考：[`scenarios/tang-744/scenario.yaml`](scenarios/tang-744/scenario.yaml)

---

## 项目结构

```
saga/
├── SKILL.md                    路由器入口（触发 + 剧本分发）
├── engine.md                   见证引擎协议（所有剧本共用）
├── skill.yaml                  包元数据
├── protocol/
│   ├── posture.md              姿态状态机（4态 FSM）
│   ├── aha-seeds.md            历史惊喜注入协议
│   ├── scene-assembly.md       场景组装规范
│   └── endings.md              结局判定协议
├── scenarios/
│   └── tang-744/
│       └── scenario.yaml       盛唐气象：诗意与酒
├── evals/
│   └── evals.json              测试用例（5个）
└── theatre.skill               打包文件（可直接安装）
```

---

## License

MIT
