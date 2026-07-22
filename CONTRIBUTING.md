# Contribution Guidelines

Thank you for helping improve Awesome Multimodal Deep Search.

## Scope

A submission should make search or browsing central to a multimodal agent task. It should satisfy at least one of the following criteria:

- Visual, video, audio, chart, or other non-text evidence is necessary to solve the task.
- The agent performs multi-step web search, image search, reverse-image search, cropping, webpage browsing, or hybrid tool use.
- The work introduces process evaluation such as checklists, sub-goals, evidence chains, entity sequences, or gold trajectories.
- The work provides training data or a training method specifically for long-horizon multimodal search agents.
- The work studies robustness or evidence reliability in a way that materially changes multimodal retrieval.

Ordinary single-step VQA, generic multimodal RAG, and text-only deep research are outside the current scope unless they solve a clearly defined prerequisite for multimodal deep search.

## Quality Requirements

- Use the official paper or project title.
- Include at least one primary source: paper, project page, code repository, or dataset page.
- Explain why the item is useful, not only what it is called.
- State the evaluation environment and dataset scale when they are public.
- Distinguish final-answer metrics from process or trajectory metrics.
- Attach the model name and evaluation setting to every reported result.
- Note material differences between paper, repository, and dataset versions.
- Do not add duplicate, abandoned, undocumented, or inaccessible resources.

## Entry Format

Add the item to the most specific section in `README.md` and keep entries alphabetized within that section.

```markdown
- [Project Name](https://primary-source.example) - A concise explanation of why the work matters. ([Project](https://example.com), [Code](https://github.com/example/repo), [Dataset](https://example.com/dataset)).
```

Descriptions must start with an uppercase letter and end with a period. Use consistent official names such as `Node.js`, `GitHub`, and `VQA`.

If the work adds a benchmark record, update all three locations:

1. The appropriate list section and comparison table in `README.md`.
2. The detailed entry in `docs/benchmark-notes.md`.
3. The machine-readable record in `data/benchmarks.json`.

## Pull Request Checklist

- [ ] The work is within scope and has a clear multimodal search contribution.
- [ ] All links point to primary sources and load successfully.
- [ ] The entry explains why the work belongs in this list.
- [ ] The item is placed in the correct section and alphabetical position.
- [ ] Reported numbers include their model and setting.
- [ ] `README.md`, detailed notes, and JSON data are consistent.
- [ ] `npx awesome-lint` passes.

Please keep each pull request focused on one resource or one coherent update.
