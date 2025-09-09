# High-Recall CYP3A4 Substrate Classifier using Ensemble Learning

This project uses machine learning to predict whether a chemical compound is a substrate for the CYP3A4 enzyme, a critical factor in early-stage drug discovery. By analyzing a compound's structure (SMILES string), this tool helps scientists prioritize laboratory testing, reduce costs, and de-risk potential drug candidates earlier in the pipeline.

---

## Live Demo

The model is deployed and ready to use as an interactive screening tool on Hugging Face Spaces.

[Try the CYP3A4 Predictor here](https://huggingface.co/spaces/<your-username>/<your-space>)

---

## Scientific Background

In drug discovery, understanding how the human body metabolizes a new medicine is crucial. A major player in this process is a family of liver enzymes called cytochrome P450s (CYPs), with CYP3A4 being responsible for breaking down approximately 30–50% of all drugs.

If a drug is a CYP3A4 substrate, it can lead to significant Drug-Drug Interactions (DDI):

- **Inhibition:** If another drug inhibits CYP3A4, the primary drug can accumulate in the body, leading to toxicity.
- **Induction:** If another drug boosts CYP3A4 production, the primary drug may be cleared too quickly, reducing its effectiveness.

Predicting whether a compound is a CYP3A4 substrate is therefore vital for ensuring drug safety and efficacy.

---

## Goal of the Project

The goal of this project is to apply machine learning to predict CYP3A4 enzyme substrates based on a compound's molecular structure (SMILES string). This predictive tool allows research teams to direct compounds to the most relevant laboratory experiments, saving time and resources.

---

## How to Use This Tool

This tool provides a straightforward way to screen molecules:

1. **Input:** Submit a compound's chemical structure as a SMILES string.
2. **Featurization:** The SMILES string is converted into a numerical Morgan fingerprint.
3. **Prediction:** A machine learning model estimates the probability of the molecule being a CYP3A4 substrate.
4. **Classification:** A recall-first threshold of 0.25 is applied to assign a label:
   - **Substrate (1):** Probability ≥ 0.25  
   - **Not a Substrate (0):** Probability < 0.25

---

## A Focus on Toxicological Safety

The most critical error in this context is a False Negative—classifying a true substrate as a non-substrate. Such a misclassification could hide serious safety issues until later, more costly stages of drug development.

To mitigate this risk, the model is optimized for high recall, ensuring it correctly identifies as many true substrates as possible. This approach accepts a higher number of False Positives, which would proceed to confirmatory testing anyway, in order to minimize the high-stakes risk of a False Negative. Our final model achieves approximately 90% recall on the test set.

---

## About the Data

The model was trained on a peer-reviewed dataset of ~2,000 compounds from Nature’s Scientific Data [1]. This dataset, sourced from Figshare [2], specifies whether a small molecule is metabolized by one of six key liver enzymes. For this project, only the CYP3A4 data was used.

- **Data Curation:** Each compound's classification was cross-verified with at least two independent, reliable sources (e.g., DrugBank, FDA lists). Compounds with conflicting references were excluded.
- **SMILES Strings:** All molecular structures were represented as canonicalized SMILES strings using RDKit to ensure consistency.

---

## Key Findings

- After evaluating several models, including Logistic Regression, Random Forest, and Gradient Boosting, a manually tuned LightGBM model delivered the best performance.
- The tuned LightGBM model achieved the highest overall performance with an ROC AUC of 0.90.
- When prioritizing high recall, the tuned LightGBM model correctly identifies 92% of true substrates while maintaining a precision of 70% at a decision threshold of 0.2.
- This balance makes it the most reliable choice for this safety-focused application, as it excels at minimizing the critical risk of false negatives.

---

## Conclusion

This project successfully developed a machine learning model to predict CYP3A4 substrates from molecular structures. By prioritizing high recall, the tuned LightGBM model provides a reliable, data-driven tool to help scientists triage compounds, focus lab resources, and de-risk potential drug candidates early in the discovery pipeline.

---

## Path Forward

- **Interactive Screening Tool:** The model has been deployed as a user-friendly web application on Hugging Face Spaces. It supports batch processing and allows users to set custom decision thresholds. You can access it here: [CYP3A4 Predictor](https://huggingface.co/spaces/<your-username>/<your-space>).
- **Expansion to Other CYP Enzymes:** The methodology can be extended to the other five CYP enzymes in the dataset (CYP1A2, 2C9, 2C19, 2D6, 2E1) to create a comprehensive DDI prediction suite.
- **Exploration of Advanced Features:** Future versions could incorporate additional molecular descriptors (e.g., LogP, molecular weight) to potentially enhance predictive accuracy.

---

## Citations

1. Ni, Y.-H., Su, Y.-W., Yang, S.-C., et al. Curated CYP450 Interaction Dataset: Covering the Majority of Phase I Drug Metabolism. Scientific Data 2025, 12, 1427.

2. Dataset (Figshare): Comprehensively-Curated Dataset of CYP4C50 Interactions: Enhancing Predictive Models for Drug Metabolism. https://doi.org/10.6084/m9.figshare.26630515
