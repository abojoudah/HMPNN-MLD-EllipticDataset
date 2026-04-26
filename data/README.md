# Dataset: Elliptic++

The Elliptic++ dataset is **not included** in this repository due to its large size (~2 GB).

## Download Instructions

1. Go to the official Elliptic++ Google Drive:  
   **https://drive.google.com/drive/folders/1MRPXz79Lu_JGLlJ21MDfML44dKN9R08l?usp=sharing**

2. Download both zip files:
   - `Elliptic++ Dataset-...-001.zip` (~339 MB)
   - `Elliptic++ Dataset-...-002.zip` (~45 MB)

3. Upload both zip files to your Google Drive

4. In the preprocessing notebook (`code/01_preprocessing.ipynb`), the zips will be extracted automatically

## Required Files

After extraction, you should have these CSV files:

| File | Description | Size |
|------|-------------|------|
| `txs_features.csv` | Transaction node features (203,769 × 183) | ~695 MB |
| `txs_classes.csv` | Transaction labels | ~2 MB |
| `txs_edgelist.csv` | Transaction-to-transaction edges | ~5 MB |
| `wallets_features.csv` | Wallet node features (1,268,260 × 56) | ~607 MB |
| `wallets_classes.csv` | Wallet labels | ~30 MB |
| `AddrTx_edgelist.csv` | Wallet-to-transaction edges | ~21 MB |
| `TxAddr_edgelist.csv` | Transaction-to-wallet edges | ~37 MB |
| `AddrAddr_edgelist.csv` | Wallet-to-wallet edges | ~201 MB |

## Preprocessed Output

After running `01_preprocessing.ipynb`, a preprocessed PyTorch file is saved:
- `hetero_graph_data.pt` — Contains the full HeteroData object with features, labels, edges, and train/test masks

## Citation

If you use this dataset, please cite:

```bibtex
@article{elmougy2023demystifying,
  title={Demystifying Fraudulent Transactions and Illicit Nodes in the Bitcoin Network for Financial Forensics},
  author={Elmougy, Youssef and Liu, Ling},
  journal={arXiv preprint arXiv:2306.06108},
  year={2023}
}
```

## Source

- GitHub: https://github.com/git-disl/EllipticPlusPlus
- Paper: https://doi.org/10.1145/3580305.3599803
