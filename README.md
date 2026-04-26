# Heterogeneous Graph Modeling for Anti-Money Laundering

A reproducible study applying heterogeneous graph neural networks to the Elliptic++ Bitcoin dataset for anti-money laundering (AML) detection.

**Authors:** Abdullah Almekhyal, Yousef Joudeh  
**Affiliation:** Computer Science Department, College of Science, Kuwait University  
**Course:** [CS466, Machine Learning]  
**Anchor Paper:** [Finding Money Launderers Using Heterogeneous Graph Neural Networks](https://doi.org/10.1016/j.jfds.2025.100175) (Johannessen & Jullum, 2025)

---

## Overview

This project reproduces and extends the heterogeneous graph-based AML detection approach proposed by Johannessen and Jullum (2025). The original paper used a private banking dataset from DNB (Norway's largest bank), limiting reproducibility. We address this by applying the same heterogeneous modeling paradigm to **Elliptic++**, a publicly available Bitcoin blockchain dataset.

### Key Results

| Metric | Value |
|--------|-------|
| Accuracy | 97.85% |
| ROC-AUC | 0.9901 |
| PR-AUC | 0.9577 |
| F1 (illicit class) | 0.89 |
| Recall (illicit class) | 91% |

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

**Expected runtime:** ~10-15 minutes on Colab

### Step 3: Train the Model

1. Open [`code/02_model_training.ipynb`](code/02_model_training.ipynb) in Google Colab
2. Enable GPU: Runtime → Change runtime type → T4 GPU
3. Update `GRAPH_FILE` path if needed
4. Run all cells in order
5. This will:
   - Load the preprocessed graph
   - Train the HeteroGNN model for 100 epochs
   - Evaluate and print final metrics
   - Save the trained model

**Expected runtime:** ~20-30 minutes on Colab with T4 GPU

### Step 4: View Results

Final results are printed at the end of the training notebook and saved in [`results/`](results/).

## Model Architecture

```
HeteroGNN (2-layer)
├── HeteroConv Layer 1 (SAGEConv per edge type, hidden_dim=64)
│   ├── wallet → sends → transaction
│   ├── transaction → received_by → wallet
│   ├── wallet → interacts_with → wallet
│   └── transaction → flows_to → transaction
├── ReLU + Dropout(0.3)
├── HeteroConv Layer 2 (SAGEConv per edge type, hidden_dim=64)
├── ReLU
└── Linear(64 → 2) → Softmax
```

**Key hyperparameters:**

| Parameter | Value |
|-----------|-------|
| Hidden channels | 64 |
| Message-passing layers | 2 |
| Dropout | 0.3 |
| Learning rate | 0.005 |
| Weight decay | 1e-4 |
| Loss | Cross-entropy (class-weighted) |
| Optimizer | Adam |
| Epochs | 100 |

## Comparison with Anchor Paper

| Aspect | Ours (Elliptic++) | Anchor (DNB) |
|--------|-------------------|--------------|
| Dataset | Public | Private |
| Domain | Bitcoin | Banking |
| Node types | 2 | 3 |
| Edge types | 4 | 2 (9 meta-steps) |
| Total nodes | 1,472,029 | 5,149,159 |
| Total edges | 4,417,560 | 10,202,950 |
| Task | Wallet classification | Individual classification |
| Train/Test | 80/20 stratified | 80/20 stratified |

## Dependencies

See [`requirements.txt`](requirements.txt) for the full list. Main libraries:

- Python 3.10+
- PyTorch 2.x
- PyTorch Geometric 2.7+
- scikit-learn
- pandas, numpy
- matplotlib, seaborn

## References

1. F. Johannessen and M. Jullum, "Finding Money Launderers Using Heterogeneous Graph Neural Networks," *Journal of Finance and Data Science*, vol. 11, 2025.
2. Y. Elmougy and L. Liu, "Demystifying Fraudulent Transactions and Illicit Nodes in the Bitcoin Network for Financial Forensics," *Proc. 29th ACM KDD*, 2023.
3. M. Weber et al., "Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics," arXiv:1908.02591, 2019.

## License

This project is for academic purposes only. The Elliptic++ dataset is subject to its own [license terms](https://github.com/git-disl/EllipticPlusPlus).
