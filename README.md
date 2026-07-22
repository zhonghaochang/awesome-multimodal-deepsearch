# Awesome Multimodal Deep Search [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Multimodal agents that search, browse, retrieve, and verify visual and textual evidence across multi-step research tasks.

[中文版本](README_CN.md)

Multimodal deep search goes beyond answering questions about an input image. It requires an agent to discover evidence on the web, decide when visual retrieval is necessary, use images as pivots for later search steps, and verify the final answer against traceable evidence. This list focuses on benchmarks and agent work where that search process is central and measurable.

## Contents

- [Benchmark Landscape](#benchmark-landscape)
- [Visual Overview](#visual-overview)
- [Foundational Search Benchmarks](#foundational-search-benchmarks)
- [Verifiable Browsing and Process Evaluation](#verifiable-browsing-and-process-evaluation)
- [Visual-Native and Interleaved Search](#visual-native-and-interleaved-search)
- [Robustness and Noisy-Web Evidence](#robustness-and-noisy-web-evidence)
- [Agent Training and Generalist Environments](#agent-training-and-generalist-environments)
- [Design Patterns](#design-patterns)

## Benchmark Landscape

The numbers below are reported by the corresponding papers or project pages. Results are not directly comparable because tool access, web snapshots, judges, and interaction budgets differ.

<!--lint disable table-pipe-alignment-->

| Work | Released | Scale and setting | Process signal | Main contribution |
| --- | --- | --- | --- | --- |
| MMSearch | 2024-09 | 300 queries; staged search pipeline | Stage-level scores | Search decomposition and diagnosis |
| MMSearch-Plus | 2025-08 | 311 tasks; open-web browsing | Trajectory analysis | Provenance and local visual clues |
| MM-BrowseComp | 2025-08 | 224 paper / 400 repository tasks; open web | Checklists | Verifiable multimodal browsing |
| VDR-Bench | 2026-02 | 2,000 VQA instances; visual web search | Entity recall | Cropped-image search and visual forcing |
| BrowseComp-V3 | 2026-02 | 300 questions and 383 images; open web | Sub-goals and process score | Visual, vertical, and verifiable search |
| V-QPP-Bench | 2026-02 | 46,700 imperfect visual queries; controlled MRAG | Tool and parameter scores | Visual-query repair before retrieval |
| AgentVista | 2026-02 | 209 tasks and 308 images; hybrid tools | Trajectory analysis | Realistic generalist multimodal agents |
| MC-Search | 2026-03 | 3,333 samples; offline multimodal knowledge base | Structured process metrics | Long reasoning graphs and search alignment |
| VSearcher / MM-SearchExam | 2026-03 | 283 generated tasks; real web tools | Training trajectories | Synthetic tasks, SFT, and web-environment RL |
| VisBrowse-Bench | 2026-03 | 169 expert-authored VQA tasks; real web tools | Trajectory analysis | Visual-native browsing and cross-image reasoning |
| MERRIN | 2026-04 | 162 questions; noisy open web | Gold-evidence analysis | Implicit modality selection and source conflict |
| InterLV-Search | 2026-05 | 2,061 samples; open-web tools | Pivot-trajectory analysis | Interleaved language-vision search control |

<!--lint enable table-pipe-alignment-->

See [Benchmark Notes](docs/benchmark-notes.md) for construction details, query examples, metrics, and reported results. The same records are available as [machine-readable JSON](data/benchmarks.json).

## Visual Overview

Each work is presented separately below in release order. Click a figure to open the corresponding paper or project. Images are loaded from official sources; attribution and usage notes are listed in [Image Sources](docs/image-sources.md).

### MMSearch

<p align="center">
  <a href="https://mmsearch.github.io/"><img src="https://mmsearch.github.io/static/images/overview_mv/image/teaser.png" alt="MMSearch benchmark overview" width="88%"></a><br>
  <sub><b>Figure.</b> Representative questions across news and knowledge domains, showing the breadth of image-grounded search tasks.</sub>
</p>

**Core contribution.** MMSearch decomposes multimodal search into query rewriting, reranking, webpage summarization, and end-to-end answer generation so that each stage can be diagnosed independently.

### MMSearch-Plus

<p align="center">
  <a href="https://mmsearch-plus.github.io/"><img src="https://mmsearch-plus.github.io/static/images/teaser.png" alt="MMSearch-Plus agentic visual search comparison" width="88%"></a><br>
  <sub><b>Figure.</b> A comparison of no search, whole-image search, and agentic search that zooms into a decisive local clue.</sub>
</p>

**Core contribution.** MMSearch-Plus emphasizes provenance, region selection, and targeted image or text search when the answer depends on small visual, spatial, or temporal details.

### MM-BrowseComp

<p align="center">
  <a href="https://github.com/MMBrowseComp/MM-BrowseComp"><img src="https://raw.githubusercontent.com/MMBrowseComp/MM-BrowseComp/main/images/case.png" alt="MM-BrowseComp multimodal browsing examples" width="88%"></a><br>
  <sub><b>Figure.</b> Two examples that turn an input image into a sequence of location, entity, and attribute checks on the web.</sub>
</p>

**Core contribution.** MM-BrowseComp makes difficult multimodal browsing verifiable through explicit checklists, separating valid evidence chains from lucky final answers.

### VDR-Bench

<p align="center">
  <a href="https://arxiv.org/html/2602.02185v1"><img src="https://arxiv.org/html/2602.02185v1/x1.png" alt="VDR-Bench visual deep research motivation" width="88%"></a><br>
  <sub><b>Figure.</b> Why text leakage and whole-image retrieval create shortcuts, and how cropped visual search makes image evidence necessary.</sub>
</p>

**Core contribution.** VDR-Bench introduces cropped-image search, multi-turn visual forcing, and entity-sequence recall to evaluate genuine visual deep-research behavior.

### BrowseComp-V3

<p align="center">
  <a href="https://halcyon-zhang.github.io/BrowseComp-V3/"><img src="https://halcyon-zhang.github.io/BrowseComp-V3/static/images/overview.png" alt="BrowseComp-V3 benchmark construction pipeline" width="88%"></a><br>
  <sub><b>Figure.</b> The human-and-tool-assisted pipeline for constructing questions, sub-goals, evidence, and gold browsing trajectories.</sub>
</p>

**Core contribution.** BrowseComp-V3 combines visual, vertical, and verifiable browsing while scoring both final success and completion of expert-authored intermediate goals.

### V-QPP-Bench

<p align="center">
  <a href="https://arxiv.org/html/2602.13179v1"><img src="https://arxiv.org/html/2602.13179v1/x1.png" alt="V-QPP-Bench visual query preprocessing and retrieval" width="88%"></a><br>
  <sub><b>Figure.</b> Standard retrieval failures on imperfect visual queries versus successful retrieval after agent-selected image repair.</sub>
</p>

**Core contribution.** V-QPP-Bench evaluates whether an agent can choose the correct preprocessing tool and parameters before multimodal retrieval begins.

### AgentVista

<p align="center">
  <a href="https://agentvista-bench.github.io/"><img src="https://agentvista-bench.github.io/static/images/data_statistics.png" alt="AgentVista task domain distribution" width="88%"></a><br>
  <sub><b>Figure.</b> The benchmark's seven real-world domains and their fine-grained task categories.</sub>
</p>

**Core contribution.** AgentVista evaluates generalist agents on realistic long-horizon tasks that combine web search, visual understanding, image processing, and code execution.

### MC-Search

<p align="center">
  <a href="https://mc-search-project.github.io/"><img src="https://mc-search-project.github.io/website/img2/teaser_fig_mc_search_2.png" alt="MC-Search benchmark and reasoning graph pipeline" width="88%"></a><br>
  <sub><b>Figure.</b> Benchmark construction, five multimodal reasoning-graph patterns, and the agentic retrieval pipeline.</sub>
</p>

**Core contribution.** MC-Search represents long search processes with structured sub-questions, supporting facts, and intermediate answers rather than evaluating only the final response.

### VSearcher

<p align="center">
  <a href="https://github.com/Ruiyang-061X/VSearcher"><img src="https://raw.githubusercontent.com/Ruiyang-061X/VSearcher/main/.asset/method.png" alt="VSearcher training method" width="88%"></a><br>
  <sub><b>Figure.</b> Rejection-sampled trajectory fine-tuning followed by reinforcement learning in a real web environment.</sub>
</p>

**Core contribution.** VSearcher synthesizes long-horizon multimodal tasks, filters their trajectories for SFT, and improves web-search behavior with GRPO.

### VisBrowse-Bench

<p align="center">
  <a href="https://github.com/ZhengboZhang/VisBrowse-Bench"><img src="https://raw.githubusercontent.com/ZhengboZhang/VisBrowse-Bench/main/images/overview.png" alt="VisBrowse-Bench visual-native browsing examples" width="88%"></a><br>
  <sub><b>Figure.</b> Visual-native questions spanning products, landmarks, culture, science, media, and other real-world domains.</sub>
</p>

**Core contribution.** VisBrowse-Bench requires agents to obtain visual evidence during browsing through image search, reverse-image search, cropping, and cross-image reasoning.

### MERRIN

<p align="center">
  <a href="https://merrin-benchmark.github.io/"><img src="https://merrin-benchmark.github.io/static/figure.png" alt="MERRIN multimodal evidence reasoning and noisy web routes" width="88%"></a><br>
  <sub><b>Figure.</b> Visual, audio, and textual evidence routes, including failure paths caused by missing, secondary, or conflicting sources.</sub>
</p>

**Core contribution.** MERRIN tests implicit modality selection and evidence reasoning on the noisy open web rather than assuming that every useful source is clean and consistent.

### InterLV-Search

<p align="center">
  <a href="https://arxiv.org/html/2605.07510v1"><img src="https://arxiv.org/html/2605.07510v1/x1.png" alt="InterLV-Search interleaved language and vision search levels" width="88%"></a><br>
  <sub><b>Figure.</b> Three levels of language-vision interaction, progressing from shallow visual use to multi-branch visual pivots.</sub>
</p>

**Core contribution.** InterLV-Search treats visual evidence as a control signal that repeatedly determines later queries, tool calls, and search branches.

## Foundational Search Benchmarks

- [MMSearch](https://arxiv.org/abs/2409.12959) - Decomposes multimodal search into query rewriting, reranking, webpage summarization, and end-to-end search for stage-level diagnosis. ([Project](https://mmsearch.github.io/), [Dataset](https://huggingface.co/datasets/CaraJ/MMSearch)).
- [MMSearch-Plus](https://arxiv.org/abs/2508.21475) - Evaluates provenance-aware search from small visual, spatial, and temporal clues with region marking and targeted image or text search. ([Project](https://mmsearch-plus.github.io/)).

## Verifiable Browsing and Process Evaluation

- [BrowseComp-V3](https://arxiv.org/abs/2602.12876) - Pairs public web evidence with expert sub-goals and gold trajectories to measure both success rate and process completion. ([HTML](https://arxiv.org/html/2602.12876v2), [Project](https://halcyon-zhang.github.io/BrowseComp-V3/)).
- [MC-Search](https://arxiv.org/html/2603.00873) - Represents multimodal search trajectories as five reasoning-graph patterns with sub-questions, supporting facts, and intermediate answers. ([Project](https://mc-search-project.github.io/)).
- [MM-BrowseComp](https://arxiv.org/html/2508.13186v1) - Extends difficult-but-verifiable browsing to multimodal webpages and uses checklists to separate valid reasoning from lucky final answers. ([Code](https://github.com/MMBrowseComp/MM-BrowseComp)).

## Visual-Native and Interleaved Search

- [InterLV-Search](https://arxiv.org/abs/2605.07510) - Treats visual evidence as a control pivot that repeatedly determines the next search step, including multi-branch trajectories. ([HTML](https://arxiv.org/html/2605.07510)).
- [VDR-Bench](https://arxiv.org/html/2602.02185v1) - Reduces whole-image and text-leakage shortcuts through cropped-image search, multi-turn visual forcing, and entity-sequence recall. ([Code](https://github.com/Osilly/Vision-DeepResearch)).
- [VisBrowse-Bench](https://arxiv.org/abs/2603.16289) - Requires agents to acquire native visual evidence during browsing through image search, reverse-image search, cropping, and cross-image reasoning. ([HTML](https://arxiv.org/html/2603.16289v1), [Code](https://github.com/ZhengboZhang/VisBrowse-Bench)).

## Robustness and Noisy-Web Evidence

- [MERRIN](https://arxiv.org/abs/2604.13418) - Tests implicit modality selection and evidence reasoning across conflicting, incomplete, and secondary sources on the open web. ([HTML](https://arxiv.org/html/2604.13418v1), [Project](https://merrin-benchmark.github.io/), [Code](https://github.com/HanNight/MERRIN), [Dataset](https://huggingface.co/datasets/HanNight/MERRIN)).
- [V-QPP-Bench](https://arxiv.org/abs/2602.13179) - Frames visual-query repair as an agentic tool-selection and parameter-prediction problem before retrieval. ([HTML](https://arxiv.org/html/2602.13179v1), [Code](https://github.com/phycholosogy/VQQP_Bench)).

## Agent Training and Generalist Environments

- [AgentVista](https://arxiv.org/abs/2602.23166) - Combines realistic images, web and image search, webpage visits, image processing, and code execution in long-horizon user tasks. ([HTML](https://arxiv.org/html/2602.23166v1), [Project](https://agentvista-bench.github.io/), [Code](https://github.com/hkust-nlp/AgentVista), [Dataset](https://huggingface.co/datasets/Warrieryes/AgentVista)).
- [VSearcher](https://arxiv.org/abs/2603.02795) - Builds long-horizon multimodal browsing tasks through iterative entity injection, then trains search agents with trajectory rejection, SFT, and GRPO in a real web environment. ([HTML](https://arxiv.org/html/2603.02795v2), [Code](https://github.com/Ruiyang-061X/VSearcher), [MM-SearchExam](https://github.com/Ruiyang-061X/VSearcher/tree/main/mmsearcheaxm)).

## Design Patterns

**Repair the query before search.** Detect rotations, blur, occlusion, watermarks, or mixed targets before retrieval, following V-QPP-Bench.

**Diagnose the pipeline.** Score rewriting, retrieval, reranking, summarization, and end-to-end behavior separately, following MMSearch.

**Make vision necessary.** Use local crops and native webpage images to prevent whole-image or text-only shortcuts, following VDR-Bench and VisBrowse-Bench.

**Let evidence control search.** Allow visual clues to select later queries and branches, following InterLV-Search.

**Verify the process.** Attach checklists, sub-goals, evidence chains, or reasoning graphs, following MM-BrowseComp, BrowseComp-V3, and MC-Search.

**Stress real-world evidence.** Include noisy sources, modality ambiguity, and hybrid tool workflows, following MERRIN and AgentVista.

## Related Lists

- [Awesome Agentic Deep Research](https://github.com/DavidZWZ/Awesome-Deep-Research) - Agentic deep-research products, implementations, papers, and benchmarks.
- [Awesome Multimodal Large Language Models](https://github.com/BradyFU/Awesome-Multimodal-Large-Language-Models) - Multimodal language models, datasets, evaluation, and training methods.
- [Awesome Multimodal Machine Learning](https://github.com/pliang279/awesome-multimodal-ml) - Multimodal representation, alignment, reasoning, generation, and applications.

## Contributing

Contributions are welcome. Read the [contribution guidelines](CONTRIBUTING.md) before submitting a pull request.

## Footnotes

- The list was last reviewed on 2026-07-22.
- Acceptance status, dataset size, and reported model results can change between paper and repository versions; verify critical claims against the linked primary sources.
