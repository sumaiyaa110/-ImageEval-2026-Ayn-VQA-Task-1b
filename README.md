# Nile Nexus at ImageEval 2026 Shared Tasks: Single-Choice Reformulation and Confidence-Calibrated Refinement for Culturally Grounded Hallucination Detection

**Authors:** Sumaiya Zaman, Miftahul Jannat Rishta  
**Team:** Nile Nexus  
**Venue:** Proceedings of The Fourth Arabic Natural Language Processing Conference: Shared Tasks, 2026, pp. 697–702  
**Location:** Budapest, Hungary  
**Publisher:** Association for Computational Linguistics  
**URL:** [https://aclanthology.org/2026.arabicnlp-sharedtasks.87/](https://aclanthology.org/2026.arabicnlp-sharedtasks.87/)

## Overview

This repository contains our submission to **ImageEval 2026 / ArabicNLP26 AynVQA Shared Task, Task 1b: Hallucination Detection (English Track)**.

The task focuses on identifying the single visually grounded statement among three candidate statements associated with an image. We reformulate hallucination detection as a single-choice selection problem using **Qwen2.5-VL-7B-Instruct** with 4-bit quantization.

To improve prediction reliability, we introduce confidence-based selective refinement using seven semantically equivalent prompt variations and majority voting. This approach applies additional inference to low-confidence predictions without requiring additional training or external data.

## Methodology

### 1. Single-Choice Reformulation

Instead of independently classifying each statement as True or False, our system selects exactly one correct statement from three candidates. This enforces the exactly-one-correct constraint by design and structurally guarantees a CFHR score of zero.

### 2. Model and Quantization

- **Base Model:** Qwen2.5-VL-7B-Instruct
- **Quantization:** 4-bit NF4
- **Computation:** FP16
- **Inference Batch Size:** 8
- **Environment:** Kaggle GPU (T4/P100)

### 3. Culture-Aware Prompting

The prompt instructs the model to consider both literal visual evidence and relevant cultural or real-world knowledge when distinguishing between candidate statements.

### 4. Confidence-Gated Selective Refinement

The system extracts confidence from the probabilities of the three candidate choices. Predictions with confidence below **0.85** undergo additional refinement using seven semantically equivalent prompt variations.

The final prediction is selected through majority voting. This strategy concentrates additional inference on uncertain examples.

### 5. Zero-Shot Inference

The system does not use the training split, additional training data, fine-tuning, or data augmentation. The approach relies on prompt design, confidence gating, and paraphrase-based self-consistency refinement.

## Results

### Official Blind-Test Performance

| Metric | Score |
|---|---:|
| Accuracy | **0.942** |
| Contrastive Instability (CI) | 0.058 |
| CFHR | 0.000 |
| Q+ Accuracy | 0.942 |
| Q− Accuracy | **0.971** |
| English Track Ranking | **6th** |

### Development-Set Comparison

| Method | Accuracy | CI |
|---|---:|---:|
| Single-Choice Prediction | 0.908 | 0.092 |
| Chain-of-Thought Prompting | 0.588 | 0.412 |
| Confidence-Gated Refinement | **0.940** | **0.060** |

On the 1,000-item blind test set, selective refinement corrected 42 errors while introducing 6 new mistakes, resulting in a net gain of 36 correct predictions.

**Metric interpretation:** CFHR is structurally zero under the single-choice formulation rather than an independently achieved empirical result. CI is directly related to accuracy under this formulation.

## Key Contributions

- Single-choice reformulation of culturally grounded visual hallucination detection.
- Confidence-based selective refinement using seven paraphrased prompts.
- Majority-voting strategy for improving low-confidence predictions.
- Zero-shot inference using a quantized vision-language model without additional training or fine-tuning.
- Official **6th-place ranking** in the ImageEval 2026 English Track.

## Code

Implementation, prompts, and experimental details:

[GitHub Repository](https://github.com/sumaiyaa110/-ImageEval-2026-Ayn-VQA-Task-1b)

## Citation

If you use this work, please cite:

```bibtex
@inproceedings{zaman-rishta-2026-nile-nexus,
    title = "Nile Nexus at {I}mage{E}val 2026 Shared Tasks: Single-Choice Reformulation and Confidence-Calibrated Refinement for Culturally Grounded Hallucination Detection",
    author = "Zaman, Sumaiya  and
      Rishta, Miftahul Jannat",
    editor = "Abdul-Mageed, Muhammad  and
      Elsayed, Tamer  and
      Bouamor, Houda  and
      El-Beltagy, Samhaa  and
      Al-Khalifa, Hend  and
      Abdelali, Ahmed  and
      Elmadany, AbdelRahim  and
      El Mekki, Abdellah  and
      Darwish, Kareem  and
      Alkhereyf, Sakhar  and
      Luqman, Hamzah  and
      Khalifa, Salam  and
      Magdy, Samar  and
      Alyafeai, Zaid  and
      Hamed, Injy  and
      Zaghouani, Wajdi  and
      Zitouni, Imed",
    booktitle = "Proceedings of The Fourth {A}rabic Natural Language Processing Conference: Shared Tasks",
    month = oct,
    year = "2026",
    address = "Budapest, Hungary",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2026.arabicnlp-sharedtasks.87/",
    pages = "697--702",
    ISBN = "979-8-89176-497-2",
    abstract = "Visual hallucination detection in culturally grounded images has remained challenging due to fine-grained visual distinctions and knowledge-dependent statements. In the ImageEval2026 / ArabicNLP26 AynVQA Shared Task, we have participated in Task 1b by developing a system based on Qwen2.5-VL-7B Instruct with 4-bit quantization. We have reformulated hallucination detection as a single choice selection problem to enforce the exactly one-correct-statement constraint and have introduced confidence-based selective refinement using paraphrase ensemble voting. Our final system has achieved 0.942 accuracy, 0.058 CI, 0.000 CFHR,0.942 Q+accuracy and 0.971 Q{\ensuremath{-}}accuracy, ranking 6th in the English track. Error analysis has revealed that the remaining failures have mostly stemmed from fine-grained cultural and visual grounding challenges rather than prediction inconsistency."
}
```
