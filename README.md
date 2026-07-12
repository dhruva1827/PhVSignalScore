# PhVSignalScore v6.0

> **A Bayesian Evidence Synthesis Network for Prioritizing Pharmacovigilance Safety Signals with Causal Bias Corrections**

[![Hugging Face Spaces](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Space-blue)](https://huggingface.co/spaces/Dhruv82/PhvSignalScore)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

PhVSignalScore v6.0 is an academic-grade, multi-dimensional signal triage framework designed to prioritize post-marketing drug safety alerts. It transitions pharmacovigilance prioritization from traditional deterministic Multi-Criteria Decision Analysis (MCDA) to a mathematically calibrated **Bayesian Evidence Synthesis Network (BESN)**. 

The framework synthesizes seven orthogonal evidence dimensions while dynamically adjusting for reporting confounders (Weber effect, notoriety bias, and stimulated reporting) to estimate a true, calibrated posterior probability score for regulatory action.

---

## 🚀 Live Demo
Test the interactive triage interface live on Hugging Face Spaces:
👉 [**PhVSignalScore Live Web Application**](https://huggingface.co/spaces/Dhruv82/PhvSignalScore)

---

## 🔑 Key Features
* **Multi-Dimensional Evidence Synthesis**: Synthesizes clinical severity ($S_1$), reporting frequency ($S_2$), causality ($S_3$), population vulnerability ($S_4$), evidence quality ($S_5$), temporal dynamics ($S_6$), and report completeness ($S_7$).
* **Causal Confounding Correction Engine**: Mathematically offsets disproportionality inflation caused by:
  * **Weber Effect**: Post-marketing reporting surge within the first two years of drug approval.
  * **Notoriety Bias**: Spikes in reports driven by high-profile class safety warnings.
  * **Stimulated Reporting**: Reporting inflation triggered by public media panics.
* **Rater Subjectivity Suppression**: Uses Bayesian Laplace MAP regularization to filter individual reviewer scoring variances, achieving near-perfect rater consensus.
* **Calibration & Decision Utility**: Calibrated using Platt scaling to output reliable posterior probabilities, verified by Decision Curve Analysis (DCA).

---

## 📊 Performance Summary (Dual-Cohort Validation)
PhVSignalScore v6.0 was validated head-to-head across two benchmarking datasets:
1. **Synthetic Stress-Test Cohort ($N = 900$)**: Simulated to test model boundary limits under clinical noise ($\sigma = 15.0$) and reporting biases, satisfying the Hanley-McNeil target for $>90\%$ statistical power.
2. **Real-World Validation Cohort ($N = 150$)**: Extracted from confirmed FDA/EMA alerts (positives) and OHDSI reference controls (negatives), with live case counts mapped in real-time via the public **OpenFDA API**.

| Validation Metric | Synthetic Cohort ($N=900$) | Real-World Cohort ($N=150$) | Benchmark Target |
| :--- | :---: | :---: | :---: |
| **Area Under ROC (AUC-ROC)** | **0.98827** | **0.99982** | $\ge 0.850$ |
| **Average Precision (AUC-PRC)** | **0.99124** | **0.99983** | $\ge 0.800$ |
| **Brier Score Loss** | **0.03790** | **0.00721** | $\le 0.150$ |
| **Sensitivity (Recall)** | **93.11%** | **100.00%** | $\ge 85.0\%$ |
| **Specificity** | **100.00%** | **98.67%** | $\ge 80.0\%$ |
| **Matthews Correlation (MCC)** | **0.93333** | **0.98675** | $\ge 0.600$ |
| **Calibration Slope** | **1.8497** | **4.7752** | $0.70 \text{ to } 1.30$ |

---

## 💻 Installation & Local Usage

### Prerequisites
* Python 3.10+
* Git

### Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/dhruva1827/PhVSignalScore.git
   cd PhVSignalScore
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Running the Validation Suite
* **To run the full simulation pipeline, regression tests, and generate Word reports**:
  ```bash
  python phvsignalscore_v6_pipeline.py
  ```
  *(Generates the 16 publication figures and saving the main report `PhVSignalScore_v5_Validation_Report.docx`)*

* **To run the comparative analysis between synthetic and real-world sets**:
  ```bash
  python real_world_validation.py
  ```
  *(Generates the comparative ROC/Calibration plots in `figures/fig17_synthetic_vs_real_world.png`)*

* **To launch the Gradio GUI locally**:
  ```bash
  python phvsignalscore_v6_inference.py
  ```

---

## 📂 Repository Structure
```text
PhVSignalScore/
│
├── data/                               # Validation datasets
│   ├── phvsignalscore_v5_dataset.csv   # 900-case synthetic cohort
│   └── verified_real_world_signals.csv # 150-case real-world cohort
│
├── figures/                            # Publication plots (300 DPI)
│   ├── fig02_posterior_weights.png
│   ├── fig06_dca.png
│   ├── fig17_synthetic_vs_real_world.png
│   └── ... (all generated figures)
│
├── docs/                               # Framework documentation
│   └── PhVSignalScore_Framework_Specification.md
│
├── phvsignalscore_v6_pipeline.py      # Main pipeline script
├── real_world_validation.py            # Comparative validation script
├── phvsignalscore_v6_inference.py      # Gradio GUI triage application (app.py)
│
├── requirements.txt                    # Python dependencies
└── LICENSE                             # MIT License
```

---

## ⚖️ License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## ✉️ Contact & Citation
For academic inquiries regarding the Bayesian network specification or validation datasets, please contact the corresponding author. 

*If utilizing PhVSignalScore in your research, please cite the parent manuscript:*
> *PhVSignalScore v6.0: A Bayesian Evidence Synthesis Network for Prioritizing Pharmacovigilance Safety Signals with Causal Bias Corrections. (Under review, 2026).*
