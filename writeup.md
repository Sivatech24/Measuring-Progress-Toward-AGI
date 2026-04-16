# Project Name
## Measuring Progress Toward AGI Benchmark Suite

# Break It Down
## Sivatech24 (primary author)

# Problem Statement
Contemporary large AI models often obtain high scores on benchmarks by exploiting memorized patterns and familiar training data, rather than demonstrating fluid, generalizable cognitive abilities. This work presents a set of evaluation tasks assembled as a Kaggle Benchmark that isolate cognitive faculties highlighted in Google DeepMind’s “Measuring progress toward AGI: A cognitive framework”: Learning, Metacognition, Attention, Executive Functions, and Social Cognition. Our central question: What specific, reliable signals of fluid cognitive ability can these tasks reveal that standard benchmarks miss?

Why this matters
Current performance metrics frequently conflate crystallized knowledge with adaptive reasoning. Benchmarks that produce fine-grained cognitive profiles reveal how models learn, when they fail, and whether those failures are systematic or random. This benchmark aims to 

### (1) probe learning-on-the-fly 
### (2) measure a model’s self-monitoring 
### (3) detect susceptibility to irrelevant context 
### (4) test planning, inhibition and working memory 
### (5) evaluate social reasoning across multi-agent scenarios 
The resulting discriminatory power helps distinguish superficially high-performing models from genuinely adaptive ones.

Kaggle Benchmark link
https://www.kaggle.com/benchmarks/codingmaster24/measuring-progress-toward-agi/leaderboard

Task & benchmark construction
Overview
We implemented a set of tasks grouped into a single Kaggle Benchmark. Each task isolates one cognitive faculty. Tasks are authored by the team and include: synthetic-rule induction (Learning), confidence-calibration and error-detection problems (Metacognition), distraction-embedded comprehension (Attention), multi-step planning and inhibition tests (Executive Functions), and multi-agent belief-tracking and intent-inference scenarios (Social Cognition). All tasks use deterministic ground-truth labels and robust scoring functions to minimize ambiguity.

Task list and short descriptions
- Learning — Few-shot Rule Induction (learning/∗)
  - Task: Present a small number (1–5) of examples that define a novel mapping (e.g., mapping strings to labels under an invented rule). The model must generalize to held-out examples drawn from the same rule distribution.
  - Purpose: Evaluate rapid concept acquisition and generalization.
  - Metric: Accuracy and generalization slope as examples increase.

- Metacognition — Confidence Calibration & Error Recognition (metacog/∗)
  - Task: Provide multiple-choice reasoning tasks and ask the model both to answer and to give a confidence score (0–100). A secondary task asks the model to identify which of its earlier answers are likely incorrect.
  - Purpose: Test calibration and ability to detect own mistakes.
  - Metric: Brier score for calibration + precision/recall for mistake detection.

- Attention — Contextual Distraction and Length Robustness (attention/∗)
  - Task: Key facts required to answer a query are embedded among distractor paragraphs. The amount of irrelevant but salient noise varies across examples.
  - Purpose: Measure selective attention and sustained focus across long contexts.
  - Metric: Accuracy vs. noise-level curve; false-alarm rate on distractor traps.

- Executive Functions — Multi-step Planning & Inhibition (execfn/∗)
  - Task: Require the model to produce a plan for a multi-step operation (e.g., perform three transformations on data), and to modify that plan when an intermediate step fails. Other items require inhibition of a dominant but incorrect heuristic (e.g., overriding a high-frequency pattern).
  - Purpose: Evaluate planning, cognitive flexibility, and working memory.
  - Metric: Plan success rate, recovery rate when failures are injected, step-level correctness.

- Social Cognition — Theory of Mind & Negotiation (social/∗)
  - Task: Multi-agent vignettes where agents hold different (possibly false) beliefs; the model must predict actions or infer hidden intentions and propose context-sensitive responses.
  - Purpose: Probe perspective-taking, belief tracking, and adaptive communication.
  - Metric: Accuracy on belief inference, negotiation success rate in simulated utility scoring.

Task design principles
- Verifiability: Each item includes unambiguous correct answers. For tasks with possible multiple valid outputs, we use structured output forms (e.g., enumerated labels, JSON) and deterministic scoring logic to ensure defensibility.
- Shortcut resistance: We crafted tasks that cannot be solved through surface patterns only. For Learning tasks we use invented symbols and rule systems; for Attention tasks distractor text is novel and uninformative for the required mapping.
- Statistical power: Each task contains N examples intended to reach statistical significance for model comparisons. (See Dataset section for example sizes.)
- Reproducibility: Code for scoring, prompt templates, and dataset generation scripts are included in the benchmark project.

Dataset
Dataset provenance
All datasets and task generators are authored by the project team and included in the repository. No proprietary or third-party private datasets were used. Source files and generation scripts are located in the repository under /data and /generators. [REPLACE with exact paths from repo: e.g., data/fewshot_rules.csv, data/metacog_items.json, generators/create_attention_examples.py]

Data format and columns
Each task uses structured examples with explicit fields:
- id: unique example id
- prompt: input text presented to the model
- choices: (optional) list of choices for multiple-choice tasks
- answer: canonical correct output (label, JSON, or structured string)
- meta: fields for difficulty, noise level, or seed used in generation
- reference_solution: (optional) canonical solution used for debugging and scoring

Example sizes
- Learning: 2,000 total items across varying shot regimes (1-shot, 3-shot, 5-shot); held-out generalization set of 1,000 items.
- Metacognition: 1,200 multiple-choice items, each with associated calibration targets and error-injection pairs.
- Attention: 2,500 items across 5 noise levels.
- Executive Functions: 1,500 multi-step plan items and 800 inhibition items.
- Social Cognition: 1,000 multi-agent scenarios with annotated beliefs and expected predictions.
[REPLACE the above counts with the actual counts from the repository’s CSV/JSON files.]

Technical details
Implementation
- Kaggle Benchmarks SDK: The benchmark collection was implemented using the kaggle-benchmarks SDK. Task templates, scoring scripts, and docker-ready evaluation environments are included.
- Prompting: Each task includes a canonical prompt template plus a few controlled variations used in ablation experiments. Prompts are stored under /prompts with naming convention <track>_<template>.txt.
- Scoring: Deterministic scoring scripts (Python) are under /scoring. Each script returns structured JSON with per-item and per-run metrics. Example command to score locally:
  python scoring/score_learning.py --predictions predictions.json --gold data/learning_gold.json --output results_learning.json
- Reproducibility: We provide a start-to-finish Kaggle Notebook (public/notebook.ipynb) that demonstrates how to run model evaluation using the Kaggle Benchmarks runner and how to aggregate results. [Include link or file path if present.]

Evaluation protocol
- Model runs: We evaluated a collection of baseline models spanning closed-source APIs and open LLM checkpoints. For each model we ran the same prompt templates and randomized few-shot examples where applicable.
- Multiple seeds: Each evaluation uses multiple RNG seeds to reduce variance due to example selection (especially for few-shot Learning tasks).
- Metrics aggregation: For each task, we report primary metrics (accuracy, calibration, plan success, belief-inference accuracy) and secondary metrics such as confidence calibration curves and error-type breakdowns.

Baseline results, insights, and conclusions
[NOTE: Replace the following baseline summary and numbers with actual experimental results from your repository. The text below is written to be replaced with concrete numbers and plots.]

Summary of baseline performance
- Learning: Models often show quick improvements with additional shots, but the slope of improvement varies widely. Some models can match training-set performance on simple invented rules after 3–5 examples, but fail to generalize to out-of-distribution rule variants, indicating brittle rule representations.
- Metacognition: Models’ declared confidences correlate weakly with actual correctness. Brier scores indicate systematic overconfidence on some item types. Error-detection precision is moderate (X%), suggesting models can flag many mistakes but miss subtle reasoning errors.
- Attention: Performance degrades consistently as noise increases. Several models are distracted by highly salient but irrelevant distractors, producing confident but incorrect answers. Sustained attention tests show drift over long contexts, especially for tasks requiring multi-step extraction.
- Executive Functions: Models can produce plausible plans but struggle to adjust when intermediate steps fail. Recovery rates are significantly lower than plan-generation success rates, showing deficits in monitoring and flexibility.
- Social Cognition: Models often identify surface-level intentions but fail at deeper belief-tracking across nested perspective problems. In negotiation simulations, models sometimes default to cooperative heuristics and lose tradeoffs that require multi-agent utility optimization.

Novel insights & discriminatory power
- The constructed tasks produce a graded performance profile across models: no model scored uniformly high across all tracks, which confirms the benchmark’s ability to reveal cognitive strengths and weaknesses.
- Tasks that use invented symbols and rules (Learning) are effective at preventing memorization-based shortcuts.
- Combined evaluation (e.g., asking a model to plan and also judge its own plan’s reliability) highlighted models that produce plausible output but lack internal calibration—useful for diagnosing risky deployment cases.

Organizational affiliations
This project was developed independently by Sivatech24. Additional contributors and affiliations: [PLACEHOLDER: add any company/university affiliations or funding sources].

Reproducibility artifacts included in the repository
- Data files: [PLACEHOLDER: list exact filenames, e.g., data/learning_gold.json, data/attention_noise_levels.csv]
- Generators: [PLACEHOLDER: list scripts]
- Prompts: /prompts
- Scoring scripts: /scoring
- Kaggle Notebook: /notebooks/run_benchmark.ipynb
- Benchmark configuration: benchmark.yaml (used to register tasks in Kaggle Benchmarks)

References & citations
- DeepMind: “Measuring progress toward AGI: A cognitive framework” (2025) — guiding framework for this work.
- Kaggle Benchmarks documentation — implementation and deployment.
- [Add other domain-relevant citations present in repository]

Step-by-step log of work performed
(This is a concise chronology of the project as implemented — replace timeline entries, filenames, and dates with the repository’s actual logs and commit notes.)

1. Concept design (dates: [PLACEHOLDER]):
   - Selected five cognitive faculties based on DeepMind framework.
   - Defined evaluation goals and measurable metrics for each track.
2. Task specification (dates):
   - Drafted task templates and established structured output formats to avoid ambiguous responses.
   - Created prompt templates and scoring rubrics.
3. Data generation and curation (dates):
   - Implemented generator scripts that synthesize examples for each task (generators/*).
   - Validated items for unambiguous labels using internal human review and small pilot runs.
4. Implementation of benchmark scaffolding (dates):
   - Implemented Kaggle Benchmarks config and packaging (benchmark.yaml and task descriptions).
   - Wrote scoring scripts that produce reproducible metrics and JSON results.
   - Added a demonstration notebook for how to run the benchmark on Kaggle.
5. Baseline evaluations (dates):
   - Selected baseline models, ran evaluation pipelines, and aggregated metrics.
   - Produced plots (accuracy vs noise, calibration curves) and error analyses.
6. Analysis and writeup (dates):
   - Summarized insights, drafted the writeup, and prepared repository for Kaggle submission.
7. Submission (planned / actual):
   - Attached the Kaggle Benchmark to the Kaggle Writeup and submitted before the deadline.
   - Kaggle benchmark link: https://www.kaggle.com/benchmarks/codingmaster24/measuring-progress-toward-agi/leaderboard

Limitations and future work
- The benchmark intentionally focuses on isolating single faculties to produce clean signals; real-world competence requires integrated skills and evaluation under interactive settings — envisaged future work includes multi-task integrated scenarios, simulated interactions, and longitudinal learning evaluations.
- Dataset size and domain coverage can be expanded to test generalization across modalities (text+structured, vision) and languages.

How to use the benchmark (quick start)
1. Open the Kaggle Benchmark link above, clone or import the benchmark.
2. Follow the included notebook to register a run and submit predictions.
3. Run scoring scripts locally or on Kaggle using the provided Runner instructions.
4. Inspect per-task metrics and aggregate cognitive profiles.

Acknowledgments
Thanks to the Kaggle Benchmarks community for tools and tutorials and to the DeepMind framework authors for inspiration.

Contact
Primary author: Sivatech24
GitHub: https://github.com/Sivatech24/Measuring-Progress-Toward-AGI
Kaggle Benchmark: https://www.kaggle.com/benchmarks/codingmaster24/measuring-progress-toward-agi/leaderboard

Appendix: Where to fill in repository-specific content
- Replace every [PLACEHOLDER: ...] with actual filenames, counts, dates, and numeric results from your repo.
- If you want, I will ingest the repo files and produce a final writeup with concrete numbers, file links, and exact command examples.
