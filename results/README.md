# Final Results

## Tables

`tables/` contains the final three-model descriptive metrics, per-band metrics, raw-versus-blended comparisons, cloud summaries, Friedman tests, Wilcoxon-Holm tests, split manifest, architecture summaries, and training hyperparameters.

The original `model_inventory.csv` is intentionally excluded because it contains machine-local absolute paths. Equivalent checkpoint and notebook provenance is published in [`../audit/MODEL_PROVENANCE.md`](../audit/MODEL_PROVENANCE.md).

## Figures

- `figures/analysis/`: aggregate plots, cloud-score examples, input-variable panels, ranking, paired differences, per-band error, and raw-versus-blended comparison.
- `figures/roi/`: ROI comparison and absolute-error views.
- `figures/samples/`: selected full-sample comparisons and raw-versus-blended panels.

Figures are copied from stored final artifacts; they were not regenerated for the GitHub curation commit.

## Training

`training/` contains training histories for all three models and the available final training curves for Baseline and GAN ResNet-like. A directly stored final Conditional training-curve image was not available, so no replacement image was generated.
