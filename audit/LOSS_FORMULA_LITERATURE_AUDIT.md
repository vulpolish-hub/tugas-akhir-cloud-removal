# LOSS_FORMULA_LITERATURE_AUDIT

Audit date: 2026-08-01

Scope: read-only scientific audit of final loss implementations, final experiment audit artifacts, and primary literature. No training, final test, recalibration, DOCX edit, DOCX save, or DOCX copy was performed.

Important source rule: formula differences against the current Word document are marked `NOT_VERIFIABLE_FROM_FINAL_SOURCE`, because the task scope says to read source code, final notebooks, experiment audits, and papers only. I did not open, edit, save, or copy any DOCX file. I also did not use `scratch` Word-extraction artifacts as evidence for this audit, even though they exist locally, because they are outside the allowed source set in this task.

Recheck note: this audit was checked again on 2026-08-01 against the final derived-loss notebooks/configs and web-accessible primary literature records. When a primary-paper equation/page could not be verified from the accessible primary text, the audit says so explicitly instead of guessing.

## Final Source Set

Final implementation sources used:

- `THREE_MODEL_DERIVED_LOSS_EXPERIMENT/baseline_derived_loss_v1/sen12ms-baseline-resunet.ipynb`
- `THREE_MODEL_DERIVED_LOSS_EXPERIMENT/gan_resnet_v20_clean_calibration/sen12ms-gan-resnet.ipynb`
- `THREE_MODEL_DERIVED_LOSS_EXPERIMENT/conditional_v5_clean_calibration/sen12ms-gan-conditional-patch-discriminator.ipynb`
- `THREE_MODEL_DERIVED_LOSS_EXPERIMENT/baseline_v20_output/derived_loss_baseline_v1/PRETRAIN_CALIBRATION_CONFIG.json`
- `THREE_MODEL_DERIVED_LOSS_EXPERIMENT/gan_resnet_v20_output/derived_loss_gan_resnet_v20/GAN_RESNET_V20_DERIVED_CONFIG.json`
- `THREE_MODEL_DERIVED_LOSS_EXPERIMENT/conditional_v5_output/derived_loss_conditional_v5/CONDITIONAL_V5_DERIVED_CONFIG.json`
- `THREE_MODEL_DERIVED_LOSS_EXPERIMENT/THREE_MODEL_LOSS_PROVENANCE_LOCK.md`

Sources deliberately not used as final evidence:

- Historical/non-derived notebooks outside `THREE_MODEL_DERIVED_LOSS_EXPERIMENT`.
- DOCX files and DOCX-derived scratch extracts, because the task disallows Word-document changes and limits reading to source code, final notebooks, experiment audits, and papers.
- Final test outputs as a source for selecting or validating loss weights.

Line notation for notebooks: `cell N, source lines a-b`. The physical JSON line where the cell begins is also listed as `phys~line` when available. For `.ipynb` files, the raw JSON often stores a whole code cell in one `"source"` field; therefore the precise and readable locator is the notebook cell plus source-line offset inside that cell, while `phys~line` is the raw JSON line containing that cell.

Primary paper URLs checked:

- Goodfellow et al. 2014, Generative Adversarial Nets: https://papers.neurips.cc/paper/5423-generative-adversarial-nets.pdf
- Mirza and Osindero 2014, Conditional Generative Adversarial Nets: https://arxiv.org/abs/1411.1784
- Isola et al. 2017, pix2pix: https://openaccess.thecvf.com/content_cvpr_2017/papers/Isola_Image-To-Image_Translation_With_CVPR_2017_paper.pdf
- Meraner et al. 2020, cloud removal / CARL: https://www.sciencedirect.com/science/article/pii/S0924271620301398 and open abstract mirror https://pmc.ncbi.nlm.nih.gov/articles/PMC7386944/
- Mathieu et al. 2016, Deep multi-scale video prediction: https://arxiv.org/abs/1511.05440
- Wang et al. 2004, SSIM: https://www.cns.nyu.edu/pub/lcv/wang03-preprint.pdf
- Wang et al. 2018, pix2pixHD feature matching: https://openaccess.thecvf.com/content_cvpr_2018/papers/Wang_High-Resolution_Image_Synthesis_CVPR_2018_paper.pdf
- Miyato et al. 2018, Spectral Normalization: https://arxiv.org/abs/1802.05957
- Pathak et al. 2016, Context Encoders: https://openaccess.thecvf.com/content_cvpr_2016/papers/Pathak_Context_Encoders_Feature_CVPR_2016_paper.pdf

## Shared Reconstruction Objective And Final Lambdas

**A. Name and symbol.** Shared reconstruction objective, `L_rec`.

**B. Exact source.**

- Baseline notebook, cell 6, phys~112, source lines 16-23:

```python
REC_LAMBDAS = {
    "global": 0.16666666666666666,
    "cloud": 0.04335288946625992,
    "raw": 0.01624632572134276,
    "preservation": 0.2893100685605546,
    "gradient": 0.7782590153407646,
    "heavy": 0.02010559125458582,
}
```

- Baseline notebook, cell 6, source lines 71-86:

```python
def derived_reconstruction_loss(
    whole,
    cloud_weighted,
    raw_cloud_weighted,
    preservation,
    gradient,
    heavy,
):
    return (
        REC_LAMBDAS["global"] * whole
        + REC_LAMBDAS["cloud"] * cloud_weighted
        + REC_LAMBDAS["raw"] * raw_cloud_weighted
        + REC_LAMBDAS["preservation"] * preservation
        + REC_LAMBDAS["gradient"] * gradient
        + REC_LAMBDAS["heavy"] * heavy
    )
```

- `PRETRAIN_CALIBRATION_CONFIG.json`, lines 2-31: status `CALIBRATION_DERIVED`, method `static_pretraining_gradient_norm_calibration`, formula `lambda_i = g_global / (K * g_i)`, K=6, source scope `32 deterministic TRAIN batches`, train/validation/test all false for calibration.

**C. Inputs.** Aggregates components computed from raw prediction, blended output, target, t0, and mask depending on component.

**D. Operational formula.**

`L_rec = lambda_global L_global + lambda_cloud L_cloud + lambda_raw L_raw + lambda_pres L_pres + lambda_grad L_grad + lambda_heavy L_heavy`.

**E/F. Closest paper and formula.** No single paper gives this exact six-term calibrated objective. It is an operational adaptation combining L1 reconstruction, cloud/mask-aware weighting inspired by cloud-adaptive loss, gradient difference inspiration, and calibration.

**G. Classification.** `CUSTOM_OPERATIONAL_DEFINITION`.

**H. From paper.** General reconstruction, mask-aware cloud restoration, gradient consistency, adversarial extensions.

**I. Adaptation.** Six-term decomposition, static pretraining gradient-norm calibration, K=6, equal semantic priority multipliers.

**J. Constants not from paper.** All REC_LAMBDAS, K=6, 32 TRAIN batches, seed 42.

**K. Safe Bab 2 citation.** "Fungsi objektif rekonstruksi penelitian ini mengadaptasi prinsip rekonstruksi piksel, regularisasi area non-awan, dan konsistensi gradien dari literatur, kemudian bobot tiap komponennya ditentukan secara operasional melalui kalibrasi norma gradien pada data latih."

**L. Claims not allowed.** Do not claim these lambdas are directly proposed by any paper. Do not claim validation/test was used to derive lambdas.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS_WITH_CORRECTION`: shared reconstruction lambdas are verifiable from final source. However, `THREE_MODEL_LOSS_PROVENANCE_LOCK.md` appears stale for GAN ResNet `lambda_adv` (see GAN ResNet section).

## 1. Global L1 Loss

**A. Name and symbol.** Global L1 loss, `L_global`.

**B. Exact source.** Baseline notebook, cell 25, phys~388, source/train lines 137-151:

```python
raw = model(x)
pred = apply_copy_outside_mask(raw, t0, mask)
weight = heavy_cloud_weight(mask)
whole = F.l1_loss(pred, y)
...
loss = derived_reconstruction_loss(
    whole,
```

**C. Uses.** Blended output `pred` and target `y`.

**D. Formula.** `L_global = mean(|pred - y|)` over all batch, channel, height, width elements. PyTorch `F.l1_loss` default reduction is mean.

**E/F. Closest paper and formula.** Pix2pix uses an L1 reconstruction term, Eq. (2), page 2: `L_L1(G)=E_{x,y,z}[||y-G(x,z)||_1]`. Pathak et al. use masked reconstruction, not this global blended L1.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** L1 reconstruction idea.

**I. Adaptation.** Uses blended prediction after copy-outside-mask, not raw generator output alone.

**J. Constants not from paper.** Lambda `0.16666666666666666`.

**K. Safe Bab 2 citation.** "Komponen L1 digunakan sebagai penalti rekonstruksi piksel global, sejalan dengan penggunaan L1 pada tugas image-to-image translation."

**L. Claims not allowed.** Do not claim pix2pix uses this satellite mask blending formula.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 2. Weighted Masked / Cloud Loss

**A. Name and symbol.** Weighted cloud masked MAE, `L_cloud`.

**B. Exact source.** Baseline notebook, cell 25, source lines 8-11 and 141-145:

```python
def weighted_masked_mae(pred, target, mask, weight):
    effective = mask * weight
    err = torch.abs(pred - target) * effective
    return err.sum() / (effective.sum() * pred.shape[1] + 1e-8)
...
weight = heavy_cloud_weight(mask)
cloud_weighted = weighted_masked_mae(pred, y, mask, weight)
```

Heavy weighting source, cell 5, phys~92, source lines 24-26 and cell 25, source lines 41-42:

```python
HEAVY_PIXEL_THRESHOLD: float = 0.66
HEAVY_LOSS_BOOST: float = 2.5
HEAVY_BINARY_BOOST: float = 2.0
...
return 1.0 + CFG.HEAVY_LOSS_BOOST * mask + CFG.HEAVY_BINARY_BOOST * (mask >= CFG.HEAVY_PIXEL_THRESHOLD).float()
```

**C. Uses.** Blended output `pred`, target `y`, soft cloud mask `mask`, and weight map.

**D. Formula.** Let `w = 1 + 2.5 m + 2.0 1[m >= 0.66]`. `L_cloud = sum(|pred-y| m w) / (sum(m w) C + eps)`, where `C=pred.shape[1]`.

**E/F. Closest paper and formula.** Meraner et al. 2020 propose cloud-adaptive regularized loss to preserve cloud-free regions and focus reconstruction on cloud-affected regions. Exact equation/page could not be verified from accessible final source mirrors during this audit; mark paper equation `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Cloud-aware weighting/preservation principle.

**I. Adaptation.** Soft mask from this project, denominator normalization by weighted mask sum times channel count, extra heavy pixel boost.

**J. Constants not from paper.** `2.5`, `2.0`, threshold `0.66`, epsilon `1e-8`.

**K. Safe Bab 2 citation.** "Loss area awan dirancang sebagai adaptasi dari gagasan cloud-adaptive regularization, yaitu memberikan perhatian lebih pada area terdampak awan sambil tetap menjaga konsistensi area lain."

**L. Claims not allowed.** Do not claim Meraner et al. used the exact `1 + 2.5m + 2.0 I[m>=0.66]` weighting.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS_WITH_LIMITATION`: implementation verified; exact Meraner equation not verified from primary text access.

## 3. Raw Masked Loss

**A. Name and symbol.** Raw masked weighted cloud MAE, `L_raw`.

**B. Exact source.** Baseline notebook, cell 25, source line 144:

```python
raw_cloud_weighted = weighted_masked_mae(raw, y, mask, weight)
```

**C. Uses.** Raw generator prediction `raw`, target `y`, mask, and cloud/heavy weight.

**D. Formula.** `L_raw = sum(|raw-y| m w) / (sum(mw) C + eps)`.

**E/F. Closest paper and formula.** No direct primary paper match found. Pix2pix L1 Eq. (2) is the closest generic reconstruction term, but it is not masked/weighted nor separated as raw-vs-blended.

**G. Classification.** `CUSTOM_OPERATIONAL_DEFINITION`.

**H. From paper.** Generic L1 reconstruction only.

**I. Adaptation.** Explicitly supervises raw generator before copy-outside-mask blending.

**J. Constants not from paper.** Same mask weighting constants and lambda `0.01624632572134276`.

**K. Safe Bab 2 citation.** "Selain output hasil blending, prediksi mentah generator juga diberi penalti rekonstruksi pada area awan agar generator tetap belajar memulihkan piksel yang tertutup awan."

**L. Claims not allowed.** Do not claim raw masked loss is a named loss from pix2pix or CARL.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 4. Preservation Loss

**A. Name and symbol.** Preservation/copy penalty, `L_pres`.

**B. Exact source.** Baseline notebook, cell 25, source line 145:

```python
copy_penalty = masked_mae(pred, t0, 1.0 - mask)
```

`masked_mae` source lines 4-6:

```python
err = torch.abs(pred - target) * mask
return err.sum() / (mask.sum() * pred.shape[1] + 1e-8)
```

**C. Uses.** Blended output `pred`, cloudy Sentinel-2 t0 `t0`, and complement mask `1-mask`.

**D. Formula.** `L_pres = sum(|pred-t0| (1-m)) / (sum(1-m) C + eps)`.

**E/F. Closest paper and formula.** Meraner et al. 2020 is the closest source concept for retaining original information in cloud-free regions. Pathak et al. 2016 use masked reconstruction for inpainting holes, not a cloud-free preservation penalty against the original input. Therefore Pathak is not a direct source for this implementation.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Preservation of reliable, non-cloud input information is closest to CARL, not Pathak.

**I. Adaptation.** Uses soft mask complement and compares blended output to t0.

**J. Constants not from paper.** Lambda `0.2893100685605546`, epsilon `1e-8`.

**K. Safe Bab 2 citation.** "Preservation loss digunakan untuk membatasi perubahan pada area non-awan, sebagai adaptasi dari prinsip cloud-adaptive regularization."

**L. Claims not allowed.** Do not claim this preservation term comes directly from Pathak et al. 2016.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS_WITH_CORRECTION`: cite Meraner/CARL concept, not Pathak as direct source.

## 5. Gradient Loss

**A. Name and symbol.** Gradient MAE, `L_grad`.

**B. Exact source.** Baseline notebook, cell 25, source lines 19-26:

```python
def gradient_mae(pred, target, mask):
    dx_p = pred[:, :, :, 1:] - pred[:, :, :, :-1]
    dx_t = target[:, :, :, 1:] - target[:, :, :, :-1]
    mx = mask[:, :, :, 1:]
    dy_p = pred[:, :, 1:, :] - pred[:, :, :-1, :]
    dy_t = target[:, :, 1:, :] - target[:, :, :-1, :]
    my = mask[:, :, 1:, :]
    return 0.5 * (masked_mae(dx_p, dx_t, mx) + masked_mae(dy_p, dy_t, my))
```

Conditional notebook repeats this exact signed finite-difference version in cell 20, source lines 19-26.

GAN ResNet v20 clean calibration notebook, cell 25, source lines 268-281, instead defines a magnitude-difference version without factor `0.5`:

```python
dy_pred = torch.abs(pred[:, :, 1:, :] - pred[:, :, :-1, :])
dy_target = torch.abs(target[:, :, 1:, :] - target[:, :, :-1, :])
...
total_loss = loss_y.sum() / (...) + loss_x.sum() / (...)
return total_loss
```

**C. Uses.** Blended output `pred`, target `y`, and mask.

**D. Formula.** Baseline/conditional final: `L_grad = 0.5 [ MAE_mx(Delta_x pred, Delta_x y) + MAE_my(Delta_y pred, Delta_y y) ]` using signed finite differences. GAN ResNet v20 source differs: `| |Delta pred| - |Delta y| |` summed x+y, no `0.5`.

**E/F. Closest paper and formula.** Mathieu et al. 2016, gradient difference loss, Eq. (8), page 4, penalizes the difference between absolute image gradients in vertical and horizontal directions.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Gradient-domain difference idea.

**I. Adaptation.** Baseline/conditional use signed finite differences and average by `0.5`, which differs from Mathieu's absolute gradient magnitude difference. GAN ResNet v20 uses magnitude difference but omits `0.5`.

**J. Constants not from paper.** Lambda `0.7782590153407646`; factor `0.5` in baseline/conditional; mask denominator.

**K. Safe Bab 2 citation.** "Gradient loss mengadaptasi gagasan gradient difference loss untuk mendorong konsistensi struktur tepi, tetapi implementasi penelitian disesuaikan dengan mask awan dan normalisasi kanal."

**L. Claims not allowed.** Do not claim the signed finite-difference version is exactly Mathieu GDL. Do not claim all three final notebooks use identical gradient implementation unless GAN ResNet v20 is corrected or clarified.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `FAIL_FOR_CROSS_MODEL_CONSISTENCY`: baseline and conditional use signed finite difference with `0.5`; GAN ResNet v20 source uses gradient magnitude difference without `0.5`.

## 6. Heavy Cloud Loss

**A. Name and symbol.** Heavy cloud MAE term, `L_heavy`.

**B. Exact source.** Baseline notebook, cell 25, source lines 13-17, 44-48, and 146-149:

```python
def bucket_mae(pred, target, bucket_mask):
    denom = bucket_mask.sum() * pred.shape[1]
    if float(denom.detach().cpu()) < 1.0:
        return torch.tensor(float("nan"), device=pred.device)
    return (torch.abs(pred - target) * bucket_mask).sum() / (denom + 1e-8)
...
heavy = (mask >= CFG.HEAVY_PIXEL_THRESHOLD).float()
...
heavy_only = bucket_mae(pred, y, heavy_m)
heavy_term = torch.where(torch.isnan(heavy_only), cloud_plain, heavy_only)
```

**C. Uses.** Blended output `pred`, target `y`, heavy mask `mask >= 0.66`, fallback cloud plain loss.

**D. Formula.** `L_heavy = MAE(pred,y | m>=0.66)` if heavy denominator exists; otherwise `L_cloud_plain = masked_mae(pred,y,m)`.

**E/F. Closest paper and formula.** Closest conceptual source is cloud-aware weighting/preservation in Meraner et al. 2020. No exact heavy threshold/fallback formula was found in the required papers.

**G. Classification.** `CUSTOM_OPERATIONAL_DEFINITION`.

**H. From paper.** General cloud-aware focus only.

**I. Adaptation.** Heavy pixel threshold, fallback behavior, and separate lambda are project-specific.

**J. Constants not from paper.** Threshold `0.66`, lambda `0.02010559125458582`, fallback to `cloud_plain`.

**K. Safe Bab 2 citation.** "Area awan tebal diperlakukan sebagai kasus operasional khusus untuk memastikan piksel dengan probabilitas awan tinggi tetap berkontribusi pada objektif pelatihan."

**L. Claims not allowed.** Do not claim heavy loss threshold `0.66` comes from Meraner or another paper.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 7. GAN ResNet Generator Adversarial Loss

**A. Name and symbol.** ResNet-like GAN generator adversarial loss, `L_adv_resnet_G`.

**B. Exact source.** GAN ResNet v20 notebook, cell 25, source lines 386-408:

```python
out_fake_G = netD(pred)
loss_G_adv = criterion_GAN(out_fake_G, torch.ones_like(out_fake_G))
...
loss = loss_recon + LAMBDA_ADV * loss_G_adv
```

Criterion and lambda, cell 25, lines 445-458:

```python
criterion_GAN = nn.BCEWithLogitsLoss()
...
LAMBDA_ADV = GAN_DERIVED_CONFIG["derived_lambda_adv"]
```

Final config, `GAN_RESNET_V20_DERIVED_CONFIG.json`, lines 5-13: `T_resnet=0.0015220244981312154`, `raw_adv_gradient_median=0.01706953812390566`, formula `lambda_adv = T_resnet / g_adv`, `derived_lambda_adv=0.08916612078680913`.

**C. Uses.** Blended output `pred` and ResNet discriminator logits.

**D. Formula.** `L_adv_G = BCEWithLogits(D(pred), 1) = mean(softplus(-D(pred)))`; `L_G = L_rec + lambda_adv L_adv_G`.

**E/F. Closest paper and formula.** Goodfellow et al. 2014 Eq. (1), page 2: minimax GAN value. The non-saturating generator objective is discussed as maximizing discriminator error / `log D(G(z))`. For image-to-image, pix2pix Eq. (1) and Eq. (3), page 2, combine cGAN with L1.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Real/fake adversarial training.

**I. Adaptation.** Uses BCEWithLogits, blended satellite prediction, and derived lambda.

**J. Constants not from paper.** `lambda_adv=0.08916612078680913`, T/g calibration.

**K. Safe Bab 2 citation.** "Adversarial loss generator mengikuti prinsip GAN non-saturating, yaitu mendorong output generator diklasifikasikan sebagai real oleh diskriminator."

**L. Claims not allowed.** Do not claim discriminator classifies cloud/non-cloud; it classifies real/fake imagery.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS_WITH_CORRECTION`: `THREE_MODEL_LOSS_PROVENANCE_LOCK.md` lists stale `lambda_adv=0.008021420735937238`; final v20 config says `0.08916612078680913`.

## 8. GAN ResNet Discriminator Loss

**A. Name and symbol.** ResNet-like discriminator BCE loss, `L_D_resnet`.

**B. Exact source.** GAN ResNet v20 notebook, cell 25, source lines 366-378:

```python
out_real = netD(y)
loss_D_real = criterion_GAN(out_real, torch.ones_like(out_real))
raw = model(x)
pred = apply_copy_outside_mask(raw, t0, mask)
out_fake = netD(pred.detach())
loss_D_fake = criterion_GAN(out_fake, torch.zeros_like(out_fake))
loss_D = (loss_D_real + loss_D_fake) * 0.5
```

Discriminator architecture, cell 23, source lines 99-121:

```python
class ResNetDiscriminator(nn.Module):
    ...
    self.gap = nn.AdaptiveAvgPool2d((1, 1))
    self.fc = nn.Linear(base*4, 1)
    ...
    return self.fc(x)
```

**C. Uses.** Real target `y`, blended fake `pred.detach()`, single logit.

**D. Formula.** `L_D = 0.5 [BCEWithLogits(D(y),1) + BCEWithLogits(D(pred.detach()),0)]`.

**E/F. Closest paper and formula.** Goodfellow et al. 2014 Eq. (1), page 2: discriminator maximizes real log-probability and fake log-complement.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Real/fake discriminator objective.

**I. Adaptation.** Single-logit ResNet-like discriminator, spectral normalization, BCEWithLogits implementation.

**J. Constants not from paper.** `0.5` averaging factor, architecture choices.

**K. Safe Bab 2 citation.** "Diskriminator dilatih sebagai pengklasifikasi real/fake dengan loss BCE, sesuai prinsip dasar GAN."

**L. Claims not allowed.** Do not claim ResNet discriminator produces a patch map; source shows one logit after global average pooling. Do not claim it classifies cloud/non-cloud.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 9. Conditional Generator Adversarial Loss

**A. Name and symbol.** Conditional generator adversarial loss, `L_adv_cond_G`.

**B. Exact source.** Conditional v5 notebook, cell 22, source lines 728-736:

```python
fake_logits_g, fake_features = discriminator(condition, raw_prediction, return_features=True)
...
adv_loss = F.softplus(-fake_logits_g).mean()
fm_loss = feature_matching_loss(fake_features, real_features)
g_loss = (rec_loss + LAMBDA_ADV * adv_loss + LAMBDA_FM * fm_loss + LAMBDA_SSIM * rec_logs["ssim_loss"])
```

Lambda source, cell 22, lines 772-774 and final config lines 5-11:

```python
LAMBDA_ADV = CONDITIONAL_DERIVED_CONFIG["derived_lambda_adv"]
LAMBDA_FM = CONDITIONAL_DERIVED_CONFIG["derived_lambda_FM"]
LAMBDA_SSIM = CONDITIONAL_DERIVED_CONFIG["derived_lambda_SSIM"]
```

Final values: `lambda_adv=0.07930882748798064`, `lambda_FM=0.10122845103340689`, `lambda_SSIM=0.03431264440153357`.

**C. Uses.** Raw generator prediction, conditional discriminator, condition tensor, patch logits.

**D. Formula.** `L_adv_cond_G = mean(softplus(-D(c, raw)))`; total `L_G = L_rec + lambda_adv L_adv + lambda_FM L_FM + lambda_SSIM L_SSIM`.

**E/F. Closest paper and formula.** Mirza and Osindero 2014 Eq. (2), page 2: conditional GAN objective conditions both G and D. Isola et al. 2017 Eq. (1), page 2: cGAN image-to-image objective.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Conditional adversarial real/fake training.

**I. Adaptation.** Uses raw prediction rather than blended output; uses softplus non-saturating loss and derived lambda.

**J. Constants not from paper.** Derived lambda and conditioning channel design.

**K. Safe Bab 2 citation.** "Conditional adversarial loss mengadaptasi conditional GAN dengan memberikan kondisi citra masukan kepada diskriminator dan mendorong output generator tampak real terhadap kondisi tersebut."

**L. Claims not allowed.** Do not say conditional adversarial uses blended output; source uses `raw_prediction`.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 10. Conditional Discriminator Loss

**A. Name and symbol.** Conditional PatchGAN discriminator loss, `L_D_cond`.

**B. Exact source.** Conditional v5 notebook, cell 22, source lines 711-722:

```python
condition = make_gan_condition(x, mask)
...
with torch.no_grad():
    raw_prediction = model(x)
    fake_detached = raw_prediction.detach()
real_logits = discriminator(condition, y)
fake_logits = discriminator(condition, fake_detached)
d_loss = 0.5 * (F.softplus(-real_logits).mean() + F.softplus(fake_logits).mean())
```

Condition and discriminator, cell 22, source lines 557-588:

```python
class ConditionalPatchDiscriminator(nn.Module):
    ...
    self.head = sn(nn.Conv2d(base * 4, 1, 3, padding=1))
...
def make_gan_condition(x, cloud_mask):
    cloudy_s2_t0 = x[:, 0:13]
    mask_t0 = cloud_mask
    sar_t0 = x[:, 56:58]
    condition = torch.cat([cloudy_s2_t0, mask_t0, sar_t0], dim=1)
```

**C. Uses.** Condition `c`, real target `y`, fake raw prediction `raw_prediction.detach()`, patch logits.

**D. Formula.** `L_D_cond = 0.5 [mean(softplus(-D(c,y))) + mean(softplus(D(c,raw.detach())))]`.

**E/F. Closest paper and formula.** Mirza and Osindero 2014 Eq. (2), page 2; Isola et al. 2017 Eq. (1), page 2. Patch discriminator concept from pix2pix Section 3.2.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Conditional discriminator and PatchGAN concept.

**I. Adaptation.** Condition is 16-channel satellite tensor: cloudy S2 t0, mask, SAR t0; fake is raw prediction.

**J. Constants not from paper.** 29 input channels, base=32, 3 conv blocks, softplus implementation.

**K. Safe Bab 2 citation.** "Diskriminator kondisional menggunakan prinsip PatchGAN untuk menilai real/fake pada patch lokal dengan kondisi citra masukan."

**L. Claims not allowed.** Do not claim fake input is blended; do not claim D outputs a single scalar.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 11. Feature Matching Loss

**A. Name and symbol.** Discriminator feature matching loss, `L_FM`.

**B. Exact source.** Conditional v5 notebook, cell 22, source lines 568-579 and 596-598:

```python
features = []
for block in self.blocks:
    z = block(z)
    features.append(z)
logits = self.head(z)
if return_features:
    return logits, features
...
def feature_matching_loss(fake_features, real_features):
    terms = [F.l1_loss(fake, real.detach()) for fake, real in zip(fake_features, real_features)]
    return torch.stack(terms).mean()
```

Generator loop, lines 730-736, obtains fake features with grad and real features under `torch.no_grad()`.

**C. Uses.** Discriminator intermediate feature maps from 3 blocks; fake features and detached/no-grad real features.

**D. Formula.** `L_FM = mean_{i=1..3} mean(|D_i(c, raw) - stopgrad(D_i(c,y))|)`.

**E/F. Closest paper and formula.** Wang et al. 2018 pix2pixHD, discriminator feature matching loss Eq. (4), page 4, averages L1 distances between discriminator intermediate activations for real and synthesized images.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** Matching discriminator intermediate activations with L1.

**I. Adaptation.** One conditional discriminator with 3 blocks; real features detached; mean across blocks.

**J. Constants not from paper.** 3 blocks, lambda `0.10122845103340689`.

**K. Safe Bab 2 citation.** "Feature matching loss mengadaptasi pix2pixHD dengan mencocokkan aktivasi fitur intermediate diskriminator antara citra target dan hasil generator."

**L. Claims not allowed.** Do not claim VGG perceptual loss; implementation uses discriminator features, not pretrained VGG.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 12. RGB SSIM Loss

**A. Name and symbol.** RGB SSIM loss, `L_SSIM`.

**B. Exact source.** Conditional v5 notebook, cell 22, source lines 542-554 and 623-642:

```python
def metric_safe_ssim_loss(pred, target, max_val=1.0):
    pred = torch.clamp(pred, 0.0, 1.0)
    target = torch.clamp(target, 0.0, 1.0)
    dims = (2, 3)
    c1 = (0.01 * max_val) ** 2
    c2 = (0.03 * max_val) ** 2
    mu_x = pred.mean(dim=dims, keepdim=True)
    mu_y = target.mean(dim=dims, keepdim=True)
    var_x = ((pred - mu_x) ** 2).mean(dim=dims, keepdim=True)
    var_y = ((target - mu_y) ** 2).mean(dim=dims, keepdim=True)
    cov_xy = ((pred - mu_x) * (target - mu_y)).mean(dim=dims, keepdim=True)
    ssim = ((2 * mu_x * mu_y + c1) * (2 * cov_xy + c2)) / ((mu_x ** 2 + mu_y ** 2 + c1) * (var_x + var_y + c2) + 1e-8)
    return 1.0 - ssim.mean()
...
ssim_term = metric_safe_ssim_loss(
    pred[:, RGB_BAND_INDICES], y[:, RGB_BAND_INDICES]
)
```

RGB indices, source line 502: `RGB_BAND_INDICES = (3, 2, 1)`.

**C. Uses.** Blended output RGB channels and target RGB channels.

**D. Formula.** `L_SSIM = 1 - mean(SSIM_global(pred_RGB, y_RGB))` with spatial/global per-channel statistics over H,W. No local window. No padding. `K1=0.01`, `K2=0.03`, `data_range=1.0`, channels `(3,2,1)`.

**E/F. Closest paper and formula.** Wang et al. 2004 SSIM, Eq. (13), page 604, defines luminance/contrast/structure combined SSIM with constants C1 and C2.

**G. Classification.** `ADAPTED_FROM_LITERATURE`.

**H. From paper.** SSIM algebraic form and constants K1/K2.

**I. Adaptation.** Global spatial statistics rather than local sliding window; RGB subset only.

**J. Constants not from paper.** RGB band selection `(3,2,1)`, epsilon `1e-8`, lambda `0.03431264440153357`.

**K. Safe Bab 2 citation.** "SSIM digunakan sebagai ukuran kemiripan struktural RGB, dengan implementasi penelitian memakai statistik global per kanal sebagai regularisasi tambahan."

**L. Claims not allowed.** Do not claim training SSIM uses an 11x11 Gaussian/local window; final source uses global H,W statistics.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS_WITH_CORRECTION_IF_WORD_SAYS_LOCAL_WINDOW`.

## 13. Spectral Normalization As Non-Loss Component

**A. Name and symbol.** Spectral normalization, `SN(W)`.

**B. Exact source.**

GAN ResNet v20 notebook, cell 23, source lines 78-91 and 99-112:

```python
from torch.nn.utils import spectral_norm
...
spectral_norm(nn.Conv2d(...))
...
self.fc = nn.Linear(base*4, 1)
```

Conditional v5 notebook, cell 22, source lines 557-566:

```python
sn = nn.utils.spectral_norm
self.blocks = nn.ModuleList([
    nn.Sequential(sn(nn.Conv2d(in_channels, base, 4, stride=2, padding=1)), ...),
...
self.head = sn(nn.Conv2d(base * 4, 1, 3, padding=1))
```

**C. Uses.** Discriminator layers only. No raw/blended/target loss tensor.

**D. Formula.** PyTorch spectral normalization constrains a module weight by normalizing with estimated largest singular value.

**E/F. Closest paper and formula.** Miyato et al. 2018, Eq. (4), page 3: `W_SN(W) = W / sigma(W)`, where `sigma(W)` is the spectral norm.

**G. Classification.** `DIRECT_FROM_LITERATURE` as a technique, `NON_LOSS_COMPONENT` in this project.

**H. From paper.** Weight spectral norm normalization for GAN discriminator stability.

**I. Adaptation.** Applied to custom ResNet-like and conditional PatchGAN discriminators.

**J. Constants not from paper.** Architecture-specific layer choices.

**K. Safe Bab 2 citation.** "Spectral normalization diterapkan pada diskriminator sebagai teknik stabilisasi GAN, bukan sebagai komponen loss tambahan."

**L. Claims not allowed.** Do not include spectral normalization as an additive term in `L_G` or `L_D`.

**M. Difference vs Word.** `NOT_VERIFIABLE_FROM_FINAL_SOURCE`.

**N. Status.** `PASS`.

## 14. Final Lambda Audit

**Shared reconstruction lambdas.** Verified from `PRETRAIN_CALIBRATION_CONFIG.json` lines 24-30:

- `lambda_global = 0.16666666666666666`
- `lambda_cloud = 0.04335288946625992`
- `lambda_raw = 0.01624632572134276`
- `lambda_preservation = 0.2893100685605546`
- `lambda_gradient = 0.7782590153407646`
- `lambda_heavy = 0.02010559125458582`

**GAN ResNet-like final adversarial lambda.** Verified from `GAN_RESNET_V20_DERIVED_CONFIG.json` lines 5-13:

- `T_resnet = 0.0015220244981312154`
- `g_adv = 0.01706953812390566`
- `lambda_adv = 0.08916612078680913`

**Conditional final lambdas.** Verified from `CONDITIONAL_V5_DERIVED_CONFIG.json` lines 5-11:

- `T_cond = 0.01126104729786551`
- `g_adv = 0.1419898346066475`
- `g_FM = 0.11124389618635178`
- `g_SSIM = 0.3281894326210022`
- `lambda_adv = 0.07930882748798064`
- `lambda_FM = 0.10122845103340689`
- `lambda_SSIM = 0.03431264440153357`

**Calibration provenance.** Verified from configs: training/validation/test were false for calibration; TRAIN deterministic batches were used for reconstruction calibration. This supports "derived-loss calibration", not historical fixed weights.

**Conflict.** `THREE_MODEL_LOSS_PROVENANCE_LOCK.md` reports GAN ResNet-like `lambda_adv = 0.008021420735937238`, which conflicts with final v20 clean output config `0.08916612078680913`. Required correction: update provenance/report narrative to final v20 clean value or explicitly label the lock file as stale for GAN ResNet-like.

## Required Verification Answers

- Global, Cloud, Preservation, Gradient, Heavy raw or blended: global/cloud/preservation/gradient/heavy use blended `pred`; raw loss uses raw prediction.
- Gradient signed or magnitude: baseline and conditional use signed finite differences with `0.5`; GAN ResNet v20 uses magnitude difference without `0.5`. Cross-model inconsistency.
- Gradient factor 0.5: yes for baseline/conditional; no for GAN ResNet v20.
- Heavy threshold/fallback: threshold `0.66`; fallback to `cloud_plain` when heavy bucket is NaN/empty.
- Factors 2.5 and 2.0: they are in `heavy_cloud_weight`, therefore affect weighted cloud loss and raw weighted loss, not the heavy-only bucket loss directly.
- ResNet BCE classifies: real/fake, not cloud/non-cloud.
- ResNet discriminator output: one logit, not patch map.
- Conditional discriminator raw or blended: raw prediction for fake.
- Conditional softplus: D loss `0.5*(softplus(-real).mean()+softplus(fake).mean())`; G adversarial `softplus(-fake).mean()`.
- Feature matching blocks/detach: 3 blocks; real features detached and also computed under `torch.no_grad()`.
- SSIM training local/global: global spatial statistics.
- SSIM window/padding/K/data/channel: no local window, no padding, K1=0.01, K2=0.03, data_range=1.0, RGB channels `(3,2,1)`.
- Historical notebook as source final: final derived notebooks/configs were used; old historical notebooks were not used as final source.
- Bobot final from derived-loss calibration: yes for verified configs; except stale provenance lock mismatch for GAN ResNet lambda requires correction.

## Final Summary Table

| loss | source paper | source equation | implementation formula | status | citation recommendation | required correction |
|---|---|---|---|---|---|---|
| Global L1 | Isola et al. 2017 | Eq. (2), p.2 | `mean(|pred-y|)` | PASS | cite as adapted L1 reconstruction | none |
| Weighted Cloud | Meraner et al. 2020 | exact equation not verified from accessible primary text | `sum(|pred-y|mw)/(sum(mw)C+eps)` | PASS_WITH_LIMITATION | cite as cloud-adaptive inspiration | do not claim exact CARL formula |
| Raw Masked | Isola et al. 2017 generic L1 only | Eq. (2), p.2 | `sum(|raw-y|mw)/(sum(mw)C+eps)` | PASS | describe as project-specific raw supervision | do not label as named paper loss |
| Preservation | Meraner et al. 2020; Pathak checked as non-direct | CARL exact not verified; Pathak masked reconstruction is not this | `sum(|pred-t0|(1-m))/(sum(1-m)C+eps)` | PASS_WITH_CORRECTION | cite CARL concept, not Pathak direct | remove direct Pathak preservation claim |
| Gradient | Mathieu et al. 2016 | GDL Eq. (8), p.4 | baseline/conditional signed `0.5*(MAE(dx)+MAE(dy))`; GAN ResNet magnitude no `0.5` | FAIL_FOR_CROSS_MODEL_CONSISTENCY | cite as adapted GDL | clarify/correct GAN ResNet gradient implementation |
| Heavy Cloud | no direct paper | none | heavy bucket MAE with fallback cloud_plain | PASS | call operational heavy-cloud term | do not attribute threshold to paper |
| GAN ResNet G adv | Goodfellow 2014 | Eq. (1), p.2; non-saturating variant discussed | `BCEWithLogits(D(pred),1)` | PASS_WITH_CORRECTION | cite GAN non-saturating principle | fix stale `lambda_adv` in lock/report |
| GAN ResNet D | Goodfellow 2014 | Eq. (1), p.2 | `0.5*(BCE(D(y),1)+BCE(D(pred.detach),0))` | PASS | cite real/fake discriminator | ensure text says one logit |
| Conditional G adv | Mirza and Osindero 2014; Isola 2017 | Mirza Eq. (2), p.2; Isola Eq. (1), p.2 | `mean(softplus(-D(c,raw)))` | PASS | cite conditional GAN / pix2pix | ensure text says raw fake |
| Conditional D | Mirza and Osindero 2014; Isola 2017 | Mirza Eq. (2), p.2; Isola Eq. (1), p.2 | `0.5*(softplus(-D(c,y))+softplus(D(c,raw.detach)))` | PASS | cite PatchGAN conditional discriminator | ensure text says patch map |
| Feature Matching | Wang et al. 2018 pix2pixHD | Eq. (4), p.4 | mean L1 over 3 D blocks, real detached | PASS | cite discriminator feature matching | do not call VGG perceptual |
| RGB SSIM | Wang et al. 2004 | Eq. (13), p.604 | `1-global_ssim(pred_RGB,y_RGB)` | PASS_WITH_CORRECTION_IF_WORD_SAYS_LOCAL | cite adapted SSIM | state no local window |
| Spectral Normalization | Miyato et al. 2018 | Eq. (4), p.3 | `spectral_norm` on D layers | PASS | cite as non-loss stabilizer | do not add to loss formula |
| Shared lambdas | no paper | operational calibration | `lambda_i=g_global/(6*g_i)` and aux `T/g` | PASS_WITH_CORRECTION | cite as derived calibration | fix stale GAN ResNet lambda in provenance lock |
