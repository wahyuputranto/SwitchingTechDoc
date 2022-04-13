# Refrensi API

---

-   [Pembelian produk top up prabayar](#Prabayar)
-   [Inquiry Prabayar](#Inquiry-Prabayar)
-   [Inquiry PascaBayar](#Inquiry-PascaBayar)
-   [Pembayaran-PascaBayar](#Pembayaran-PascaBayar)
-   [Cek Status Transaksi](#Status)
-   [Balance](#Balance)
-   [Echo](#Echo)

<a name="Prabayar"></a>

## Pembelian produk top up prabayar

Pembelian produk pra-bayar, meliputi tetapi tidak terbatas seperti pulsa pra-bayar handphone, top up pra-bayar PLN, voucher game, voucher Google Play, iTunes dan lain sebagainya.

Berikut ini adalah contoh XML dan JSON yang akan digunakan. Untuk selanjutnya, semua parameter akan dirupakan dalam bentuk tabel, bukan dalam bentuk format XML atau JSON tetapi bisa dengan mudak dimapping dalam format XML atau JSON yang sesuai.

<u>Format JSON Request:</u>

```json
{
    "transactioncode": "OkeBayar-prabayar",
    "productcode": "productCode",
    "accnumber": "081234567890",
    "userid": "userId",
    "userpin": "userPin",
    "signature": "1234567890abcdef",
    "referencecode": "kodeReferensiPartner"
}
```

<u>Format XML Request:</u>

```xml
<transaction>
<transactioncode>OkeBayar-prabayar</transactioncode>
<productcode>productCode</productcode>
<accnumber>081234567890</accnumber>
<userid>userId</userid>
<userpin>userPin</userpin>
<signature>1234567890abcdef</signature>
<referencecode>kodeReferensiPartner</referencecode>
</transaction>
```

<b>
    Response Parameters
</b>

```json
{
"tracecode": "1234567890123456",
"transactionstatus": "000",
"transactiontime": "200501013059123",
"serialnumber": "11223344556677889900", "
harga": 11000.00,
"balance": 510000123.00,
"productcode": "productCode",
"accnumber": "081234567890",
"userid": "userId",
"referencecode": "kodeReferensiPartner"
 }


```

```xml
{
<transaction-status>
<tracecode>1234567890123456</tracecode>
<transactionstatus>000</transactionstatus>
<transactiontime>YYMMddHHmmssSSS</transactiontime>
<serialnumber>11223344556677889900</serialnumber>
<harga>110000</harga>
<balance>50590000</balance>
<productcode>productCode</productcode>
<accnumber>081234567890</accnumber>
<userid>userId</userid>
<referencecode>kodeReferensiPartner</referencecode>
</transaction-status>

}

```

Response transaksi ini akan langsung diterima partner setelah melakukan HTTP HIT. Apabila terjadi kegagalan karena jaringan (time-out) dan lain sebagainya, maka partner bisa melakukan cek status dengan menggunakan kode transaksi (transactioncode) atau referencecode dengan fungsi OkeBayar-cek-status.

> {info}Semua transaksi yang mengalami time-out, harus dilakukan cek-status.

Jadi apabila dirangkum dalam bentuk tabel, maka data parameter untuk request dan response adalah sebagain berikut:

<b>
    Request Transaksi Prabayar
</b>

| Parameter              | Tipe Data            | Deskripsi                                                                                                    |
| :--------------------- | :------------------- | :----------------------------------------------------------------------------------------------------------- |
| <b>transactioncode</b> | String (pre-defined) | Diisi dengan “OkeBayar-prabayar” (tanpa double quotes).                                                      |
| <b>productcode</b>     | String               | Diisi dengan kode produk yang dibeli. Silahkan hubungi tim sales atau marketing kami untuk data productcode. |
| <b>accnumber</b>       | String               | Diisi dengan userId yang sudah terdaftar di system OKEBAYAR.                                                 |
| <b>userid</b>          | String               | Diisi dengan userId yang sudah terdaftar di system OKEBAYAR                                                  |
| <b>userpin</b>         | String               | PIN yang sesuai dengan userId.                                                                               |
| <b>signature</b>       | String               | Kode yang digenerate menggunakan MD5. Signature = MD5(userid+userpin+referencecode).                         |
| <b>referencecode</b>   | String               | Kode unik yang dipakai oleh PARTNER untuk identifikasi transaksi ke system OKEBAYAR.                         |

<b>
    Response Transaksi Prabayar
</b>

| Parameter                | Tipe Data | Deskripsi                                                                                                                                                                                        |
| :----------------------- | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <b>tracecode</b>         | String    | TraceCode adalah kode unik yang digenerate oleh system OKEBAYAR untuk identifikasi suatu transaksi.                                                                                              |
| <b>transactionstatus</b> | String    | Adalah status dari transaksi yang dikirimkan ke OkeBayar. “000” berarti transaksi sukses. Lihat lampiran status transaksi untuk detailnya.                                                       |
| <b>transactiontime</b>   | String    | Tanggal dan jam transaksi dijalankan. Formatnya adalah YYMMDDHHmmssSSS.                                                                                                                          |
| <b>serialnumber</b>      | String    | Serial number yang diberikan oleh biller. Bisa digunakan untuk check dan recheck transaksi ke biller langsung.                                                                                   |
| <b>harga</b>             | String    | Harga yang akan dideduct oleh OkeBayar ke saldo prepaid PARTNER untuk transaksi tersebut.                                                                                                        |
| <b>balance </b>          | String    | Saldo deposit/wallet PARTNER yang tersisa.                                                                                                                                                       |
| <b>productcode </b>      | String    | Kode produk prabayar yang dibeli.                                                                                                                                                                |
| <b>accnumber </b>        | String    | Account number pelanggan. Untuk prepaid pulsa, accnumber diisi dengan nomor handphone prepaid pelanggan.                                                                                         |
| <b>userid </b>           | String    | UserId yang digunakan oleh PARTNER untuk melakukan transaksi ini                                                                                                                                 |
| <b>info </b>             | String    | Additional info yang digenerate oleh OkeBayar untuk menjadi perhatian PARTNER. Kadang kala bisa berupa iklan. Sebaiknya PARTNER menampung message ini dalam suatu database inbox untuk direview. |
| <b>referencecode </b>    | String    | Kode referensi yang digenerate oleh PARTNER, dikirimkan kembali kepada PARTNER untuk kepentingan trace transaksi                                                                                 |

Apabila ternyata proses transaksi memerlukan waktu, maka status dari transaksi di atas akan dikembalikan dengan status “transaksi pending, menunggu final status” atau kode status 002.

Untuk transaksi dengan status pending (002), update status dan final status akan dikirimkan melalui callback atau bisa partner dapatkan dengan melakukan cek status.

Untuk bisa menerima callback, partner harus mendaftarkan URL callback server partner ke kami terlebih dahulu.

Callback akan dikirimkan ke client dengan 4 parameters utama:

1.              tracecode.
2.              referencecode. 3.     status.
3.              serialnumber.

Callback bisa menggunakan HTTP Get atau HTTP Post dengan format XML atau JSON sesuai dengan request transaksi yang dikirimkan sebelumnya, dengan minimal 4 parameters di atas.

Status yang dikirimkan akan berupa update status dari proses yang dilakukan saat ini. Dalam satu kali proses transaksi bisa terjadi beberapa kali callback sampai dengan final status dikirimkan. Yang termasuk dalam final
status adalah status sukses (000) atau gagal (status yang tidak diawali 0 dan 1 di depannya). Callback untuk suatu transaksi yang diterima setelah status final bisa diabaikan.

<a name="Status"></a>






## Inquiry prabayar

Transaksi PLN Prabayar, untuk beberapa kode produk perlu untuk dilakukan inquiry terlebih dahulu. Berikut ini contoh XML untuk inquiry dan responsenya.

<u>Inquiry Prabayar – Request: </u>

<u>Format XML Request:</u>

```xml
<transaction>
<transactioncode>OkeBayar-prabayar</transactioncode>
<querycode>inquiry</querycode>
<productcode>HPLNPRA20</productcode>
<accnumber>14234567895</accnumber>
<userid>pcuXXXXX</userid>
<userpin>pcuXXXXX</userpin>
<referencecode>1234567890</referencecode>
<signature>5917165a3098bd16eb009c04aaf0b93a</signature>
</transaction>
```

<b>
    Response Parameters
</b>

```xml
{
<transaction-status>
<tracecode>17cc33e002c247edbe2badddb084d9c2</tracecode>
<transactionstatus>000</transactionstatus>
<transactiontime>200324200911377</transactiontime>
<productcode>HPLNPRA20</productcode>
<accnumber1>14234567895</accnumber1>
<accnumber2></accnumber2>
<accnumber3></accnumber3>
<inquirycode>d165899c0dfd45e883d79a8c7742336e</inquirycode>
<inquiryexpired>2020-03-25 20:09:12.374</inquiryexpired>
<PLN_IDPELANGGAN>514444444444</PLN_IDPELANGGAN>
<PLN_NOMETER>14234567895</PLN_NOMETER>
<PLN_NAMAPELANGGAN>MELISA</PLN_NAMAPELANGGAN>
<PLN_TARIFDAYA>I3/54321</PLN_TARIFDAYA>
<PLN_TARIF>I3</PLN_TARIF>
<PLN_KATEGORIDAYA>54321</PLN_KATEGORIDAYA>
<PLN_NOREFPLN>0MUP210Z83A4C2E165D4522042E5CF9E</PLN_NOREFPLN>
<PLN_NOREFVENDOR>8D722CD016BD085AFF58FA0FC4320431</PLN_NOREFVENDOR>
<PLN_KODEDISTRIBUSI>51</PLN_KODEDISTRIBUSI>
<PLN_UPJ>51106</PLN_UPJ>
<PLN_UPJPHONE>021222222</PLN_UPJPHONE>
<PLN_KWHLIMIT>6000</PLN_KWHLIMIT>
<PLN_TOKENUNSOLD1></PLN_TOKENUNSOLD1>
<PLN_TOKENUNSOLD2></PLN_TOKENUNSOLD2>
<ref1>1234567890</ref1>
<ref2></ref2>
<ref3>320431</ref3>
<serialnumber></serialnumber>
<userid>pcuXXXXX</userid>
<referencecode>1234567890</referencecode>
</transaction-status>
}

```

Kemudian setelah melakukan inquiry, lakukan transaksi pembelian product prabayar PLN dengan xml seperti di bawah ini.

<u>Inquiry Prabayar – Request: </u>

<u>Format XML Request:</u>

```xml
<transaction>
<transactioncode>OkeBayar-prabayar</transactioncode>
<productcode>HPLNPRA20</productcode>
<accnumber>14234567895</accnumber>
<userid>pcuXXXXX</userid>
<userpin>pcuXXXXX</userpin>
<referencecode>1234567890</referencecode>
<signature>5917165a3098bd16eb009c04aaf0b93a</signature>
</transaction>
```

Yang perlu diperhatikan di request pembayaran ini, tag inquirycode dihilangkan, kemudian tag ref1, ref2 dan ref3 sama perish dengan hasil inquiry.

<a name="Inquiry-PascaBayar"></a>

## Inquiry Pasca Bayar

Inquiry pasca bayar adalah transaksi untuk “meminta” bill atau tagihan kepada biller. Berdasarkan bill atau tagihan inilah maka transaksi pembayaran tagihan pasca bayar dilakukan.

Setiap proses inquiry pasca bayar akan memberikan “inquiry response”. Data dari inquiry response ini bisa dipakai untuk melakukan proses pembayaran dalam rentang waktu tertentu karena inquiry response memiliki expiry date. Apabila batas expiry date dari inquiry response sudah terlewati, maka PARTNER wajib melakukan proses inquiry lagi.

Di dalam data inquiry response, terdapat data “Inquiry Code” atau inquirycode. Data inquirycode ini harus dibawa sebagai parameter mandatory dalam melakukan proses pembayaran billing atau tagihan, tetap dengan memperhatikan batas expiry date di atas.

<b>
    Request 
</b>

| Parameter              | Tipe Data | Deskripsi                                                                                                                                                                       |
| :--------------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <b>transactioncode</b> | String    | Diisi dengan pre-defined parameter untuk inquiry pasca bayar: “OkeBayar-inquiry” (tanpa double quotes).                                                                         |
| <b>userid </b>         | String    | UserId yang terdaftar di system OKEBAYAR untuk PARTNER.                                                                                                                         |
| <b>userpin </b>        | String    | PIN yang sesuai dengan userId.                                                                                                                                                  |
| <b>signature</b>       | String    | Kode yang digenerate menggunakan MD5. Signature = MD5(userid+userpin+referencecode).                                                                                            |
| <b>referencecode </b>  | String    |                                                                                                                                                                                 |
| <b>productcode </b>    | String    | Kode produk yang akan di-inquiry tagihannya.                                                                                                                                    |
| <b>accnumber1 </b>     | String    | No Acc. Pelanggan. Khusus untuk telepon PSTN, berisi kode area telepon pelanggan. Untuk produk lain, nomor atau ID pelanggan.                                                   |
| <b>accnumber2 </b>     | String    | Jika diperlukan. Khusus untuk telepon PSTN, berisi nomor telepon pelanggan tanpa kode area. Apabila tidak diperlukan cukup dikosongkan (tetapi parameternya sendiri tetap ada). |
| <b>accnumber3 </b>     | String    | Jika diperlukan.                                                                                                                                                                |
| <b>contactnumber </b>  | String    | Nomor HP atau email client/pemesan/pembayar.                                                                                                                                    |
| <b>ref1 </b>           | String    | Kode referensi dari Client, apabila ada.                                                                                                                                        |

> {success}Inquiry BPJS. Untuk inquiry produk BPJS, data yang digunakan akan sedikit berbeda, sebagai berikut: accnumber3, diisi dengan periode pembayaran BPJS. Periode pembayaran berupa numeric 1 sampai dengan 12.

<b>
    Response 
</b>

| Parameter              | Tipe Data | Deskripsi                                                                                                                                                                       |
| :--------------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <b>tracecode</b>       | String    | TraceCode adalah kode unik yang digenerate oleh system OKEBAYAR untuk identifikasi suatu transaksi.                                                                             |
| <b>inquirycode </b>    | String    | Inquiry code.                                                                                                                                                                   |
| <b>Inquiryexpired </b> | String    | yyMMddhhmmss, expiry inquiry date and time.                                                                                                                                     |
| <b>responsecode </b>   | String    | Lihat lampiran response status.                                                                                                                                                 |
| <b>referencecode </b>  | String    | Kode unik dari PARTNER yang ditampilkan kembali untuk digunakan oleh PARTNER sebagai referensi transaksi.                                                                       |
| <b>responsestatus </b> | String    | Deskripsi tentang responsecode.                                                                                                                                                 |
| <b>responsetime </b>   | String    | Tanggal dan Jam transaksi dilakukan                                                                                                                                             |
| <b>accnumber1 </b>     | String    | No Acc. Pelanggan. Khusus untuk telepon PSTN, berisi kode area telepon pelanggan. Untuk produk lain, nomor atau ID pelanggan.                                                   |
| <b>accnumber2 </b>     | String    | Jika diperlukan. Khusus untuk telepon PSTN, berisi nomor telepon pelanggan tanpa kode area. Apabila tidak diperlukan cukup dikosongkan (tetapi parameternya sendiri tetap ada). |
| <b>accnumber3 </b>     | String    | Jika diperlukan.                                                                                                                                                                |
| <b>contactnumber </b>  | String    | Nomor HP atau email client/pemesan/pembayar.                                                                                                                                    |
| <b>nominal </b>        | String    | Nominal billing yang harus dibayar.                                                                                                                                             |
| <b>adminfee </b>       | String    | Admin fee, biaya admin yang muncul dalam tagihan/billing yang harus dibayar. Total yang harus dibayar = nominal + adminfee.                                                     |
| <b>ref1 </b>           | String    | Kode referensi dari Client, apabila ada.                                                                                                                                        |
| <b>ref2 </b>           | String    | Kode referensi tambahan dari OkeBayar, apabila ada. Isikan di parameter ref2 waktu melakukan payment pasca bayar.                                                               |
| <b>ref3 </b>           | String    | Kode referensi tambahan lain, apabila ada. Isikan di parameter ref3 waktu melakukan payment pasca bayar.                                                                        |
| <b>additionaldata </b> | String    | Additional data in XML format. Additional data can be different from one product to another. PARTNER can use it to gather detai information about the billing and payment.      |

> {info}Addiional data dalam response inquiry untuk PLN pascabayar adalah sebagai berikut (dalam tag xml dan tidak selalu semua field terisi).

| Parameter                      | Tipe Data                                                                   |
| :----------------------------- | :-------------------------------------------------------------------------- |
| <b>caid</b>                    | 0000                                                                        |
| <b>subscriberid </b>           | ID Pelanggan                                                                |
| <b>billingstatus </b>          | Berisi tagihan status 1 s/d 4                                               |
| <b>billingstatus </b>          | Berisi tagihan status 1 s/d 4                                               |
| <b>paymentstatus </b>          | Jumlah pembayaran yang harus dilakukan, biasanya sama seperti billingstatus |
| <b>totalmonthoutstanding </b>  | Jumlah tunggakan dalam satuan bulan, 2 digit (01, 02, dst)                  |
| <b>swreferenceid </b>          | Switcher reference                                                          |
| <b>subscribername </b>         | Nama pelanggan                                                              |
| <b>serviceunit </b>            | Service unit (KDUP)                                                         |
| <b>serviceunitphone </b>       | Nomor telepon informasi PLN                                                 |
| <b>subscribersegmentation </b> | Segmen pelanggan                                                            |
| <b>subscribercategory </b>     | Kategory pelanggan                                                          |
| <b>totaladmincharge</b>        | Admin fee                                                                   |
| <b>blth1 </b>                  | Bill period                                                                 |
| <b>duedate1</b>                | Due date bill                                                               |
| <b>meterreaddate1 </b>         | Meter read date                                                             |
| <b>RPTAG1 </b>                 | Total electric bill                                                         |
| <b>incentive1 </b>             | C0000000000 = incentive, D0000000000 = discentive                           |
| <b>valuaddedtax1 </b>          |                                                                             |
| <b>penaltyfee1 </b>            | denda                                                                       |
| <b>SLALWBP1 </b>               | Previous meter reading 1                                                    |
| <b>SAHLWBP1 </b>               | Current meter reading 1                                                     |
| <b>SLAWBP1 </b>                | Previous meter reading 2                                                    |
| <b>SAHWBP1 </b>                | Current meter reading 2                                                     |
| <b>SLAKVARH1 </b>              | Previous meter reading 3                                                    |
| <b>SAHKVARH1 </b>              | Current meter reading 3                                                     |
| <b>blth2</b>                   | Bill period                                                                 |
| <b>duedate2 </b>               | Due date bill                                                               |
| <b>meterreaddate2 </b>         | Meter read date                                                             |
| <b>RPTAG2 </b>                 | Total electric bill                                                         |
| <b>incentive2 </b>             | C0000000000 = incentive, D0000000000 = discentive                           |
| <b>valuaddedtax2</b>           |                                                                             |
| <b>penaltyfee2</b>             | denda                                                                       |
| <b>SLALWBP2 </b>               | Previous meter reading 1                                                    |
| <b>SAHLWBP2 </b>               | Current meter reading 1                                                     |
| <b>SLAWBP2 </b>                | Previous meter reading 2                                                    |
| <b>SAHWBP2 </b>                | Current meter reading 2                                                     |
| <b>SLAKVARH2 </b>              | Previous meter reading 3                                                    |
| <b>SAHKVARH2 </b>              | Current meter reading 3                                                     |
| <b>blth3 </b>                  | Bill period                                                                 |
| <b>duedate3</b>                | Due date bill                                                               |
| <b>meterreaddate3 </b>         | Meter read date                                                             |
| <b>RPTAG3 </b>                 | Total electric bill                                                         |
| <b>incentive3 </b>             | C0000000000 = incentive, D0000000000 = discentive                           |
| <b>valuaddedtax3 </b>          |                                                                             |
| <b>penaltyfee3 </b>            | denda                                                                       |
| <b>SLALWBP3 </b>               | Previous meter reading 1                                                    |
| <b>SAHLWBP3 </b>               | Current meter reading 1                                                     |
| <b>SLAWBP3 </b>                | Previous meter reading 2                                                    |
| <b>SAHWBP3 </b>                | Current meter reading 2                                                     |
| <b>SLAKVARH3 </b>              | Previous meter reading 3                                                    |
| <b>SAHKVARH3 </b>              | Current meter reading 3                                                     |
| <b>blth4</b>                   | Bill period                                                                 |
| <b>duedate4 </b>               | Due date bill                                                               |
| <b>meterreaddate4 </b>         | Meter read date                                                             |
| <b>RPTAG4 </b>                 | Total electric bill                                                         |
| <b>incentive4 </b>             | C0000000000 = incentive, D0000000000 = discentive                           |
| <b>valuaddedtax4 </b>          |                                                                             |
| <b>penaltyfee4 </b>            | denda                                                                       |
| <b>SLALWBP4 </b>               | Previous meter reading 1                                                    |
| <b>SAHLWBP4 </b>               | Current meter reading 1                                                     |
| <b>SLAWBP4 </b>                | Previous meter reading 2                                                    |
| <b>SAHWBP4 </b>                | Current meter reading 2                                                     |
| <b>SLAKVARH4 </b>              | Previous meter reading 3                                                    |
| <b>SAHKVARH4 </b>              | Current meter reading 3                                                     |
| <b>alamat</b>                  |                                                                             |
| <b>plnnpwp </b>                |                                                                             |
| <b>subscribernpwp </b>         |                                                                             |
| <b>totalrptag</b>              |                                                                             |
| <b>info</b>                    |                                                                             |



<a name="Pembayaran-PascaBayar"></a>

## Pembayaran Billing/Tagihan Pasca Bayar

Untuk melakukan pembayaran, partner terlebih dahulu harus melakukan inquiry seperti point. 5 di atas. Data dari inquiry di atas menjadi dasar dari transaksi pembayaran billing atau tagihan pasca bayar ini. Partner juga harus memperhatikan batas kadaluarsa (expiry) dari inquiry yang dijadikan referensi.

<b>
    Request 
</b>

| Parameter              | Tipe Data | Deskripsi                                                                                                                                                                       |
| :--------------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <b>transactioncode</b> | String    | Diisi dengan pre-defined parameter untuk inquiry pasca bayar: “OkeBayar-inquiry” (tanpa double quotes).                                                                         |
| <b>userid </b>         | String    | UserId yang terdaftar di system OKEBAYAR untuk PARTNER.                                                                                                                         |
| <b>userpin </b>        | String    | PIN yang sesuai dengan userId.                                                                                                                                                  |
| <b>signature</b>       | String    | Kode yang digenerate menggunakan MD5. Signature = MD5(userid+userpin+referencecode).                                                                                            |
| <b>referencecode </b>  | Sring     | Kode unik yang dipakai oleh PARTNER untuk                                                                                                                                       |
| <b>productcode </b>    | String    | Kode produk yang akan di-inquiry tagihannya.                                                                                                                                    |
| <b>accnumber1 </b>     | String    | No Acc. Pelanggan. Khusus untuk telepon PSTN, berisi kode area telepon pelanggan. Untuk produk lain, nomor atau ID pelanggan.                                                   |
| <b>accnumber2 </b>     | String    | Jika diperlukan. Khusus untuk telepon PSTN, berisi nomor telepon pelanggan tanpa kode area. Apabila tidak diperlukan cukup dikosongkan (tetapi parameternya sendiri tetap ada). |
| <b>accnumber3 </b>     | String    | Jika diperlukan.                                                                                                                                                                |
| <b>contactnumber </b>  | String    | Nomor HP atau email client/pemesan/pembayar. |
| <b>nominal </b>  | Double    | Nominal billing yang harus dibayar. Sama dengan hasil inquiry. |
| <b>adminfee </b>           | Double    | Admin fee, biaya admin yang muncul dalam tagihan/billing yang harus dibayar. Total yang harus dibayar = nominal + adminfee. |
| <b>ref1 </b>           | String    | Kode referensi dari Client, apabila ada.|
| <b>ref2 </b>           | String    | Kode referensi tambahan dari OkeBayar, apabila ada. Sama dengan hasil inquiry. Copy sama persis dari hasil inquiry. |
| <b>ref3 </b>           | String    | Kode referensi tambahan lain, apabila ada. Sama dengan hasil inquiry. Copy sama persis dari hasil inquiry |
| <b>inquirycode </b>    | String    | Inquiry code dari data inquiry sebelumnya.  |

> {success}Pembayaran BPJS. accnumber2, diisi dengan no HP dari personal yang melakukan pembayaran BPJS tersebut. Accnumber3, diisi dengan periode pembayaran BPJS (sesuai dengan inquiry di atas). Periode ini adalah numeric dari 1 sampai dengan 12.


<b>
    Response 
</b>


| Parameter              | Tipe Data | Deskripsi                                                                                                                                                                       |
| :--------------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <b>tracecode</b>       | String    | TraceCode adalah kode unik yang digenerate oleh system OKEBAYAR untuk identifikasi suatu transaksi.                                                                             |
| <b>inquirycode </b>    | String    | Inquiry code.                                                                                                                                                                   |
| <b>responsecode </b>   | String    | Lihat lampiran response status.                                                                                                                                                 |
| <b>referencecode </b>  | String    | Kode unik dari PARTNER yang ditampilkan kembali untuk digunakan oleh PARTNER sebagai referensi transaksi.                                                                       |
| <b>responsestatus </b> | String    | Deskripsi tentang responsecode.                                                                                                                                                 |
| <b>responsetime </b>   | String    | Tanggal dan Jam transaksi dilakukan                                                                                                                                             |
| <b>accnumber1 </b>     | String    | No Acc. Pelanggan. Khusus untuk telepon PSTN, berisi kode area telepon pelanggan. Untuk produk lain, nomor atau ID pelanggan.                                                   |
| <b>accnumber2 </b>     | String    | Jika diperlukan. Khusus untuk telepon PSTN, berisi nomor telepon pelanggan tanpa kode area. Apabila tidak diperlukan cukup dikosongkan (tetapi parameternya sendiri tetap ada). |
| <b>accnumber3 </b>     | String    | Jika diperlukan.                                                                                                                                                                |
| <b>contactnumber </b>  | String    | Nomor HP atau email client/pemesan/pembayar.                                                                                                                                    |
| <b>nominal </b>        | String    | Nominal billing yang harus dibayar.                                                                                                                                             |
| <b>adminfee </b>       | String    | Admin fee, biaya admin yang muncul dalam tagihan/billing yang harus dibayar. Total yang harus dibayar = nominal + adminfee.                                                     |
| <b>billingperiod  </b>       | String    | Billing period dari tagihan atau bill.  |
| <b>ref1 </b>           | String    | Kode referensi dari Client, apabila ada.                                                                                                                                        |
| <b>ref2 </b>           | String    | Kode referensi tambahan dari OkeBayar, apabila ada. Isikan di parameter ref2 waktu melakukan payment pasca bayar.                                                               |
| <b>ref3 </b>           | String    | Kode referensi tambahan lain, apabila ada. Isikan di parameter ref3 waktu melakukan payment pasca bayar.                                                                        |
| <b>Info  </b>       | String    | Info text dari OkeBayar untuk PARTNER.  |
| <b>additionaldata </b> | String    | Additional data in XML format. Additional data can be different from one product to another. PARTNER can use it to gather detai information about the billing and payment.      |



## Cek Status Transaksi

Untuk melakukan cek status transaksi yang sebelumnya sudah dikirimkan oleh partner ke system OKEBAYAR.

| Parameter                | Tipe Data | Deskripsi                                                                                                                                                                                        |
| :----------------------- | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <b>tracecode</b>         | String    | TraceCode adalah kode unik yang digenerate oleh system OKEBAYAR untuk identifikasi suatu transaksi.                                                                                              |
| <b>transactionstatus</b> | String    | Adalah status dari transaksi yang dikirimkan ke OkeBayar. “000” berarti transaksi sukses. Lihat lampiran status transaksi untuk detailnya.                                                       |
| <b>transactiontime</b>   | String    | Tanggal dan jam transaksi dijalankan. Formatnya adalah YYMMDDHHmmssSSS.                                                                                                                          |
| <b>serialnumber</b>      | String    | Serial number yang diberikan oleh biller. Bisa digunakan untuk check dan recheck transaksi ke biller langsung.                                                                                   |
| <b>harga</b>             | String    | Harga yang akan dideduct oleh OkeBayar ke saldo prepaid PARTNER untuk transaksi tersebut.                                                                                                        |
| <b>balance </b>          | String    | Saldo deposit/wallet PARTNER yang tersisa.                                                                                                                                                       |
| <b>productcode </b>      | String    | Kode produk prabayar yang dibeli.                                                                                                                                                                |
| <b>accnumber </b>        | String    | Account number pelanggan. Untuk prepaid pulsa, accnumber diisi dengan nomor handphone prepaid pelanggan.                                                                                         |
| <b>userid </b>           | String    | UserId yang digunakan oleh PARTNER untuk melakukan transaksi ini                                                                                                                                 |
| <b>info </b>             | String    | Additional info yang digenerate oleh OkeBayar untuk menjadi perhatian PARTNER. Kadang kala bisa berupa iklan. Sebaiknya PARTNER menampung message ini dalam suatu database inbox untuk direview. |
| <b>referencecode </b>    | String    | Kode referensi yang digenerate oleh PARTNER, dikirimkan kembali kepada PARTNER untuk kepentingan trace transaksi                                                                                 |

Untuk melakukan cek status transaksi yang sebelumnya sudah dikirimkan oleh partner ke system OKEBAYAR.

| Parameter                | Tipe Data | Deskripsi                                                                                                                                                                                        |
| :----------------------- | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <b>tracecode</b>         | String    | TraceCode adalah kode unik yang digenerate oleh system OKEBAYAR untuk identifikasi suatu transaksi.                                                                                              |
| <b>transactionstatus</b> | String    | Adalah status dari transaksi yang dikirimkan ke OkeBayar. “000” berarti transaksi sukses. Lihat lampiran status transaksi untuk detailnya.                                                       |
| <b>transactiontime</b>   | String    | Tanggal dan jam transaksi dijalankan. Formatnya adalah YYMMDDHHmmssSSS.                                                                                                                          |
| <b>serialnumber</b>      | String    | Serial number yang diberikan oleh biller. Bisa digunakan untuk check dan recheck transaksi ke biller langsung.                                                                                   |
| <b>harga</b>             | String    | Harga yang akan dideduct oleh OkeBayar ke saldo prepaid PARTNER untuk transaksi tersebut.                                                                                                        |
| <b>balance </b>          | String    | Saldo deposit/wallet PARTNER yang tersisa.                                                                                                                                                       |
| <b>productcode </b>      | String    | Kode produk prabayar yang dibeli.                                                                                                                                                                |
| <b>accnumber </b>        | String    | Account number pelanggan. Untuk prepaid pulsa, accnumber diisi dengan nomor handphone prepaid pelanggan.                                                                                         |
| <b>userid </b>           | String    | UserId yang digunakan oleh PARTNER untuk melakukan transaksi ini                                                                                                                                 |
| <b>info </b>             | String    | Additional info yang digenerate oleh OkeBayar untuk menjadi perhatian PARTNER. Kadang kala bisa berupa iklan. Sebaiknya PARTNER menampung message ini dalam suatu database inbox untuk direview. |
| <b>referencecode </b>    | String    | Kode referensi yang digenerate oleh PARTNER, dikirimkan kembali kepada PARTNER untuk kepentingan trace transaksi                                                                                 |


<a name="Balance"></a>

## Balance

Untuk melihat sisa deposit saat ini, parameter yang digunakan adalah:

<b>
    Request 
</b>

| Parameter              | Tipe Data | Deskripsi                                                                                           |
| :--------------------- | :-------- | :-------------------------------------------------------------------------------------------------- |
| <b>transactioncode</b> | String    | Diisi dengan pre-defined parameter untuk cek balance: “OkeBayar-cek-balance” (tanpa double quotes). |
| <b>tracecode</b>       | String    | TraceCode dari transaksi sebelumnya yang akan dicek statusnya.                                      |
| <b>userid</b>          | String    | UserId yang terdaftar di system OKEBAYAR untuk PARTNER.                                             |
| <b>userpin </b>        | String    | PIN yang sesuai dengan userId.                                                                      |
| <b>signature</b>       | String    | Kode yang digenerate menggunakan MD5. Signature = MD5(userid+userpin+referencecode).                |
| <b>referencecode </b>  | String    | Kode unik yang dipakai oleh PARTNER untuk identifikasi transaksi ke system OKEBAYAR.                |

<b>
    Response 
</b>

| Parameter              | Tipe Data | Deskripsi                                                                                                 |
| :--------------------- | :-------- | :-------------------------------------------------------------------------------------------------------- |
| <b>tracecode</b>       | String    | TraceCode adalah kode unik yang digenerate oleh system OKEBAYAR untuk identifikasi suatu transaksi.       |
| <b>responsecode </b>   | String    | Lihat lampiran response status.                                                                           |
| <b>referencecode</b>   | String    | Kode unik dari PARTNER yang ditampilkan kembali untuk digunakan oleh PARTNER sebagai referensi transaksi. |
| <b>responsestatus </b> | String    | Deskripsi tentang responsecode.                                                                           |
| <b>responsetime </b>   | String    | Tanggal dan Jam transaksi dilakukan.                                                                      |
| <b>balance </b>        | String    | Sisa saldo dalam mata uang rupiah.                                                                        |

<a name="Echo"></a>

## Echo

Echo dipergunakan untuk testing koneksi ke server OkeBayar. Apabila menggunakan protocol koneksi ISO 8583, echo digunakan untuk memastikan apakah koneksi soket ke server OkeBayar masih tersambung dan berjalan dengan baik.

<b>
    Request 
</b>

| Parameter             | Tipe Data | Deskripsi                                                                            |
| :-------------------- | :-------- | :----------------------------------------------------------------------------------- |
| <b>userid</b>         | String    | UserId yang terdaftar di system OKEBAYAR untuk PARTNER.                              |
| <b>userpin </b>       | String    | PIN yang sesuai dengan userId.                                                       |
| <b>signature</b>      | String    | Kode yang digenerate menggunakan MD5. Signature = MD5(userid+userpin+referencecode). |
| <b>referencecode </b> | String    |                                                                                      |
| <b>message </b>       | String    | Message yang akan di-echo oleh server OkeBayar.                                      |

<b>
    Response 
</b>

| Parameter             | Tipe Data | Deskripsi                                               |
| :-------------------- | :-------- | :------------------------------------------------------ |
| <b>userid</b>         | String    | UserId yang terdaftar di system OKEBAYAR untuk PARTNER. |
| <b>referencecode </b> | String    |                                                         |
| <b>message </b>       | String    | Message yang akan di-echo oleh server OkeBayar.         |

<a name="Inquiry-Prabayar"></a>
