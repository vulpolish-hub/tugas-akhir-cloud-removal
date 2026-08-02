# Model and Checkpoint Provenance

## Canonical Notebook Sources

| Model | Source status | Notebook SHA256 | Kaggle slug |
|---|---|---|---|
| Baseline Multi-Temporal ResUNet | VERIFIED_ACTIVE | `bd4f6e0d561ac980a5548c3bde81a24f77b900399ddf5abf02b22bc84043c996` | `vulpolish/sen12ms-baseline-resunet` |
| GAN ResNet-like | VERIFIED_REFERENCED | `bd09ddfe667e1fd3073673561a0317a62b27bb5f91d9ed28a49504e47f43ccfb` | `vulpolish/sen12ms-gan-resnet` |
| Conditional Patch GAN | VERIFIED_ACTIVE | `e4caeee9537a911dd297c5b741a73c39205762cb3cd10c79ec5d6dd7e10038bf` | `vulpolish/sen12ms-gan-conditional-patch-discriminator` |

## Final Checkpoints

Checkpoint tidak disimpan di GitHub. Nilai berikut memungkinkan verifikasi terhadap salinan eksternal.

| Model | Epoch | Selection metric | Best validation value | Checkpoint SHA256 |
|---|---:|---|---:|---|
| Baseline | 49 | `val_model_cloud_mae` | 0.0111471523 | `9177ef6baec441ffcd78bb4b232cf066a5f8bf75be76bd822a5da19352e65303` |
| GAN ResNet-like | 71 | `val_model_cloud_mae` | 0.0129800887 | `0ca11a62eb4b4f77ed7add858cdba2bd4e2488a5965a0f43641eaeac2e0c6e36` |
| Conditional Patch GAN | 13 | `val_model_cloud_mae` / best blended validation checkpoint | 0.0104941261 | `d20ee9c49f84a5df6699fe77c274af6b3fb9e0338075b821c14228feeb51c169` |

Semua checkpoint dipilih berdasarkan metrik validasi dan tidak dipilih berdasarkan hasil test.

## Shared Evaluation

- Total dataset index: 1,920 patch
- Split seed: 42
- Global test samples: 192
- Mask-valid test samples: 141
- Test-sample SHA256: `6151bee4138eb76aee5555f55f4ff6c77d5ed7019725f9fd3f1e712849318eb3`
- Final statistics: Friedman, pairwise Wilcoxon signed-rank, Holm correction

## Known Limitation

Exact historical source for GAN ResNet `scriptVersionId=331684678` was not independently retrievable. The current/default canonical snapshot is retained as `VERIFIED_REFERENCED`; no claim of byte-identical historical recovery is made.
