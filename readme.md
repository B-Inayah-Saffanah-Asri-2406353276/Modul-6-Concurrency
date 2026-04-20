# Modul 6 Concurrency

## Reflection 1
Pada Milestone 1, saya mengimplementasikan single-threaded web server sederhana menggunakan Rust yang mendengarkan koneksi TCP di `127.0.0.1:7878` dengan `TcpListener::bind()`. Setiap koneksi masuk diiterasi menggunakan `incoming()`, lalu di dalam fungsi `handle_connection`, `TcpStream` dibungkus dengan `BufReader` untuk membaca HTTP request baris demi baris menggunakan `.lines()`, berhenti saat menemukan blank line via `.take_while()`, dan hasilnya dicetak ke terminal. Di tahap ini server belum mengirim response apapun ke browser, sehingga halaman tidak menampilkan konten tapi koneksi berhasil terbentuk.

## Reflection 2
![alt text](/assets/milestone2.png)
Pada Milestone 2, saya memodifikasi fungsi `handle_connection` agar server tidak hanya menerima koneksi, tetapi juga mengirim response HTTP yang valid ke browser. Caranya adalah dengan membaca file `hello.html` menggunakan `fs::read_to_string()`, menyusun HTTP response format `HTTP/1.1 200 OK\r\nContent-Length: {length}\r\n\r\n{contents}`, lalu mengirimkannya ke browser menggunakan `stream.write_all()`. Dengan penambahan ini, browser kini berhasil menampilkan halaman HTML sederhana dan membuktikan bahwa server sudah bisa merespons request.