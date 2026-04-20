# Modul 6 Concurrency

## Reflection 1
Pada Milestone 1, saya mengimplementasikan single-threaded web server sederhana menggunakan Rust yang mendengarkan koneksi TCP di `127.0.0.1:7878` dengan `TcpListener::bind()`. Setiap koneksi masuk diiterasi menggunakan `incoming()`, lalu di dalam fungsi `handle_connection`, `TcpStream` dibungkus dengan `BufReader` untuk membaca HTTP request baris demi baris menggunakan `.lines()`, berhenti saat menemukan blank line via `.take_while()`, dan hasilnya dicetak ke terminal. Di tahap ini server belum mengirim response apapun ke browser, sehingga halaman tidak menampilkan konten tapi koneksi berhasil terbentuk.

## Reflection 2
![alt text](/assets/milestone2.png)
Pada Milestone 2, saya memodifikasi fungsi `handle_connection` agar server tidak hanya menerima koneksi, tetapi juga mengirim response HTTP yang valid ke browser. Caranya adalah dengan membaca file `hello.html` menggunakan `fs::read_to_string()`, menyusun HTTP response format `HTTP/1.1 200 OK\r\nContent-Length: {length}\r\n\r\n{contents}`, lalu mengirimkannya ke browser menggunakan `stream.write_all()`. Dengan penambahan ini, browser kini berhasil menampilkan halaman HTML sederhana dan membuktikan bahwa server sudah bisa merespons request.

## Reflection 3
![alt text](/assets/milestone3.png)
Pada Milestone 3, saya menambahkan kondisi untuk membedakan request yang masuk dan merespons secara beda. Daripada membaca seluruh request ke dalam vector, saya hanya mengambil baris pertama menggunakan `.next()` untuk mendapatkan `request_line`, lalu mengecek apakah isinya `"GET / HTTP/1.1"`. Jika ya, server mengembalikan `hello.html` dengan status `200 OK`; jika tidak, server mengembalikan `404.html` dengan status `404 NOT FOUND`. Saya juga melakukan refactoring untuk menghilangkan duplikasi kode blok `if/else` kini hanya menentukan `status_line` dan `filename` sebagai tuple, sementara logika membaca file dan menulis response cukup ditulis sekali di luar blok tersebut, membuat kode lebih ringkas.

## Reflection 4
![alt text](/assets/milestone4.png)
Pada Milestone 4, saya menambahkan endpoint `/sleep` yang sengaja menunda response selama beberapa detik menggunakan `thread::sleep(Duration::from_secs(10))`. Ketika dua browser window dibuka bersamaan, satu mengakses `/sleep` dan satu mengakses `/`, window kedua harus menunggu hingga request `/sleep` selesai diproses sebelum mendapat response. Ini terjadi karena server hanya memiliki satu thread, sehingga setiap request harus mengantri secara sekuensial. Simulasi ini membuktikan bahwa arsitektur single-threaded tidak scalable apalagi jika ratusan user mengakses server secara bersamaan.