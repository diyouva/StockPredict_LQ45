# Index Stock Prediction

DESKRIPSI PROJECT

Background Permasalahan
Pada dunia pasar modal, tentunya banyak orang yang ingin mendapatkan keuntungan yang maksimal. Untuk mencapai tujuan tersebut, sebenarnya ada banyak metode analisa yang dapat dilakukan. Namun untuk mempelajari dan menerapkannya diperlukan waktu dan ketekunan yang konsisten setiap harinya. Hal itu relatif sulit bagi sebagian orang, terlebih lagi bagi mereka yang mempunyai rutinitas yang padat.

Pada project ini, dimaksudkan untuk mengatasi problematik tersebut. Project ini akan mencoba melakukan prediksi harga penutupan salah satu indeks saham yang cukup populer di kalangan dunia pasar modal, yaitu Indeks LQ45.

Nilai indeks LQ45 yang akan diprediksi adalah harga penutupan pada 3 hari ke depan. Pemilihan angka 3 hari didapatkan dari hasil analisa dari beberapa analis saham, yang mengatakan bahwa kejadian di T0 idealnya akan terasa efeknya hingga 3-4 hari ke depan.

Diharapkan dengan mengetahui harga penutupan pada 3 hari ke depan, User dapat melakukan tindakan yang tepat (beli atau jual) untuk memaksimalkan keuntungan mereka.

Arsitektur:

![image](https://github.com/diyouva/StockPredict_LQ45/assets/82955663/07651e66-222f-47ca-bc3b-e1ac14372da4)

Penjelasan:

1. Pada Proses prediksi (tanda panah warna biru dan merah) data input didapatkan dari User melalui API. Sedangkan untuk Proses retrain model (tanda panah warna hijau) data didapat dalam bentuk file csv.
2. Pada Proses prediksi, tahapan proses yang dilalui di dalam Sistem Machine Learning adalah: Validasi Data, Preprocessing Data & Feature Engineering, dan Predictor.
3. Predictor adalah output yang dihasilkan pada Proses retrain model setelah berhasil didapatkan Model Best-Fit.
4. Pada Proses retrain model, ada tambahan proses Splitting Data, Fit & Train Model.
5. Output dari Predictor akan dikembalikan ke User melalui API.

Output yang diharapkan: Prediksi harga penutupan Indeks LQ45 pada T+3.

Data:

1 Harga penutupan index LQ45 pada T+3 (hanya digunakan untuk pemodelan, namun tidak digunakan saat proses prediksi).
2 Harga penutupan Indeks IHSG pada T0.
3 Harga penutupan Indeks IDX-30 pada T0.
4 Harga penutupan Indeks EIDO pada T0.
5 Harga penutupan Indeks S&P 500 pada T0.
6 Jumlah nominal transaksi pembelian domestik pada T0.
7 Jumlah nominal transaksi penjualan domestik pada T0.
8 Jumlah nominal transaksi pembelian asing pada T0.
9 Jumlah nominal transaksi penjualan asing pada T0.
