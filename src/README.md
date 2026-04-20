# Modul 6 Concurrency

# Reflection 1
Dari dokumentasi Rust, saya memahami bahwa BufReader digunakan sebab membaca langsung dari TcpStream  tidak efisien dimana setiap pembacaan akan memanggil system call. Dengan BufReader, data dibuffer dulu di memori sehingga lebih efisien.

Di fungsi handle_connection, BufReader membaca HTTP request baris per baris via .lines(), dan berhenti saat menemukan baris kosong menggunakan .take_while(|line| !line.is_empty()) karena HTTP memang menggunakan blank line sebagai penanda akhir header. Hasilnya dicetak ke console. Pada tahap ini server belum mengirim response, sehingga browser tidak menampilkan apapun namun request sudah berhasil terbaca.