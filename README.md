# Soil-VNIR-SOC-Prediction

> A reproducible workflow for predicting Soil Organic Carbon (SOC) from VNIR (500–2499 nm) reflectance spectra using chemometrics and machine‑learning in Python.

---

## 📂 Repository layout

```
.
├── data/                 # Raw inputs (not tracked >20 MB; see .gitignore)
│   ├── Italy_lab.csv     # Laboratory SOC measurements
│   └── Italy_spc.csv     # VNIR spectra (500–2499 nm)
├── notebooks/
│   ├── final.ipynb       # Main analysis notebook (70 / 30 split, models, plots)
│   └── exploration.ipynb # Optional scratchpad work
├── src/                  # Re‑usable python modules
│   ├── preprocessing.py  # SNV, Savitzky–Golay helpers
│   └── models.py         # PLSR & Random‑Forest wrappers
├── results/
│   ├── figures/          # Auto‑saved PNGs & PDFs
│   └── tables/           # CSV summaries of metrics
├── environment.yml       # Conda environment (Python 3.11, scikit‑learn 1.5+)
├── .gitignore            # Ignore large data, checkpoints, cache, etc.
├── LICENSE               # MIT license
└── README.md             # You are here ✔︎
```

> **Tip ⚠️ :** The raw `Italy_spc.csv` is \~90 MB—too large for GitHub tracking.  Either keep it locally in `data/` (ignored by default) or push it with Git LFS if you need cloud storage.

---

## 🔧 Quick‑start

```bash
# 1 · Create the repo on GitHub, then clone:
$ git clone https://github.com/<YOUR‑HANDLE>/soil‑vnir‑soc‑prediction.git
$ cd soil‑vnir‑soc‑prediction

# 2 · Add the project files (copy them into the tree), then
$ git add notebooks/ src/ environment.yml .gitignore README.md
$ git commit -m "Initial analysis notebook and helpers"
$ git push origin main

# 3 · (Re)create the conda environment
$ conda env create -f environment.yml
$ conda activate soil-vnir

# 4 · Launch JupyterLab
$ jupyter lab
```

---

## 🖥 Reproducing the analysis

1. **Data split:** The notebook fixes the random seed (`42`) and performs a 70 / 30 train‑test split that remains constant for all models.
2. **Baseline:** PLSR on raw spectra.
3. **Strategy 1:** SNV → Savitzky–Golay (1st deriv.) → PLSR.
4. **Strategy 2:** Same pre‑processing → 500‑tree Random Forest.
5. **Optional:** PCA scores → PLSR (benchmark).
6. **Validation metrics** (R², RMSE, Bias, RPD) and scatter plots are saved to `results/`.

Running the notebook end‑to‑end should reproduce the numbers reported in the manuscript:

| Model               | R²       | RMSE (g kg⁻¹) | Bias  | RPD      |
| ------------------- | -------- | ------------- | ----- | -------- |
| Raw + PLSR          | 0.64     | 13.70         | –0.83 | 1.67     |
| SNV + Deriv. + PLSR | 0.74     | 11.73         | 0.01  | 1.95     |
| SNV + Deriv. + RF   | **0.74** | **11.64**     | –1.24 | **1.97** |
| PCA + PLSR          | 0.54     | 15.53         | –1.76 | 1.47     |

---

## 📜 License

This project is released under the [MIT License](LICENSE).

---

## 🙌 Acknowledgements

* JRC‑LUCAS open soil dataset (Italian subset)
* Barnes et al. (1989) – SNV transformation
* Stevens et al. (2013) – EU‑scale VNIR SOC prediction framework

---

## 🤝 Contributing

Pull requests are welcome!  Please open an issue first to discuss major changes.  Make sure new notebooks run top‑to‑bottom without errors and add/ update unit tests in `src/tests/` where applicable.
