# Conflict Log

Konflik dicatat tanpa perubahan diam-diam terhadap sumber.

## CONFLICT-01
Topik: Sumber historis GAN ResNet-like
Sumber A: Snapshot kanonik lokal berstatus VERIFIED_REFERENCED.
Sumber B: scriptVersionId Kaggle 331684678 tidak dapat diambil secara independen dari sumber lokal.
Dampak: Kepastian provenance source GAN ResNet lebih rendah daripada dua model lain.
Rekomendasi: Pertahankan label VERIFIED_REFERENCED dan jangan menyebut snapshot sebagai salinan historis identik.
Status: OPEN

## CONFLICT-02
Topik: Rumus loss laporan dan kode final
Sumber A: Laporan Word memakai beberapa notasi ringkas.
Sumber B: Kode final memakai weighted denominator, preservation/gradient/heavy pada blended output, gradient signed dengan faktor 0.5, dan SSIM statistik global.
Dampak: Rumus laporan dapat dibaca berbeda dari operasi aktual.
Rekomendasi: Gunakan formula kode dalam pembahasan implementasi dan revisi narasi laporan secara terpisah.
Status: OPEN

## CONFLICT-03
Topik: Semantik discriminator
Sumber A: ResNet D: 13 kanal, fake blended, satu logit, BCE.
Sumber B: Conditional D: condition+raw 29 kanal, patch logits, softplus.
Dampak: Penggabungan narasi akan menukar dua eksperimen.
Rekomendasi: Pisahkan model secara eksplisit.
Status: RESOLVED_IN_APPENDIX

## CONFLICT-04
Topik: Uji statistik lama dan final
Sumber A: Paket lama memuat paired t-test dua model.
Sumber B: Sumber final memuat Friedman tiga model dan Wilcoxon-Holm.
Dampak: Kesimpulan statistik lama tidak mewakili perbandingan akhir.
Rekomendasi: Gunakan hanya Friedman/Wilcoxon-Holm final.
Status: RESOLVED_IN_APPENDIX

## CONFLICT-05
Topik: Jumlah sampel mask-valid
Sumber A: Evaluator primer menyimpan 141 sampel.
Sumber B: Re-threshold NPZ float16 dalam audit menghasilkan 140.
Dampak: Satu sampel berada dekat batas threshold.
Rekomendasi: Gunakan 141 untuk hasil primer; catat 140 hanya sebagai sensitivitas representasi.
Status: OPEN

## CONFLICT-06
Topik: Threshold awan
Sumber A: Heavy pixel loss memakai M>=0.66 dan heavy sample training >=0.30.
Sumber B: Evaluasi mask-only memakai threshold final 0.5; visual high-cloud memakai ranking coverage.
Dampak: Istilah heavy dapat merujuk unit berbeda.
Rekomendasi: Sebutkan threshold dan unit setiap kali.
Status: RESOLVED_IN_APPENDIX

## CONFLICT-07
Topik: Output komponen loss per batch
Sumber A: Notebook menghitung komponen loss.
Sumber B: Tidak ada log final yang menyimpan semuanya untuk satu batch.
Dampak: H.10 tidak dapat diisi angka aktual.
Rekomendasi: Tulis NOT AVAILABLE; jangan menghitung ulang.
Status: OPEN

## CONFLICT-08
Topik: Environment per-run
Sumber A: Log environment umum tersedia.
Sumber B: Log run-specific lengkap untuk ketiga model tidak tersedia.
Dampak: Kesetaraan runtime tidak dapat dibuktikan penuh.
Rekomendasi: Batasi klaim pada environment yang tersimpan.
Status: OPEN

## CONFLICT-09
Topik: Nama laporan
Sumber A: Instruksi menyebut fixed(2).docx.
Sumber B: File terbaru lokal bernama Laporan dari Penyuka Idol Banyak Makan Revise fixed.docx.
Dampak: Nama berbeda, isi terbaru dipakai sebagai referensi read-only.
Rekomendasi: Catat hash dan jangan rename.
Status: RESOLVED_IN_APPENDIX

## CONFLICT-10
Topik: Terminologi probability
Sumber A: Beberapa nama fungsi/file memakai cloud_probability.
Sumber B: Makna metodologis adalah skor indikasi awan heuristik.
Dampak: Istilah probabilitas terkalibrasi akan berlebihan.
Rekomendasi: Gunakan 'skor indikasi awan' pada narasi.
Status: RESOLVED_IN_APPENDIX

## CONFLICT-11
Topik: Kontribusi loss individual
Sumber A: Loss gabungan digunakan dalam training.
Sumber B: Tidak ditemukan ablation study final.
Dampak: Kontribusi individual tidak dapat diatribusikan kausal.
Rekomendasi: Jangan mengklaim tiap loss meningkatkan metrik secara individual.
Status: OPEN

## CONFLICT-12
Topik: Warm-start
Sumber A: Conditional dimulai dari checkpoint Baseline.
Sumber B: GAN ResNet-like tidak memiliki bukti warm-start yang sama.
Dampak: Generalization narasi warm-start ke semua GAN salah.
Rekomendasi: Batasi warm-start pada Conditional.
Status: RESOLVED_IN_APPENDIX

## CONFLICT-13
Topik: Label pemilihan sampel
Sumber A: BAB4 final mendefinisikan improvement Conditional vs Baseline.
Sumber B: selected_samples all-three memakai beberapa label dengan kriteria berbeda, termasuk raw-vs-blended Conditional.
Dampak: Label serupa tidak selalu memiliki definisi sama.
Rekomendasi: Gunakan BAB4 untuk best/worst formal dan beri konteks pada label all-three.
Status: OPEN
