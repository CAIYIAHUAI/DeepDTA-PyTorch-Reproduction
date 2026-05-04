# DeepDTA Reproduction (PyTorch)

A clean, single-notebook PyTorch reproduction of [DeepDTA](https://github.com/hkmztrk/DeepDTA) — Öztürk, Özgür & Ozkirimli, *Bioinformatics* 2018.

The original implementation is TF1 + legacy `keras`; this notebook rewrites it with `torch` and packages everything (data download → encoding → model → training → evaluation) in a single notebook that *Runs All* end-to-end on a Kaggle / Colab GPU.

## Results (Davis dataset)

| Metric | Paper | This repo |
|---|---|---|
| MSE | 0.261 | 0.263 ± 0.008 *(5-fold)* |
| CI  | 0.878 | 0.873 *(single fold)* |
| r²m | 0.630 | 0.634 *(single fold)* |

All three metrics fall within ±1% of the paper, with r²m slightly above.

## How to run

The first cell of the notebook auto-clones the upstream DeepDTA repo for the data — no manual download is needed.

- **Kaggle**: New Notebook → upload `DeepDTA_reproduction.ipynb` → in the right sidebar set Accelerator = `GPU T4 x2` and `Internet = ON` → *Run All*.
- **Colab**: Open the notebook → *Runtime → Change runtime type → T4 GPU* → *Run All*.
- **Local**: clone this repo, install `torch matplotlib numpy`, run with Jupyter.

Single-fold demo finishes in ~15 min on a T4. Set `RUN_FULL_CV = True` in §4.2 to reproduce the 5-fold table above (~1 h).

## Differences from the original

Faithful to the original on architecture, character vocabularies, hyper-parameters (Adam, lr=1e-3, batch=256, epochs=100, patience=15), the Davis pKd transform, and the official 5-fold split files.

Intentional small deviations:
- `DataLoader(shuffle=True)` (the original sets `shuffle=False`)
- `restore_best_weights=True` for early stopping (manually implemented)
- `padding_idx=0` on the embedding layers
- The 4-config grid search over `(smi_window, seq_window)` is **not** included; only the best config from the paper `(4, 8)` is run

## Citation

```bibtex
@article{ozturk2018deepdta,
  title   = {DeepDTA: deep drug--target binding affinity prediction},
  author  = {{\"O}zt{\"u}rk, Hakime and {\"O}zg{\"u}r, Arzucan and Ozkirimli, Elif},
  journal = {Bioinformatics},
  volume  = {34},
  number  = {17},
  pages   = {i821--i829},
  year    = {2018}
}
```
