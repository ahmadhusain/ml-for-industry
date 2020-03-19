# Finance



## Credit Risk Analysis 

### Background

Credit scoring membutuhkan berbagai data profil calon peminjam sehingga tingkat resiko dapat dihitung dengan tepat. Semakin benar dan lengkap data yang disediakan, maka semakin akurat perhitungan yang dilakukan. Proses tersebut tentunya merupakan hal yang baik, namun di sisi calon peminjam proses yang harus dilalui dirasa sangat merepotkan dan membutuhkan waktu untuk menunggu. Dan seiring tingkat kompetisi yang samkin tinggi di industri finansial, customer memiliki
banyak alternatif. Semakin cepat proses yang ditawarkan, semakin tinggi kesempatan untuk mendapatkan peminjam.

Tantangan pun muncul, bagaimana mendapatkan pelanggan dengan proses yang efisien namun akurasi dari credit scoring tetap tinggi. Disinilah machine learning dapat membantu menganalisa datadata profil peminjam dan proses pembayaran sehingga dapat mengeluarkan rekomendasi profil pelanggan yang beresiko rendah.

### Expected Outcome

Harapannya setelah mempunyai model machine learning dengan perfomance model yang baik, pegawai bank dapat dengan mudah mengidentifikasi karakteristik customer yang memiliki peluang besar untuk melunasi pinjaman dengan lancar. Dengan adanya model machine learning ini tentunya akan mengurangi biaya dan waktu yang lebih cepat.

### Recommendation


```
#>           Model  Evaluation Estimate
#> 1 Random Forest    Accurary    0.843
#> 2 Random Forest Specificity    0.898
#> 3 Random Forest   Precision    0.822
#> 4 Random Forest      Recall    0.755
```


Model machine learning untuk memprediksi kredit pinjaman customer yang lancar dan tidak lancar memiliki perfomance model yang cukup baik. Nantinya, pegawai bank dapat menggunakan model tersebut dengan mengisikan data pribadi setiap customer, kemudian hasil yang diperoleh dapat di visualisasikan sebagai berikut:


<img src="assets/02-finance/lime.png" width="60%" style="display: block; margin: auto;" />


Hasil visualisasi tersebut adalah contoh prediksi salah satu customer, customer tersebut terprediksi no yang memiliki arti customer tersebut berpeluang besar sebagai customer yang gagal bayar. Tentunya ketika hasil prediksi menyatakan customer tersebut berpeluang besar untuk gagal bayar, artinya bank tidak akan memberikan pinjaman kepada customer tersebut. Dari hasil visual tersebut juga ditunjukkan variabel mana yang support dan contradicts terhadap hasil prediksi yang dihasilkan.
