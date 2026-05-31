# Dokumentasi Web Embed

Repositori ini berisi kode HTML, CSS, dan JavaScript untuk menyematkan (embed) grafik interaktif, ringkasan data langsung (live), dan korsel gambar (image carousels) ke dalam Google Sites. Proyek ini disusun ke dalam halaman-halaman spesifik termasuk pelacakan Penyu (Sea Turtle), pelacakan Pengelolaan Sampah (Waste Management), dan arsip historis.

## 📂 Struktur Folder
```text
JMImages
|   README-EN.md
|   README.md
|
+---2024
|
+---2025
|
+---2026
|
+---Archive2024
|       2025_ActivityGalerry.html
|       2025_BarChart.html
|
+---Archive2025
|       2024_ActivityGalerry.html
|       2024_BarChart.html
|
+---Clean
|
+---SeaTurtle
|       2026_SeaTurtleBarChart.html
|       2026_SeaTurtleGalerry.html
|       2026_SeaTurtleSummary.html
|
\---WasteManagement
        HeroesInAction.html
        TotalWasteCollected.html
        WeightAndIncome.html
```

---

## 📊 1. Live Updates via CSV Google Sheets

Halaman Sea Turtle menggunakan umpan data langsung dari CSV Google Sheets yang telah dipublikasikan untuk menjaga widget tetap mutakhir tanpa perlu mengedit kode secara manual.

**File Terkait:**

* [2026_SeaTurtleSummary.html](https://www.google.com/search?q=./SeaTurtle/2026_SeaTurtleSummary.html)
* [2026_SeaTurtleBarChart.html](https://www.google.com/search?q=./SeaTurtle/2026_SeaTurtleBarChart.html)

### Cara Kerja Impor Data

Baik penghitung Ringkasan (Summary counters) maupun Grafik Batang (Bar Charts) menggunakan API `fetch()` JavaScript untuk mengunduh data spreadsheet melalui tautan CSV yang dipublikasikan (lihat [Baris 106](https://www.google.com/search?q=./SeaTurtle/2026_SeaTurtleSummary.html), atau [Baris 85](https://www.google.com/search?q=./SeaTurtle/2026_SeaTurtleBarChart.html)).

* **Penghitung Ringkasan (Summary Counters):** Kode ini memecah teks CSV yang diunduh menjadi baris-baris, secara spesifik menargetkan baris ketiga (indeks `2`), dan mengekstrak nilai numerik untuk Sarang (Nests), Telur (Eggs), dan Tukik (Hatchlings) menggunakan fungsi bantuan `cleanNumber()` yang menghapus koma atau format teks lainnya. Angka-angka ini kemudian dimasukkan ke dalam atribut `data-turtle-target` untuk memicu animasi penghitungan.
* **Grafik Batang (Bar Charts):** Skrip grafik menyertakan fungsi `parseCSVLine` yang kuat, yang membaca baris dengan aman bahkan jika angkanya mengandung koma (misal, `"1,958"`). Skrip ini secara otomatis memindai baris tajuk/header (indeks `0`) untuk mencari kata kunci "Green" dan "Hawksbill" guna menentukan kolom mana yang akan diambil datanya, memberikan sistem cadangan (failsafe) jika posisi kolom bergeser. Skrip ini melakukan perulangan (loop) pada data selama 12 bulan, mengisi array JavaScript (`greenNests`, `hawksNests`, dll.), dan memasukkannya langsung ke dalam Chart.js.

---

## 🖼️ 2. Memperbarui Korsel Gambar (Image Carousels)

Korsel gambar menampilkan galeri 3 kotak yang berputar (looping) di desktop (dan 1 kotak di layar ponsel). Korsel ini mengandalkan gambar-gambar yang di-hosting di dalam folder GitHub `JMImages`.

**File Terkait:**

* [2024_ActivityGalerry.html](https://www.google.com/search?q=./Archive2025/2024_ActivityGalerry.html)
* [2025_ActivityGalerry.html](https://www.google.com/search?q=./Archive2024/2025_ActivityGalerry.html)
* [2026_SeaTurtleGalerry.html](https://www.google.com/search?q=./SeaTurtle/2026_SeaTurtleGalerry.html)
* [HeroesInAction.html](https://www.google.com/search?q=./WasteManagement/HeroesInAction.html)

### Cara Menambahkan atau Mengubah Gambar

Untuk mengubah gambar, Anda harus mengedit HTML secara langsung di dalam blok `<div class="carousel-track" id="imageTrack">`.

1. Unggah gambar baru Anda ke folder yang sesuai di dalam repositori GitHub `JMImages`.
2. Dapatkan URL **Raw** GitHub untuk setiap gambar. Anda dapat mengklik kanan pada gambar di GitHub dan menyalin URL-nya (copy URL).
3. Di dalam file HTML, temukan elemen-elemen `<div class="carousel-slide">`.
4. Ganti atribut `src` dengan tautan langsung (direct link) Anda yang baru. Tambahkan atau hapus blok ini untuk mengubah jumlah total gambar dalam rotasi:

```html
<div class="carousel-slide">
    <img src="[https://github.com/Kayynnn/JMImages/blob/main/2026/GAMBAR_BARU_ANDA.jpg?raw=true](https://github.com/Kayynnn/JMImages/blob/main/2026/GAMBAR_BARU_ANDA.jpg?raw=true)" alt="Gallery Image">
</div>


```

---

## 📈 3. Custom Bar Chart

Berbeda dengan grafik Sea Turtle 2026 yang bersifat langsung (live), file-file arsip berisi **data statis yang di-hardcode** yang tidak akan berubah, memastikan bahwa catatan historis tetap terjaga.

**File Terkait:**

* [2024_BarChart.html](https://www.google.com/search?q=./Archive2025/2024_BarChart.html)
* [2025_BarChart.html](https://www.google.com/search?q=Archive2024/2025_BarChart.html)

### Cara Mengedit Data Lama (Legacy Data)

Jika Anda perlu memperbaiki data historis di dalam arsip, Anda harus mengedit array JavaScript mentah di dalam tag `<script>`.

1. Buka file HTML arsip yang relevan.
2. Gulir ke bawah hingga menemukan variabel `nestConfig`, `eggConfig`, dan `hatchConfig`.
3. Temukan array `data: [...]` dan perbarui angka-angkanya secara manual. Pastikan angka-angka tersebut berurutan sesuai dengan array `monthLabels`.

Contoh dari [2024_BarChart.html](https://www.google.com/search?q=./Archive2025/2024_BarChart.html):

```javascript
const nestConfig = {
  type: 'bar',
  data: {
    labels: monthLabels, // ['Jan', 'Feb', 'Mar', ...]
    datasets: [
      { label: 'Green', data: [0, 2, 7, 21, 50, 68, 63, 63, 36, 20, 1, 0], backgroundColor: colorGreenTurtle, borderRadius: 4 },
      { label: 'Hawksbill', data: [0, 11, 22, 11, 12, 13, 2, 5, 2, 0, 0, 0], backgroundColor: colorHawksbill, borderRadius: 4 }
    ]
  },
  options: commonOptions
};


```

```

```