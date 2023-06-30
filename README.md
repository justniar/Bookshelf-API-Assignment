# Bookshelf-API-Assignment

## **Kriteria**

Terdapat 7 kriteria utama yang harus Anda penuhi dalam membuat proyek Bookshelf API.

### **Kriteria 1 : Aplikasi menggunakan port 9000**

Aplikasi yang Anda buat harus menggunakan port 9000. Jika komputer yang Anda gunakan untuk membuat submission tidak bisa memakai port 9000,  buatlah submission dengan port lain, lalu ketika submission hendak dikirimkan silakan ganti portnya ke 9000.

### **Kriteria 2 : Aplikasi dijalankan dengan perintah npm run start.**

Aplikasi yang Anda buat harus memiliki runner script start. Cara membuatnya, Anda tambahkan properti start ke dalam properti scripts pada **package.json** seperti berikut:

```bash

1. {
2.   "name": "submission",
3.   ...
4.   "scripts": {
5.     **"start": "node src/server.js",**
6.   }
7. }
```

Pastikan aplikasi **tidak** dijalankan dengan menggunakan **nodemon**. Jika Anda ingin menggunakan nodemon dalam proses development, masukkan nodemon kedalam runner script lain, contohnya:

```bash

1. {
2.   "name": "submission",
3.   ...
4.   "scripts": {
5.     "start": "node src/server.js",
6.     **"start-dev": "nodemon src/server.js",**
7.   }
8. }
```

### **Kriteria 3 : API dapat menyimpan buku**

API yang Anda buat harus dapat menyimpan buku melalui *route*:

- Method : **POST**
- URL : **/books**
- Body Request:
    
    ```bash
    
        1. {
        2.     "name": string,
        3.     "year": number,
        4.     "author": string,
        5.     "summary": string,
        6.     "publisher": string,
        7.     "pageCount": number,
        8.     "readPage": number,
        9.     "reading": boolean
        10. }
    ```
    

Objek buku yang disimpan pada *server* harus memiliki struktur seperti contoh di bawah ini:

```bash

1. {
2.     **"id": "Qbax5Oy7L8WKf74l",**
3.     "name": "Buku A",
4.     "year": 2010,
5.     "author": "John Doe",
6.     "summary": "Lorem ipsum dolor sit amet",
7.     "publisher": "Dicoding Indonesia",
8.     "pageCount": 100,
9.     "readPage": 25,
10.     **"finished": false,**
11.     "reading": false,
12.     **"insertedAt": "2021-03-04T09:11:44.598Z",**
13.     **"updatedAt": "2021-03-04T09:11:44.598Z"**
14. }
```

Properti yang ditebalkan diolah dan didapatkan di sisi *server*. Berikut penjelasannya:

- id : nilai id haruslah unik. Untuk membuat nilai unik, Anda bisa memanfaatkan [nanoid](https://www.npmjs.com/package/nanoid).
- finished : merupakan properti *boolean* yang menjelaskan apakah buku telah selesai dibaca atau belum. Nilai finished didapatkan dari observasi pageCount === readPage.
- insertedAt : merupakan properti yang menampung tanggal dimasukkannya buku. Anda bisa gunakan new Date().toISOString() untuk menghasilkan nilainya.
- updatedAt : merupakan properti yang menampung tanggal diperbarui buku. Ketika buku baru dimasukkan, berikan nilai properti ini sama dengan insertedAt.

Server harus merespons **gagal** bila:

- Client tidak melampirkan properti namepada *request body*. Bila hal ini terjadi, maka *server* akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        
        ```bash
        
                1. {
                2.     "status": "fail",
                3.     "message": "Gagal menambahkan buku. Mohon isi nama buku"
                4. }
        ```
        
- Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka *server* akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        
        ```bash
        
                1. {
                2.     "status": "fail",
                3.     "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
                4. }
        ```
        

Bila buku **berhasil** dimasukkan, *server* harus mengembalikan respons dengan:

- Status Code : **201**
- Response Body:
    
    
        `1. {
        2.     "status": "success",
        3.     "message": "Buku berhasil ditambahkan",
        4.     "data": {
        5.         "bookId": "1L7ZtDUFeGs7VlEt"
        6.     }
        7. }`
    

### **Kriteria 4 : API dapat menampilkan seluruh buku**

API yang Anda buat harus dapat menampilkan seluruh buku yang disimpan melalui *route*:

- Method : **GET**
- URL: **/books**

Server harus mengembalikan respons dengan:

- Status Code : **200**

```bash
Response Body
    1. {
    2.     "status": "success",
    3.     "data": {
    4.         "books": [
    5.             {
    6.                 "id": "Qbax5Oy7L8WKf74l",
    7.                 "name": "Buku A",
    8.                 "publisher": "Dicoding Indonesia"
    9.             },
    10.             {
    11.                 "id": "1L7ZtDUFeGs7VlEt",
    12.                 "name": "Buku B",
    13.                 "publisher": "Dicoding Indonesia"
    14.             },
    15.             {
    16.                 "id": "K8DZbfI-t3LrY7lD",
    17.                 "name": "Buku C",
    18.                 "publisher": "Dicoding Indonesia"
    19.             }
    20.         ]
    21.     }
    22. }
```

```bash
Jika **belum** terdapat buku yang dimasukkan, *server* bisa merespons dengan *array* books kosong.
1. {
2.     "status": "success",
3.     "data": {
4.         "books": []
5.     }
6. }
```

### **Kriteria 5 : API dapat menampilkan detail buku**

API yang Anda buat harus dapat menampilkan seluruh buku yang disimpan melalui *route*:

- Method : **GET**
- URL: **/books/{bookId}**

Bila buku dengan id ****yang dilampirkan oleh *client* tidak ditemukan, maka *server* harus mengembalikan respons dengan:

- Status Code : **404**

```bash
Response Body
    1. {
    2.     "status": "fail",
    3.     "message": "Buku tidak ditemukan"
    4. }
```

Bila buku dengan id ****yang dilampirkan **ditemukan**, maka *server* harus mengembalikan respons dengan:

- Status Code : **200**
- Response Body:
    
    ```bash
        1. {
        2.     "status": "success",
        3.     "data": {
        4.         "book": {
        5.             "id": "aWZBUW3JN_VBE-9I",
        6.             "name": "Buku A Revisi",
        7.             "year": 2011,
        8.             "author": "Jane Doe",
        9.             "summary": "Lorem Dolor sit Amet",
        10.             "publisher": "Dicoding",
        11.             "pageCount": 200,
        12.             "readPage": 26,
        13.             "finished": false,
        14.             "reading": false,
        15.             "insertedAt": "2021-03-05T06:14:28.930Z",
        16.             "updatedAt": "2021-03-05T06:14:30.718Z"
        17.         }
        18.     }
        19. }
    ```
    

### **Kriteria 6 : API dapat mengubah data buku**

API yang Anda buat harus dapat mengubah data buku berdasarkan id melalui *route*:

- Method : **PUT**
- URL : **/books/{bookId}**
- Body Request:
    
    ```bash
    
        1. {
        2.     "name": string,
        3.     "year": number,
        4.     "author": string,
        5.     "summary": string,
        6.     "publisher": string,
        7.     "pageCount": number,
        8.     "readPage": number,
        9.     "reading": boolean
        10. }
    ```
    

Server harus merespons **gagal** bila:

- Client tidak melampirkan properti name pada *request body*. Bila hal ini terjadi, maka *server* akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        
        ```bash
        
                1. {
                2.     "status": "fail",
                3.     "message": "Gagal memperbarui buku. Mohon isi nama buku"
                4. }
        ```
        
- Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka *server* akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        
        ```bash
        
                1. {
                2.     "status": "fail",
                3.     "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
                4. }
        ```
        
- Idyang dilampirkan oleh *client* tidak ditemukkan oleh *server*. Bila hal ini terjadi, maka *server* akan merespons dengan:
    - Status Code : **404**
    - Response Body:
        
        ```bash
        
                1. {
                2.     "status": "fail",
                3.     "message": "Gagal memperbarui buku. Id tidak ditemukan"
                4. }
        ```
        

Bila buku **berhasil diperbarui**, *server* harus mengembalikan respons dengan:

- Status Code : **200**
- Response Body:
    
    ```bash
    
        1. {
        2.     "status": "success",
        3.     "message": "Buku berhasil diperbarui"
        4. }
    ```
    

### **Kriteria 7 : API dapat menghapus buku**

API yang Anda buat harus dapat menghapus buku berdasarkan id melalui *route* berikut:

- Method : **DELETE**
- URL: /books/{bookId}

Bila id yang dilampirkan tidak dimiliki oleh buku manapun, maka *server* harus mengembalikan respons berikut:

- Status Code : **404**
- Response Body:
    
    ```bash
    
        1. {
        2.     "status": "fail",
        3.     "message": "Buku gagal dihapus. Id tidak ditemukan"
        4. }
    ```
    

Bila id dimiliki oleh salah satu buku, maka buku tersebut harus dihapus dan *server* mengembalikan respons berikut:

- Status Code : **200**
- Response Body:
    
    ```bash
    
        1. {
        2.     "status": "success",
        3.     "message": "Buku berhasil dihapus"
        4. }
    ```
    
## **Pengujian**

Ketika membangun Bookshelf API, tentu Anda perlu menguji untuk memastikan API berjalan sesuai dengan kriteria yang ada. Kami sudah menyediakan berkas Postman Collection dan Environment yang dapat Anda gunakan untuk pengujian. Silakan unduh berkasnya pada tautan berikut:

- [Postman Bookshelf API Test Collection dan Environment](https://github.com/dicodingacademy/a261-backend-pemula-labs/raw/099-shared-files/BookshelfAPITestCollectionAndEnvironment.zip)

Anda perlu meng-*import* kedua berkas tersebut pada Postman untuk menggunakannya. Caranya, ekstrak **berkas yang sudah diunduh hingga menghasilkan dua berkas file JSON.

Kemudian pada aplikasi Postman, klik tombol **import** yang berada di atas panel kiri aplikasi Postman.

Kemudian klik tombol **Upload Files** untuk meng-*import* kedua berkas JSON hasil ekstraksi.

Setelah itu, Bookshelf API Test Collection dan Environment akan tersedia pada Postman Anda.
