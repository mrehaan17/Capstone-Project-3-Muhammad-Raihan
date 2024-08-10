# E-commerce Customer Churn
### Created by : Muhammad Raihan

# Business Problem Understanding
Perusahaan e-commerce di Indonesia saat ini sangat banyak, sehingga persaingannya pun sangat ketat. Ada banyak hal yang dapat mempengaruhi customer untuk berpindah platform ketika berbelanja sesuatu, salah satunya harga. Salah satu perusahaan online retail (E-commerce) ingin mengetahui/memprediksi customer mereka yang berpotensi untuk berpindah platform. Tujuannya adalah agar customer tersebut bisa diberikan **Promo Khusus** sehingga tidak berpindah platform.

Dari data yang dimiliki, tim DS diminta untuk memprediksi customer dengan berpotensi untuk berpindah platform dengan menggunakan data historical. Hal ini bertujuan juga agar perusahaan memberikan budget promo lebih secara tepat sasaran. Berikut merupakan klasifikasi target dari data yang ada :

Target :
- 0 : Customer **masih** menggunakan layanan
- 1 : Customer **berhenti** menggunakan layanan

Hal ini menunjukan bahwa kita akan melakukan model **Supervised Learning Classification**

# Problem Understanding
Perusahaan e-commerce ingin mengeluarkan budget promosi yang cukup besar. Tetapi, mereka ingin menggunakannya ke orang yang tepat sasaran (Customer ingin berpindah platform). Jika budget tersebut dikeluarkan ke semua customer, maka budget yang dibutuhkan akan sangat besar dan tidak ada "rasa special" bagi customer yang berpotensi berpindah platform. Dataset yang akan digunakan memiliki

# Goals
Maka berdasarkan masalah tersebut, perusahaan ingin memprediksi customer mana saja yang berpotensi pindah ke platform competitor. Perusahaan juga ingin mengetahui variabel atau hal apa saja yang mempengaruhi customer tersebut berpindah platform. Hasil dari prediksi ini akan menjadikan landasan perusahaan dalam pengeluaran budget promo terhadap customer churn

# Analytic approach
Membuat sebuah model yang dapat mempelajari history customer churn dan dapat memprediksi customer existing yang berpotensi untuk melakukan churn.

# Metric Evaluation
- True Positive : Customer yang melakukan churn dan berhasil diprediksi
- False Positive : Customer yang tidak melakukan churn, tetapi kita prediksi churn
- True Negative : Customer yang tidak melakukan churn dan berhasil diprediksi
- False Negative : Customer yang melakukan churn tetapi kita salah prediksi

Jika kita berfokus pada 4 parameter ini, maka metric evaluasi yang digunakan adalah :
1. Accuracy : Untuk melihat seberapa tepat model memprediksi customer yang melakukan churn dibandingkan dengan total prediksi yang dilakukan oleh model.
2. Precision : Untuk melihat seberapa tepat model memprediksi customer yang akan churn. Tujuannya agar tidak salah prediksi dan menggunakan budget secara sia-sia.
3. Recall : Untuk melihat seberapa tepat model dalam memprediski customer yang akan churn dibandingkan dengan semua customer yang akan churn. Hal ini bertujuan untuk melihat akurasi model kita dalam membaca semua customer churn.
4. F1-Score : Menggabungkan data precision dan recall sehingga dapat melihat baiknya model secara menyeluruh

Tetapi, dari ke-4 data tersebut. Karena kebutuhan perusahaan adalah agar penggunaan budget promo diberikan ke orang yang tepat (Churn), maka kita akan berfokus pada **Recall**. Hal ini dikarenakan parameter ini akan membandingkan semua customer yang Churn

# Proses Pembuatan Model
1. Data Understanding
   Pada tahap ini, saya melakukan pemahaman terhadap jenis dataset yang akan digunakan. Summarynya adalah :
   - Data memiliki 11 Kolom dan 3941 Baris
   - Dari 11 kolom, 2 kolom merupakan datatype text/string dan 9 kolom numerik
   - Dari 11 kolom, terdapat 10 kolom merupakan feature dan 1 kolom merupakan target
   - Terdapat 3 Kolom yang memiliki missing values, yaitu `Tenure` dengan **194 data (4.92%)**, `WarehouseToHome` dengan **169 data (4.29%)**, dan `DaySinceLastOrder` dengan **213 data (5.40%)**
   - Semua missing values akan di drop karena nilainya dibawah 6%
   - Dataset merupakan imbalance dataset, karena terdapat 84.84% data dengan klasifikasi **0 atau tidak churn** dan 15.16% nya adlaah **1 atau churn**
   - Saya melakukan feature importance, yang hasilnya feature `WarehouseToHome`, `PreferedOrderCat`, `SatisfactionScore`, `NumberOfAddress`akan di takeout karena tidak memiliki korelasi yang cukup besar terhadap target

2. Pembuatan model machine learning
   Pada tahap ini, saya melakukan pembuatan model dengan tahapan :
   - Mencari basemodel : hasilnya adalah model dengan algorithm DecissionTreeClassifier memiliki nilai metriks yang paling baik
   - Membuat model dengan balancing data (model panalized): hasilnya adalah model menjadi lebih baik nilai metriksnya ketika ditambahkan parameter **class weight**
   - Membuat model dengan balancing data (model oversampling) : hasilnya model dengan metode oversampling tidak lebih baik dari model panalized
   - Melakukan hyperpameter tunning pada model panalized : hasilnya metriks tidak lebih baik dari sebelumnya yang hanya menggunakan parameter **class weight** (sisanya default)
  
3. Conlusion & Recommendation
   **Conlusion** : 
    * Model dapat memprediksi 90.69% customer churn dari total semua kasus churn atau tidak yang dilakukan oleh model yang kita buat (Accuracy).
    * Model dapat melakukan  81.45%% prediksi customer churn dibandingkan dengan semua customer yang melakukan churn. Dengan kata lain, jika terdapat 100 customer churn, model dapat memprediksi sebanyak 81 orang. Hal ini dapat menjadi landasan perusahaan agar tepat 
      mengeluarkan budget ke customer yang tepat (Recall)
    * Model dapat melakukan 84.51% prediksi secara benar baik customer tersebut melakukan churn atau tidak churn. Dengan kata lain, jika terdapat 100 customer (baik churn atau tidak), model dapat memprediksi 84 orang secara benar (baik churn atau tidak)
    * Model menunjukan keseimbangan sebesar 82.84% antara recall maupun precision (f1-score). Model memiliki keseimbangan yang baik antara kemampuan untuk mendeteksi churn dan menghindari prediksi palsu/salah.
  
   Jika kita berfokus dalam penghematan budget agar semua budget dikeluarkan ke customer yang tepat (yang benar melakukan Churn), maka kita harus fokus pada 2 parameter yaitu Recall & Precision. Berikut penjelasannya :
    1. Recall = Berarti kita fokus terhadap model agar dapat memprediksi sebanyak-banyaknya customer yang melakukan churn
    2. Precision = Berarti kita fokus agar menghindari prediksi terhadap pelanggan yang tidak melakukan churn. Hal ini bertujuan agar budget yang kita keluarkan tepat sasaran.

   **Perhitungan Budget**
   Bila seandainya budget yang dimiliki perusahaan untuk alokasi khusus Customer Churn agar tidak melakukan churn adalah 250.000/Customer. Dan total customer yang berpotensi melakukan churn adalah 2.000 customer. Maka perhitungan penghematan budgetnya akan seperti ini :
    
    * Total Budget yang dibutuhkan jika **tanpa model** = 250.000 x 2.000 = 500.000.000
    
    Dari total 2.000 customer, maka :
    
    * Jumlah pelanggan yang diprediksi churn (dengan presisi 81.45%) = 2.000 × 81.45% = 1.628 pelanggan.
    * Dari 1.628 pelanggan yang diprediksi churn, 84.51% benar-benar churn, yaitu: 1.628 × 84.51% ≈ 1.375 pelanggan churn sebenarnya.
    * Jadi, 1.628 - 1.375 = 253 pelanggan adalah false positives.
    
    Jadi, penghematan yang dapat dilakukan dengan model ini adalah :
    
    * Penghematan = False Positives × Biaya per Pelanggan
      Pengehematan = 253 x 250.000 = **63.250.000**
    
    * Total Budget yang dibutuhkan **dengan model** = 500.000.000 - 63.250.000 = **456.750.000**
    * Presentase penghematan budget **dengan model** = (63.250.000/500.000.000)x100 = **12.65%**
      
   Berdasarkan perhitungan diatas, dapat dilihat bahwa model kita dapat menghemat budget sekitar 12.65%. Hal ini menunjukan bahwa ketika kita menggunakan model, perusahaan dapat mengurangi False Positive yang akan menghemat 12.65% budget yang mereka miliki.

   **Recommendation** :
    1. Membuat kebijakan kepada perusahaan untuk mengisi data-data yang memiliki korelasi terhadap customer churn sebagai *kewajiban*. Hal ini agar mengurangi missing values pada feature yang penting
    2. Jika memungkinkan, perusahaan menambahkan feature atau data terbaru yang mungkin bisa memiliki hubungan terhadap customer churn. Seperti umur customer, amount of spent customer, dan hal lainnya.
    3. Mencoba algorithm lain ada Machine Learning yang memungkinkan kenaikan nilai metriks pada model. Atau melakukan tunning parameter dengan skala yang lebih panjang lagi yang memungkinkan nilai metriks membaik
    4. Melakukan analisa pada data yang salah kita prediksi agar dapat meningkatkan kemampuan model dalam memprediksi
