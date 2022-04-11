# WebServer-ExpressJS-04-Express-ContactAppV2-Lanjutan
Melanjutkan Pembuatan ContactApp dari Latihan sebelumnya.. Kali ini dengan menambahkan beberapa fitur pengelolaan kontak, seperti tombol tambah data, validasi, dan flash massage


TAMBAH DATA KONTAK

01. Buat dulu tombol "Tambabh Data" di halaman contact.ejs, buat dengan tag <a href=""> yang href-nya mengarah ke halaman "/contact/add"
02. Buat rout nya di halaman app.js dimana rout ini akan menangani reques dengan link "/contact/add" .Harap diingat!! rout semacam in harus diletakkan pada posisi yang tepat. Hal ini karena default EXPRESS akan menjalankan perintah seperti air mengalir, dari atas ke bawah
03. Posisi yang tepat untuk meletakkan rout  ini adalah di atas rout yang menangani request "/contact/:nama". Sehingga susunannya seperti ini:

    app.get('/contact/add', (req, res) => {
        res.render()
    })

    app.get('/contact/:nama', (req, res) => {
        res.render()
    })

04. Jangan Lupa buat halaman add-contact.ejs. Agar ketika ada request dengan url "/contact/add", maka sistem akan meresponse dengan halaman tersebut.
05. Halaman add-contact.ejs berisi formulir untuk menambahkan data contact.

==============================================================================
PROSES TAMBAH DATA
Setelah halaman add-contact.ejs jadi, selanjutnya adalah mengelola data yang ditambahkan oleh user. Ada beberapa tahap sebelum sistem menyimpan data yang di tambahkan, yaitu pembuatan rout penyimpanan, proses validasi, dan proses penyimpanan.

PEMBUATAN ROUT
01. Buat terlebih dahulu rout pada app.js yang akan menangani request tambah data.
02. Disaat bersamaan setting atribut form yang ada di halaman add-contact.ejs dengan metode="post" dan action="/contact".
03. Kembali lagi ke halaman app.js, buat rout
    app.post('/contact', (req, res) => {

    })

    middleware diatas akan menangani request dengan url "/contact" yang metodenya post. Metode post adalah perintah untuk menambahkan data pada sistem. Beda dengan get, kalau get adalah perintah untuk meminta informasi dari sistem.
04. Apa yang diinput oleh user akan ditangkap oleh req, yang tersimpan didalam parameter body. Sehingga ketika akan melihatnya/menggunakannya, maka tinggal panggil saja req.body()

    app.post('/contact', (req, res) => {
        console.log(req.body)
    })

    Hati-hati, Yang Pertama, ketika rout diatas disimpan, kemudian pada halaman browser diklik "simpan data", maka sistem akan hanging. Pada halaman tersebut url nya akan muter-muter dan pada terminal akan hang. Oleh karena itu setelah consol.log(req.body), maka bisa ditambahkan dengan metode yang lain, semisal res.send(). seperti dibawah ini:

    app.post('/contact', (req, res) => {
        console.log(req.body)
        res.send("data berhasil dikirim")
    })

    Yang Kedua, saat rout diatas sudah dijalankan maka tulisan "data berhasil dikirim" akan dimunculkan dilayar. Yang artinya bahwa data tersebut sudah berhasil dikirim ke sistem.
    Seharusnya data yang dikirim tersebut bisa dilihat diterminal, karena pada rout ada perintah console.log(req.body) yang akan mencetak data yang ditangkap oleh body ke terminal. Namun faktanya saat dicek diterminal ternyata data tersebut statusnya undifined. Hal ini terjadi karena data yang dikirim harus diparsing dulu menggunakan builtin-middleware express.urlencoded(). Oleh karena itu segera impor middleware tersebut, dan refresh halaman browsernya.

        app.use(express.urlencoded())
    
    Kemudian cek pada terminal, maka akan muncul data2 yang telah dikirim. Selain dicetak pada console.log(), data tersebut juga bisa langsung dikirim ke halaman browser dengan menggunakan metode re.send(). Sehingga rout akan berubah seperti ini:

        app.post('/contact', (req, res) => {
            res.send(req.body)
        })

05. Langkah-langkah diatas sebenarnya adalah uji coba untuk melihat apakah benar data yang dimasukkan user ditangkap oleh req.body, Selain itu juga menjadi jalan untuk menghindari status hang yang dialami oleh terminal atau loading terus menerus oleh browser akibat sistem tidak tahu harus direspon dengan apakah sebuah request dg url /contact yang metode nya post. Setelah tahu dengan pasti dimana data yang diinput user disimpan maka langkah selanjutnya adalah membuat skenario proses data.

06. Sebelumnya hapus terlebih dahulu metode res.send() yang ada didalam app.post('/contact', (req, res) => {}). Karena itu sudah tidak digunakan, dan akan diganti dengan function proses tambah data dan halaman browser yang tampil setelah data berhasil disimpan.

07. Buat dulu function yang akan memproses penambahan data. Ada dua function yang akan dibuat yaitu saveContacts dan addContact. Keduanya dibuat pada file contact.js yang berada di folder utils

08. Function saveContacts berfungsi untuk menimpa isi dari contacts.json dengan data baru (data yang sudah ada penambahannya)

09. Function addContact bertugas untuk menambahkan data user yang baru ke contacts.json

10. Jangan lupa export functioan addContact() dan import ke app.js

11. Selesai



PROSES VALIDASI
01. Agar kolom isian disetting wajib diisi maka tambahkan atribut "required" pada html tag input.
02. Untuk validasi email, bisa dengan mengubah atribut type menjadi email.
03. 

PROSES PENYIMPANAN
