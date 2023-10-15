# Submission-Bookshelf-API
Terdapat 7 Kriteria utama yang harus dipenuhi dalam membuat proyek Bookshelf API.

* KRITERIA 1 : APLIKASI MENGGUNAKAN PORT 9000
  Aplikasi yang dibuat harus menggunakan port 9000. Jika komputer yang digunakan untuk membuat
  submission tidak bisa memakai port 9000, buatlah submission dengan port lain, lalu ketika
  submission hendak dikirimkan silahkan ganti portnya ke 9000.
  
* KRITERIA 2 :  APLIKASI DIJALANKAN DENGAN PERINTAH "npm run start"
  Aplikasi yang dibuat harus memiliki runner script start. Cara membuatnya, tambahkan properti
  start ke dalam properti scripts pada package.json seperti berikut :

  {
  "name": "submission",
  ...
  "scripts": {
    "start": "node src/server.js",
              }
  }

  Pastikan aplikasi tidak dijalankan dengan menggunakan nodemon. Jika ingin menggunakan nodemon
  dalam proses development, masukkan nodemon ke dalam runner script lain. contohnya :

  {
  "name": "submission",
  ...
  "scripts": {
    "start": "node src/server.js",
    "start-dev": "nodemon src/server.js",
              }
  }
  
* KRITERIA 3 : API DAPAT MENYIMPAN BUKU
  API yang dibuat harus dapat menyimpan buku melalui route :
  • Method : POST
  • URL : /books
  • Body Request :
    {
    "name": string,
    "year": number,
    "author": string,
    "summary": string,
    "publisher": string,
    "pageCount": number,
    "readPage": number,
    "reading": boolean
    }
  
  Objek buku yang disimpan pada server harus memiliki struktur seperti contoh di bawah ini:
  {
    "id": "Qbax5Oy7L8WKf74l",
    "name": "Buku A",
    "year": 2010,
    "author": "John Doe",
    "summary": "Lorem ipsum dolor sit amet",
    "publisher": "Dicoding Indonesia",
    "pageCount": 100,
    "readPage": 25,
    "finished": false,
    "reading": false,
    "insertedAt": "2021-03-04T09:11:44.598Z",
    "updatedAt": "2021-03-04T09:11:44.598Z"
  }

  Properti yang ditebalkan diolah dan didapatkan di sisi server. Berikut penjelasannya :
  • id
    Nilai id haruslah unik. Untuk membuat nilai unik, anda bisa menggunakan nanoid. untuk anda yang
    menggunakan CommonJS untuk sistem modularisasi, pastikan memasang nanoid versi 3 melalui perintah :
    npm install nanoid@3.
  • finished
    Merupakan properti boolean yang menjelaskan apakah buku telah selesai dibaca atau belum. Nilai finished
    didapatkan dari observasi pageCount === readPage.
  • insertedAt
    Merupakan properti yang menampung tanggal dimasukkannya buku. Anda bisa gunakan new.Date().tolSOString()
    untuk menghasilkan nilainya.
  • updatedAt
    Merupakan properti yang menampung tanggal diperbarui buku. Ketika buku baru dimasukkan, berikan nilai properti
    ini sama dengan insertedAt.

Server harus merespons gagal bila :
• Client tidak melampirkan properti name pada request body.
  Bila hal ini terjadi, maka server akan merespons dengan :
  𓈒 Status Code : 400
  𓈒 Response Body :
    {
    "status": "fail",
    "message": "Gagal menambahkan buku. Mohon isi nama buku"
    }
    
• Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, 
  maka server akan merespons dengan :
  𓈒 Status Code : 400
  𓈒 Response Body :
    {
    "status": "fail",
    "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
    }

  Bila buku berhasil dimasukkan, server harus mengembalikan respons dengan :
  • Status Code : 201
  • Response Body :
    {
    "status": "success",
    "message": "Buku berhasil ditambahkan",
    "data": {
        "bookId": "1L7ZtDUFeGs7VlEt"
            }
    }

* KRITERIA 4 : API DAPAT MENAMPILKAN SELURUH BUKU
  API yang anda buat harus dapat menampilkan seluruh buku yang disimpan melalui route :
  • Method : GET
  • URL : /books

  Server harus mengembalikan respons dengan :
  • Status Code : 200
  • Response Body :
  {
    "status":  "success",
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
  }

  Jika belum terdapat buku yang dimasukkan, server bisa merespons dengan array books kosong.
  {
    "status": "success",
    "data": {
        "books": []
    }
  }

* KRITERIA 5 : API DAPAT MENAMPILKAN DETAIL BUKU
  API yang anda buat harus dapat menampilkan seluruh buku yang disimpan melalui route :
  • Method : GET
  • URL : /books/{bookId}

  Bila buku dengan Id yang ditampilkan oleh client tidak ditemukan, maka server harus mengembalikan respons dengan :
  • Status Code : 404
  • Response Body :
    {
    "status": "fail",
    "message": "Buku tidak ditemukan"
    }

  Bila buku dengan id yang dilampirkan ditemukan, maka server harus mengembalikan respons dengan :
  • Status Code : 200
  • Response Body :
  {
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
  }

* KRITERIA 6 : API DAPAT MENGUBAH DATA BUKU
  API yang anda buat harus dapat mengubah data buku berdasarkan id melalui route :
  • Method : PUT
  • URL : /books/{bookId}
  • Body Request :
    {
    "name": string,
    "year": number,
    "author": string,
    "summary": string,
    "publisher": string,
    "pageCount": number,
    "readPage": number,
    "reading": boolean
    }

  Server harus merespons gagal bila :
  • Client tidak melampirkan properti name pada request body. Bila hal ini terjadi,
    maka server akan merespons dengan :
    𓈒 Status Code : 400
    𓈒 Response Body:
    {
    "status": "fail",
    "message": "Gagal memperbarui buku. Mohon isi nama buku"
    }

  • Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount.
    Bila hal ini terjadi, maka server akan merespons dengan :
    𓈒 Status Code : 400
    𓈒 Response Body:
    {
    "status": "fail",
    "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
    }

  • Id yang dilampirkan oleh client tidak ditemukan oleh server. Bila hal ini terjadi, maka server
    akan merespons dengan :
    𓈒 Status Code : 400
    𓈒 Response Body:
    {
    "status": "fail",
    "message": "Gagal memperbarui buku. Id tidak ditemukan"
    }

  • Bila buku berhasil diperbarui, server harus mengembalikan respons dengan :
    𓈒 Status Code : 200
    𓈒 Response Body:
    {
    "status": "success",
    "message": "Buku berhasil diperbarui"
    }

* KRITERIA 7 : API DAPAT MENGHAPUS BUKU
  API yang anda buat harus dapat menghapus buku berdasarkan id melalui route berikut:
  𓈒 Method : DELETE
  𓈒 URL : /books/{bookId}

  Bila id yang dilampirkan tidak dimiliki oleh buku manapun, maka server harus mengembalikan
  respons berikut :
  𓈒 Status Code : 404
  𓈒 Response Body:
  {
    "status": "fail",
    "message": "Buku gagal dihapus. Id tidak ditemukan"
  }

  Bila id dimiliki oleh salah satu buku, maka buku tersebut harus dihapus dan server mengembalikan respons berikut :
  𓈒 Status Code : 200
  𓈒 Response Body:
  {
    "status": "success",
    "message": "Buku berhasil dihapus"
  }
  
  
  
