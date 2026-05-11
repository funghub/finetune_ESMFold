# Project Overview
This project fine-tunes Meta's **ESMFold** protein structure prediction model to improve pLDDT (predicted Local Distance Difference Test) confidence scores on low-confidence proteins. 
The model is loaded from Hugging Face (`facebook/esmfold_v1`) and fine-tuned using PyTorch and the Hugging Face `transformers` library.

---

# Methodology
## Data (in /data folder is subset II): 
Protein sequences from TASmania_hits_seqtk.faa > TASmania_hits_seqtk-short.faa (~440 sequences total; ~25% used due to memory constraints)
## Filtering: 
Sequences over 128 amino acids were excluded to fit within Colab GPU memory limits
## Training: 
Lightweight fine-tuning with frozen base layers; custom loss based on pLDDT confidence (trained on sequences that generated pLDDT scores >= 0.7)
## Evaluation: 
Baseline pLDDT vs. fine-tuned pLDDT of low-confidence proteins (< 0.7 pLDDT) previously from baseline model

---

# Results
 
### Data Subset I
- Fine-tuning improved pLDDT for **10/10** proteins
- Consistent improvement across the board
### Data Subset II (larger, ~4.6x more training sequences)
- Fine-tuning improved pLDDT for **14/29** proteins
- Avg baseline pLDDT: **56.22**
- Avg fine-tuned pLDDT: **56.20**
- Avg change: **-0.02** (negligible)
### Key Observations
- The model overfits after the 1st epoch — validation loss is lowest at epoch 1, then increases while training loss continues to drop
- Results are limited by Colab memory and disk space constraints
---
 
## Limitations
 
- GPU memory restricted sequence length to ≤128 amino acids
- Only ~25% of the full dataset was used
- Training beyond 1 epoch leads to overfitting
## Future Work
 
With fewer memory constraints:
- Use the full dataset
- Train on longer protein sequences (>128 amino acids)
- Experiment with more training epochs with early stopping
