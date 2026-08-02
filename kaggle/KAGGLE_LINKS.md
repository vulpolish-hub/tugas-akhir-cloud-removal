# Tautan Kaggle dan Dataset

## Notebook Model

| Model | Kaggle notebook | Source repository |
|---|---|---|
| Baseline Multi-Temporal ResUNet | [vulpolish/sen12ms-baseline-resunet](https://www.kaggle.com/code/vulpolish/sen12ms-baseline-resunet) | [`notebooks/01_baseline_multitemporal_resunet.ipynb`](../notebooks/01_baseline_multitemporal_resunet.ipynb) |
| GAN ResNet-like | [vulpolish/sen12ms-gan-resnet](https://www.kaggle.com/code/vulpolish/sen12ms-gan-resnet) | [`notebooks/02_gan_resnet_like.ipynb`](../notebooks/02_gan_resnet_like.ipynb) |
| Conditional Patch GAN | [vulpolish/sen12ms-gan-conditional-patch-discriminator](https://www.kaggle.com/code/vulpolish/sen12ms-gan-conditional-patch-discriminator) | [`notebooks/03_conditional_patch_gan.ipynb`](../notebooks/03_conditional_patch_gan.ipynb) |

## Final Evaluation Run

- Notebook: [vulpolish/sen12ms-gan-final-cloudclean](https://www.kaggle.com/code/vulpolish/sen12ms-gan-final-cloudclean)
- Versi laporan: 3
- Status run tersimpan: `COMPLETE`
- Mode: evaluation-only; tidak melakukan training ulang
- Test-sample SHA256: `6151bee4138eb76aee5555f55f4ff6c77d5ed7019725f9fd3f1e712849318eb3`
- Visibility saat audit: private

## Dataset SEN12MS-CR-TS

- [Kaggle mirror](https://www.kaggle.com/datasets/mahmoud7abib/sen12mscrts)
- [Halaman proyek resmi](https://patricktum.github.io/cloud_removal/sen12mscrts/)

Gunakan halaman proyek resmi untuk deskripsi dataset dan Kaggle mirror apabila membutuhkan alur unduh melalui Kaggle. Dataset mentah tidak disimpan dalam repository ini.

## Catatan Provenance GAN ResNet

Versi historis yang pernah dirujuk adalah:

`https://www.kaggle.com/code/vulpolish/sen12ms-gan-resnet?scriptVersionId=331684678`

Audit lokal tidak dapat membuktikan secara independen bahwa snapshot current/default identik dengan `scriptVersionId=331684678`. Karena itu snapshot repository diperlakukan sebagai `VERIFIED_REFERENCED`, bukan salinan historis yang diklaim identik. Detail tersedia pada `gan_resnet_version_provenance.json`.
