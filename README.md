# WebServer-ExpressJS-04-Express-ContactAppV2-Lanjutan
Melanjutkan Pembuatan ContactApp dari Latihan sebelumnya.. Kali ini dengan menambahkan beberapa fitur pengelolaan kontak, seperti tombol tambah data, validasi, dan flash massage


TAMBAH DATA KONTAK

01. Buat dulu tombol "Tambabh Data" di halaman contact.ejs, buat dengan tag <a href=""> yang href-nya mengarah ke halaman "/contact/add"
02. Buat rout nya di halaman app.js dimana rout ini akan menangani reques dengan link "/contact/add" .Harap diingat!! rout semacam in harus diletakkan pada posisi yang tepat. Hal ini karena default EXPRESS akan menjalankan perintah seperti air mengalir, dari atas ke bawah
03. Oleh karena itu rout ini harus diletakkan di atas rout yang menangani request "/contact/:nama". Sehingga susunannya seperti ini:

    app.get('/contact/add', (req, res) => {
        res.render()
    })

    app.get('/contact/:nama', (req, res) => {
        res.render()
    })

04. Jangan Lupa buat halaman add-contact.ejs untuk meampilkan respon dari request "/contact/add"