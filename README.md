<div align="center">

<h1>🎥 RecruitView: Multimodal Dataset for Personality & Interview Performance for Human Resources Applications</h1>

[![Paper](https://img.shields.io/badge/arXiv-2512.00450-b31b1b.svg)](https://arxiv.org/abs/2512.00450)
[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)
[![Task](https://img.shields.io/badge/Task-Multimodal%20Regression-blue)](https://huggingface.co/datasets/AI4A-lab/RecruitView)

**Recorded Evaluations of Candidate Responses for Understanding Individual Traits**

</div>

## ⚠️ **Note**  
> Only a limited number of video examples and their metadata are provided in this repository as samples.  
> The **complete dataset** is available on [Hugging Face: AI4A-lab/RecruitView](https://huggingface.co/datasets/AI4A-lab/RecruitView) after submitting the requirement form and agreeing to the terms and conditions.
> This restriction is in place to **protect the identity and privacy of participants**.

## 👋 Welcome to RecruitView

We are excited to introduce **RecruitView**, a robust multimodal dataset designed to push the boundaries of affective computing, automated personality assessment, and soft-skill evaluation. 

In the realm of Human Resources and psychology, judging a candidate involves a complex interplay of **what** they say (text), **how** they say it (audio), and their **non-verbal** behavior (video). Existing datasets often suffer from artificial settings or unreliable absolute ratings. 

We built RecruitView to solve this. It features **2,011 "in-the-wild" video responses** from over 300 participants, answering 76 curated interview questions. What makes this dataset truly unique is our ground truth: labels are derived from **27,000+ pairwise comparisons** made by clinical psychologists, mathematically converted into psychometrically grounded continuous scores.

---

## 📂 Dataset Structure

The dataset is organized to facilitate seamless multimodal learning (Video + Audio + Text).

### Data Splits
For model building, use stratified splits based on user IDs to prevent identity leakage.

### File Format
Each sample entry in the dataset `JSON` metadata contains the following fields:

- **`id`**: Unique entry identifier.
- **`file_name`**: Name of the MP4 file.
- **`duration`**: Categorical duration (Short/Medium/Long).
- **`question_id`**: Unique identifier for each interview question.
- **`question`**: The specific interview prompt (e.g., "Introduce yourself").
- **`video_quality`**: Quality assessment of the recording.
- **`user_no`**: Anonymized unique participant ID.
- **`[Target Metrics]`**: 12 continuous scores (normalized, almost centered around 0).
- **`transcript`**: Verbatim speech-to-text (generated via Whisper-large-v3).
- **`gemini_summary`**: Summary of the interview response created using Google Gemini 2.5 Pro. This field provides an AI-generated analysis and breakdown of the candidate’s answer, capturing the main ideas and intent.

---

## 🎯 The 12 Target Dimensions

Unlike datasets that focus only on personality, RecruitView bridges the gap between psychological traits and practical interview performance. We provide continuous regression targets for two categories:

### 🧠 1. Personality Traits (The Big Five)
These scores are based on Big Five Personality Traits:
* **Openness (O):** Imagination, creativity, and intellectual curiosity.
* **Conscientiousness (C):** Self-discipline, organization, and goal-directed behavior.
* **Extraversion (E):** Sociability, assertiveness, and energy in social interactions.
* **Agreeableness (A):** Compassion, cooperativeness, and trustworthiness.
* **Neuroticism (N):** Emotional stability vs. tendency toward anxiety/stress.
* **Overall Personality:** A holistic index derived from the combination of traits.

### 👔 2. Interview Performance Metrics
Competency-based evaluations crucial for HR applications:
* **Interview Score:** Holistic quality of the interview segment.
* **Answer Score:** Relevance, coherence, and structure of the content for the question asked.
* **Speaking Skills:** Clarity, pace, tone, and avoidance of filler words.
* **Confidence Score:** Self-assurance projected through verbal/non-verbal cues.
* **Facial Expression:** Engagement and emotional conveyance via facial cues.
* **Overall Performance:** A comprehensive summary of the candidate's display.

---

## 🛠️ Data Collection & Curation

### 1. In-the-Wild Collection
We prioritized ecological validity. Instead of a lab setting, participants recorded themselves in their natural environments (homes, classrooms) using our custom platform, **QAVideoShare**. This results in diverse lighting, backgrounds, and audio conditions, making models trained on this data more robust to real-world noise.

### 2. The Annotation Process
Subjective rating is noisy. To fix this, we employed **Comparative Judgment**:
1.  **Expert Annotators:** We hired clinical psychologists, not crowd-workers.
2.  **Pairwise Comparisons:** Instead of asking "Rate this person 1-10", we showed two videos side-by-side and asked, *"Who appears more confident?"*
3.  **Nuclear-Norm Regularization:** We collected ~27,310 of these judgments and processed them using a **Multinomial Logit (MNL) model** with nuclear norm regularization.

**The Result:** High-fidelity, continuous labels that reduce individual rater bias and capture the subtle geometric structure of human traits.

---

## 📊 Benchmarking

We benchmarked RecruitView using our novel framework, **Cross-Modal Regression with Manifold Fusion (CRMF)**, against state-of-the-art Large Multimodal Models (LMMs).

| Model | Macro Spearman (ρ) | Macro C-Index |
| :--- | :---: | :---: |
| MiniCPM-o 2.6 (8B) | 0.5102 | 0.6779 |
| VideoLLaMA2.1-AV (7B) | 0.5002 | 0.6778 |
| Qwen2.5-Omni (7B) | 0.4882 | 0.6658 |
| **CRMF (Ours)** | **0.5682** | **0.7183** |

*Our dataset proves that task-specific geometric inductive biases (as used in CRMF) can outperform massive general-purpose models.*

---

## ⚠️ Ethical Considerations & Usage Policy

**Please read carefully before using this dataset. The dataset will be made available only after the requester formally agrees to comply with all the listed points (on Huggingface page), including but not limited to those outlined here.**

We are committed to Responsible AI. Human behavioral data is sensitive, and its application requires strict ethical guardrails.

1.  **Academic Research Only:** This dataset is released under **CC BY-NC 4.0**. Commercial use is strictly prohibited.
2.  **No Automated Hiring:** This dataset and models trained on it **must not** be used for automated decision-making in real-world hiring, employment screening, or psychological profiling. The data is for research into *how* machines perceive behavior, not to replace human judgment.
3.  **Prohibited uses:** The videos, dataset, and all associated information must not be used for any purposes that could harm, exploit, or misrepresent the individuals featured. This explicitly prohibits applications such as deepfakes, identity manipulation, harassment, discriminatory profiling, or any other misuse that could negatively impact the dignity, privacy, or safety of the people in the videos.
4.  **No identity identification:** The dataset, videos, and any related information must never be used to identify, trace, or reveal the personal identity of individuals featured. This includes attempts at facial recognition, voice recognition, demographic profiling, or any other activity that could compromise the privacy, anonymity, or dignity of participants.
5.  **Bias Awareness:** While our participants are diverse in gender and background, they are primarily university students. Models may not generalize perfectly to all demographics. Users should perform fairness audits.
6.  **Privacy:** All Personally Identifiable Information (PII) has been anonymized. User IDs are randomized.

---

## 📚 Citation

If you use RecruitView in your research, please **cite** our paper:

```bibtex
@misc{gupta2025recruitview,
  title={RecruitView: A Multimodal Dataset for Predicting Personality and Interview Performance for Human Resources Applications}, 
  author={Amit Kumar Gupta and Farhan Sheth and Hammad Shaikh and Dheeraj Kumar and Angkul Puniya and Deepak Panwar and Sandeep Chaurasia and Priya Mathur},
  year={2025},
  eprint={2512.00450},
  archivePrefix={arXiv},
  primaryClass={cs.CV},
  url={https://arxiv.org/abs/2512.00450}, 
}
```

---

## ✉️ Contact & Acknowledgments

**👤 Authors:** **Amit Kumar Gupta\*, Farhan Sheth\*, Hammad Shaikh, Dheeraj Kumar, Angkul Puniya, Deepak Panwar, Sandeep Chaurasia, Priya Mathur**

*\*Equal contribution as first authors*

**📧 Correspondence:**
For questions regarding the dataset, usage, or the paper, please contact the corresponding author:
* **Amit Kumar Gupta**: `amit.gupta@jaipur.manipal.edu`

**🙏 Funding & Support:**
This work was funded by the **Manipal Research Board (MRB)** Research Grant (Letter No. DoR/MRB/2023/SG-08). We strictly acknowledge Manipal University Jaipur (MUJ) for providing the research infrastructure, computing resources, and institutional support that made this work possible.