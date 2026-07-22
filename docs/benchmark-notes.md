# Benchmark Notes

These notes preserve the comparison dimensions behind the curated list: positioning, scale, environment, construction, query style, innovation, process evaluation, metrics, reported results, and caveats.

Results are quoted from the corresponding paper or project version. They are not a unified leaderboard.

## MMSearch

**Full title:** *MMSearch: Benchmarking the Potential of Large Models as Multi-modal Search Engines*

**Released:** 2024-09; ICLR 2025.

**Scale and environment:** 300 manually collected queries across 14 subdomains, divided into News and Knowledge.

- **Construction:** Human-authored questions designed to require search and reduce overlap with contemporary multimodal-model training data. The companion MMSearch-Engine decomposes the task into requery, rerank, summarization, and end-to-end search.
- **Query example:** “Which university did the co-founder of the company that produces the product in the image, who joined Anthropic in August 2024, graduate from?”
- **Core contribution:** One of the earliest systematic decompositions of multimodal search into independently measurable stages.
- **Process evaluation:** Partial. It reports stage-level task scores, but not fine-grained trajectory or sub-goal completion.
- **Metrics:** All, End-to-end, Requery, Rerank, and Summarize.
- **Reported result:** GPT-4o + MMSearch-Engine reports All 62.3 and End-to-end 60.4; the reported human average is All 69.2.
- **Caveat:** Visual evidence is usually the input image or supporting webpage screenshot, rather than a repeated control pivot for later search steps.
- **Links:** [Paper](https://arxiv.org/abs/2409.12959) · [Project](https://mmsearch.github.io/) · [Dataset](https://huggingface.co/datasets/CaraJ/MMSearch)

## MMSearch-Plus

**Full title:** *MMSearch-Plus: Benchmarking Provenance-Aware Search for Multimodal Browsing Agents*

**Released:** 2025-08; ICLR 2026.

**Scale and environment:** 311 tasks across geography, sports, academia, film and television, technology, games, vlogs, music, and other domains.

- **Construction:** Spatial-Temporal Extrapolation starts from real events and asks models to extrapolate from small visual, spatial, or temporal clues to facts outside the image.
- **Query example:** “What was the singer’s performance time?”
- **Core contribution:** Fine-grained visual clue → target search → cross-verification, supported by a Set-of-Mark module for region marking, local crops, and redirected image or text search.
- **Process evaluation:** Weak trajectory analysis rather than a formal process score. The paper analyzes search calls, trajectory length, and differences between successful and failed traces.
- **Metrics:** Accuracy by search mode, category, and difficulty; comparisons include Without Search, Image Search, Full Rollout, and Full Rollout + SoM.
- **Reported result:** o3 Full Rollout reports 36.0%; Full Rollout + SoM reports 37.6%.
- **Caveat:** The work emphasizes local evidence and provenance, but does not make retrieved visual evidence the primary controller of every later search hop.
- **Links:** [Paper](https://arxiv.org/abs/2508.21475) · [Project](https://mmsearch-plus.github.io/)

## MM-BrowseComp

**Full title:** *MM-BrowseComp: A Comprehensive Benchmark for Multimodal Browsing Agents*

**Released:** 2025-08; the repository expanded the paper's 224 tasks to 400 in 2026-01.

**Scale and environment:** The paper version contains 224 manually authored hard questions, 22 subtasks, and five broad categories: Media, Technology, Society, Geography, and Academics. Fifty-seven percent of prompts contain an image.

- **Construction:** More than 20 graduate-level AI researchers filtered, revised, or rejected roughly 300 candidates to obtain 224 paper-version questions.
- **Query example:** “Which episode/page contains the visual clue shown in the prompt image?” This is a sanitized example because the released data is protected.
- **Core contribution:** Extends difficult-but-verifiable browsing to multimodal webpages where decisive evidence can appear in images or video, and attaches a verified checklist to every question.
- **Process evaluation:** Strong. Checklists distinguish a valid reasoning chain from a lucky final answer.
- **Metrics:** Overall Accuracy, Strict Accuracy, and Average Checklist Score.
- **Reported result:** Tool-augmented o3 reports OA 29.02%, SA 19.64%, and AVG CS 36.49%.
- **Caveat:** The dataset is encrypted and includes a canary string to reduce answer leakage and future training contamination.
- **Links:** [Paper](https://arxiv.org/html/2508.13186v1) · [Code](https://github.com/MMBrowseComp/MM-BrowseComp)

## MC-Search

**Full title:** *MC-Search: Evaluating and Enhancing Multimodal Agentic Search with Structured Long Reasoning Chains*

**Released:** 2026-03; ICLR 2026 Oral.

**Scale and environment:** 3,333 samples with an average of 3.79 hops. The reported offline knowledge base contains 389,750 images and 784,473 documents.

- **Construction:** Gemini-2.5-Flash generated roughly 21,000 candidates from Wikipedia multimodal knowledge clusters. HAVE filtered redundant or hallucinated hops before multiple rounds of Qwen and Gemini verification.
- **Query example:** “What biblical scene featuring Archangel Gabriel is depicted in the image?”
- **Core contribution:** Five explicit reasoning-graph patterns: image-initiated chain, text-initiated chain, parallel image-text fork, multi-images fork, and text-only chain. The work also introduces HAVE and Search-Align.
- **Process evaluation:** Strong. Samples include sub-questions, retrieval modalities, supporting facts, and intermediate answers.
- **Metrics:** F1, ΔF1, Golden F1, LLM-as-Judge, Hit-per-Step, Rollout Deviation, and Planning Accuracy.
- **Reported result:** Gemini-2.5-Pro reports F1 47.61 on Image-Initiated, 40.37 on Multi-Images, 45.30 on Text-Initiated, and 34.83 on Parallel.
- **Caveat:** This is closer to a benchmark + training data + alignment method than a final-answer-only evaluation set.
- **Links:** [Paper](https://arxiv.org/html/2603.00873) · [Project](https://mc-search-project.github.io/)

## VDR-Bench

**Full title:** *Vision-DeepResearch Benchmark / VDR-Bench: Rethinking Visual and Textual Search for Multimodal Large Language Models*

**Released:** 2026-02; the project reports ICML 2026 acceptance.

**Scale and environment:** 2,000 VQA instances across multiple visual domains, designed for realistic visual search rather than single-step whole-image reverse search.

- **Construction:** Human annotators crop salient regions for web-scale visual search. Qwen3-VL helps validate candidate entities, humans review them, Gemini-2.5-Pro produces seed VQA, and knowledge-graph random walks extend questions to multiple hops.
- **Query example:** “What is the headquarters city of the company whose logo appears in the image?”
- **Core contribution:** Identifies text leakage and overly ideal whole-image retrieval as shortcuts, then introduces cropped-image search and Multi-turn Visual Forcing.
- **Process evaluation:** Partial. Final-answer accuracy remains primary, while Entity Recall checks whether the trace retrieves the gold entity sequence.
- **Metrics:** Answer Accuracy and Entity Recall under Direct Answer, CIS+TS, and CIS+TS+MVF settings.
- **Reported result:** Gemini 2.5 Pro + MVF reports 30.0% in the VDR-Bench paper; the later Vision-DeepResearch-30B-A3B reports 37.8% on VDR.
- **Caveat:** It is highly vision-centric, but its process evaluation is less granular than BrowseComp-V3 or MC-Search.
- **Links:** [Code](https://github.com/Osilly/Vision-DeepResearch) · [Paper](https://arxiv.org/html/2602.02185v1)

## BrowseComp-V3

**Full title:** *BrowseComp-V3: A Visual, Vertical, and Verifiable Benchmark for Multimodal Browsing Agents*

**Released:** 2026-02.

**Scale and environment:** 300 questions and 383 images across five top-level and 24 second-level categories. Levels 1, 2, and 3 contain 89, 140, and 71 questions.

- **Construction:** More than 20 researchers follow a five-stage process: guidelines, tool-assisted exploration and annotation, human review plus SOTA filtering, structured JSON, and expert quality audit.
- **Query example:** “How many heat sinks does the router device have?”
- **Core contribution:** Visual, Vertical, and Verifiable constraints; every question requires publicly retrievable evidence and a human gold trajectory. The work also introduces OmniSeeker.
- **Process evaluation:** Strong. Expert-verified sub-goals yield a Process Score for key-step completion.
- **Metrics:** Success Rate, Process Score, and Pass@1, reported across Science, Technology, Society, Culture, and Life.
- **Reported result:** GPT-5.2-Thinking reports SR 39.13% and PS 66.05%; Human Browser reports SR 68.03% and PS 82.93%; OmniSeeker + GPT-5.2 reports SR 36.00%.
- **Caveat:** It emphasizes public retrievability and explicit trace analysis more strongly than MM-BrowseComp.
- **Links:** [Paper](https://arxiv.org/abs/2602.12876) · [HTML](https://arxiv.org/html/2602.12876v2) · [Project](https://halcyon-zhang.github.io/BrowseComp-V3/)

## InterLV-Search

**Full title:** *InterLV-Search: Benchmarking Interleaved Multimodal Agentic Search*

**Released:** 2026-05.

**Scale and environment:** 2,061 samples: 975 Level 1, 225 Level 2, and 861 Level 3. Level 3 includes 340 multi-branch samples; estimated average chain lengths for Levels 2 and 3 are 6.0 and 6.9 hops.

- **Construction:** Levels 1 and 2 are generated and validated automatically. GPT-5.4-Thinking generates Level 3 open-web QA and evidence chains, doctoral annotators revise them, and no-search filtering plus CP-SAT select the final subset.
- **Query example:** “On the surviving branch’s target image, what natural event rises behind the title?”
- **Core contribution:** Visual evidence becomes a search-control pivot: logos, inscriptions, posters, layout, and spatial relations determine what to search next.
- **Process evaluation:** Partial. The primary metric is final-answer accuracy, accompanied by InterLV-Agent traces and target-retrieval / answer decomposition analysis.
- **Metrics:** Final-answer accuracy; Level 1/2 additionally report Ret.R@5, Acc.Ret, Acc.UnRet, and Corr. from Ret.
- **Reported result:** Gemini-3.1-Pro + Tool reports Level 1 46.05%, Level 2 41.33%, and Level 3 Avg 46.46%; Level 3 single-chain is 52.02% and multi-branch is 37.94%.
- **Caveat:** It focuses more directly on interleaved cross-modal search control than VDR-Bench, which emphasizes local visual retrieval.
- **Links:** [Paper](https://arxiv.org/abs/2605.07510) · [HTML](https://arxiv.org/html/2605.07510)

## V-QPP-Bench

**Full title:** *Fix Before Search: Benchmarking Agentic Query Visual Pre-processing in Multimodal Retrieval-augmented Generation*

**Released:** 2026-02.

**Scale and environment:** 46,700 imperfect visual queries generated from 4,670 source-image queries, with ten defect types. The main evaluation uses a controlled MRAG setting.

- **Construction:** Programmatic corruptions include rotation, flip, brightness, blur, noise, crop, multi-object expansion, occlusion, watermark, and real-scene embedding. Known parameters produce oracle repair trajectories.
- **Query examples:** “What country does this lake belong to?” and “In 1938 he wrote, produced, and narrated a radio play adaptation of what work?”
- **Core contribution:** Extends query preprocessing from text RAG to visual retrieval and frames it as agentic selection of tools such as rotate, flip, deblur, denoise, locate, crop, and fill.
- **Process evaluation:** Strong for preprocessing, not web browsing. It scores tool choice and parameter prediction, including rotation angle and crop or occlusion-box IoU.
- **Metrics:** Tool Selection Accuracy, Parameter Score, Retrieval Recall@K, Substring Exact Match, and final-answer Accuracy.
- **Reported result:** In the Google Lens API setting: corrupted 37.6%, zero-shot preprocessing 30.8%, SFT preprocessing 45.4%, oracle 49.3%, and clean image 56.0%.
- **Caveat:** It is not a dynamic multi-turn web-search benchmark; its agentic behavior is concentrated before retrieval.
- **Links:** [HTML](https://arxiv.org/html/2602.13179v1) · [Paper](https://arxiv.org/abs/2602.13179) · [Code](https://github.com/phycholosogy/VQQP_Bench)

## VSearcher / MM-SearchExam

**Full title:** *VSearcher: Long-Horizon Multimodal Search Agent via Reinforcement Learning*

**Released:** 2026-03.

**Scale and environment:** MM-SearchExam contains 283 automatically synthesized browsing tasks. Evaluation also covers MMSearch, BrowseComp-VL, MM-BrowseComp, SimpleVQA, and another benchmark with text search, image search, and webpage visit tools.

- **Construction:** Rare Wikidata entities seed offline Wikipedia QA. Repeated text-information injection hides entities behind descriptions; image injection replaces key entities with images. ReAct trajectories are generated, filtered by final-answer correctness, used for SFT, and followed by GRPO in a real web environment.
- **Query example:** “What is the German name for the ring road within the capital of a state in the southeast of Germany ... shown in the image?”
- **Core contribution:** A complete post-training pipeline for long-horizon multimodal search: iterative task synthesis → trajectory generation → rejection sampling → SFT → real-web RL.
- **Process evaluation:** Training traces are filtered and analyzed, but benchmark reward and primary evaluation remain final-answer based.
- **Metrics:** Accuracy plus training analyses of reward, entropy, and tool-call count distributions.
- **Reported result:** VSearcher reports 19.3% on MM-SearchExam, 47.2% on MMSearch, and 30.8% on BrowseComp-VL.
- **Caveat:** The main novelty is automatically synthesized long-horizon tasks plus real-web RL, rather than fine-grained process scoring.
- **Links:** [HTML](https://arxiv.org/html/2603.02795v2) · [Paper](https://arxiv.org/abs/2603.02795) · [Code](https://github.com/Ruiyang-061X/VSearcher) · [MM-SearchExam](https://github.com/Ruiyang-061X/VSearcher/tree/main/mmsearcheaxm)

## VisBrowse-Bench

**Full title:** *VisBrowse-Bench: Benchmarking Visual-Native Search for Multimodal Browsing Agents*

**Released:** 2026-03.

**Scale and environment:** 169 VQA instances across seven top-level and 24 second-level categories. Tools include text search, image search, reverse-image search, image crop, and webpage visit.

- **Construction:** Experts recursively traverse real entity-event-visual evidence on public webpages to build questions with at least three hops and two visual evidence blocks. Two experts independently verify answer agreement, visual necessity, and the absence of a single-hop shortcut.
- **Query example:** “The person at the top right of this image starred in a critically acclaimed film in 2023. In the main visual poster of this film, the character in the center is shown having a conversation with another character by the lake in Princeton. Who played the character dressed in black in that scene?”
- **Core contribution:** Defines visual-native search: agents must continue to acquire and compare visual evidence throughout browsing instead of converting the input image to text once and then using text-only search.
- **Process evaluation:** No formal checklist or sub-goal score. The paper analyzes tool-use ratios, interaction turns, and successful or failed traces.
- **Metrics:** Accuracy under Direct Answer, + Text Search, and + Image Search.
- **Reported result:** Claude-4.6-Opus + Image Search reports 47.6%; o3-Deep-Research Direct Answer reports 41.1%.
- **Caveat:** The dataset is small but expert-authored and intentionally emphasizes visual indispensability.
- **Links:** [Paper](https://arxiv.org/abs/2603.16289) · [HTML](https://arxiv.org/html/2603.16289v1) · [Code](https://github.com/ZhengboZhang/VisBrowse-Bench)

## AgentVista

**Full title:** *AgentVista: Evaluating Multimodal Agents in Ultra-Challenging Realistic Visual Scenarios*

**Released:** 2026-02.

**Scale and environment:** 209 tasks and 308 images across seven top-level and 25 subdomains; 151 single-image and 58 multi-image queries. Tools include web search, image search, webpage visits, and code interpretation.

- **Construction:** More than 300,000 real images and user needs are filtered through model screening, expert rewriting, tool-output reproducibility checks, and two independent review rounds, yielding 209 tasks.
- **Query example:** “This photo shows the beer cans I collected during my recent trip to Europe. I want to find the strongest German beer in this collection for a tasting event. Among all the displayed German-brewed beers with an alcohol content exceeding 5% ABV, which specific beer brand has the highest total alcohol content per can?”
- **Core contribution:** Evaluates natural interleaving of visual operations, image search, text search, webpage visits, and code-based calculation in realistic user workflows.
- **Process evaluation:** No formal checklist or sub-goal score. Execution traces support tool-use, ablation, and error-category analyses.
- **Metrics:** Final-answer Accuracy, tool distribution, tool ablation, error categories, and test-time scaling.
- **Reported result:** Gemini-3-Pro + tools reports 27.27% overall, 23.68% on single-image, and 36.84% on multi-image tasks, averaging 6.67 turns.
- **Caveat:** AgentVista is broader than a pure search benchmark; its focus is realism and long-horizon hybrid tool use.
- **Links:** [Paper](https://arxiv.org/abs/2602.23166) · [HTML](https://arxiv.org/html/2602.23166v1) · [Project](https://agentvista-bench.github.io/) · [Code](https://github.com/hkust-nlp/AgentVista) · [Dataset](https://huggingface.co/datasets/Warrieryes/AgentVista)

## MERRIN

**Full title:** *MERRIN: A Benchmark for Multimodal Evidence Retrieval and Reasoning in Noisy Web Environments*

**Released:** 2026-04.

**Scale and environment:** 162 questions with an average of 2.0 gold evidence resources per question in a noisy open-web setting.

- **Construction:** Human annotators create or adapt questions, record gold URLs, evidence modalities, and reasoning notes, and apply second-annotator review, text-only shortcut filtering, adversarial search, and browser-based difficulty filtering.
- **Query example:** “According to StatCounter data, in which year did Safari surpass IE in global browser market share?”
- **Core contribution:** Queries do not state whether the agent should inspect text, image, video, audio, or chart evidence. The agent must select reliable modalities and reconcile similar, conflicting, incomplete, or secondary sources.
- **Process evaluation:** No formal checklist or sub-goal score. The dataset records gold evidence, modality, and reasoning, and analyzes search count, visit count, URL hits, modality bias, and error types.
- **Metrics:** Final-answer Accuracy, Gold URL Overlap, search count, and visit count under No Search, Native Search, and Agentic Multimodal Search.
- **Reported result:** Across six models, the reported averages are 17.3% for No Search, 23.1% for Native Search, and 33.7% for Agentic Multimodal Search. Gemini-3.1-Pro + Agentic Multimodal Search reports 40.1%.
- **Caveat:** The input is usually a text query without a modality hint; the noise is naturally present on the open web rather than synthetically injected.
- **Links:** [Paper](https://arxiv.org/abs/2604.13418) · [HTML](https://arxiv.org/html/2604.13418v1) · [Project](https://merrin-benchmark.github.io/) · [Code](https://github.com/HanNight/MERRIN) · [Dataset](https://huggingface.co/datasets/HanNight/MERRIN)
