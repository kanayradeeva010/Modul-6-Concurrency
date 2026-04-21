# Modul 6 Concurrency

# Reflection 1
Dari dokumentasi Rust, saya memahami bahwa BufReader digunakan sebab membaca langsung dari TcpStream  tidak efisien dimana setiap pembacaan akan memanggil system call. Dengan BufReader, data dibuffer dulu di memori sehingga lebih efisien.

Di fungsi handle_connection, BufReader membaca HTTP request baris per baris via .lines(), dan berhenti saat menemukan baris kosong menggunakan .take_while(|line| !line.is_empty()) karena HTTP memang menggunakan blank line sebagai penanda akhir header. Hasilnya dicetak ke console. Pada tahap ini server belum mengirim response, sehingga browser tidak menampilkan apapun namun request sudah berhasil terbaca.

# Reflection 2
Di Milestone 2, saya menambahkan kemampuan server untuk benar-benar membalas request dari browser. Sebelumnya server hanya bisa menerima koneksi tanpa mengirim apapun balik. Sekarang, handle_connection membaca file hello.html menggunakan fs::read_to_string(), lalu menyusun HTTP response lengkap yang terdiri dari status line HTTP/1.1 200 OK, header Content-Length yang berisi ukuran konten, dan isi HTML-nya sendiri. Semua itu dikirim ke browser lewat stream.write_all(). Hasilnya, browser sekarang bisa menampilkan halaman HTML dengan benar.

# Reflection 3
Di Milestone 3, saya belajar bahwa server tidak perlu membaca seluruh HTTP request untuk menentukan response yang tepat tetapi cukup baca baris pertama saja menggunakan .lines().next() karena di situlah method dan path request berada. Dari request_line tersebut, server bisa memutuskan apakah akan mengembalikan hello.html dengan 200 OK atau 404.html dengan 404 NOT FOUND.

Refactoring dengan match juga membuat kode jauh lebih bersih. Sebelumnya logika baca file dan kirim response berulang di tiap kondisi, sekarang bagian tersebut cukup ditulis sekali di luar blok match karena yang berbeda hanyalah status_line dan filename-nya saja.

# Reflection 4
Pada Milestone 4, saya mensimulasikan kelemahan single-threaded server dengan menambahkan route /sleep yang membuat server tidur selama 10 detik menggunakan thread::sleep(Duration::from_secs(10)). Ketika dua browser window dibuka bersamaan, satu ke /sleep dan satu ke / window kedua ikut menunggu hingga /sleep selesai diproses. Ini terjadi karena server hanya punya satu thread, sehingga request hanya bisa diproses satu per satu secara berurutan. Hal ini membuktikan bahwa single-threaded server tidak cocok untuk production karena satu request yang lambat bisa memblokir semua request lainnya.


