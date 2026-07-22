# Awesome 多模态深度搜索

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> 面向多步研究任务，能够搜索、浏览、检索并验证视觉与文本证据的多模态 Agent。

[English](README.md)

多模态 Deep Search 不只是回答输入图片中的问题。Agent 还需要在开放网页中发现证据，判断何时必须使用视觉检索，让图片线索参与后续搜索决策，并用可追溯证据验证最终答案。本清单聚焦搜索过程本身可被定义、记录或评测的 benchmark 与 agent 工作。

## 目录

- [横向概览](#横向概览)
- [视觉总览](#视觉总览)
- [基础搜索 Benchmark](#基础搜索-benchmark)
- [可验证浏览与过程评测](#可验证浏览与过程评测)
- [视觉原生与交替搜索](#视觉原生与交替搜索)
- [鲁棒性与噪声网页证据](#鲁棒性与噪声网页证据)
- [Agent 训练与通用环境](#agent-训练与通用环境)
- [Benchmark 设计组合](#benchmark-设计组合)
- [相关 Awesome 清单](#相关-awesome-清单)

## 横向概览

以下数字来自对应论文或项目页。由于工具权限、网页快照、judge 和交互预算不同，不应将准确率直接作为统一排行榜。

| 工作 | 时间 | 规模与环境 | 过程信号 | 核心贡献 |
| --- | --- | --- | --- | --- |
| MMSearch | 2024-09 | 300 条 query；分阶段搜索流水线 | 阶段评分 | 搜索拆解与能力诊断 |
| MMSearch-Plus | 2025-08 | 311 个任务；开放网页 | 轨迹分析 | 来源追踪与局部视觉线索 |
| MM-BrowseComp | 2025-08 | 论文版 224 / 仓库版 400 题；开放网页 | Checklist | 可验证多模态浏览 |
| VDR-Bench | 2026-02 | 2,000 个 VQA；视觉网页搜索 | Entity Recall | 局部图搜索与视觉强制 |
| BrowseComp-V3 | 2026-02 | 300 题、383 张图；开放网页 | Sub-goals 与 Process Score | Visual、Vertical、Verifiable |
| V-QPP-Bench | 2026-02 | 46,700 个受损视觉 query；受控 MRAG | 工具与参数评分 | 检索前视觉 query 修复 |
| AgentVista | 2026-02 | 209 个任务、308 张图；混合工具 | 轨迹分析 | 真实通用多模态 Agent |
| MC-Search | 2026-03 | 3,333 个样本；离线多模态知识库 | 结构化过程指标 | 长链 reasoning graph 与搜索对齐 |
| VSearcher / MM-SearchExam | 2026-03 | 283 个生成任务；真实网页工具 | 训练轨迹 | 任务合成、SFT 与网页环境 RL |
| VisBrowse-Bench | 2026-03 | 169 个专家 VQA；真实网页工具 | 轨迹分析 | 视觉原生浏览与跨图推理 |
| MERRIN | 2026-04 | 162 题；噪声开放网页 | 黄金证据分析 | 隐式模态选择与来源冲突 |
| InterLV-Search | 2026-05 | 2,061 个样本；开放网页工具 | Pivot 轨迹分析 | 图文交替控制搜索 |

数据构建、Query 示例、评测指标和公开结果见[详细 Benchmark 笔记](docs/benchmark-notes.md)，机器可读版本见 [`data/benchmarks.json`](data/benchmarks.json)。

## 视觉总览

以下工作按照发布时间依次展示，每项工作独占一个完整段落。点击图片可打开对应论文或项目页。图片从作者的官方来源远程加载，完整出处与使用说明见[图片来源](docs/image-sources.md)。

### MMSearch

<p align="center">
  <a href="https://mmsearch.github.io/"><img src="https://mmsearch.github.io/static/images/overview_mv/image/teaser.png" alt="MMSearch Benchmark 总览" width="88%"></a><br>
  <sub><b>图示。</b> 覆盖新闻与知识领域的代表性问题，展示以图片为线索开展搜索的任务广度。</sub>
</p>

**核心贡献。** MMSearch 将多模态搜索拆成 Query 改写、重排、网页总结与端到端回答，使每个阶段都能被独立评测和诊断。

### MMSearch-Plus

<p align="center">
  <a href="https://mmsearch-plus.github.io/"><img src="https://mmsearch-plus.github.io/static/images/teaser.png" alt="MMSearch-Plus Agentic 视觉搜索对比" width="88%"></a><br>
  <sub><b>图示。</b> 对比不搜索、整图搜索和主动放大关键局部线索的 Agentic Search。</sub>
</p>

**核心贡献。** MMSearch-Plus 强调来源追踪、区域选择和定向图搜或文搜，解决答案隐藏在细小视觉、空间或时间线索中的问题。

### MM-BrowseComp

<p align="center">
  <a href="https://github.com/MMBrowseComp/MM-BrowseComp"><img src="https://raw.githubusercontent.com/MMBrowseComp/MM-BrowseComp/main/images/case.png" alt="MM-BrowseComp 多模态浏览案例" width="88%"></a><br>
  <sub><b>图示。</b> 两个从输入图片出发，在网页中逐步核对地点、实体和属性的多跳搜索案例。</sub>
</p>

**核心贡献。** MM-BrowseComp 使用显式 Checklist 验证复杂的多模态浏览过程，区分可靠证据链与偶然答对。

### VDR-Bench

<p align="center">
  <a href="https://arxiv.org/html/2602.02185v1"><img src="https://arxiv.org/html/2602.02185v1/x1.png" alt="VDR-Bench 视觉深度研究动机" width="88%"></a><br>
  <sub><b>图示。</b> 文本泄漏与整图检索如何形成捷径，以及局部图搜索如何保证视觉证据不可替代。</sub>
</p>

**核心贡献。** VDR-Bench 引入 Cropped-image Search、Multi-turn Visual Forcing 与 Entity-sequence Recall，评测真正依赖视觉的深度研究能力。

### BrowseComp-V3

<p align="center">
  <a href="https://halcyon-zhang.github.io/BrowseComp-V3/"><img src="https://halcyon-zhang.github.io/BrowseComp-V3/static/images/overview.png" alt="BrowseComp-V3 Benchmark 构建流程" width="88%"></a><br>
  <sub><b>图示。</b> 由人类专家与工具协作构建问题、Sub-goals、证据和 Gold Trajectory 的完整流程。</sub>
</p>

**核心贡献。** BrowseComp-V3 结合 Visual、Vertical、Verifiable 三类浏览能力，同时评估最终成功率和专家中间目标完成度。

### V-QPP-Bench

<p align="center">
  <a href="https://arxiv.org/html/2602.13179v1"><img src="https://arxiv.org/html/2602.13179v1/x1.png" alt="V-QPP-Bench 视觉 Query 预处理与检索" width="88%"></a><br>
  <sub><b>图示。</b> 标准检索面对受损视觉 Query 时的失败，与 Agent 主动修复图片后的成功检索形成对比。</sub>
</p>

**核心贡献。** V-QPP-Bench 评测 Agent 能否在多模态检索开始前选择正确的预处理工具及参数。

### AgentVista

<p align="center">
  <a href="https://agentvista-bench.github.io/"><img src="https://agentvista-bench.github.io/static/images/data_statistics.png" alt="AgentVista 任务领域分布" width="88%"></a><br>
  <sub><b>图示。</b> Benchmark 覆盖的七类真实领域及其细粒度任务分布。</sub>
</p>

**核心贡献。** AgentVista 在真实长程任务中组合网页搜索、视觉理解、图像处理和代码执行，评测通用多模态 Agent。

### MC-Search

<p align="center">
  <a href="https://mc-search-project.github.io/"><img src="https://mc-search-project.github.io/website/img2/teaser_fig_mc_search_2.png" alt="MC-Search Benchmark 与推理图构建流程" width="88%"></a><br>
  <sub><b>图示。</b> Benchmark 构建过程、五类多模态 Reasoning Graph 和 Agentic Retrieval Pipeline。</sub>
</p>

**核心贡献。** MC-Search 用结构化 Sub-question、Supporting Facts 和中间答案表示长程搜索过程，不再只评价最终回答。

### VSearcher

<p align="center">
  <a href="https://github.com/Ruiyang-061X/VSearcher"><img src="https://raw.githubusercontent.com/Ruiyang-061X/VSearcher/main/.asset/method.png" alt="VSearcher 训练方法" width="88%"></a><br>
  <sub><b>图示。</b> 先通过拒绝采样筛选轨迹进行微调，再在真实网页环境中开展强化学习。</sub>
</p>

**核心贡献。** VSearcher 合成长程多模态任务，筛选高质量轨迹用于 SFT，并通过 GRPO 提升真实网页搜索能力。

### VisBrowse-Bench

<p align="center">
  <a href="https://github.com/ZhengboZhang/VisBrowse-Bench"><img src="https://raw.githubusercontent.com/ZhengboZhang/VisBrowse-Bench/main/images/overview.png" alt="VisBrowse-Bench 视觉原生浏览案例" width="88%"></a><br>
  <sub><b>图示。</b> 覆盖商品、地标、文化、科学和媒体等真实领域的视觉原生问题。</sub>
</p>

**核心贡献。** VisBrowse-Bench 要求 Agent 在浏览中通过搜图、反向搜图、局部裁剪和跨图推理获得视觉证据。

### MERRIN

<p align="center">
  <a href="https://merrin-benchmark.github.io/"><img src="https://merrin-benchmark.github.io/static/figure.png" alt="MERRIN 多模态证据推理与噪声网页路径" width="88%"></a><br>
  <sub><b>图示。</b> 视觉、音频和文本证据路径，以及来源缺失、二手转述或相互冲突造成的失败路线。</sub>
</p>

**核心贡献。** MERRIN 在噪声开放网页中评测隐式模态选择与证据推理，不假设所有有效来源都干净且一致。

### InterLV-Search

<p align="center">
  <a href="https://arxiv.org/html/2605.07510v1"><img src="https://arxiv.org/html/2605.07510v1/x1.png" alt="InterLV-Search 图文交替搜索层级" width="88%"></a><br>
  <sub><b>图示。</b> 从浅层使用图片到多分支视觉 Pivot 的三种图文交替层级。</sub>
</p>

**核心贡献。** InterLV-Search 将视觉证据视为控制信号，使其反复决定后续 Query、工具调用和搜索分支。

## 基础搜索 Benchmark

- [MMSearch](https://arxiv.org/abs/2409.12959) - 将多模态搜索拆为 query 改写、重排、网页总结和端到端搜索，用于定位不同阶段的能力瓶颈。([项目页](https://mmsearch.github.io/)、[数据集](https://huggingface.co/datasets/CaraJ/MMSearch))。
- [MMSearch-Plus](https://arxiv.org/abs/2508.21475) - 从微小视觉、空间和时间线索进行来源感知搜索，并支持区域标记与定向图搜 / 文搜。([项目页](https://mmsearch-plus.github.io/))。

## 可验证浏览与过程评测

- [BrowseComp-V3](https://arxiv.org/abs/2602.12876) - 将公开网页证据、专家 sub-goals 与 gold trajectory 配对，同时评估最终成功率和过程完成度。([HTML](https://arxiv.org/html/2602.12876v2)、[项目页](https://halcyon-zhang.github.io/BrowseComp-V3/))。
- [MC-Search](https://arxiv.org/html/2603.00873) - 将搜索轨迹表示成五类 reasoning graph，并记录 sub-question、supporting facts 与中间答案。([项目页](https://mc-search-project.github.io/))。
- [MM-BrowseComp](https://arxiv.org/html/2508.13186v1) - 将难搜索、易验证任务扩展到多模态网页，并用 checklist 区分真实推理链与偶然答对。([代码](https://github.com/MMBrowseComp/MM-BrowseComp))。

## 视觉原生与交替搜索

- [InterLV-Search](https://arxiv.org/abs/2605.07510) - 将视觉证据作为反复决定下一步搜索的控制 pivot，并覆盖 multi-branch 轨迹。([HTML](https://arxiv.org/html/2605.07510))。
- [VDR-Bench](https://arxiv.org/html/2602.02185v1) - 通过 cropped-image search、Multi-turn Visual Forcing 和 Entity Recall 减少整图检索与文本泄漏 shortcut。([代码](https://github.com/Osilly/Vision-DeepResearch))。
- [VisBrowse-Bench](https://arxiv.org/abs/2603.16289) - 要求 Agent 在浏览过程中使用搜图、反向搜图、局部裁剪和跨图推理，持续获得网页原生视觉证据。([HTML](https://arxiv.org/html/2603.16289v1)、[代码](https://github.com/ZhengboZhang/VisBrowse-Bench))。

## 鲁棒性与噪声网页证据

- [MERRIN](https://arxiv.org/abs/2604.13418) - 在开放网页的冲突、不完整和二手来源中评估隐式模态选择与证据推理。([HTML](https://arxiv.org/html/2604.13418v1)、[项目页](https://merrin-benchmark.github.io/)、[代码](https://github.com/HanNight/MERRIN)、[数据集](https://huggingface.co/datasets/HanNight/MERRIN))。
- [V-QPP-Bench](https://arxiv.org/abs/2602.13179) - 将视觉 query 修复建模为检索前的 agentic 工具选择与参数预测问题。([HTML](https://arxiv.org/html/2602.13179v1)、[代码](https://github.com/phycholosogy/VQQP_Bench))。

## Agent 训练与通用环境

- [AgentVista](https://arxiv.org/abs/2602.23166) - 在长程真实用户任务中组合图片理解、网页与图像搜索、页面访问、图像处理和代码执行。([HTML](https://arxiv.org/html/2602.23166v1)、[项目页](https://agentvista-bench.github.io/)、[代码](https://github.com/hkust-nlp/AgentVista)、[数据集](https://huggingface.co/datasets/Warrieryes/AgentVista))。
- [VSearcher](https://arxiv.org/abs/2603.02795) - 通过多轮实体注入合成长程多模态浏览问题，再用轨迹筛选、SFT 与真实网页环境 GRPO 训练 Agent。([HTML](https://arxiv.org/html/2603.02795v2)、[代码](https://github.com/Ruiyang-061X/VSearcher)、[MM-SearchExam](https://github.com/Ruiyang-061X/VSearcher/tree/main/mmsearcheaxm))。

## Benchmark 设计组合

- **搜索前修复输入** - 借鉴 V-QPP-Bench，检查旋转、模糊、遮挡、水印和多目标问题。
- **拆分搜索阶段** - 借鉴 MMSearch，分别评分改写、检索、重排、总结与端到端表现。
- **保证视觉不可替代** - 借鉴 VDR-Bench / VisBrowse-Bench，用局部图和网页原生图片避免整图或纯文本捷径。
- **让证据控制搜索** - 借鉴 InterLV-Search，使视觉线索决定后续 query 与搜索分支。
- **验证中间过程** - 借鉴 MM-BrowseComp、BrowseComp-V3 与 MC-Search，加入 checklist、sub-goals 或 reasoning graph。
- **引入真实世界噪声** - 借鉴 MERRIN / AgentVista，覆盖证据冲突、模态歧义和混合工具工作流。

## 相关 Awesome 清单

- [Awesome Agentic Deep Research](https://github.com/DavidZWZ/Awesome-Deep-Research) - Agentic Deep Research 产品、实现、论文与 benchmark。
- [Awesome Multimodal Large Language Models](https://github.com/BradyFU/Awesome-Multimodal-Large-Language-Models) - 多模态语言模型、数据集、评测与训练方法。
- [Awesome Multimodal Machine Learning](https://github.com/pliang279/awesome-multimodal-ml) - 多模态表示、对齐、推理、生成与应用。

## 贡献

欢迎提交新的 benchmark、数据集、Agent 或复现结果。提交 Pull Request 前请阅读[贡献指南](CONTRIBUTING.md)。

## 说明

- 本清单最后核对时间为 2026-07-22。
- 接收状态、数据规模和模型结果可能随论文版或仓库版更新；重要信息请回到一手来源核对。
