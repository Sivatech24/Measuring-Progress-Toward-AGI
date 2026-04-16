# Measuring Progress Toward AGI With Model
Measuring Progress Toward AGI Benchmark Suite

# Break It Down
Sivatech24 (primary author)

# Problem Statement
Modern Large Language Models (LLMs) often achieve high scores on static benchmarks by relying on crystallized knowledge and patterns seen during training. This benchmark aims to move beyond "benchmaxing" by isolating **fluid cognitive abilities**. Drawing from the Google DeepMind "Measuring progress toward AGI" framework, we address the lack of empirical tools that measure a model's ability to adapt, reason about its own logic, and navigate complex social or executive constraints. 

Our suite specifically probes:
1. **Learning on the Fly:** Assessing rapid concept acquisition from minimal examples.
2. **Metacognitive Self-Monitoring:** Measuring calibration and error detection.
3. **Contextual Robustness:** Detecting susceptibility to salient but irrelevant noise.
4. **Executive Function:** Testing multi-step planning and inhibitory control.
5. **Social Reasoning:** Evaluating Theory of Mind in multi-agent vignettes.

Why this matters
Current performance metrics frequently conflate crystallized knowledge with adaptive reasoning. Benchmarks that produce finegrained cognitive profiles reveal how models learn, when they fail, and whether those failures are systematic or random. This benchmark aims to 

### (1) probe learning on the fly 
### (2) measure a model’s self monitoring 
### (3) detect susceptibility to irrelevant context 
### (4) test planning, inhibition and working memory 
### (5) evaluate social reasoning across multi agent scenarios 
The resulting discriminatory power helps distinguish superficially highperforming models from genuinely adaptive ones.

# Kaggle Benchmark link

https://www.kaggle.com/benchmarks/codingmaster24/measuringprogresstowardagi/leaderboard

### **Task & Benchmark Construction**
The benchmark consists of five distinct tracks, each utilizing custom-built tasks implemented via the **Kaggle Benchmarks SDK**. Each task is designed to minimize shortcut solutions through the use of novel symbols and synthetic rule sets. 
* **Evaluation Framework:** We leverage the Community Benchmarks platform to create reproducible cognitive profiles.
* **Granular Scoring:** Instead of simple pass/fail, we utilize robust scoring functions that measure learning curves, calibration scores (Brier), and plan recovery rates.

## Overview
We implemented a set of tasks grouped into a single Kaggle Benchmark. Each task isolates one cognitive faculty. Tasks are authored by the team and include: synthetic rule induction (Learning), confidence calibration and error detection problems (Metacognition), distraction embedded comprehension (Attention), multi step planning and inhibition tests (Executive Functions), and multi agent belief tracking and intent inference scenarios (Social Cognition). All tasks use deterministic groundtruth labels and robust scoring functions to minimize ambiguity.

## list and short descriptions
 Learning Few shot Rule Induction (learning/∗)
   Task: Present a small number (1–5) of examples that define a novel mapping (e.g., mapping strings to labels under an invented rule). The model must generalize to heldout examples drawn from the same rule distribution.
   Purpose: Evaluate rapid concept acquisition and generalization.
   Metric: Accuracy and generalization slope as examples increase.

 Metacognition Confidence Calibration & Error Recognition (metacog/∗)
   Task: Provide multiplechoice reasoning tasks and ask the model both to answer and to give a confidence score (0–100). A secondary task asks the model to identify which of its earlier answers are likely incorrect.
   Purpose: Test calibration and ability to detect own mistakes.
   Metric: Brier score for calibration + precision/recall for mistake detection.

 Attention  Contextual Distraction and Length Robustness (attention/∗)
   Task: Key facts required to answer a query are embedded among distractor paragraphs. The amount of irrelevant but salient noise varies across examples.
   Purpose: Measure selective attention and sustained focus across long contexts.
   Metric: Accuracy vs. noiselevel curve; falsealarm rate on distractor traps.

 Executive Functions  Multistep Planning & Inhibition (execfn/∗)
   Task: Require the model to produce a plan for a multistep operation (e.g., perform three transformations on data), and to modify that plan when an intermediate step fails. Other items require inhibition of a dominant but incorrect heuristic (e.g., overriding a highfrequency pattern).
   Purpose: Evaluate planning, cognitive flexibility, and working memory.
   Metric: Plan success rate, recovery rate when failures are injected, steplevel correctness.

 Social Cognition  Theory of Mind & Negotiation (social/∗)
   Task: Multiagent vignettes where agents hold different (possibly false) beliefs; the model must predict actions or infer hidden intentions and propose contextsensitive responses.
   Purpose: Probe perspectivetaking, belief tracking, and adaptive communication.
   Metric: Accuracy on belief inference, negotiation success rate in simulated utility scoring.

## Task design principles
 Verifiability: Each item includes unambiguous correct answers. For tasks with possible multiple valid outputs, we use structured output forms (e.g., enumerated labels, JSON) and deterministic scoring logic to ensure defensibility.
 Shortcut resistance: We crafted tasks that cannot be solved through surface patterns only. For Learning tasks we use invented symbols and rule systems; for Attention tasks distractor text is novel and uninformative for the required mapping.
 Statistical power: Each task contains N examples intended to reach statistical significance for model comparisons.
 Reproducibility: Code for scoring, prompt templates, and dataset generation scripts are included in the benchmark project.

### **Dataset**
While real world meteorological and solar datasets (e.g., from Kaggle) were used as references for structural complexity, the actual task data is **entirely synthetic**. 
* **Provenance:** Authored specifically for this benchmark to ensure zero data leakage.
* **Design:** We implemented "dummy" values and controlled variables to create clean signals. This allows us to isolate specific failures (e.g., a model's failure to ignore irrelevant weather data in a planning task) without the confounding variable of the model already knowing the data from its training set.

# Technical details

The implementation followed a rigorous pipeline using Python and the `kaggle benchmarks` SDK:
1. **Prompt Engineering:** Standardized templates with controlled variations for ablation.
2. **Model Evaluation:** We tested a variety of frontier models using randomized seeds to ensure statistical significance.
3. **Scoring Logic:** Custom deterministic scripts analyze the model's structured JSON outputs, providing per item metrics that reveal systematic failure modes.

## Implementation
 Kaggle Benchmarks SDK: The benchmark collection was implemented using the kagglebenchmarks SDK. Task templates, scoring scripts, and dockerready evaluation environments are included.
 Prompting: Each task includes a canonical prompt template plus a few controlled variations used in ablation experiments. Prompts are stored under /prompts with naming convention <track>_<template>.txt.
 Scoring: Deterministic scoring scripts (Python) are under /scoring. Each script returns structured JSON with peritem and perrun metrics. Example command to score locally:
  python scoring/score_learning.py predictions predictions.json gold data/learning_gold.json output results_learning.json
 Reproducibility: We provide a starttofinish Kaggle Notebook (public/notebook.ipynb) that demonstrates how to run model evaluation using the Kaggle Benchmarks runner and how to aggregate results.

### **Results, Insights, and Conclusions**
Our testing across 14 frontier models yielded a clear gradient of performance:
* **Top Performers:** **Claude Opus 4.6**, **Qwen 3 235B A22B Instruct**, and **Gemini** outperformed other models in reasoning and long-term memory.
* **Metacognition:** Even top models like Claude 4.6 showed systematic overconfidence in certain tracks, particularly when identifying their own reasoning errors.
* **Reasoning vs. Memory:** While **DeepSeek** performed admirably in logical reasoning, larger-parameter models (Qwen 3) demonstrated superior "fluidity" in adjusting plans when intermediate steps were intentionally failed (Executive Function track).
* **Conclusion:** The results prove that size doesn't always equal adaptability. Our benchmark successfully reveals that even the most "knowledgeable" models still struggle with inhibition and rapid rule-switching.

# Evaluation protocol
 Model runs: We evaluated a collection of baseline models spanning closedsource APIs and open LLM checkpoints. For each model we ran the same prompt templates and randomized fewshot examples where applicable.
 Multiple seeds: Each evaluation uses multiple RNG seeds to reduce variance due to example selection
 Metrics aggregation: For each task, we report primary metrics (accuracy, calibration, plan success, beliefinference accuracy) and secondary metrics such as confidence calibration curves and errortype breakdowns.

# Baseline results, insights, and conclusions

Summary of baseline performance
 Learning: Models often show quick improvements with additional shots, but the slope of improvement varies widely. Some models can match trainingset performance on simple invented rules after 3–5 examples, but fail to generalize to outofdistribution rule variants, indicating brittle rule representations.
 Metacognition: Models’ declared confidences correlate weakly with actual correctness. Brier scores indicate systematic overconfidence on some item types. Errordetection precision is moderate (65%), suggesting models can flag many mistakes but miss subtle reasoning errors.
 Attention: Performance degrades consistently as noise increases. Several models are distracted by highly salient but irrelevant distractors, producing confident but incorrect answers. Sustained attention tests show drift over long contexts, especially for tasks requiring multistep extraction.
 Executive Functions: Models can produce plausible plans but struggle to adjust when intermediate steps fail. Recovery rates are significantly lower than plangeneration success rates, showing deficits in monitoring and flexibility.
 Social Cognition: Models often identify surfacelevel intentions but fail at deeper belieftracking across nested perspective problems. In negotiation simulations, models sometimes default to cooperative heuristics and lose tradeoffs that require multiagent utility optimization.

# Novel insights & discriminatory power
 The constructed tasks produce a graded performance profile across models: no model scored uniformly high across all tracks, which confirms the benchmark’s ability to reveal cognitive strengths and weaknesses.
 Tasks that use invented symbols and rules (Learning) are effective at preventing memorizationbased shortcuts.
 Combined evaluation (e.g., asking a model to plan and also judge its own plan’s reliability) highlighted models that produce plausible output but lack internal calibrationuseful for diagnosing risky deployment cases.

# Organizational affiliations
This project was developed independently by **Sivatech24**, a Computer Science and Engineering (CSE) student. This submission represents an academic exploration into the intersection of cognitive science and artificial intelligence.

### **References & Additional Links**
* **Kaggle Benchmark:** [Measuring Progress Toward AGI](https://www.kaggle.com/benchmarks/codingmaster24/measuring-progress-toward-agi)
* **GitHub Repository:** [Measuring-Progress-Toward-AGI](https://github.com/Sivatech24/Measuring-Progress-Toward-AGI.git)
* **Framework Inspiration:** DeepMind: *Measuring progress toward AGI: A cognitive framework* (2026).
* **Reference Data:** * [Norway Meteorological Data](https://www.kaggle.com/datasets/annbengardt/noway-meteorological-data)
    * [Solar Radiation Dataset](https://www.kaggle.com/datasets/ibrahimkiziloklu/solar-radiation-dataset)
    * [Global Ecological Footprint 2023](https://www.kaggle.com/datasets/jainaru/global-ecological-footprint-2023)

# Limitations and future work
 The benchmark intentionally focuses on isolating single faculties to produce clean signals; realworld competence requires integrated skills and evaluation under interactive settings envisaged future work includes multitask integrated scenarios, simulated interactions, and longitudinal learning evaluations.

# How to use the benchmark (quick start)
1. Open the Kaggle Benchmark link above, clone or import the benchmark.
2. Follow the included notebook to register a run and submit predictions.
3. Run scoring scripts locally or on Kaggle using the provided Runner instructions.
4. Inspect pertask metrics and aggregate cognitive profiles.

# Acknowledgments
Thanks to the Kaggle Benchmarks community for tools and tutorials and to the DeepMind framework authors for inspiration.

# Contact
Primary author: Sivatech24
GitHub: https://github.com/Sivatech24/MeasuringProgressTowardAGI
Kaggle Benchmark: https://www.kaggle.com/benchmarks/codingmaster24/measuringprogresstowardagi/leaderboard
