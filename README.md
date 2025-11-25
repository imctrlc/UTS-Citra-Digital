#  Proyek Segmentasi Citra dengan Algoritma Watershed
# Penerapan Algoritma Watershed untuk Segmentasi dan Pemisahan Objek yang Saling Bersentuhan

Ini adalah proyek demo sederhana dari mata kuliah **Pengolahan Citra Digital**.
Proyek ini bertujuan untuk mendemonstrasikan salah satu teknik segmentasi citra, yaitu **Algoritma Watershed**, menggunakan library OpenCV (`cv2`) dan Matplotlib di Python.

Program ini akan mengambil sebuah gambar input dari pengguna (dalam contoh ini, gambar mikroskopis sel darah), memprosesnya untuk menemukan objek yang saling bersentuhan/tumpang tindih, dan kemudian memisahkannya secara visual dengan menggambar garis batas berwarna merah.

---

##  Fungsi & Tujuan Proyek

Fungsi utama dari program ini adalah untuk **memisahkan objek-objek yang saling bersentuhan** dalam sebuah citra.

Dalam banyak kasus segmentasi sederhana (seperti *thresholding*), jika dua objek (misalnya dua **sel darah**) saling menempel atau berkelompok, komputer akan melihatnya sebagai satu objek besar. Ini menjadi masalah besar dalam aplikasi *bio-imaging* di mana penghitungan jumlah sel (cell counting) yang akurat sangat penting.

Algoritma Watershed adalah teknik yang canggih untuk mengatasi masalah ini. Ia bekerja dengan menganalogikan citra sebagai peta topografi (dataran dengan bukit dan lembah). Ia kemudian "membanjiri" lembah-lembah (objek/sel) dari titik-titik "marker" yang sudah kita tentukan. Ketika air dari dua lembah yang berbeda bertemu, algoritma akan membangun "bendungan" (garis batas) di antara keduanya.

Hasil akhirnya adalah citra di mana setiap sel teridentifikasi secara terpisah, memungkinkan untuk penghitungan yang lebih akurat.

---

##  Contoh Hasil

Berikut adalah contoh hasil akhir dari program saat memproses gambar sel darah. Garis batas berwarna merah menunjukkan di mana algoritma berhasil memisahkan sel-sel yang tumpang tindih atau berkelompok.


![Contoh Hasil Segmentasi Sel Darah](https://drive.google.com/file/d/1IAUy5Da-CX15fqrSaq9QURtxGLNOEh9n/view?usp=sharing)

---

##  Cara Kerja Program (Alur Logika)

Program ini bekerja melalui beberapa tahapan utama:

1.  **Muat Gambar**: Membaca file gambar (`cv2.imread`) dari *drive* lokal Anda.
2.  **Preprocessing**:
    * Mengubah gambar menjadi *grayscale* (skala abu-abu).
    * Menerapkan **Gaussian Blur** untuk mengurangi *noise* dan mencegah *over-segmentation* (terlalu banyak segmen kecil yang tidak perlu).
3.  **Thresholding**:
    * Menggunakan **Otsu's Thresholding** untuk secara otomatis memisahkan *foreground* (sel) dari *background* (latar belakang).
    * Hasilnya dibalik (`THRESH_BINARY_INV`) sehingga sel menjadi hitam dan *background* menjadi putih.
4.  **Penentuan Marker (Langkah Kunci)**:
    * **Mencari Pasti Background**: Menggunakan operasi morfologi `cv2.dilate` untuk memperluas area *background*. Area ini 100% kita yakini sebagai *background*.
    * **Mencari Pasti Foreground**: Menggunakan `cv2.distanceTransform` untuk menemukan "pusat" dari setiap sel. Area ini 100% kita yakini sebagai "inti" sel.
    * **Mencari Area Tidak Dikenal**: Area sisa di antara *pasti background* dan *pasti foreground*. Ini adalah area perbatasan tempat Watershed akan bekerja.
5.  **Pelabelan**:
    * Memberi label angka unik pada setiap area *pasti foreground* (misal: **sel 1 = label 1, sel 2 = label 2**, dst.).
    * Menandai area yang tidak dikenal dengan label `0`.
6.  **Eksekusi Watershed**:
    * Menjalankan algoritma `cv2.watershed()` pada gambar asli berwarna dengan menggunakan `markers` yang sudah kita buat.
    * Algoritma akan mengisi "lembah" dari *marker* dan menandai garis pertemuan (bendungan) dengan nilai `-1`.
7.  **Visualisasi**:
    * Menggambar garis **merah** pada gambar asli di semua piksel yang bernilai `-1` (batas sel).
    * Menampilkan semua langkah proses (gambar asli, *threshold*, *distance transform*, *marker*, dan hasil akhir) menggunakan `matplotlib`.

---

##  Kebutuhan & Instalasi

Proyek ini dibuat dengan **Python 3**. Anda memerlukan beberapa *library* Python utama untuk menjalankannya.

Anda dapat menginstal semua *library* yang diperlukan dengan satu perintah menggunakan `pip`:

```bash
pip install opencv-python numpy matplotlib



