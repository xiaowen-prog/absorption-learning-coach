# Absorption Learning Coach / 吸收学习教练

一份可复用的开源 AI 辅导技能（skill）：帮助学习者真正**吸收**长文与视频，而不是堆阅读量或照抄标准答案。

An open coaching recipe for AI tutors that maximize absorption of articles and videos — prediction, judgment, and closed-book recall over answer dumps.

## 它是什么 / What it is

- **五步吸收环**：拆解 → 价值一句话 → 闭卷回忆 → 新场景迁移 → 可选反驳
- **分层讲解**：Light / Medium / Full，卡住再升级
- **测验规则**：一次一题；只诊缺口，不贴完整骨架逼重做；错题换场景再探；疲劳自动降档
- **活的学习者模型**：持续更新吸收度、缺口、疲劳与有效形式；建议每周复盘

## 安装 / Install

把技能文件复制到你的 agent workflows（或等价 skills）目录即可：

```bash
# 示例：复制到本机 workflows 目录
cp skills/absorption-learning-coach/SKILL.md /path/to/your/agent/workflows/absorption-learning-coach/SKILL.md
```

核心文件：[`skills/absorption-learning-coach/SKILL.md`](./skills/absorption-learning-coach/SKILL.md)

## 落地页 / Landing page

在线介绍页（GitHub Pages）：

**https://xiaowen-prog.github.io/absorption-learning-coach/**

设计说明见 [`DESIGN.md`](./DESIGN.md)。

## License

MIT © 2026 xiaowen-prog
