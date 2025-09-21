# 🧬 Project 3 — Biomedical Image Ensemble (OCTMNIST → Fallbacks)

A robust, Windows-safe, CPU-friendly notebook for grayscale medical image classification. The pipeline first tries **OCTMNIST (MedMNIST)** and automatically falls back to **FashionMNIST** or a tiny **synthetic** dataset so the full training–evaluation loop always runs.

## ✨ Features
- Dataset ladder: **OCTMNIST → FashionMNIST → Synthetic**
- Windows-safe transforms (no lambdas), `num_workers=0`
- Models: **ResNet18**, **DenseNet121 (patched for 28×28)**, **SimpleCNN**
- Confidence‑weighted **ensemble** of model probabilities
- Small cells; readable and easy to extend
- Saves checkpoints to `./checkpoints_oct`

## Quickstart
```bash
# (optional) create env
python -m venv .venv && . .venv/Scripts/activate  # Windows PowerShell: .venv\Scripts\Activate.ps1

# install deps
pip install -r requirements.txt

# open the notebook and run cells top -> bottom
```

## How it works
1. **Load data** with graceful fallbacks (OCTMNIST → FashionMNIST → Synthetic).  
2. **Transforms** use a top‑level `AddGaussianNoise` class (picklable).  
3. **Models** are adapted for 1‑channel inputs; DenseNet is patched (stride=1, no initial pooling).  
4. **Train & Eval** quickly by default (subsets + 1 epoch).  
5. **Ensemble** uses confidence‑weighted averaging of softmax outputs.  
6. **Artifacts**: checkpoints saved under `./checkpoints_oct`.

## Project structure (suggested)
```
.
├─ notebooks/Project3_Biomed_Ensemble.ipynb
├─ checkpoints_oct/              # created at runtime
├─ README.md
├─ requirements.txt
├─ LICENSE
└─ .gitignore
```

## Troubleshooting
- **Pickling/lambda errors (Windows):** Use the provided `AddGaussianNoise` class; keep `num_workers=0`.
- **DenseNet “output size too small”:** Ensure you’re using the patched DenseNet (3×3, stride=1, no `pool0`).
- **Label shape errors:** The helpers squeeze `(N,1)` targets or `argmax` one‑hot `(N,C)` to `(N,)`.

## Ethics & Responsible Use
This is an educational template. Clinical use requires the right dataset licensing, expert validation, bias/safety analysis, and careful deployment practices.

## License
MIT © You (see `LICENSE`).
