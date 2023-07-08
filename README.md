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

Workflow Project (Proses Retrain Model)

Berikut workflow untuk Proses Retrain Model, setelah melalui beberapa kali proses trial-error:

1. Pengambilan data dari file csv dengan format yang sudah sesuai.
2. Validasi data untuk memastikan bahwa format sudah sesuai dengan ketentuan.
3. Resampling data dengan interval hari kerja (senin, selasa, rabu, kamis, jumat), sekaligus imputasi data point yang hilang dengan metode ffill.
4. Membentuk kolom target yang dibentuk dari kolom lq45 yang di-shift-forward sejauh 3 data point.
5. Penambahan 4 fitur baru (dom_tot,dom_net,for_tot,for_net) yang bertujuan untuk memudahkan model dalam melakukan training.
    > dom_tot merupakan kolom dom_b dijumlahkan dengan kolom dom_s.
    
    > dom_net merupakan kolom dom_b dikurangkan dengan kolom dom_s.
    
    > for_tot merupakan kolom for_b dijumlahkan dengan kolom for_s.
    
    > for_net merupakan kolom for_b dikurangkan dengan kolom for_s.
7. Splitting data dengan komposisi:
    >data train adalah data sejak awal hingga 31-12-2020.
    
    >data validasi adalah data sejak 01-01-2021 hingga 31-07-2021.
    
    >data test adalah data sejak 01-08-2021 hingga data terakhir.
8. Penambahan fitur seasonal yang mempresentasikan trend dari kolom target dalam interval bulanan.
9. Mendeteksi data outlier pada semua kolom dengan metode 1.5 x IQR, serta melakukan imputasi dengan nilai percentile 10% untuk outlier bawah, dan percentile 90% untuk outlier atas.
10. Proses scaling standardisasi untuk semua kolom, kecuali kolom target.
11. Proses pelatihan model dengan pustaka pmdarima, yang secara otomatis dapat menentukan parameter-parameter model SARIMAX yang terbaik.
12. Evaluasi model dilakukan pada data validasi dan data test dengan cara membandingkan hasil prediksi pred dengan kolom target.

Berikut ilustrasinya,

![image](https://github.com/diyouva/StockPredict_LQ45/assets/82955663/b551b4e6-6d6c-458f-a2cd-67fea4483ba3)

Workflow (Proses Prediksi)

Berikut workflow untuk Proses Prediksi:

1. Penerimaan data dari User melalui API dengan format yang sudah sesuai.
2. Validasi data untuk memastikan bahwa format sudah sesuai dengan ketentuan.
3. Proses konversi data kedalam bentuk Pandas Dataframe.
4. Penambahan 4 fitur baru (dom_tot,dom_net,for_tot,for_net) yang bertujuan untuk memudahkan model dalam melakukan training.
    >dom_tot merupakan kolom dom_b dijumlahkan dengan kolom dom_s.

    >dom_net merupakan kolom dom_b dikurangkan dengan kolom dom_s.
    
    >for_tot merupakan kolom for_b dijumlahkan dengan kolom for_s.
    
    >for_net merupakan kolom for_b dikurangkan dengan kolom for_s.
5. Penambahan fitur seasonal dari pickle dataframe yang sebelumnya dibuat saat proses pemodelan (file: [root]\data\remodel\value_for_seasonal.pkl).
6. Mendeteksi data outlier pada semua kolom, serta melakukan imputasi dengan nilai dari pickle dataframe yang sebelumnya dibuat saat proses pemodelan (file: [root]\data\remodel\value_for_outlier.pkl).
7. Proses scaling standardisasi untuk semua kolom menggunakan pickle scaler yang sebelumnya dibuat saat proses pemodelan (file: [root]\data\remodel\scaler_x.pkl).
8. Proses prediksi menggunakan model yang sebelumnya dibuat saat proses pemodelan (file: [root]\models\sarimax.pkl).
9. Pengiriman hasil prediksi kembali ke User melalui API.

Berikut ilustrasinya,

![image](https://github.com/diyouva/StockPredict_LQ45/assets/82955663/d9cbec72-afb7-4c1b-9409-9ec33a6cacd1)



