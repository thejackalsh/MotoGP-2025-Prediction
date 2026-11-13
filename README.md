<div align="center">

<br>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/assets/header-dark.svg">
  <img src=".github/assets/header-light.svg" alt="MotoGP 2025 Prediction, proyek data mining" width="800">
</picture>

<br><br>

Memprediksi juara dunia MotoGP 2025 dari data musim 2024 dengan Decision Tree.

</div>

---

### ◼ TENTANG

Proyek UAS mata kuliah **Data Mining**. Data balapan musim 2024 (Sprint dan Race) dibersihkan, dianalisis, lalu dipakai untuk memprediksi juara dunia 2025 dari pembalap yang ada di susunan tim 2025.

Seluruh pekerjaan ada di satu notebook: [`MotoGP 2025 Prediction.ipynb`](MotoGP%202025%20Prediction.ipynb).

---

### ◼ HASIL

Hasil prediksi dibandingkan dengan klasemen akhir MotoGP 2025 yang sudah selesai.

<div align="center">

| Posisi | Prediksi | Aktual |
|:--:|:--|:--|
| 🥇 **1** | F. Bagnaia (Ducati Lenovo Team) | **Marc Márquez** (Ducati Lenovo Team) |
| 🥈 **2** | J. Martin (Aprilia Racing) | **Alex Márquez** (Gresini Racing) |
| 🥉 **3** | M. Marquez (Ducati Lenovo Team) | **Marco Bezzecchi** (Aprilia Racing) |

</div>

**Prediksi.** Model di notebook hanya menghasilkan **satu** pembalap, yaitu juara: **F. Bagnaia** dengan 370 poin Race pada 2024. Kolom prediksi posisi 2 dan 3 adalah pembalap di susunan tim 2025 dengan poin Race 2024 tertinggi berikutnya, yaitu data yang sama dengan grafik perbandingan di notebook (J. Martin 337, M. Marquez 270).

**Aktual.** Marc Márquez juara dunia 2025 bersama Ducati Lenovo Team, disusul saudaranya Alex Márquez di posisi kedua dan Marco Bezzecchi bersama Aprilia di posisi ketiga. Sumber: [klasemen akhir Valencia di Crash.net](https://www.crash.net/motogp/results/1086801/1/valencia-final-2025-motogp-world-championship-standings) dan [Corsedimoto](https://www.corsedimoto.com/en/motogp/2025-motogp-world-championship-final-riders-standings-after-valencia).

**Kesimpulan.** Juara yang diprediksi tidak sama dengan juara sebenarnya. Dari enam nama di tabel, hanya **Marc Márquez** muncul di kedua kolom: diprediksi urutan 3, hasil aktualnya juara.

---

### ◼ DATASET

Dua berkas Excel dengan struktur sama. `dataset_motogp_updated.xlsx` adalah versi yang dipakai notebook.

| Sheet | Isi | Baris |
|:--|:--|--:|
| `Sprint` | Hasil sprint race 2024 per pembalap | 450 |
| `Race` | Hasil race utama 2024 per pembalap | 446 |
| `Line Up 2024` | Pembalap, tim, dan pabrikan 2024 | 28 |
| `Line Up 2025` | Pembalap, tim, dan pabrikan 2025 | 22 |

- **Cakupan:** 20 seri balap, 29 pembalap, musim 2024
- **Kolom hasil:** `pos.`, `pts`, `no`, `Rider`, `Team`, `time / gap`, `session`, `event`, `year`, `Constructor`
- Kolom `Constructor` yang kosong diisi dari sheet `Line Up 2024`
- Posisi `DNF` diberi nilai 25 saat dirata-rata

---

### ◼ METODE

1. **Persiapan data:** membaca sheet Sprint, Race, dan Line Up, lalu melengkapi pabrikan
2. **EDA:** rata-rata poin per tim, 5 pembalap dengan poin terbanyak, dan 5 pembalap yang paling sering finis di posisi 1 sampai 5, terpisah untuk Sprint dan Race
3. **Visualisasi:** posisi finis tiap pembalap di setiap seri, warna mengikuti warna tim
4. **Klasifikasi performa:** poin dikelompokkan menjadi `Excellent` (25 ke atas), `Good` (15 ke atas), `Fair` (5 ke atas), dan `Poor`
5. **Decision Tree untuk nama pembalap:** fitur `pos.`, `pts`, `session`, `year`, dengan GridSearchCV 5 lipatan. Akurasi **10%**.
6. **Prediksi juara 2025:** Decision Tree (CART) dengan GridSearchCV 3 lipatan dan pembagian data 70/30

Untuk langkah 6, data Race 2024 diringkas per pembalap menjadi total poin, rata-rata posisi, dan jumlah sesi, lalu hanya pembalap yang ada di susunan 2025 yang dipertahankan. Target `Champion` bernilai 1 untuk pembalap dengan poin tertinggi.

---

### ◼ KETERBATASAN

Catatan agar hasil dibaca dengan tepat:

- **Akurasi 1.0 pada langkah 6 tidak berarti model pandai memprediksi.** Target `Champion` ditentukan dari `pts` (poin tertinggi), sedangkan `pts` juga menjadi fitur. Data uji hanya 6 baris. Praktisnya hasil prediksi sama dengan "pembalap dengan poin Race 2024 terbanyak".
- **Perubahan 2025 tidak terwakili.** Data hanya dari 2024, sedangkan pada 2025 sejumlah pembalap pindah tim dan motor, termasuk Marc Márquez yang naik ke tim pabrikan Ducati.
- **Lima pembalap susunan 2025 tidak ikut diprediksi.** F. Aldeguer, A. Ogura, dan S. Chantra tidak punya data 2024 di dataset. M. Vinales dan M. Olivera sebenarnya punya data, tetapi ejaan namanya berbeda antara sheet `Line Up 2025` dan hasil balapan (`M. Viñales`, `M. Oliveira`), jadi tidak cocok saat disaring dan ikut terbuang. Ini kesalahan pembersihan data.
- **Sprint tidak dipakai** pada prediksi akhir, hanya data Race.
- **Filter di EDA Race terlalu ketat.** Filter `pts <= 12` yang cocok untuk Sprint juga dipakai untuk Race, sehingga 80 baris (empat finis teratas di tiap seri, poin 13 sampai 25) ikut terbuang. Karena itu daftar "5 pembalap dengan poin terbanyak di Race" pada EDA lebih rendah dari kenyataan.
- **Ada nilai poin tidak wajar di sheet Sprint** (16 baris dengan poin di atas 12, maksimum 120). Filter yang sama di EDA menyingkirkannya.

---

### ◼ STACK

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/assets/stack-dark.svg">
  <img src=".github/assets/stack-light.svg" alt="Stack. Language: Python. Data: pandas, NumPy, scikit-learn. Notebook: Jupyter, Google Colab." width="800">
</picture>

</div>

Visualisasi memakai matplotlib dan seaborn, dan pencocokan nama tim memakai RapidFuzz.

---

### ◼ MENJALANKAN

Notebook ditulis di **Google Colab**, jadi jalur berkas masih menunjuk ke Google Drive (`/content/drive/MyDrive/UAS Datamin/...`). Untuk menjalankan di tempat lain, ubah `file_path` di setiap sel agar menunjuk ke berkas Excel di folder ini.

```bash
pip install pandas openpyxl matplotlib seaborn scikit-learn rapidfuzz tabulate jupyter
jupyter notebook "MotoGP 2025 Prediction.ipynb"
```

> [!NOTE]
> `requirment.txt` berisi daftar `import` dari notebook, bukan daftar paket yang bisa dipasang dengan `pip install -r`. Gunakan perintah `pip install` di atas.

---

### ◼ STRUKTUR

```text
MotoGP 2025 Prediction.ipynb   notebook utama: EDA, klasifikasi, prediksi
dataset_motogp_updated.xlsx    dataset yang dipakai notebook
dataset motogp 2024.xlsx       dataset awal
requirment.txt                 daftar import notebook
```

---

<div align="center">

<sub>Proyek UAS Data Mining. MotoGP adalah merek dagang pemiliknya. Logo diambil dari <a href="https://commons.wikimedia.org/wiki/File:MotoGP_logo_(2024).svg">Wikimedia Commons</a> (domain publik, bermerek dagang), dan proyek ini tidak berafiliasi dengan MotoGP.</sub>

</div>
