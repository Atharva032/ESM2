# ESM-2 Based Protein Optimal pH Prediction

Predicting the optimal pH of proteins from their amino acid sequences using pretrained protein language models and machine learning.

## Overview

This project explores whether protein language model representations can predict the optimal pH of proteins from their amino acid sequences. It uses Meta AI's Evolutionary Scale Modeling (ESM-2) and compares different approaches to protein property prediction.

The project investigates three approaches:

1. **Amino Acid Composition (AAC) + Ridge Regression** — a traditional machine learning baseline using amino acid frequencies.
2. **Frozen ESM-2 + Ridge Regression** — uses pretrained ESM-2 representations without fine-tuning the language model.
3. **Fine-tuned ESM-2 + MLP** — adapts a pretrained ESM-2 model to the optimal pH regression task.

The objective is to evaluate whether pretrained protein representations and task-specific fine-tuning improve optimal pH prediction compared with simpler sequence-based methods.

## Project Structure

```text
ESM2/
├── data/
├── ESM2.ipynb
├── ESM2_Model_Comparison.ipynb
└── README.md
```

- `data/` — dataset files used by the notebooks.
- `ESM2.ipynb` — initial ESM-2 fine-tuning experiment.
- `ESM2_Model_Comparison.ipynb` — comparison of classical and ESM-2-based approaches, including evaluation and error analysis.

## Methodology

### 1. Data Preparation

Protein amino acid sequences are paired with their corresponding optimal pH values. The data is divided into training, validation, and test sets.

Target normalization is used where appropriate, with predictions evaluated on the original pH scale for interpretable error metrics.

### 2. Amino Acid Composition Baseline

Each protein sequence is represented using the relative frequencies of the 20 standard amino acids.

A Ridge Regression model uses these features to establish a traditional machine learning baseline.

### 3. Frozen ESM-2 Embeddings

A pretrained ESM-2 model converts protein sequences into contextual representations. Mean pooling over residue representations produces a fixed-length vector for each protein.

Ridge Regression is then trained on these embeddings while the pretrained language model remains frozen.

### 4. Fine-tuned ESM-2

A pretrained ESM-2 model is adapted for regression using a neural network prediction head.

The model uses mean-pooled residue representations and fine-tunes selected transformer layers for the optimal pH prediction task.

### 5. Evaluation and Error Analysis

The approaches are evaluated using:

- **R²** — coefficient of determination.
- **MAE** — mean absolute error.
- **RMSE** — root mean squared error.

The comparison notebook also investigates prediction bias across different pH ranges and examines the model's tendency to overpredict acidic proteins and underpredict alkaline proteins.

## Results

The following results are recorded in the model comparison notebook.

| Model | Test R² | Test MAE | Test RMSE |
|---|---:|---:|---:|
| Amino Acid Composition + Ridge | 0.2182 | 0.7540 | 1.0208 |
| Frozen ESM-2 + Ridge | 0.3798 | 0.6690 | 0.9092 |
| Fine-tuned ESM-2 + MLP | **0.4610** | **0.6066** | **0.8477** |

*Note: These values reflect the recorded evaluation run. Confirm that all models use the same test examples and target scale before interpreting the comparison. The original notebook reports MSE on normalized targets, so its MSE should not be directly compared with the MAE or RMSE values above.*

### Key Observations

- ESM-2 embeddings outperform the amino acid composition baseline in the recorded experiment.
- Fine-tuning produces the best recorded test performance among the three evaluated approaches.
- Prediction errors are larger at extreme pH values, with overprediction for acidic proteins and underprediction for alkaline proteins.
- Sequence similarity between training and test proteins is an important consideration when evaluating generalization.

## Technologies Used

- Python
- PyTorch
- Hugging Face Transformers
- ESM-2 protein language models
- Scikit-learn
- NumPy and Pandas
- Matplotlib
- Jupyter Notebook

## How to Run

### 1. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd ESM2
```

Replace `<YOUR_GITHUB_REPOSITORY_URL>` with your repository's actual URL.

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

On macOS or Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install torch transformers scikit-learn numpy pandas matplotlib jupyter
```

Depending on the notebook's imports and dataset format, additional dependencies may be required.

### 4. Open the notebooks

```bash
jupyter notebook
```

Run `ESM2.ipynb` for the initial experiment or `ESM2_Model_Comparison.ipynb` for the model comparison.

Ensure the required dataset files are available in the expected locations under `data/`.

The first execution may download pretrained model weights. Runtime and memory requirements depend on the ESM-2 model variant, sequence lengths, and hardware.

## Limitations and Future Work

Potential improvements include:

- Evaluating generalization with sequence-similarity-aware data splits.
- Investigating class or target-distribution imbalance across pH ranges.
- Exploring alternative regression losses and sample-weighting strategies.
- Comparing additional ESM-2 model sizes and pooling strategies.
- Reporting variability across multiple random seeds.
- Evaluating prediction uncertainty and improving performance on extreme pH values.

## Research Context

This project demonstrates the application of pretrained protein language models to a protein property regression task. The experiments compare sequence composition features, frozen protein embeddings, and fine-tuned representations.

The reported results are experimental findings from this implementation and should not be interpreted as evidence of superiority over published methods without direct, controlled comparisons.
