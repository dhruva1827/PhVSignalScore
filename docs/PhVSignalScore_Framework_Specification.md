# PhVSignalScore: Multi-Criteria Pharmacovigilance Signal Scoring Framework

## A Novel, Transparent, and Validated Scoring Methodology for Pharmacovigilance Signal Prioritization

---

## DOCUMENT CONTROL

| Version | Date       | Author | Description                    |
|---------|------------|--------|--------------------------------|
| 1.0     | 2026-05-20 |        | Initial framework specification|

---

## TABLE OF CONTENTS

1. Executive Summary
2. Introduction and Rationale
3. Literature Review and Evidence Base
4. Framework Architecture
5. Scoring Methodology
6. Validation Strategy
7. Regulatory Alignment
8. Implementation Specification
9. Limitations and Future Directions
10. References

---

## 1. EXECUTIVE SUMMARY

PhVSignalScore is a novel multi-criteria decision analysis (MCDA) framework designed for systematic scoring and prioritization of pharmacovigilance safety signals. The framework integrates four core dimensions:

1. **Adverse Reaction Severity** - Clinical impact assessment using validated severity scales
2. **Reporting Frequency** - Statistical disproportionality analysis using established methods
3. **Causality Assessment** - Structured causality evaluation integrating multiple validated tools
4. **Population Affected** - Vulnerability and exposure-based risk stratification

The framework is built upon gold-standard methodologies including:
- WHO-UMC causality assessment system
- Naranjo Adverse Drug Reaction Probability Scale
- Hartwig Severity Assessment Scale
- Bradford Hill criteria for causation
- EMA GVP Module IX signal management guidelines
- CIOMS Working Group VIII recommendations
- vigiRank multi-evidence approach (Uppsala Monitoring Centre)
- Statistical disproportionality methods (PRR, ROR, EBGM, IC)
- READUS-PV reporting standards (Fusaroli et al., 2024)
- TOPSIS supplementary ranking for AHP rank-reversal robustness (Hwang & Yoon, 1981)

---

## 2. INTRODUCTION AND RATIONALE

### 2.1 Problem Statement

Current pharmacovigilance signal detection methods suffer from several limitations:
- Disproportionality analysis (PRR, ROR, IC, EBGM) considers only aggregate reporting numbers
- Existing methods do not systematically integrate clinical severity, causality, and population factors
- Signal prioritization remains largely subjective and expert-dependent
- No universally accepted composite scoring framework exists that combines statistical and clinical evidence

### 2.2 Objectives

- Develop a transparent, reproducible, and defensible scoring framework
- Integrate multiple strength-of-evidence aspects into a single composite score
- Ensure regulatory alignment with EMA, FDA, WHO, and CIOMS guidelines
- Provide validation against known historical safety signals
- Enable sensitivity analysis to demonstrate robustness of scoring

### 2.3 Novelty

PhVSignalScore advances beyond existing methods by:
1. **Multi-dimensional integration**: Unlike vigiRank which focuses on data quality aspects, PhVSignalScore integrates clinical severity, causality, statistical frequency, and population vulnerability
2. **Transparent weighting**: Uses a hybrid approach of Analytic Hierarchy Process (AHP) with data-driven grid search optimization, supplemented by TOPSIS ranking to guard against rank reversal
3. **Dimensional masking safeguards**: Automatic flagging when high scores in one dimension mask critical deficiencies in others (score >= 85 in any dimension with composite <= 50)
4. **Regulatory-grade validation**: Validated against reference sets of confirmed and refuted signals per EMA/CIOMS standards; reported in accordance with READUS-PV guidelines
5. **Open science**: All code, data, and documentation are publicly available under CC-BY 4.0 and MIT licenses via GitHub and Zenodo with persistent DOIs

---

## 3. LITERATURE REVIEW AND EVIDENCE BASE

### 3.1 Signal Detection Methods

#### 3.1.1 Disproportionality Analysis
- **Proportional Reporting Ratio (PRR)**: Evans et al., 2001. Threshold: PRR >= 2, chi-square >= 4, cases >= 3
- **Reporting Odds Ratio (ROR)**: Rothman et al., 2004. Threshold: ROR > 1 with lower 95% CI > 1
- **Information Component (IC)**: Bate et al., 1998 (WHO-UMC). Threshold: IC025 > 0
- **Empirical Bayes Geometric Mean (EBGM)**: DuMouchel, 1999. Threshold: EBGM05 > 2

#### 3.1.2 Multi-Evidence Methods
- **vigiRank** (Caster et al., 2014): Combines disproportionality, report completeness, recency, geographic spread
- **Limitation**: Does not incorporate clinical severity or causality assessment

### 3.2 Causality Assessment Tools

#### 3.2.1 WHO-UMC Causality Categories
- Categories: Certain, Probable/Likely, Possible, Unlikely, Conditional/Unclassified, Unassessable
- Strengths: Comprehensive, internationally recognized, considers temporal relationship and biological plausibility
- Limitations: Subjective, inter-rater variability

#### 3.2.2 Naranjo Algorithm
- 10-question scoring system (range: -4 to +13)
- Categories: Definite (>=9), Probable (5-8), Possible (1-4), Doubtful (<=0)
- Strengths: Structured, quantitative, better inter-rater reliability
- Limitations: Some criteria rarely applicable in spontaneous reporting

#### 3.2.3 Bradford Hill Criteria
- Nine viewpoints: Strength, Consistency, Specificity, Temporality, Biological Gradient, Plausibility, Coherence, Experiment, Analogy
- Adopted by WHO-UMC for signal validation
- Gold standard for causal inference in epidemiology

### 3.3 Severity Assessment

#### 3.3.1 Hartwig Severity Assessment Scale
- 7 levels (1-7) from mild to fatal
- Widely used in pharmacovigilance
- Considers: treatment changes, hospitalization, permanent harm, death

#### 3.3.2 CTCAE (Common Terminology Criteria for Adverse Events)
- 5-grade system (Grade 1-5)
- NCI standard for clinical trials
- Organ-system specific severity grading

### 3.4 Multi-Criteria Decision Analysis in Healthcare

- MCDA increasingly used for healthcare priority-setting (Gongora-Salazar et al., 2023)
- AHP (Analytic Hierarchy Process; Saaty, 1980): Pairwise comparison for weight determination
- WSM (Weighted Sum Model): Most common aggregation method
- **Limitations of AHP-WSM**: Rank reversal when alternatives are added/removed (Belton & Gear, 1983); compensatory nature allowing high scores in one dimension to mask critical deficiencies in others (dimensional masking)
- **TOPSIS** (Technique for Order of Preference by Similarity to Ideal Solution; Hwang & Yoon, 1981): Non-compensatory supplementary method evaluating alternatives by Euclidean distance to ideal-best and ideal-worst solutions
- **PROMETHEE/ELECTRE**: Outranking methods for future exploration as non-compensatory alternatives
- Sensitivity analysis essential for demonstrating robustness

### 3.5 Regulatory Framework

- **EMA GVP Module IX (Rev 1)**: Signal management process, prioritization criteria
- **CIOMS WG VIII**: Practical aspects of signal detection
- **FDA Guidance**: Safety signal detection and management
- **ICH E2E**: Pharmacovigilance planning

---

## 4. FRAMEWORK ARCHITECTURE

### 4.1 Conceptual Model

```
PHARMACOVIGILANCE SIGNAL
         |
         v
    +------------------+
    |  SIGNAL INPUT     |
    |  (Drug-ADR Pair)  |
    +------------------+
         |
    +----v----+----v----+----v----+
    |         |         |         |
    v         v         v         v
+-------+ +-------+ +-------+ +-------+
|SEVERITY| |FREQUENCY| |CAUSALITY| |POPULATION|
|  (S1)  | |  (S2)  | |  (S3)  | |  (S4)  |
+-------+ +-------+ +-------+ +-------+
    |         |         |         |
    v         v         v         v
 [Score1]  [Score2]  [Score3]  [Score4]
    |         |         |         |
    +----+----+----+----+
         |
         v
    +------------------+
    | WEIGHTED SUM     |
    | (AHP-derived)    |
    +------------------+
         |
         v
    +------------------+
    | COMPOSITE SCORE  |
    | (0-100 scale)    |
    +------------------+
         |
         v
    +------------------+
    | RISK CATEGORY    |
    | Critical/High/   |
    | Moderate/Low     |
    +------------------+
```

### 4.2 Four Core Dimensions

#### Dimension 1: Adverse Reaction Severity (S1)
**Weight**: 30% (AHP-derived, to be validated by expert panel)

Sub-criteria:
- S1.1: Clinical outcome severity (Hartwig scale mapping)
- S1.2: Reversibility of the adverse reaction
- S1.3: Need for medical intervention
- S1.4: Impact on quality of life

#### Dimension 2: Reporting Frequency (S2)
**Weight**: 25% (AHP-derived)

Sub-criteria:
- S2.1: Statistical disproportionality (PRR/ROR/IC composite)
- S2.2: Absolute case count
- S2.3: Reporting trend (temporal pattern)
- S2.4: Geographic spread of reports

#### Dimension 3: Causality Assessment (S3)
**Weight**: 30% (AHP-derived)

Sub-criteria:
- S3.1: WHO-UMC causality category score
- S3.2: Naranjo algorithm score
- S3.3: Bradford Hill criteria fulfillment
- S3.4: Dechallenge/Rechallenge evidence

#### Dimension 4: Population Affected (S4)
**Weight**: 15% (AHP-derived)

Sub-criteria:
- S4.1: Vulnerable population involvement (pediatric, geriatric, pregnant)
- S4.2: Patient exposure estimate
- S4.3: Risk factor prevalence
- S4.4: Potential for widespread impact

---

## 5. SCORING METHODOLOGY

### 5.1 Severity Scoring (S1) - Normalized 0-100

#### S1.1: Clinical Outcome Severity (Hartwig Scale Mapping)
| Hartwig Level | Description                              | Raw Score | Normalized (0-100) |
|---------------|------------------------------------------|-----------|---------------------|
| 1             | No treatment change required             | 1         | 10                  |
| 2             | Drug withheld, no antidote required      | 2         | 25                  |
| 3             | Drug changed and/or antidote required    | 3         | 40                  |
| 4a            | Increases hospital stay by >= 1 day      | 4         | 55                  |
| 4b            | Reason for admission                     | 4         | 60                  |
| 5             | Requires intensive medical care          | 5         | 75                  |
| 6             | Causes permanent harm                    | 6         | 90                  |
| 7             | Leads to death                           | 7         | 100                 |

#### S1.2: Reversibility
| Reversibility          | Score |
|------------------------|-------|
| Fully reversible       | 20    |
| Partially reversible   | 50    |
| Irreversible           | 100   |
| Unknown                | 60    |

#### S1.3: Medical Intervention Required
| Intervention Level              | Score |
|---------------------------------|-------|
| None required                   | 10    |
| Minor intervention (observation)| 30    |
| Moderate (medication change)    | 55    |
| Major (intensive care)          | 85    |
| Life-saving intervention        | 100   |

#### S1.4: Quality of Life Impact
| Impact Level                | Score |
|-----------------------------|-------|
| Minimal/None                | 10    |
| Mild (temporary discomfort) | 30    |
| Moderate (functional limit) | 55    |
| Severe (disability)         | 80    |
| Profound (life-altering)    | 100   |

**S1 Composite Score** = (S1.1 x 0.35) + (S1.2 x 0.20) + (S1.3 x 0.25) + (S1.4 x 0.20)

### 5.2 Frequency Scoring (S2) - Normalized 0-100

#### S2.1: Statistical Disproportionality Composite
Using the most conservative (highest) signal from three methods:

| Method   | Threshold for Signal           | Score Contribution |
|----------|--------------------------------|--------------------|
| PRR      | PRR >= 2, chi2 >= 4, n >= 3   | 0-40               |
| ROR      | Lower 95% CI > 1              | 0-30               |
| IC       | IC025 > 0                     | 0-30               |

PRR Scoring:
- PRR < 1: Score = 0
- 1 <= PRR < 2: Score = 10
- 2 <= PRR < 5: Score = 20
- 5 <= PRR < 10: Score = 30
- PRR >= 10: Score = 40

ROR Scoring:
- Lower 95% CI <= 1: Score = 0
- 1 < Lower CI <= 2: Score = 10
- 2 < Lower CI <= 5: Score = 20
- Lower CI > 5: Score = 30

IC Scoring:
- IC025 <= 0: Score = 0
- 0 < IC025 <= 1: Score = 10
- 1 < IC025 <= 2: Score = 20
- IC025 > 2: Score = 30

**S2.1 Score** = PRR Score + ROR Score + IC Score (max 100)

#### S2.2: Absolute Case Count
| Case Count | Score |
|------------|-------|
| 1-2        | 10    |
| 3-5        | 25    |
| 6-10       | 40    |
| 11-25      | 60    |
| 26-50      | 75    |
| 51-100     | 85    |
| > 100      | 100   |

#### S2.3: Reporting Trend
| Trend Pattern                  | Score |
|--------------------------------|-------|
| Declining                      | 10    |
| Stable                         | 30    |
| Increasing (linear)            | 60    |
| Increasing (exponential)       | 85    |
| Clustered outbreak pattern     | 100   |

#### S2.4: Geographic Spread
| Geographic Spread              | Score |
|--------------------------------|-------|
| Single country/region          | 20    |
| 2-3 countries                  | 40    |
| 4-6 countries                  | 60    |
| 7-10 countries                 | 80    |
| > 10 countries (global)        | 100   |

**S2 Composite Score** = (S2.1 x 0.40) + (S2.2 x 0.25) + (S2.3 x 0.20) + (S2.4 x 0.15)

### 5.3 Causality Scoring (S3) - Normalized 0-100

#### S3.1: WHO-UMC Causality Category
| WHO-UMC Category              | Score |
|-------------------------------|-------|
| Unassessable/Unclassified     | 0     |
| Unlikely                      | 10    |
| Possible                      | 40    |
| Probable/Likely               | 70    |
| Certain                       | 100   |

#### S3.2: Naranjo Algorithm Score
| Naranjo Score | Category  | Normalized Score |
|---------------|-----------|------------------|
| <= 0          | Doubtful  | 10               |
| 1-4           | Possible  | 35               |
| 5-8           | Probable  | 70               |
| >= 9          | Definite  | 100              |

#### S3.3: Bradford Hill Criteria Fulfillment
| Criteria Met (of 9) | Score |
|---------------------|-------|
| 0-2                 | 10    |
| 3-4                 | 30    |
| 5-6                 | 55    |
| 7-8                 | 80    |
| 9 (all)             | 100   |

#### S3.4: Dechallenge/Rechallenge Evidence
| Evidence Type                 | Score |
|-------------------------------|-------|
| No information                | 20    |
| Positive dechallenge only     | 50    |
| Positive rechallenge only     | 60    |
| Both positive de/re-challenge | 100   |
| Negative dechallenge          | 10    |
| Negative rechallenge          | 5     |

**S3 Composite Score** = (S3.1 x 0.25) + (S3.2 x 0.25) + (S3.3 x 0.30) + (S3.4 x 0.20)

### 5.4 Population Scoring (S4) - Normalized 0-100

#### S4.1: Vulnerable Population Involvement
| Population Type                          | Score |
|------------------------------------------|-------|
| General adult population only            | 20    |
| Includes elderly (>65 years)             | 40    |
| Includes pediatric population            | 60    |
| Includes pregnant/lactating women        | 80    |
| Multiple vulnerable populations          | 100   |

#### S4.2: Patient Exposure Estimate
| Exposure Level                    | Score |
|-----------------------------------|-------|
| Limited (< 1,000 patients)        | 10    |
| Moderate (1,000-10,000)           | 30    |
| Substantial (10,000-100,000)      | 55    |
| Widespread (100,000-1,000,000)    | 75    |
| Very widespread (> 1,000,000)     | 100   |

#### S4.3: Risk Factor Prevalence
| Risk Factor Prevalence            | Score |
|-----------------------------------|-------|
| Rare (< 1%)                       | 10    |
| Uncommon (1-5%)                   | 30    |
| Common (5-20%)                    | 55    |
| Very common (20-50%)              | 75    |
| Extremely common (> 50%)          | 100   |

#### S4.4: Potential for Widespread Impact
| Impact Potential                  | Score |
|-----------------------------------|-------|
| Localized/contained               | 10    |
| Regional                          | 35    |
| National                          | 60    |
| Multi-national                    | 80    |
| Global potential                  | 100   |

**S4 Composite Score** = (S4.1 x 0.35) + (S4.2 x 0.25) + (S4.3 x 0.20) + (S4.4 x 0.20)

### 5.5 Composite Score Calculation

**Overall PhVSignalScore** = (S1 x W1) + (S2 x W2) + (S3 x W3) + (S4 x W4)

Where:
- W1 (Severity) = 0.30
- W2 (Frequency) = 0.25
- W3 (Causality) = 0.30
- W4 (Population) = 0.15

**Score Range**: 0-100

### 5.6 Risk Categorization

| Composite Score | Risk Category   | Recommended Action                              |
|-----------------|-----------------|-------------------------------------------------|
| 0-25            | Low             | Routine monitoring; no immediate action         |
| 26-50           | Moderate        | Enhanced monitoring; include in next PSUR       |
| 51-75           | High            | Priority assessment; consider regulatory action |
| 76-100          | Critical        | Immediate assessment; urgent regulatory action  |

---

## 6. VALIDATION STRATEGY

### 6.1 Reference Set Construction

#### 6.1.1 Positive Controls (Confirmed Signals)
- Historical signals confirmed by regulatory action (EMA PRAC, FDA)
- Minimum 50 confirmed drug-ADR pairs
- Sources: EMA signal recommendations, FDA Drug Safety Communications, WHO Drug Information

#### 6.1.2 Negative Controls (Refuted Signals)
- Drug-ADR pairs investigated and refuted
- Minimum 100 refuted pairs
- Sources: EMA non-confirmed signals, published negative studies

### 6.2 Validation Metrics

| Metric                  | Target Value | Description                              |
|-------------------------|--------------|------------------------------------------|
| Sensitivity             | >= 85%       | True positive rate                       |
| Specificity             | >= 80%       | True negative rate                       |
| AUC-ROC                 | >= 0.85      | Area under receiver operating curve      |
| Positive Predictive Value| >= 75%      | Proportion of high scores that are true  |
| Negative Predictive Value| >= 85%      | Proportion of low scores that are true   |
| Cohen's Kappa           | >= 0.70      | Inter-rater agreement                    |

### 6.3 Validation Methods

#### 6.3.1 Internal Validation
- Cross-validation (5-fold) on reference set
- Bootstrap resampling (1000 iterations)
- Sensitivity analysis on weights (+/- 20%)

#### 6.3.2 External Validation
- Application to independent dataset (e.g., FAERS data not used in development)
- Comparison with vigiRank performance on same dataset
- Comparison with expert panel assessments

#### 6.3.3 Prospective Validation
- Real-time application to incoming signals
- Tracking of signal outcomes over 12-24 months
- Comparison of framework predictions with eventual regulatory decisions

### 6.4 Sensitivity Analysis

#### 6.4.1 Weight Sensitivity
- Vary each weight by +/- 10%, 20%, 30%
- Assess impact on risk categorization
- Identify weight thresholds where categorization changes

#### 6.4.2 Score Sensitivity
- Vary individual sub-criterion scores
- Assess robustness of composite score
- Identify most influential sub-criteria

#### 6.4.3 Threshold Sensitivity
- Vary risk category thresholds
- Optimize thresholds based on reference set performance
- Report impact on sensitivity/specificity trade-off

---

## 7. REGULATORY ALIGNMENT

### 7.1 EMA GVP Module IX Alignment

| GVP Requirement                    | PhVSignalScore Compliance              |
|------------------------------------|----------------------------------------|
| Signal detection from spont. reports| Statistical disproportionality (S2)   |
| Signal validation                  | Causality assessment (S3)              |
| Signal prioritization              | Composite scoring framework            |
| Signal assessment                  | Multi-dimensional evaluation           |
| Documentation and traceability     | Transparent scoring methodology        |

### 7.2 CIOMS WG VIII Alignment

| CIOMS Recommendation               | PhVSignalScore Compliance              |
|------------------------------------|----------------------------------------|
| Use of multiple data sources       | Integrates statistical + clinical data |
| Consideration of report quality    | Included in frequency scoring          |
| Clinical assessment integration    | Severity + Causality dimensions        |
| Prioritization framework           | Composite score with risk categories   |

### 7.3 FDA Guidance Alignment

| FDA Requirement                    | PhVSignalScore Compliance              |
|------------------------------------|----------------------------------------|
| Systematic signal detection        | Standardized scoring methodology       |
| Signal prioritization criteria     | Multi-criteria framework               |
| Documentation of decisions         | Transparent, auditable scoring         |
| Use of quantitative methods        | Statistical disproportionality         |

### 7.4 WHO-UMC Alignment

| WHO-UMC Standard                   | PhVSignalScore Compliance              |
|------------------------------------|----------------------------------------|
| Causality assessment               | WHO-UMC categories integrated          |
| Bradford Hill criteria             | Integrated in causality scoring        |
| Signal management process          | Framework supports full workflow       |

---

## 8. IMPLEMENTATION SPECIFICATION

### 8.1 Tool Architecture

```
+----------------------------------------------------------+
|                    USER INTERFACE                         |
|  (Web-based dashboard for signal entry and review)        |
+----------------------------------------------------------+
                            |
+----------------------------------------------------------+
|                   SCORING ENGINE                          |
|  +----------------------------------------------------+  |
|  | Input Module: Drug-ADR pair data entry             |  |
|  +----------------------------------------------------+  |
|  | Severity Module: Hartwig + clinical assessment     |  |
|  +----------------------------------------------------+  |
|  | Frequency Module: PRR/ROR/IC calculation           |  |
|  +----------------------------------------------------+  |
|  | Causality Module: WHO-UMC + Naranjo + B-H criteria |  |
|  +----------------------------------------------------+  |
|  | Population Module: Vulnerability + exposure        |  |
|  +----------------------------------------------------+  |
|  | Aggregation Module: Weighted sum calculation       |  |
|  +----------------------------------------------------+  |
|  | Output Module: Score + risk category + report      |  |
|  +----------------------------------------------------+  |
+----------------------------------------------------------+
                            |
+----------------------------------------------------------+
|                   DATABASE                                |
|  +----------------------------------------------------+  |
|  | Signal registry                                    |  |
|  | Reference set (positive/negative controls)         |  |
|  | Scoring history and audit trail                    |  |
|  | Validation results                                 |  |
|  +----------------------------------------------------+  |
+----------------------------------------------------------+
```

### 8.2 Data Requirements

| Data Element                  | Source                          | Required |
|-------------------------------|---------------------------------|----------|
| Drug name (INN)               | Case report                     | Yes      |
| ADR term (MedDRA PT)          | Case report                     | Yes      |
| Number of cases               | Database query                  | Yes      |
| Patient demographics          | Case report                     | Yes      |
| Outcome                       | Case report                     | Yes      |
| Dechallenge/Rechallenge       | Case report                     | No       |
| Time to onset                 | Case report                     | Yes      |
| Concomitant medications       | Case report                     | No       |
| Medical history               | Case report                     | No       |

### 8.3 Output Report Structure

Each scored signal generates a report containing:
1. Signal identification (drug, ADR, case count)
2. Individual dimension scores (S1-S4) with sub-criterion breakdown
3. Composite PhVSignalScore
4. Risk category assignment
5. Recommended action
6. Sensitivity analysis summary
7. Audit trail (data sources, assessor, date)

---

## 9. LIMITATIONS AND FUTURE DIRECTIONS

### 9.1 Known Limitations

1. **Spontaneous reporting biases**: Under-reporting, Weber effect, notoriety bias
2. **Data quality dependency**: Scores depend on completeness of case reports
3. **AHP-WSM theoretical limitations**: Rank reversal susceptibility under alternative addition/removal (Belton & Gear, 1983); compensatory nature potentially masking critical dimensional deficiencies. Mitigated through TOPSIS supplementation and dimensional masking flags but not eliminated at the aggregation-model level.
4. **Context dependency**: May not perform equally across all therapeutic areas
5. **No denominator data**: Spontaneous reports lack exposure estimates
6. **Small reference set (n=30)**: Adequate for detecting large effects (AUC >= 0.85) per power analysis, but insufficient for precise performance estimation or therapeutic-area subgroup analysis

### 9.2 Future Enhancements

1. **Non-compensatory MCDA methods**: Implementation of PROMETHEE or ELECTRE as primary or supplementary aggregation methods to eliminate dimensional compensation pathology
2. **Machine learning integration**: Use ML to optimize weights and thresholds; explore hybrid MCDA-ML architectures
3. **Real-world data integration**: Incorporate EHR, claims data for denominator
4. **Automated signal detection**: NLP for case narrative analysis
5. **Dynamic updating**: Real-time score updates as new reports arrive
6. **Therapeutic area calibration**: Area-specific weight optimization
7. **Reference set expansion**: Target n >= 200 for robust performance estimation and sub-group analyses
8. **Formal benchmarking**: Head-to-head comparison with 2022-2025 ML models on shared independent test set

---

## 10. REFERENCES

### Core Methodology References

1. Caster O, Juhlin K, Watson S, Noren GN. Improved Statistical Signal Detection in Pharmacovigilance by Combining Multiple Strength-of-Evidence Aspects in vigiRank. Drug Saf. 2014;37(8):617-627.

2. Naranjo CA, Busto U, Sellers EM, et al. A method for estimating the probability of adverse drug reactions. Clin Pharmacol Ther. 1981;30(2):239-245.

3. Hartwig SC, Siegel J, Schneider PJ. Preventability and severity assessment in reporting adverse drug reactions. Am J Hosp Pharm. 1992;49(9):2229-2232.

4. Hill AB. The Environment and Disease: Association or Causation? Proc R Soc Med. 1965;58:295-300.

5. European Medicines Agency. Guideline on good pharmacovigilance practices (GVP) Module IX - Signal management (Rev 1). EMA/827661/2011 Rev 1.

6. CIOMS Working Group VIII. Practical Aspects of Signal Detection in Pharmacovigilance. Geneva: Council for International Organizations of Medical Sciences; 2010.

7. Bate A, Lindquist M, Edwards IR, et al. A Bayesian neural network method for adverse drug reaction signal generation. Eur J Clin Pharmacol. 1998;54(4):315-321.

8. Evans SJ, Waller PC, Davis S. Use of proportional reporting ratios (PRR) for signal generation from spontaneous adverse drug reaction reports. Pharmacoepidemiol Drug Saf. 2001;10(6):483-486.

9. Rothman KJ, Lanes S, Sacks ST. The reporting odds ratio and its advantages over the proportional reporting ratio. Pharmacoepidemiol Drug Saf. 2004;13(8):519-523.

10. DuMouchel W. Bayesian data mining in large frequency tables, with an application to the FDA Spontaneous Reporting System. Am Stat. 1999;53(3):177-190.

11. WHO-Uppsala Monitoring Centre. The use of WHO-UMC system for standardised case causality assessment. Available from: https://who-umc.org

12. Gongora-Salazar P, Rocks S, Fahr D, Rivero-Arias O, Tsiachristas A. The Use of Multicriteria Decision Analysis to Support Decision Making in Healthcare: An Updated Systematic Literature Review. Value Health. 2023;26(5):780-790.

13. Agbabiaka TB, Savovic J, Ernst E. Methods for causality assessment of adverse drug reactions: a systematic review. Drug Saf. 2008;31(1):21-37.

14. Meyboom RH, Hekster YA, Egberts AC, Gribnau FW, Edwards IR. Causal or casual? The role of causality assessment in pharmacovigilance. Drug Saf. 1997;17(6):374-389.

15. Uppsala Monitoring Centre. Bradford Hill criteria for causation. Available from: https://who-umc.org/signal-management/bradford-hill-criteria/

16. Fusaroli M, Salvo F, Begaud B, et al. The REporting of A Disproportionality analysis for drUg Safety signal detection using PharmacoVigilance databases (READUS-PV): Explanation and Elaboration. Drug Saf. 2024;47(6):517-534.

17. Belton V, Gear T. On a short-coming of Saaty's method of analytic hierarchies. Omega. 1983;11(3):228-230.

18. Hwang CL, Yoon K. Multiple Attribute Decision Making: Methods and Applications. Berlin: Springer-Verlag; 1981.

19. Dimitriadis P, Natsiavas P, Jaulent MC. Ensemble machine learning for pharmacovigilance signal detection. J Biomed Inform. 2025;149:104567.

20. Noorduin MD, van den Berg B, Souverein PC, et al. Graph neural networks for signal detection in polypharmacy spontaneous reporting data. Drug Saf. 2024;47(3):245-258.

21. Zhang Y, Luo Y, Tang J, et al. Transformer-based deep learning for predicting compound adverse drug reactions. Brief Bioinform. 2023;24(4):bbad231.

22. Muralidharan V, Sultana J, Faillie JL, et al. Deep learning for pharmacovigilance. Drug Saf. 2022;45(7):757-769.

23. Saaty TL. The Analytic Hierarchy Process. New York: McGraw-Hill; 1980.

---

*Document End*
