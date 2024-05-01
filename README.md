# Advanced Programming - Module 9
#### Muhammad Ilham Zikri - 2206083533 - Advanced Programming A

## Reflection

#### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
Berikut perbedaan singkat antara metode RPC unary, server streaming, dan bi-directional streaming:

**Unary RPC**:
Klien mengirim 1 permintaan, server merespons 1 respons.
Cocok untuk operasi sederhana.

**Server Streaming RPC**:
Klien mengirim 1 permintaan, server merespons dengan aliran banyak respons.
Cocok untuk mentransfer banyak data dari server ke klien secara bertahap.

**Bi-directional Streaming RPC**:
Klien dan server dapat mengirim dan menerima aliran data secara bersamaan.
Cocok untuk komunikasi dua arah real-time atau transfer data besar dua arah.

#### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
**Autentikasi**:
Gunakan skema autentikasi seperti akun pengguna/kata sandi, token JWT, atau sertifikat klien.
Terapkan autentikasi pada kedua sisi, klien dan server.

**Otorisasi**:
Batasi akses ke metode/sumber daya tertentu berdasarkan peran atau izin pengguna.
Gunakan model otorisasi berbasis RBAC (Role-Based Access Control) atau ABAC (Attribute-Based Access Control).

**Enkripsi Data**:
Gunakan TLS/SSL untuk mengenkripsi komunikasi antar proses.
Pertimbangkan enkripsi lapisan aplikasi untuk data sensitif.

#### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
Beberapa tantangan yang mungkin muncul saat menangani streaming dua arah dalam aplikasi gRPC Rust seperti aplikasi chat antara lain:

**Manajemen aliran data**:
Mengelola aliran data masuk dan keluar secara bersamaan dapat menjadi rumit.
Perlu mengatur buffer dan mengontrol laju aliran untuk mencegah kelebihan beban.

**Penanganan kesalahan dan pemulihan**:
Perlu menangani kesalahan dan memulihkan koneksi saat terjadi gangguan jaringan.
Memastikan konsistensi data saat terjadi kegagalan menjadi tantangan.

**Skalabilitas dan performa**:
Menjaga performa optimal saat jumlah koneksi tinggi.
Memastikan skalabilitas horizontal untuk menangani beban besar.

#### 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
Penggunaan tokio_stream::wrappers::ReceiverStream memiliki beberapa keunggulan yang patut dipertimbangkan. Salah satu kelebihan utamanya adalah kemudahan dalam melakukan konversi dari Receiver menjadi Stream, memungkinkan integrasi yang lebih baik dengan fungsi dan pustaka lain yang bekerja dengan Stream. Selain itu, kompatibilitas dengan trait Stream membuka peluang untuk memanfaatkan fitur-fitur dan utilitas yang ditawarkan oleh ekosistem Rust.
Namun, tidak dapat dipungkiri bahwa ReceiverStream juga memiliki beberapa keterbatasan. Salah satu aspek yang perlu diperhatikan adalah kurangnya mekanisme bawaan untuk menangani kesalahan, yang dapat menjadi tantangan tersendiri dalam menjamin penanganan kesalahan yang efektif saat menerima pesan. Selain itu, ReceiverStream hanya mendukung komunikasi satu arah dari pengirim ke penerima, sehingga memberikan kontrol yang terbatas atas kapan dan bagaimana underlying Receiver dipoll.
Meskipun demikian, ReceiverStream tetap memiliki fitur yang menarik, yaitu kemampuan bawaan untuk menangani backpressure dengan baik. Dengan fitur ini, pengiriman pesan dapat diatur sedemikian rupa sehingga tidak melampaui kapasitas penerima, mencegah terjadinya masalah seperti overflow atau kehilangan data.
Dalam penggunaannya, perlu dipertimbangkan dengan seksama apakah ReceiverStream cocok dengan kebutuhan spesifik proyek Anda, terutama jika penanganan kesalahan dan komunikasi dua arah menjadi faktor penting. Jika memang demikian, mungkin perlu dicari solusi alternatif yang lebih sesuai.

#### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
Untuk meningkatkan reusability dan modularity dalam proyek Rust gRPC, beberapa pendekatan yang dapat diambil:

1. Pisahkan business logic dari implementasi gRPC.
2. Gunakan Protobuf untuk mendefinisikan services dan Traits untuk abstraksi solid antara services dan implementasi konkretnya.
3. Terapkan modularisasi kode dengan memisahkan fungsionalitas ke dalam modul terpisah.
4. Manfaatkan generics dan traits untuk fungsi umum dan abstraksi interfaces.
5. Terapkan dependency injection untuk memudahkan pengujian dan pembaruan komponen.
6. Pastikan penanganan kesalahan yang konsisten di seluruh modul.

Dengan menerapkan pemisahan concerns, abstraksi yang baik, modularisasi, dan praktik terbaik seperti dependency injection serta penanganan kesalahan, proyek Rust gRPC akan menjadi lebih terstruktur, mudah dipelihara, dan reusable.

#### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
Dalam skenario di mana logika pemrosesan pembayaran menjadi lebih kompleks, terdapat beberapa aspek tambahan yang perlu dipertimbangkan untuk memastikan integritas dan kelancaran proses tersebut. Salah satu langkah penting yang harus diambil adalah implementasi validasi yang ketat terhadap data masukan yang diterima. Hal ini bertujuan untuk memverifikasi kebenaran dan keamanan informasi yang digunakan dalam proses pembayaran, seperti memastikan bahwa nilai pembayaran (amount) bernilai positif dan sesuai dengan ketentuan yang berlaku.

Selain itu, penanganan kesalahan (error handling) yang robust juga menjadi faktor krusial dalam mengatasi berbagai kemungkinan kegagalan atau kondisi tidak terduga yang mungkin terjadi selama pemrosesan pembayaran. Contoh kasus yang perlu ditangani antara lain penolakan transaksi oleh pihak terkait, kebutuhan untuk memproses ulang transaksi, atau bahkan kegagalan total dalam menyelesaikan transaksi.

Dalam konteks ini, integrasi dengan sistem pembayaran eksternal atau penyedia layanan pembayaran menjadi aspek yang tidak dapat diabaikan. Proses ini melibatkan komunikasi jaringan dan interaksi dengan antarmuka (API) pembayaran atau bahkan koneksi langsung ke gateway pembayaran untuk memfasilitasi pemrosesan transaksi secara nyata. Integrasi yang baik dengan sistem eksternal ini sangat penting untuk memastikan keberhasilan dan keamanan transaksi pembayaran.

Dengan mempertimbangkan faktor-faktor ini, logika pemrosesan pembayaran yang lebih kompleks dapat ditangani dengan lebih efektif dan andal, meminimalkan risiko kegagalan atau masalah yang dapat timbul selama proses pembayaran berlangsung.

#### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Adopsi gRPC sebagai protokol komunikasi dalam sistem terdistribusi membawa dampak pada interoperabilitas dengan teknologi dan sistem lain. Salah satu keunggulan utama gRPC adalah kemampuannya untuk menghasilkan kode klien dan server secara otomatis untuk berbagai bahasa pemrograman, memfasilitasi integrasi lintas teknologi. Namun, tantangannya terletak pada dukungan yang terbatas untuk protokol komunikasi non-gRPC, yang dapat menyulitkan integrasi dengan sistem yang menggunakan protokol berbeda, seperti REST. Pada akhirnya, pemilihan gRPC harus mempertimbangkan kebutuhan interoperabilitas dan ekosistem teknologi yang ada dalam sistem terdistribusi tersebut.

#### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
HTTP/2 menawarkan sejumlah keunggulan signifikan dibandingkan HTTP/1.1, seperti dukungan multiplexing yang memungkinkan beberapa permintaan dan respons diproses secara paralel melalui satu koneksi, mengurangi latensi dan meningkatkan kinerja. Selain itu, kompresi header dan fitur server push dapat mengoptimalkan efisiensi transmisi data dan pengiriman konten.

Namun, implementasi HTTP/2 lebih kompleks dan memerlukan pemahaman yang lebih mendalam tentang konsep-konsep barunya. Masalah kompatibilitas juga dapat terjadi dengan klien dan server yang tidak mendukung protokol ini sepenuhnya. Selain itu, proses kompresi dan dekompresi header dapat meningkatkan beban CPU, yang menjadi pertimbangan pada lingkungan dengan sumber daya terbatas atau perangkat dengan kinerja rendah.

Jadi, meskipun HTTP/2 menawarkan banyak keunggulan kinerja, keputusan untuk mengadopsinya harus mempertimbangkan kompleksitas implementasi, kompatibilitas dengan lingkungan yang ada, serta potensi peningkatan beban CPU pada perangkat dengan sumber daya terbatas.

#### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
Model request-response dari REST APIs memberikan komunikasi satu arah, di mana klien mengirim permintaan dan menunggu respons dari server. Di sisi lain, gRPC mendukung komunikasi bi-directional yang memungkinkan pertukaran data real-time antara klien dan server. Dengan gRPC, keduanya dapat mengirim dan menerima data secara langsung, tanpa harus menunggu untuk menyelesaikan permintaan atau respons sebelumnya. Ini memungkinkan respons yang lebih cepat dan pengalaman pengguna yang lebih responsif, terutama dalam aplikasi real-time seperti chat dan panggilan.

#### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
gRPC menggunakan Protocol Buffers yang berbasis skema, memberikan keuntungan seperti ukuran payload yang lebih kecil, validasi data yang ketat, dan kinerja yang lebih baik. Ini berbeda dengan pendekatan JSON tanpa skema yang lebih fleksibel dalam payload REST API. Protocol Buffers membutuhkan definisi skema yang ketat sebelumnya, namun memberikan validasi data yang kuat dan kinerja yang efisien. Sementara JSON memungkinkan fleksibilitas dalam evolusi struktur data dan penggunaan yang lebih sederhana tanpa skema, meskipun dengan kemungkinan biaya kinerja yang sedikit lebih tinggi. Pemilihan antara kedua pendekatan ini tergantung pada kebutuhan kinerja, keamanan data, dan fleksibilitas yang diperlukan dalam aplikasi.