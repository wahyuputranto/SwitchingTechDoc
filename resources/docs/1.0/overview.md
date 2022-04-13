# Overview

---

- [Informasi Umum](#section-1)
- [Informasi Umum](#section-2)

<a name="section-1"></a>
## Informasi Umum

Semua transaksi melalui system OKEBAYAR akan menggunakan `HTTP API` dengan metode `POST`, dan request dalam format `XML` atau `JSON` di dalam body requestnya.

Transaksi bisa terjadi secara synchronous maupun asynchronous. Apabila proses memerlukan waktu lebih dari 30 detik (time out – bisa berubah kemudian) status akan diberikan terlebih dahulu, kemudian proses baru dilakukan di engine core kami.

<b>Partner</b>  bisa mengirimkan cek status request ke system OKEBAYAR untuk melihat real status dari transaksi yang sudah terjadi, atau menunggu update status dikirimkan melalui callback ke HTTP Server yang disediakan oleh partner.

Semua parameter XML tag atau JSON, harus ditulis menggunakan huruf kecil semua (lower case). Contoh `<transactioncode></transactioncode>`. Bukan `<transactionCode></transactionCode>`. Begitu juga dengan parameter dalam JSON, akan menjadi `{“transactioncode”: … }`, bukan
`{“transactionCode”: …}`.

Kesalahan dalam penulisan dan pemakaian huruf besar atau kecil akan mengakibatkan response kesalahan parameter atau bad xml format.

Kode produk, atau productcode adalah kode produk yang tersedia. Beberapa produk, memiliki lebih dari satu kode produk (productcode) karena hal-hal berikut ini:
-	Sedang ada promo untuk product tersebut. Promotional products akan diberikan dalam bentuk productcode yang berbeda dengan productcode standart.
-	Request partner untuk membedakan product dari vendor tertentu.

Trace code (tracecode) adalah kode unik yang digenerate oleh system OKEBAYAR sebagai referensi bagi partner untuk trace atau cek status transaksi. Setiap hit request akan mendapatkan satu tracecode yang unik.

Kode referensi partner atau referencecode adalah kode unik yang digenerate oleh partner untuk keperluan tracing transaksi oleh partner. referencecode seharusnya unik dan sebaiknya berupa UUID yang terdiri dari karakter alfa numeric. Berbeda dengan tracecode yang digenerate oleh OkeBayar, referencecode dibuat dan dipakai untuk keperluan internal PARTNER sendiri.

Apabila transaksi mengalami timeout, di mana partner tidak mendapatkan response tracecode, maka partner bisa melakukan cek status transaksi menggunakan referencecode.

Signature adalah MD5 dari userid, userpin dan referencecode yang dikirimkan oleh PARTNER.

Date dan Time format dalam API OkeBayar ini selalu menggunakan format YYMMDDHHmmssSSS, di mana YY adalah tahun 2 digit, MM adalah bulan 2 digit, DD adalah tanggal 2 digit, HH ada jam 2 digit, mm adalah menit 2 digit, ss adalah detik 2 digit dan SSS adalah milisecond dalam format 3 digit.

Detail tentang status, silahkan lihat di lampiran 1 (Status Transaksi).

Berikut adalah fungsi-fungsi utama dalam system OKEBAYAR.



> {info} Here is an example of danger alarm message

<!-- ![alt](https://i.ibb.co/9sJZqpX/android-kotlin-wallpaper.png) -->

> {danger.fa-close} Here is an example of a font awesome icon


- [Example](#example-link)

<a name="example-link">
## [Example](#example-link)

### H3
## H2
Inline code is `cool`


```json
{
"tracecode": "1234567890123456", 
"transactionstatus": "000", 
"transactiontime": "200501013059123", 
"serialnumber": "11223344556677889900", 
"harga": 11000.00,
"balance": 510000123.00, 
"productcode": "productCode", 
"accnumber": "081234567890", "userid": "userId",
"referencecode": "kodeReferensiPartner" 
}

```
```xml
{
<transaction-status> <tracecode>1234567890123456</tracecode> <transactionstatus>000</transactionstatus> <transactiontime>YYMMddHHmmssSSS</transactiontime> <serialnumber>11223344556677889900</serialnumber> <harga>110000</harga>
<balance>50590000</balance> <productcode>productCode</productcode> <accnumber>081234567890</accnumber> <userid>userId</userid> <referencecode>kodeReferensiPartner</referencecode>
</transaction-status>

}

```




<a name="section-2">
## [Example2](#example-link)

### H3--oiuoiu
## H2
Inline code is `cool`