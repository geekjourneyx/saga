# 历史剧场（Theatre）

**沉浸式历史叙事体验技能 · 见证引擎驱动 · 支持多剧本扩展**

> 历史已经发生。李白必然离开长安。你改变不了结局——这正是沉浸的来源。

---

## 快速开始

安装（Claude Code）：

  git clone <repo-url> ~/.claude/skills/theatre

启动剧场：

  /theatre                        列出可用剧本
  /theatre 盛唐                    启动盛唐气象剧本
  /theatre --scenario tang-744    直接启动（精确ID）

---

## 当前剧本

### 盛唐气象：诗意与酒（tang-744）

天宝三载（744 CE）。你是一位来自河北道的游学士子，来到当世最伟大的城市——长安。

你将与即将离开长安的李白相遇，随他告别贺知章、王维、岑参、王昌龄，
再东行洛阳，见证李白与杜甫的相遇，一同游历梁宋，邂逅高适，
最终在诗酒盛宴中结束这段旅程。

- 时代：天宝三载，744 CE
- 地点：长安 → 洛阳 → 梁宋
- 角色：李白、杜甫、王维、贺知章、岑参、王昌龄、高适
- 轮次：约 20-26 轮
- 历史惊喜：5 个真实历史细节，等你发现

---

## 见证引擎原理（简介）

这不是一个「选择剧情分支」的游戏。历史不会因为你的选择而改变。

你能改变的是：**你以什么样的在场方式见证这一切**。

引擎追踪你的「姿态」——一种内在的情感方位：
- 刚到长安时，你是个旁观者（distant）
- 当你开始真正发问，历史开始向你说话（curious）
- 当你与这些人建立了真实联结（warm）
- 当你意识到这一切必将消逝（grieving）

姿态决定世界向你展示什么。李白在不同深度的对话中，会说完全不同的话。

---

## 添加新剧本

在 scenarios/ 下新建目录，创建 scenario.yaml，遵循以下最小结构：

  scenarios/
    your-scenario-id/
      scenario.yaml

scenario.yaml 必填字段：
  scenario.id, title, subtitle, user_role
  scenario.posture_init, total_rounds_max
  opening_scene
  posture_transitions
  phases（含 scenes, exit_condition）
  aha_seeds（建议 3-5 个）
  characters（含 inner_state 和 posture_texture 四态）
  endings（witnessing_success 和 witnessing_failure）

然后在 SKILL.md 的「可用剧本」部分新增一行记录。引擎和协议无需修改。

参考模板：scenarios/tang-744/scenario.yaml

---

## 项目结构

  theatre/
    SKILL.md                   主入口路由器
    engine.md                  核心见证引擎（所有剧本共用）
    skill.yaml                 包元数据
    README.md                  本文件
    protocol/
      posture.md               姿态状态机规范
      aha-seeds.md             历史惊喜注入协议
      scene-assembly.md        场景组装规范
      endings.md               结局判定协议
    scenarios/
      tang-744/
        scenario.yaml          盛唐气象：诗意与酒

---

## License

MIT
