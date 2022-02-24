# Bookshelf API
Proyek <b>Bookshelf API</b> adalah submission <a href="https://www.dicoding.com">dicoding</a> yang saya kerjakan untuk lulus dari kelas back-end.
<br>Berikut beberapa kriteria dari Bookshelf API dalam project ini:</br>

<b>Kriteria 1 : API dapat menyimpan buku</b>
<br>API yang Anda buat harus dapat menyimpan buku melalui route:

<ul>
  <li>Method : POST</li>
  <li>URL : /books</li>
  <li>Body Request:</li>
</ul>

<pre>{
    "name": string,
    "year": number,
    "author": string,
    "summary": string,
    "publisher": string,
    "pageCount": number,
    "readPage": number,
    "reading": boolean
}</pre>
Objek buku yang disimpan pada server harus memiliki struktur seperti contoh di bawah ini:

<pre>{
    <b>"id"</b>: "Qbax5Oy7L8WKf74l",
    "name": "Buku A",
    "year": 2010,
    "author": "John Doe",
    "summary": "Lorem ipsum dolor sit amet",
    "publisher": "Dicoding Indonesia",
    "pageCount": 100,
    "readPage": 25,
    <b>"finished"</b>: false,
    "reading": false,
    <b>"insertedAt"</b>: "2021-03-04T09:11:44.598Z",
    <b>"updatedAt"</b>: "2021-03-04T09:11:44.598Z"
}</pre>
Properti yang ditebalkan diolah dan didapatkan di sisi server. Berikut penjelasannya:

<br><b>id</b> : nilai id haruslah unik. Untuk membuat nilai unik, Anda bisa memanfaatkan nanoid.
<br><b>finished</b> : merupakan properti boolean yang menjelaskan apakah buku telah selesai dibaca atau belum. Nilai finished didapatkan dari observasi pageCount === readPage.
<br><b>insertedAt</b> : merupakan properti yang menampung tanggal dimasukkannya buku. Anda bisa gunakan new Date().toISOString() untuk menghasilkan nilainya.
<br><b>updatedAt</b> : merupakan properti yang menampung tanggal diperbarui buku. Ketika buku baru dimasukkan, berikan nilai properti ini sama dengan insertedAt.

<br>Server harus merespons gagal bila:

<ul><li>Client tidak melampirkan properti name pada request body. Bila hal ini terjadi, maka server akan merespons dengan:</li>
<ul type="disc">
  <li>Status Code : 400</li>
  <li>Response Body:</li>
</ul></ul>

<pre>{
    "status": "fail",
    "message": "Gagal menambahkan buku. Mohon isi nama buku"
}</pre>
Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka server akan merespons dengan:
<ul>
  <li>Status Code : 400</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "fail",
    "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
}</pre>
Server gagal memasukkan buku karena alasan umum (generic error). Bila hal ini terjadi, maka server akan merespons dengan:
<ul>
  <li>Status Code : 500</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "error",
    "message": "Buku gagal ditambahkan"
}</pre>
Bila buku berhasil dimasukkan, server harus mengembalikan respons dengan:

<ul>
  <li>Status Code : 201</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "success",
    "message": "Buku berhasil ditambahkan",
    "data": {
        "bookId": "1L7ZtDUFeGs7VlEt"
    }
}</pre>


<br><b>Kriteria 2 : API dapat menampilkan seluruh buku</b></br>
API yang Anda buat harus dapat menampilkan seluruh buku yang disimpan melalui route:

<ul>
  <li>Method : GET</li>
  <li>URL: /books</li>
</ul>

Server harus mengembalikan respons dengan:

<ul>
  <li>Status Code : 200</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "success",
    "data": {
        "books": [
            {
                "id": "Qbax5Oy7L8WKf74l",
                "name": "Buku A",
                "publisher": "Dicoding Indonesia"
            },
            {
                "id": "1L7ZtDUFeGs7VlEt",
                "name": "Buku B",
                "publisher": "Dicoding Indonesia"
            },
            {
                "id": "K8DZbfI-t3LrY7lD",
                "name": "Buku C",
                "publisher": "Dicoding Indonesia"
            }
        ]
    }
}</pre>
Jika belum terdapat buku yang dimasukkan, server bisa merespons dengan array books kosong.

<pre>{
    "status": "success",
    "data": {
        "books": []
    }
}</pre>


<br><b>Kriteria 3 : API dapat menampilkan detail buku</b></br>
API yang Anda buat harus dapat menampilkan seluruh buku yang disimpan melalui route:

<ul>
  <li>Method : GET</li>
  <li>URL: /books/{bookId}</li>
</ul>

Bila buku dengan id yang dilampirkan oleh client tidak ditemukan, maka server harus mengembalikan respons dengan:

<ul>
  <li>Status Code : 404</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "fail",
    "message": "Buku tidak ditemukan"
}</pre>
Bila buku dengan id yang dilampirkan ditemukan, maka server harus mengembalikan respons dengan:

<ul>
  <li>Status Code : 200</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "success",
    "data": {
        "book": {
            "id": "aWZBUW3JN_VBE-9I",
            "name": "Buku A Revisi",
            "year": 2011,
            "author": "Jane Doe",
            "summary": "Lorem Dolor sit Amet",
            "publisher": "Dicoding",
            "pageCount": 200,
            "readPage": 26,
            "finished": false,
            "reading": false,
            "insertedAt": "2021-03-05T06:14:28.930Z",
            "updatedAt": "2021-03-05T06:14:30.718Z"
        }
    }
}</pre>


<br><b>Kriteria 4 : API dapat mengubah data buku</b></br>
API yang Anda buat harus dapat mengubah data buku berdasarkan id melalui route:

<ul>
  <li>Method : PUT</li>
  <li>URL : /books/{bookId}</li>
  <li>Body Request:</li>
</ul>

<pre>{
    "name": string,
    "year": number,
    "author": string,
    "summary": string,
    "publisher": string,
    "pageCount": number,
    "readPage": number,
    "reading": boolean
}</pre>
Server harus merespons gagal bila:

<ul>
  <li>Client tidak melampirkan properti name pada request body. Bila hal ini terjadi, maka server akan merespons dengan:</li>
<ul type='disc'>
  <li>Status Code : 400</li>
  <li>Response Body:</li>
</ul></ul>

<pre>{
    "status": "fail",
    "message": "Gagal memperbarui buku. Mohon isi nama buku"
}</pre>
Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka server akan merespons dengan:
<ul>
  <li>Status Code : 400</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "fail",
    "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
}</pre>
Idyang dilampirkan oleh client tidak ditemukkan oleh server. Bila hal ini terjadi, maka server akan merespons dengan:
<ul>
  <li>Status Code : 404</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "fail",
    "message": "Gagal memperbarui buku. Id tidak ditemukan"
}</pre>
Bila buku berhasil diperbarui, server harus mengembalikan respons dengan:

<ul>
  <li>Status Code : 200</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "success",
    "message": "Buku berhasil diperbarui"
}</pre>


<br><b>Kriteria 5 : API dapat menghapus buku</b></br>
API yang Anda buat harus dapat menghapus buku berdasarkan id melalui route berikut:

<ul>
  <li>Method : DELETE</li>
  <li>URL: /books/{bookId}</li>
</ul>
Bila id yang dilampirkan tidak dimiliki oleh buku manapun, maka server harus mengembalikan respons berikut:

<ul>
  <li>Status Code : 404</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "fail",
    "message": "Buku gagal dihapus. Id tidak ditemukan"
}</pre>
Bila id dimiliki oleh salah satu buku, maka buku tersebut harus dihapus dan server mengembalikan respons berikut:

<ul>
  <li>Status Code : 200</li>
  <li>Response Body:</li>
</ul>

<pre>{
    "status": "success",
    "message": "Buku berhasil dihapus"
}</pre>

# Pengujian API
Untuk melakukan pengujian <b>Bookshelf API</b> ini, saya menggunakan <b>postman</b>. Berkas Collection dan Environment terdapat pada directory <i>postman</i> saya atau bisa <a href="https://www.">klik disini</a>
