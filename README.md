# Restorasi Citra Satelit Berawan SEN12MS-CR-TS

Repository ini berisi artefak inti Tugas Akhir mengenai restorasi citra Sentinel-2 berawan menggunakan **Multi-Temporal ResUNet** dan dua skema GAN:

1. Baseline Multi-Temporal ResUNet
2. GAN dengan Custom ResNet-like Discriminator
3. GAN dengan Conditional Patch Discriminator

Isi repository dikurasi dari sumber kanonik dan artefak final yang dipakai dalam laporan. Dataset mentah dan checkpoint tidak disimpan di GitHub.

## Dokumen Utama

- [Laporan Tugas Akhir final](report/Laporan_Tugas_Akhir_Final.docx)
- [Baseline Multi-Temporal ResUNet](notebooks/01_baseline_multitemporal_resunet.ipynb)
- [GAN ResNet-like](notebooks/02_gan_resnet_like.ipynb)
- [Conditional Patch GAN](notebooks/03_conditional_patch_gan.ipynb)
- [Tautan notebook Kaggle dan dataset](kaggle/KAGGLE_LINKS.md)
- [Audit provenance model](audit/MODEL_PROVENANCE.md)
- [Audit formula loss](audit/LOSS_FORMULA_LITERATURE_AUDIT.md)

## Dataset

SEN12MS-CR-TS dapat diperoleh dari:

- [Kaggle mirror - SEN12MS-CR-TS](https://www.kaggle.com/datasets/mahmoud7abib/sen12mscrts)
- [Halaman proyek resmi SEN12MS-CR-TS](https://patricktum.github.io/cloud_removal/sen12mscrts/)

Dataset tidak disertakan karena ukurannya besar. Struktur input penelitian menggunakan empat timestep Sentinel-2, empat timestep Sentinel-1, skor indikasi awan temporal, dan pseudo ground truth temporal.

## Ringkasan Hasil Final

Evaluasi terpadu menggunakan 192 sampel test untuk metrik global dan 141 sampel dengan piksel mask-valid untuk metrik mask-only.

| Model | RGB global MAE | RGB mask MAE | RGB global PSNR |
|---|---:|---:|---:|
| Baseline Multi-Temporal ResUNet | 0.007019 | 0.015089 | 42.4441 |
| GAN ResNet-like | 0.007660 | 0.017445 | 41.8298 |
| Conditional Patch GAN | 0.006835 | 0.013959 | 42.6618 |

Hasil statistik final menggunakan Friedman, Wilcoxon signed-rank berpasangan, dan koreksi Holm. Tabel lengkap tersedia di [`results/tables/`](results/tables/).

## Struktur Repository

```text
notebooks/          tiga notebook model kanonik
report/             laporan Word final
kaggle/             tautan run Kaggle dan provenance versi
results/tables/     tabel evaluasi dan statistik final
results/figures/    visualisasi analisis, ROI, dan sampel penuh
results/training/   training history dan kurva yang tersedia
audit/              audit formula, konflik, output hilang, dan source mapping
SHA256SUMS.txt      checksum semua artefak repository
```

## Catatan Reproduksibilitas

- Split menggunakan seed 42 dan test-sample SHA256 `6151bee4138eb76aee5555f55f4ff6c77d5ed7019725f9fd3f1e712849318eb3`.
- Checkpoint dipilih berdasarkan metrik validasi, bukan metrik test.
- Pseudo ground truth bukan citra bebas awan aktual pada waktu yang sama.
- Istilah *skor indikasi awan* tidak berarti probabilitas statistik terkalibrasi.
- Snapshot GAN ResNet adalah sumber kanonik yang direferensikan; keterbatasan verifikasi versi historis dicatat di [`kaggle/gan_resnet_version_provenance.json`](kaggle/gan_resnet_version_provenance.json).
- Checkpoint dan dataset tidak disertakan. Hash checkpoint final tersedia dalam audit provenance.

## Integritas

Gunakan `SHA256SUMS.txt` untuk memverifikasi byte seluruh file yang dipublikasikan. Konflik sumber dan output yang tidak tersedia dicatat secara terbuka pada [`audit/CONFLICT_LOG.md`](audit/CONFLICT_LOG.md) dan [`audit/MISSING_OUTPUTS.md`](audit/MISSING_OUTPUTS.md).
