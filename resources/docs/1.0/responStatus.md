# Respon Status

| Parameter | Tipe Data                                                                            | Deskripsi            |
| :-------- | :----------------------------------------------------------------------------------- | :------------------- |
| 000       | Suskses, SN Transaksi berhasil dan PARTNER mendapatkan serial number.                |                      |
| 001       | Sukses, No SN Transaksi berhasil tetapi Serial Number belum tersedia atau tidak ada. | Cek status transaksi |
| 002       | Pending Transaksi sudah diterima dan proses sedang dilakukan.  | Tunggu Callback Status atau lakukan cek status setelah 5 menit kemudian.|
| 003       | Transaksi pertama gagal, akan diretry secara otomatis oleh system (re-queued).| Tunggu Callback Status atau lakukan cek status setelah 5 menit kemudian.|
| 200       | Gagal, produk tidak tersedia atau kosong Transaksi gagal karena produk tidak tersedia.                | Cek product status, web report atau call customer service.                     |
| 201       | Gagal, gangguan biller atau operator. Transaksi gagal karena problem atau gangguan di billers atau operator. |Hubungi customer service.|
| 202       | Gagal, gangguan server. Transaksi gagal karena problem di server.               | Cek web report atau hubungi customer service.                     |
| 203       | Gagal, saldo habis.Transaksi gagal karena saldo tidak cukup.                | Cek web report atau hubungi customer service.                     |
| 204       | Gagal, account number atau nomor tujuan top up salah. |Koreksi nomor tujuan atau data yang diisikan atau hubungi customer service. |
| 205       | Gagal, kode produk salah. Transaksi gagal karena kode produk salah.                 |Koreksi kode produk atau hubungi customer service. |
| 206       | Gagal, salah format. Transaksi gagal karena kesalahan format XML. | Review lagi format XML atau data yang digunakan untuk request transaksi. |
| 207       | Gagal, autentikasi.Transaksi gagal karena userId, userPin atau Signature salah. | Koreksi userId, Pin dan Signature atau hubungi customer service.  |
| 208       | Gagal, duplikasi. Transaksi gagal karena duplikasi atau pengiriman ulang (account number atau nomor tujuan top up, nominal dan kode referensi sama).|Cek status transaksi|
| 210       | Gagal, transaksi cross region/cluster. Transaksi ditolak karena cross cluster.  |                      |
| 211       | Gagal, invalid signature.                |   Perbaiki signature.                    |
| 212       | Gagal, IP Address invalid.               | Cek IP Address atau hubungi customer service.|
| 213       | Gagal, userId invalid.                |  Hubungi customer service.                    |
| 214       | Gagal, request expired.                | Perhatikan setting date/time. Drop transaksi.                     |
| 215       | Gagal, accNumber tidak bisa menerima transaksi yang diminta. | Periksa kembali accNumber sebelum diulangi. |
| 216       | Gagal, tanggal yang di-inquiry tidak valid.                 | Periksa kembali tanggal yang dimaksud. |
| 217       | Gagal, tanggal yang dipesan atau tanggal keberangkatan tidak valid.                |Periksa kembali tanggal yang dipesan atau tanggal keberangkatan.|
| 232       | Kode produk (productcode) dan nomor account tidak sesuai.  | Perbaiki productcode yang digunakan  |
| 233       | Nomor account telah melampaui batas maksimum jumlah bill.                |                      |
| 234       | Transaksi dibatalkan  | Transaksi dibatalkan  |
| 240       | Gagal, kesalahan belum diidentifikasikan.                |Hubungi customer service.                      |
| 241       | Gagal, transactionCode tidak dikenal.  |  Perbaiki transactionCode di XML request. |
| 260       | Transaction management – refund failed.                | Hubungi customer service. |
| 261       | Transaction management – refund failed, invalid PIN                 | Hubungi customer service.  |
| 270       | Kode product salah untuk check status product.               |Perbaiki kode product (productcode) yang dipakai dan ulangi transaksi.  |
| 400       | Tagihan atau billing tidak diketemukan atau belum tersedia saat ini.               |Tagihan belum tersedia saat ini.|
| 401       | Inquiry sudah dibayar atau tidak valid.                |Tagihan sudah lunas atau inquiry tidak valid, misalnya inquiry ke no pelanggan yang salah.|
| 402       | Data pembayaran berbeda dengan data inquiry.                | Ulangi proses inquiry dan pembayaran lagi. |
| 403       | Inquiry sudah expired.                | Ulangi proses inquiry dan kemudian pembayaran. Max jeda waktu antara inquiry dan pembayaran adalah 30 menit (atau tergantung produk).|
| 404       | Data pelanggan atau nomor meter salah, atau melampaui jumlah maksimum billing.| Nomor pelanggan atau nomor meter yang dimasukkan salah. |
| 405       | Inquiry belum pernah dilakukan sebelumnya. |Inquiry belum pernah dilakukan sebelumnya.|
| 500       | Blacklisted account number                | Account number yang diberikan di-blacklist oleh system OKEBAYAR. Hubungi CS OKEBAYAR.  |
| 510       | Invalid re-hit request.               | Data yang diberikan untuk request re-hit tidak valid. |
