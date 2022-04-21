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
02. Untuk validasi email, bisa dengan mengubah value dari atribut 'type' menjadi email. Validasi ini hanya akan mengecek apakah data yang dimasukkan ada karrakter @ atau tidak.

VALIDASI EMAIL & NOHP
01. Untuk validasi email tidak cukup dengan mengandalkan validasi bawaan browser. Express sudah memiliki validasi email yang bisa digunakan, yaitu express-validator. Silahkan kunjungi dokumentasi nya di:

    https://www.npmjs.com/package/express-validator

02. Install express-validator versi 6.10.1
03. Jangan lupa untuk melakukan require(). Cara require nya bisa kunjungi

    https://express-validator.github.io/docs/

04. Kemudian gunakan pada app.post()
05. Matikan dulu dua metode dibawah ini agar data ujicoba yang diinput tidak langsung masuk ke contacts.json :
        addContact(req.body)
        res.redirect('/contact')


06. Tambahkan parameter body pada metode app.post(). Tambahkan sesuai jumlah kolom yang akan divalidasi. Sementara yang akan divalidasi kali ini adalah email dan nohp. Sehingga metode nya akan nampak seperti di bawah ini:

    app.post('/contact', [
        body('email').isEmail(),
        body('nohp').isMobilePhone('id-ID')
        ], (req, res) => {

    })

    Keterangan:
    parameter yang ada di dalam body() adalah nilai dari atribute name yang ada di add-contacts.ejs form tambah data. Nilai tersebut harus ditulis persis (baik besar maupun kecilnya huruf), sebab dari elemen yang memiliki nilai atribut tersebut, data yang dinput user ditangkap dan disimpan kedalam req.body

    isEmail() dan isMobilePhone() adalah metode yang disediakan express-validator untuk validasi email dan nohp

07. Kemudian hasil pengecekan akan masuk ke validationResault(). Metode ini digunakan didalam arrow function. Karena data yang diinput oleh user ditangkap oleh body, sedangkan body itu sendiri tersimpan di dalam parameter req, maka untuk mengetahui hasil validasi parameter req harus dicek apakah ada error atau tidak. cara ngeceknya bisa seperti yang ada documentasinya:

    (req, res) => {
        const errors = validationResult(req);
        if(!errors.isEmpty()){
            return res.status(400).json({ errors: errors.array() });
        }
    })

    Keterangan:
    Dengan metode res.status(400).json({ errors: errors.array() }) , maka jika terdapat error akan ditampilkan errornya pada browser

08. Selanjutnya lakukan ujicoba dengan memasukkan data yang tidak valid pada form tambah data.
09. Berikut hasil ujicoba

    errors: [
                {
                    value: "email!@kkk",
                    msg: "Invalid value",
                    param: "email",
                    location: "body"
                },
                {
                    value: "asassa",
                    msg: "Invalid value",
                    param: "nohp",
                    location: "body"
                }
            ]
    
    Keterangan:
    hasil dari validationResault() nya, akan menampilkan 4 informasi. Pertama, adalah value, informasi ini berisi data yang diinput oleh user. Kedua, msg, yaitu pesan error yang ingin disampaikan. Nilai msg bisa disetting sesuai keinginan, caranya ada pada tahap selanjutnya. Ketiga, param, ini adalah informasi nilai dari atribute name suatu elemen. Sehingga bisa diketahui berasal dari elemen yang manakah data tersebut diinput. Keempat adalah Location, berisi informasi tentng data yang diinput oleh user ditangkap/disimpan.

MEMBUAT CUSTOM ERROR MESSAGE
Masih menggunakan Express-Validator, adapun fasilitas yang bisa digunakan untuk menyetting pesan error sesua dengan keinginan. Silahkan lihat dokumentasi nya disini

    https://express-validator.github.io/docs/custom-error-messages.html

01. Sesuai dengan dokumentasinya. Silahkan require terlebih dahulu check.
02. Pada app.post(), ganti body dengan check.
03. Tambahkan parameter yang berisi pesan error yang ingin ditampilkan.

VALIDASI NAMA
Tidak boleh ada nama yang sama, jadi jika ada data baru dengan nama yang sudah ada pada data sebelumnya maka data tersebut akan ditolak. Caranya silahkan gunakan Custom validators/sanitizers yang sudah disediakan oleh express. Dokumentasi nya silahkan cek disini:

    https://express-validator.github.io/docs/custom-validators-sanitizers.html

01. Sesuai dokumentasi, silahkan tambahkan parameter body().custom((value) => {}) didalam array parameter
02. isi parameter body dengan atribut yang ada pada elemen tempat user input data nama, yaitu atribut nama.
03. Pada custom(value), value merujuk pada data nama yang akan dicek statusnya.
04. Buat skema pengecekan nama double. terlebih dahulu buat function cekDuplikat() di file /utils/contacts.js
05. Jangan lupa untuk export dan import function tersebut.
06. Kemudian gunakan function tersebut kedalam arrow functionnya body().custom((value) => {})
07. Cek variabel duplikat, apakah ada nama yang sama atau tidak. Jika ada maka ini salah, sehingga harus dibuat lah error baru dengan throw new Error()

    body('nama').custom((value) => {
        const duplikat = cekDuplikat(value)
        if(duplikat){
            throw new Error('Nama yang diinput sudah ada. Silahkan gunakan nama lain!')
        }
    }),

Selain itu tujuan membuat error baru adalah agar data yang dikirim masuk ke dalam validationResault. Jangan lupa apabila tidak ada duplikat, maka kembalikan dengan return true.

    body('nama').custom((value) => {
        const duplikat = cekDuplikat(value)
        if(duplikat){
            throw new Error('Nama yang diinput sudah ada. Silahkan gunakan nama lain!')
        }
        return true
    }),


HANDLE ERROR
Setelah melakukan custom error, maka tampilan error nya bisa dibuat lebih menarik. Jadi bukan menggunakan return res.status(400).json({ errors: errors.array() }); Tapi akan dikembalikan ke halaman add-contact.ejs dengan menampilkan alert error nya.

01. Matikan return res.status(400).json({ errors: errors.array() });
02. Render kembali halaman add-contact.ejs, dengan menambahkan nilai error pada paramater objeknya. Sehingga rendernya menjadi seperti ini:

        res.render('add-contact', {
            title: "Form Tambah Data Contact",
            layout: "layouts/main-layout",
            errors: errors.array()
        })
    
    Keterangan:
    Jika ada error pada data yang diinput oleh user, maka nilai error pada parameter objek di atas akan membawa 4 informasi seperti yang sudah dijelaskan sebelumnya, yaitu ada value, msg, param, dan location.

03. Dengan memanfaatkan informasi tersebut maka bisa dibuat tampilan html yang menarik agar user tahu bagian error nya yang mana
04. Buka kembali file add-contact.ejs
05. Buat pengkondisian yang akan mengecek isi dari errors, apakah ada error atau tidak. Hati-hati! Gunakan pengkondisian yang tepat.

    Pengkondisian yang tidak tepat.
    if ( errors ){}
    Pengkondisian diatas tidak akurat karena ada atau tidak ada isinya, maka errors akan mengembalikan nilai. Jika ada isinya maka errors akan mengembalikan nilai2 yang error, jika tidak ada isinya maka akan mengembalikan undifined. Oleh karena itu perlu hati-hati menggunakan pengkondisian yang parameternya merupakan array.

    Pengkondisian yang tepat adalah dengan mengecek status errornya, apakah undefined atau tidak. Jika errors ada isinya maka statusnya tidak undefined. Jika tidak ada isinya maka statusnya undefined. seperti di bawah ini

    <% if (typeof errors != 'undefined') { %>
             
    <% } %>

08. Lakukan sesuatu untuk menampilkan error yang ada. Siapkan template bootstrap berupa alert untuk menampilkan error
09. Isi template tersbut dengan ul>li, karena jika ada error maka error2 tersebut akan ditampilkan dalam bentuk list. Oleh karena itu bisa dilakukan looping pada elemen li nya. Sehingga akan nampak seperti ini

            <% if (typeof errors != 'undefined') { %>
              <div class="alert alert-danger" role="alert">
                <ul>
                    <% errors.forEach(error => { %>
                        <li><%= error.msg %> </li>
                    <% }) %>
                  </ul> 
                </div>
            <% } %>

JIKA SEMUA DATA YANG DIINPUT BENAR
Jika semua data yang diinput user benar, maka data tersebuy harus disimpan kedalam database. Fungsi untuk menyimpan data tersebut sudah pernah dibuat dan tinggal digunakan.

01. Pada app.js, metode app.post() tambahkan else dibawah pengkondisian if.
02. Isi else tersebut dengan function penyimpanan contact. sehingga kodenya menjadi seperti ini

    else{
        addContact(req.body)
        res.redirect('/contact')
    }


MEMBUAT FLASH MESSAGE
Selanjutnya adalah membuat flash message. Dimana pesan error yang tampilan akan hilang dalam waktu terttentu, tepatnya setelah browser di refresh.

Install terlebih dahulu modul yang digunakan untuk bekerja di dalam session dan cookie
01. Install Express-session@1.17.2
02. Install cookie-parser@1.4.5

Kemudian install modul yang akan bekerja ada session flash
03. Install npm i connect-flash@0.1.1

04. lakukan require di app.js
05. Lakukan Konfigurasi flash
06. jalankan dengan beberapa langkah berikut

Menjalankan Proses Flash
01. Set di dalam app.post(), set dengan menambahkan metode res.flash() sebelum halaman menampilkan data baru. Tepatnya sebelum res.redirect()

    req.flash('pesan', 'Data Contact berhasil ditambahkan')

    Ket:
    pada metode res.flash() terdapat dua parameter. Parameter pertama adalah nama variable, dan yang kedua adalah isi dari variabel tersebut.

02. Set di dalam metode yang merender halaman contact, yaitu app.get('contact', )
03. Didalam metode tersebut ada metode res.render(), tambahkan variabel 'msg' (namanya bebas, tidak harus msg) didalam parameter objectnya. Dimana variabel tersebut akan berisi data pesan (message yang dikirim oleh app.post()). sehingga code nya akan menjadi seperti dibawah ini:

    res.render('contact', {
        title: "Halaman Contact",
        layout: "layouts/main-layout",
        contacts,
        msg: req.flash('pesan')
    })

04. setting di halaman contact.ejs, karena halamn ini lah yang nantinya akan menampilkan message nya.
05. Lakukan pengkondisian pada variable 'msg', apakah kosong atau ada isinya, seperti berikut ini:

    <% if (msg.length !== 0) { %>
        <div class="alert alert-success mt-3" role="alert">
        <%= msg %> 
        </div>
    <% } %>



