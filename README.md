
#  NAFDAC Drug Verification & Risk Classifier

An AI-enabled post-market surveillance lookup and machine learning tool designed to identify counterfeit and non-compliant NAFDAC-registered pharmaceuticals in Nigeria.

---

link to dataset: https://drive.google.com/file/d/1idSgo9XaryjETV7pvqz6a1_U2s6R3VP_/view?usp=drivesdk

##  Project Overview
Counterfeit and substandard medications pose severe public health risks. This project delivers a hybrid verification engine that pairs strict NAFDAC registry validation with natural language machine learning to detect suspicious drug entries, brand-to-registration mismatches, and failed surveillance samples.

### Key Capabilities
- **Registry Lookup Engine**: Validates provided NAFDAC registration numbers against genuine database records.
- **Mismatch Detection**: Flags instances where a registered NAFDAC number is applied to unauthorized brands or manufacturers.
- **Machine Learning Risk Model**: Analyzes text patterns (brand, generic name, dosage, manufacturer) using TF-IDF and Random Forest to score suspicious entries.
- **Surveillance Flagging**: Identifies historical quality test failures (e.g., active ingredient deficiencies or weight uniformity issues).

---

##  Dataset

Drive link to dataset: https://drive.google.com/file/d/1idSgo9XaryjETV7pvqz6a1_U2s6R3VP_/view?usp=drivesdk

The pipeline utilizes post-market surveillance data comprising:
- **822 records** of sampled pharmaceutical products across multiple Nigerian states.
- **25 attributes** including `NAFDAC REG. NO`, `BRAND NAME`, `GENERIC NAME`, `MANUFACTURER`, `DOSAGE FORM`, `REMARKS`, and laboratory assay `COMMENT` details.
- **Binary Target**: Standardized lab outcomes mapped into `Genuine/Satisfactory (0)` vs. `Suspicious/Unsatisfactory (1)`.

---

## Architecture & Pipeline

User Input (NAFDAC Reg No, Brand Name, Manufacturer)
│
▼
┌─────────────────────────────────┐
│   1. Exact NAFDAC No. Lookup    │
└────────────────┬────────────────┘
│
┌─────────────┴─────────────┐
│                           │
[ Found ]                   [ Not Found ]
│                           │
▼                           ▼
┌──────────────────┐        ┌──────────────────┐
│ Match Brand &    │        │ ML Risk Score    │
│ Manufacturer     │        │ (TF-IDF + RF)    │
└─────────┬────────┘        └─────────┬────────┘
│                           │
└─────────────┬─────────────┘
│
▼
Structured Risk Report:
[ Genuine / Suspicious / Needs Review ]

---

## Getting Started

### Prerequisites
- Python 3.9+
- Google Colab or local Jupyter Notebook environment

### Installation
```bash
git clone [https://github.com/yourusername/fake-drug-checker.git](https://github.com/yourusername/fake-drug-checker.git)
cd fake-drug-checker
pip install -r requirements.txt

### Dependencies (requirements.txt)

pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.2.0
imbalanced-learn>=0.11.0

### Usage

from drug_verifier import verify_drug_product

# 1. Genuine product check
result = verify_drug_product(nafdac_no="04-9439", brand_name="MEPIRYL")
print(result)
# Output: {'status': 'GENUINE', 'confidence': 0.98, ...}

# 2. Mismatched brand detection
result = verify_drug_product(nafdac_no="04-9439", brand_name="COUNTERFEIT BRAND")
print(result)
# Output: {'status': 'SUSPICIOUS', 'confidence': 0.90, 'reason': 'NAFDAC Reg No exists, but is registered to MAY & BAKER NIGERIA PLC...'}

### Evaluation & Results

Handling Class Imbalance: Incorporates class-weighted loss and stratified splits to account for the disproportionate genuine-to-suspicious sample ratio (~93% vs ~7%).
Primary Metrics: Evaluated using Precision, Recall, and F_1\text{-score} on the minority (suspicious) class to minimize false negatives in critical healthcare screening.

Class          Precision   Recall  F1-Score
Genuine (0).    0.95.      0.99.   0.97
Suspicious (1)  0.67.      0.20.   0.31
Overall Accuracy.——0.94


### Roadmap
[ ] Add barcode (pyzbar) and package OCR input modules.
[ ] Implement SMOTE oversampling to improve minority class recall.
[ ] Deploy interactive Streamlit / web interface for rapid frontline verification.

📄 License
Distributed under the MIT License. See LICENSE for more information.

