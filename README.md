# Heterogeneous Graph Modeling for Anti-Money Laundering: A Reproducible Study on Elliptic++

A reproducible study applying heterogeneous graph neural networks to the Elliptic++ Bitcoin dataset for anti-money laundering (AML) detection.

**Authors:** Abdullah Almekhyal, Yousef Joudeh  
**Affiliation:** Computer Science Department, College of Science, Kuwait University  
**Anchor Paper:** [Finding Money Launderers Using Heterogeneous Graph Neural Networks](https://doi.org/10.1016/j.jfds.2025.100175) (Johannessen & Jullum, 2025)

---

## Overview

This project reproduces and extends the heterogeneous graph-based AML detection approach proposed by Johannessen and Jullum (2025). The original paper used a private banking dataset from DNB (Norway's largest bank), limiting reproducibility. We address this by applying the same heterogeneous modeling paradigm to **Elliptic++**, a publicly available Bitcoin blockchain dataset.

### Key Results

| Metric | Value |
|--------|-------|
| ROC-AUC | 0.9718 |
| PR-AUC | 0.8289 |
| F1 (weighted) | 0.9183 |
| Accuracy | 0.9037 |
| P@R1% | 1.0000 |
| P@R5% | 0.9995 |
| P@R10% | 0.9952 |
| P@R50% | 0.9445 |
| Recall (illicit class) | 0.92 |
| F1 (illicit class) | 0.60 |

## Repository Structure

```
AML-HeteroGNN/
├── README.md                          # This file
├── requirements.txt                   # Python dependencies
├── code/
│   ├── 01_preprocessing.ipynb         # Data loading, cleaning, graph construction
│   └── 02_model_training.ipynb        # Model training and evaluation
├── data/
│   └── README.md                      # Instructions to download Elliptic++ dataset
├── results/
│   ├── final_results.txt              # Final test metrics
│   └── training_log.txt               # Training loss and metrics per epoch
└── docs/
    └── paper.pdf                      # Project report (IEEE format)
```

## Dataset

We use the **Elliptic++ dataset** (Elmougy & Liu, KDD 2023):

- **Source:** [GitHub - git-disl/EllipticPlusPlus](https://github.com/git-disl/EllipticPlusPlus)
- **Download:** [Google Drive](https://drive.google.com/drive/folders/1MRPXz79Lu_JGLlJ21MDfML44dKN9R08l?usp=sharing)

See [`data/README.md`](data/README.md) for detailed download and setup instructions.

### Dataset Statistics

| Property | Value |
|----------|-------|
| Wallet (account) nodes | 1,268,260 |
| Transaction nodes | 203,769 |
| Total nodes | 1,472,029 |
| Total edges | 4,417,560 |
| Edge types | 4 (sends, received_by, interacts_with, flows_to) |
| Illicit wallets | 14,266 |
| Licit wallets | 251,088 |
| Illicit ratio (labeled) | 5.38% |

## How to Reproduce

### Prerequisites

- Google account (for Colab and Drive)
- Python 3.10+ (if running locally)

### Step 1: Download the Dataset

1. Download the Elliptic++ dataset from [Google Drive](https://drive.google.com/drive/folders/1MRPXz79Lu_JGLlJ21MDfML44dKN9R08l?usp=sharing)
2. Upload the two zip files to your Google Drive
3. Create a folder in Google Drive (e.g., `EllipticPlusPlus/`)
4. Place the zip files inside that folder

### Step 2: Run Preprocessing

1. Open [`code/01_preprocessing.ipynb`](code/01_preprocessing.ipynb) in Google Colab
2. Update `BASE_PATH` to match your Google Drive folder path
3. Run all cells in order
4. This will:
   - Extract and load all CSV files
   - Clean and normalize features
   - Construct the heterogeneous graph (HeteroData)
   - Create 80/20 stratified train/test split
   - Save the preprocessed graph as `hetero_graph_data.pt`

**Expected runtime:** ~10–15 minutes on Colab

### Step 3: Train the Model

1. Open [`code/02_model_training.ipynb`](code/02_model_training.ipynb) in Google Colab
2. Enable GPU: Runtime → Change runtime type → T4 GPU
3. Update `GRAPH_FILE` path if needed
4. Run all cells in order
5. Results will be printed and saved to `results/`

**Expected runtime:** ~20–30 minutes on T4 GPU

## Model Architecture

The model is a heterogeneous GNN inspired by the HMPNN-sum variant from the anchor paper, implemented using PyTorch Geometric's `HeteroConv` module with `SAGEConv` operators per relation type.

| Hyperparameter | Value |
|----------------|-------|
| Hidden channels | 64 |
| Conv layers | 2 |
| Dropout | 0.3 |
| Optimizer | Adam |
| Learning rate | 0.005 |
| Weight decay | 1e-4 |
| Loss | Cross-Entropy (class-weighted) |
| Epochs | 100 |

## References

1. F. Johannessen and M. Jullum, "Finding Money Launderers Using Heterogeneous Graph Neural Networks," *Journal of Finance and Data Science*, vol. 11, 2025.
2. Y. Elmougy and Y. Liu, "Demystifying Fraudulent Transactions and Illicit Nodes in the Bitcoin Network for Financial Forensics," in *Proc. 29th ACM SIGKDD*, 2023.
3. M. Fey and J. E. Lenssen, "Fast Graph Representation Learning with PyTorch Geometric," 2019.
