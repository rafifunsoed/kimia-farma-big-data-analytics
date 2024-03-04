# **Virtual Internship Experience: Big Data Analytics - Kimia Farma**

Tool : Google BigQuery <br>
Visualization : Looker Data Studio [Lihat dashboard](https://lookerstudio.google.com/reporting/7d5af651-c6da-4f60-ab7f-0f69de712f41) <br>

---

## **Introduction**

Big Data Analytics Kimia Farma merupakan virtual internship experience yang difasilitasi oleh [Rakamin Academy](https://www.rakamin.com/virtual-internship-experience/kimiafarma-big-data-analytics-virtual-internship-program). Pada project ini saya berperan sebagai Data Analyst Intern yang diminta untuk menganalisis dan membuat laporan penjualan perusahaan menggunakan data-data yang telah disediakan. Dari project ini, saya juga banyak belajar tentang data data warehouse, datalake, dan datamart. <br>
<br>

**Objectives**

- Importing Dataset to BigQuery
- Membuat design datamart (tabel analisa)
- Membuat visualisasi/dashboard Performance Analytics Kimia Farma 2020-2023
  <br>

---

## **Importing Dataset to BigQuery**

### Dataset

Dataset yang disediakan terdiri dari tabel-tabel berikut: <br>

- kf_final_transaction.csv
- kf_inventory.csv
- kf_kantor_cabang.csv
- kf_product.csv

---

## 📂 **Design Datamart**

### Tabel Aggregat

Tabel agregat adalah tabel yang dibuat dengan mengumpulkan dan menghitung data dari tabel basis. Tabel aggregat ini berisi informasi yang lebih ringkas dan digunakan untuk menganalisis data lebih cepat dan efisien. Hasil tabel ini akan digunakan untuk sumber pembuatan dashboard laporan penjualan. Berikut ini adalah kolom-kolom yang mandatory pada tabel tersebut: <br>

● transaction_id : kode id transaksi<br>
● date : tanggal transaksi dilakukan<br>
● branch_id : kode id cabang Kimia Farma<br>
● branch_name : nama cabang Kimia Farma<br>
● kota : kota cabang Kimia Farma<br>
● provinsi : provinsi cabang Kimia Farma<br>
● rating_cabang : penilaian konsumen terhadap cabang Kimia Farma <br>
● customer_name : Nama customer yang melakukan transaksi<br>
● product_id : kode product obat<br>
● product_name : nama obat<br>
● actual_price : harga obat<br>
● discount_percentage : Persentase diskon yang diberikan pada obat<br>
● persentase_gross_laba : Persentase laba yang seharusnya diterima dari obat <br>
● nett_sales : harga setelah diskon<br>
● nett_profit : keuntungan yang diperoleh Kimia Farma<br>
● rating_transaksi : penilaian konsumen terhadap transaksi yang dilakukan<br>

<details>
  <summary> Klik untuk melihat Query </summary>
    <br>
    
```sql
CREATE TABLE kimia_farma.tabel_analisa AS -- Membuat tabel baru dengan nama tabel_analisa di dalam skema kimia_farma
SELECT -- Memilih kolom-kolom yang akan dimasukkan ke dalam tabel
      kft.transaction_id,
      kft.date,
      kkc.branch_id,
      kkc.branch_name,
      kkc.kota,
      kkc.provinsi,
      kkc.rating AS rating_cabang,
      kft.customer_name,
      kp.product_id,
      kp.product_name,
      kp.price AS actual_price,
      kft.discount_percentage,
      CASE --  Menggunakan syntax CASE untuk menentukan persentase laba kotor berdasarkan harga produk
          WHEN kp.price <= 50000 THEN 0.10
          WHEN kp.price BETWEEN 50000 AND 100000 THEN 0.15
          WHEN kp.price BETWEEN 100000 AND 300000 THEN 0.20
          WHEN kp.price BETWEEN 300000 AND 500000 THEN 0.25
          ELSE 0.30
      END AS persentase_gross_laba,
      (kp.price * (1 - kft.discount_percentage / 100)) AS nett_sales, -- Perhitungan untuk menentukan penjualan bersih (nett_sales)
      (kp.price * (1 - kft.discount_percentage / 100) * -- Perhitungan untuk menentukan keuntungan bersih (nett_profit)
      CASE
          WHEN kp.price <= 50000 THEN 0.10
          WHEN kp.price BETWEEN 50000 AND 100000 THEN 0.15
          WHEN kp.price BETWEEN 100000 AND 300000 THEN 0.20
          WHEN kp.price BETWEEN 300000 AND 500000 THEN 0.25
          ELSE 0.30
      END) AS nett_profit,
      kft.rating AS rating_transaksi
FROM -- Perintah untuk menentukan tabel-tabel yang akan di-join
  kimia_farma.kf_final_transaction AS kft
INNER JOIN -- Perintah untuk menggabungkan (join) beberapa tabel berdasarkan kriteria tertentu
  kimia_farma.kf_kantor_cabang AS kkc ON kft.branch_id = kkc.branch_id
INNER JOIN
  kimia_farma.kf_product AS kp ON kft.product_id = kp.product_id;

```

<br>
</details>
<br>

---

## 📂 **Data Visualization**
Dashboard ini anda buat berdasarkan tabel analisa yang telah anda buat sebelumnya pada BigQuery, sehingga anda perlu menghubungkan table tersebut ke Google Looker Studio. Anda dapat mendesain dashboard sesuai dengan kreativitas anda masing-masing, namun dashboardnya harus mencangkup:<br>

● Judul Dashboard <br>
● Summary Dashboard <br>
● Filter Control <br>
● Snapshot Data <br>
● Perbandingan Pendapatan Kimia Farma dari tahun ke tahun <br>
● Top 10 Total transaksi cabang provinsi <br>
● Top 10 Nett sales cabang provinsi <br>
● Top 5 Cabang Dengan Rating Tertinggi, namun Rating Transaksi Terendah <br>
● Indonesia's Geo Map Untuk Total Profit Masing-masing Provinsi <br>
● Dan analisis lainnya yang dapat anda eksplorasi.<br>

[Lihat pada halaman Looker Data Studio](https://lookerstudio.google.com/reporting/7d5af651-c6da-4f60-ab7f-0f69de712f41).

<p align="center">
    <kbd> <img width="1000" alt="Kimia_Farma_page-0001" src=""> </kbd> <br>
</p>
<br>
```
