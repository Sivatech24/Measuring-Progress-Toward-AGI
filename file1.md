## **Measuring Progress Toward AGI: A Cognitive Faculty Benchmark Suite**

### **Problem Statement**
Modern Large Language Models (LLMs) often achieve high scores on static benchmarks by relying on crystallized knowledge and patterns seen during training. This benchmark aims to move beyond "benchmaxing" by isolating **fluid cognitive abilities**. Drawing from the Google DeepMind "Measuring progress toward AGI" framework, we address the lack of empirical tools that measure a model's ability to adapt, reason about its own logic, and navigate complex social or executive constraints. 

Our suite specifically probes:
1. **Learning on the Fly:** Assessing rapid concept acquisition from minimal examples.
2. **Metacognitive Self-Monitoring:** Measuring calibration and error detection.
3. **Contextual Robustness:** Detecting susceptibility to salient but irrelevant noise.
4. **Executive Function:** Testing multi-step planning and inhibitory control.
5. **Social Reasoning:** Evaluating Theory of Mind in multi-agent vignettes.

### **Task & Benchmark Construction**
The benchmark consists of five distinct tracks, each utilizing custom-built tasks implemented via the **Kaggle Benchmarks SDK**. Each task is designed to minimize shortcut solutions through the use of novel symbols and synthetic rule sets. 
* **Evaluation Framework:** We leverage the Community Benchmarks platform to create reproducible cognitive profiles.
* **Granular Scoring:** Instead of simple pass/fail, we utilize robust scoring functions that measure learning curves, calibration scores (Brier), and plan recovery rates.

### **Dataset**
While real world meteorological and solar datasets (e.g., from Kaggle) were used as references for structural complexity, the actual task data is **entirely synthetic**. 
* **Provenance:** Authored specifically for this benchmark to ensure zero data leakage.
* **Design:** We implemented "dummy" values and controlled variables to create clean signals. This allows us to isolate specific failures (e.g., a model's failure to ignore irrelevant weather data in a planning task) without the confounding variable of the model already knowing the data from its training set.

### **Technical Details**
The implementation followed a rigorous pipeline using Python and the `kaggle benchmarks` SDK:
1. **Prompt Engineering:** Standardized templates with controlled variations for ablation.
2. **Model Evaluation:** We tested a variety of frontier models using randomized seeds to ensure statistical significance.
3. **Scoring Logic:** Custom deterministic scripts analyze the model's structured JSON outputs, providing per item metrics that reveal systematic failure modes.

### **Results, Insights, and Conclusions**
Our testing across 14 frontier models yielded a clear gradient of performance:
* **Top Performers:** **Claude Opus 4.6**, **Qwen 3 235B A22B Instruct**, and **Gemini** outperformed other models in reasoning and long-term memory.
* **Metacognition:** Even top models like Claude 4.6 showed systematic overconfidence in certain tracks, particularly when identifying their own reasoning errors.
* **Reasoning vs. Memory:** While **DeepSeek** performed admirably in logical reasoning, larger-parameter models (Qwen 3) demonstrated superior "fluidity" in adjusting plans when intermediate steps were intentionally failed (Executive Function track).
* **Conclusion:** The results prove that size doesn't always equal adaptability. Our benchmark successfully reveals that even the most "knowledgeable" models still struggle with inhibition and rapid rule-switching.

### **Organizational Affiliations**
This project was developed independently by **Sivatech24**, a Computer Science and Engineering (CSE) student. This submission represents an academic exploration into the intersection of cognitive science and artificial intelligence.

### **References & Additional Links**
* **Kaggle Benchmark:** [Measuring Progress Toward AGI](https://www.kaggle.com/benchmarks/codingmaster24/measuring-progress-toward-agi)
* **GitHub Repository:** [Measuring-Progress-Toward-AGI](https://github.com/Sivatech24/Measuring-Progress-Toward-AGI.git)
* **Framework Inspiration:** DeepMind: *Measuring progress toward AGI: A cognitive framework* (2026).
* **Reference Data:** * [Norway Meteorological Data](https://www.kaggle.com/datasets/annbengardt/noway-meteorological-data)
    * [Solar Radiation Dataset](https://www.kaggle.com/datasets/ibrahimkiziloklu/solar-radiation-dataset)
    * [Global Ecological Footprint 2023](https://www.kaggle.com/datasets/jainaru/global-ecological-footprint-2023)

---