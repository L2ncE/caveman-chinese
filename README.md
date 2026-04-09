<p align="center">
  <img src="https://em-content.zobj.net/source/apple/391/rock_1faa8.png" width="120" />
</p>

<h1 align="center">caveman-chinese</h1>

<p align="center">
  <strong>为何用多字，少字亦可</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/github/license/L2ncE/caveman-chinese?style=flat" alt="License"></a>
</p>

---

中文版穴居人压缩 Skill，适配 [Claude Code](https://docs.anthropic.com/en/docs/claude-code)。

删客套话、删过渡废话、删不确定措辞，缩写高频术语，保留技术术语和代码，**平均压缩 54%，最高单条 69%**，同时不损失技术准确性。

## 效果对比

| | 示例 |
|---|---|
| **正常输出** | 这个问题出现的原因是每次渲染都会创建一个新的对象引用，导致 React 认为 props 发生了变化，从而触发子组件重新渲染。建议你用 `useMemo` 包裹这个对象。 |
| **压缩后** | 每次渲染新建对象引用 → React 认为 props 变 → 子组件重渲。用 `useMemo` 包。 |

相同结论，少 70% 字。

## 中文压缩的核心

英文删冠词（a/an/the），中文没有冠词，压缩收益来自：

- **客套开场**：「好的」「当然」「我来帮你」「这是个好问题」
- **过渡废话**：「首先/其次/然后/最后」「综上所述」「值得注意的是」
- **不确定措辞**：「可能」「也许」「建议你」「我认为」
- **解释性赘述**：「这意味着」「换句话说」→ 直接给结论
- **程度副词**：「非常」「十分」「极其」「相当」

用 `→` 替代「因此/所以/导致」，信息密度最大化。

## Benchmark

由 Claude Opus 4.6 真实压缩 25 条样本，结果存于 [`benchmarks/results.json`](benchmarks/results.json)：

| 指标 | 数值 |
|---|---|
| 字符压缩率（均值） | **54.2%** |
| 最高单条 | **69.0%** |
| 规则违反次数 | **0** |
| 例外场景处理 | **通过** |

> 纯文字类（技术解释、错误诊断）压缩率 60%+；命令行/配置类因命令本身不可删，压缩率约 35–52%。

复现：将 `pairs.json` 中每条 `normal` 文本发给已激活 caveman-chinese 的 Claude，收集输出后统计字符数差异。

## 安装

**方式一：告诉 Claude Code 安装**

直接把下面这句话发给 Claude Code：

```
帮我安装 caveman-chinese skill：把 https://raw.githubusercontent.com/L2ncE/caveman-chinese/main/SKILL.md 的内容保存到 ~/.claude/skills/caveman-chinese/SKILL.md
```

**方式二：clone 后复制**

```bash
git clone https://github.com/L2ncE/caveman-chinese.git
cd caveman-chinese

# 用户级（所有项目生效）
mkdir -p ~/.claude/skills/caveman-chinese
cp SKILL.md ~/.claude/skills/caveman-chinese/SKILL.md

# 或项目级
mkdir -p .claude/skills/caveman-chinese
cp SKILL.md .claude/skills/caveman-chinese/SKILL.md
```

## 使用

**激活**：说「穴居人」「精简」「压缩」「极简模式」「少说废话」

**退出**：说「正常模式」「详细说」「解释一下」

## 例外

以下情况自动恢复正常输出：

- 安全警告、破坏性操作（`rm -rf`、`DROP TABLE`、`force push`）
- 用户明确要求完整文档或报告

## 参考

- [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) — 英文穴居人压缩原始项目
- [dlepold/caveman-distillate](https://github.com/dlepold/caveman-distillate) — 压缩逻辑参考

## License

MIT
