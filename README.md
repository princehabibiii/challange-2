##  Challenge [2]: 

### Description:
Ditemukan celah keamanan pada logika pemrosesan keranjang belanja. Aplikasi mempercayai input kuantitas barang dari sisi klien tanpa validasi batas minimum. Hal ini memungkinkan pengguna untuk mengirimkan jumlah barang dalam angka negatif.

### Hint: 
Sang Saudagar mencatat setiap pesanan berdasarkan banyaknya barang yang diminta. Berpegang pada sumpah lama yang tak pernah ia pertanyakan. Setiap ritual dijalankan persis seperti tertulis, tanpa melihat niat di baliknya. Mereka yang hanya meniru langkah akan pulang dengan tangan kosong. Namun bagi yang memahami cara sumpah itu bekerja bukan sekadar bunyinya arah pertukaran bisa berubah, tanpa satu aturan pun dilanggar.
### Analysis:
Masalah utama terletak pada hilangnya pemeriksaan logika di sisi server:

JavaScript
Contoh logika yang salah di server
total = price * quantity; Tanpa pengecekan if (quantity < 1)
Server mengeksekusi perintah persis seperti yang tertulis (sesuai clue), tanpa mempertimbangkan bahwa secara fisik, jumlah barang tidak mungkin negatif.
### Solution:
1. Penambahan Produk
User memilih produk apa saja di halaman utama dan memasukkannya ke dalam keranjang (Add to Basket).
2. Intersepsi via Proxy (Burp Suite)
User membuka menu keranjang (Basket). Proxy Burp Suite diaktifkan untuk menangkap lalu lintas data. User melakukan aksi yang memicu update jumlah barang (seperti menambah atau mengurangi item).
3. Manipulasi Kuantitas (The Exploit)
Ditemukan request PUT menuju endpoint /api/BasketItems/. Di dalam body request, terdapat parameter quantity.
Payload Original: {"quantity": 2}
Payload Manipulasi: {"quantity": -1000}
Request yang telah dimodifikasi kemudian dikirim (Forward) ke server.
4. Server menerima angka -1000 dan mengalikannya dengan harga produk. Hasilnya, keranjang menampilkan total harga negatif. User kemudian menekan tombol Checkout dan menyelesaikan proses pemesanan. Sistem menganggap transaksi valid karena seluruh "ritual" (langkah-langkah) terpenuhi secara teknis.
### Flag:
713e7b9e4f63848bde6ba8a4bf05b5645c840f5d
