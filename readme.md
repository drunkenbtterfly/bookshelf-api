# Bookshelf API

API sederhana untuk mengelola koleksi buku menggunakan Hapi.js dan JavaScript.

## Fitur

- ✅ Menambahkan buku baru
- ✅ Mendapatkan semua buku dengan filter
- ✅ Mendapatkan detail buku berdasarkan ID
- ✅ Memperbarui informasi buku
- ✅ Menghapus buku

## Teknologi yang Digunakan

- **Node.js** - Runtime JavaScript
- **Hapi.js** - Framework web server
- **nanoid** - Generator ID unik
- **ESLint** - Linting dan code formatting

## Instalasi

### Prerequisites

- Node.js (versi 14 atau lebih baru)
- npm atau yarn

### Langkah Instalasi

1. Clone repositori ini:
```bash
git clone <repository-url>
cd bookshelf-api
```

2. Install dependencies:
```bash
npm install
```

3. Jalankan server:
```bash
# Development mode (dengan nodemon)
npm run start-dev

# Production mode
npm start
```

4. Server akan berjalan pada `http://localhost:9000`

## Struktur Proyek

```
bookshelf-api/
├── src/
│   ├── server.js      # Entry point aplikasi
│   ├── routes.js      # Definisi routing
│   ├── handler.js     # Handler untuk setiap endpoint
│   └── books.js       # Data storage (array in-memory)
├── package.json
└── README.md
```

## API Endpoints

### 1. Menambahkan Buku

**POST** `/books`

**Request Body:**
```json
{
  "name": "Buku A",
  "year": 2010,
  "author": "John Doe",
  "summary": "Lorem ipsum dolor sit amet",
  "publisher": "Dicoding Indonesia",
  "pageCount": 100,
  "readPage": 25,
  "reading": false
}
```

**Response Success (201):**
```json
{
  "status": "success",
  "message": "Buku berhasil ditambahkan",
  "data": {
    "bookId": "1L7ZtDUFeGs7VlEt"
  }
}
```

**Response Error (400):**
```json
{
  "status": "fail",
  "message": "Gagal menambahkan buku. Mohon isi nama buku"
}
```

### 2. Mendapatkan Semua Buku

**GET** `/books`

**Query Parameters (Optional):**
- `name`: Filter berdasarkan nama buku (case-insensitive)
- `reading`: Filter berdasarkan status membaca (0 = false, 1 = true)
- `finished`: Filter berdasarkan status selesai (0 = false, 1 = true)

**Contoh Request:**
```
GET /books?name=dicoding&reading=1&finished=0
```

**Response Success (200):**
```json
{
  "status": "success",
  "data": {
    "books": [
      {
        "id": "Qbax5Oy7L8WKf74l",
        "name": "Buku A",
        "publisher": "Dicoding Indonesia"
      }
    ]
  }
}
```

### 3. Mendapatkan Detail Buku

**GET** `/books/{bookId}`

**Response Success (200):**
```json
{
  "status": "success",
  "data": {
    "book": {
      "id": "aWZBUW3JN_VBE-9I",
      "name": "Buku A Revisi",
      "year": 2011,
      "author": "Jane Doe",
      "summary": "Lorem Dolor sit Amet",
      "publisher": "Dicoding Indonesia",
      "pageCount": 200,
      "readPage": 26,
      "finished": false,
      "reading": false,
      "insertedAt": "2021-03-05T06:14:28.930Z",
      "updatedAt": "2021-03-05T06:14:30.718Z"
    }
  }
}
```

**Response Error (404):**
```json
{
  "status": "fail",
  "message": "Buku tidak ditemukan"
}
```

### 4. Memperbarui Buku

**PUT** `/books/{bookId}`

**Request Body:**
```json
{
  "name": "Buku A Revisi",
  "year": 2011,
  "author": "Jane Doe",
  "summary": "Lorem Dolor sit Amet",
  "publisher": "Dicoding Indonesia",
  "pageCount": 200,
  "readPage": 26,
  "reading": false
}
```

**Response Success (200):**
```json
{
  "status": "success",
  "message": "Buku berhasil diperbarui"
}
```

**Response Error (404):**
```json
{
  "status": "fail",
  "message": "Gagal memperbarui buku. Id tidak ditemukan"
}
```

### 5. Menghapus Buku

**DELETE** `/books/{bookId}`

**Response Success (200):**
```json
{
  "status": "success",
  "message": "Buku berhasil dihapus"
}
```

**Response Error (404):**
```json
{
  "status": "fail",
  "message": "Buku gagal dihapus. Id tidak ditemukan"
}
```

## Validasi

### Validasi Menambahkan Buku
- `name` wajib diisi
- `readPage` tidak boleh lebih besar dari `pageCount`

### Validasi Memperbarui Buku
- `name` wajib diisi
- `readPage` tidak boleh lebih besar dari `pageCount`

## Properti Buku

Setiap buku memiliki properti berikut:

| Properti | Tipe | Wajib | Deskripsi |
|----------|------|-------|-----------|
| `id` | String | Auto | ID unik buku (generated) |
| `name` | String | ✅ | Nama buku |
| `year` | Number | ✅ | Tahun terbit |
| `author` | String | ✅ | Penulis |
| `summary` | String | ✅ | Ringkasan |
| `publisher` | String | ✅ | Penerbit |
| `pageCount` | Number | ✅ | Jumlah halaman |
| `readPage` | Number | ✅ | Halaman yang sudah dibaca |
| `reading` | Boolean | ✅ | Status sedang membaca |
| `finished` | Boolean | Auto | Status selesai dibaca (readPage === pageCount) |
| `insertedAt` | String | Auto | Waktu ditambahkan (ISO string) |
| `updatedAt` | String | Auto | Waktu diperbarui (ISO string) |

## Scripts

```bash
# Menjalankan server
npm start

# Menjalankan server dengan nodemon (development)
npm run start-dev

# Menjalankan ESLint
npm run lint

# Memperbaiki masalah ESLint secara otomatis
npm run lint:fix
```

## CORS

API ini sudah dikonfigurasi untuk menerima request dari semua origin (`*`). Jika ingin membatasi akses, ubah konfigurasi CORS di `src/server.js`.

## Penyimpanan Data

Saat ini aplikasi menggunakan penyimpanan in-memory (array). Data akan hilang ketika server di-restart. Untuk production, disarankan menggunakan database seperti MongoDB, PostgreSQL, atau MySQL.

## Lisensi

Proyek ini menggunakan lisensi ISC. Lihat file `LICENSE` untuk detail lebih lanjut.

## Testing

Untuk menguji API, Anda dapat menggunakan tools seperti:
- [Postman](https://www.postman.com/)
- [Insomnia](https://insomnia.rest/)
- [curl](https://curl.se/)

Contoh testing dengan curl:

```bash
# Menambahkan buku baru
curl -X POST http://localhost:9000/books \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Buku Test",
    "year": 2021,
    "author": "Test Author",
    "summary": "Test summary",
    "publisher": "Test Publisher",
    "pageCount": 100,
    "readPage": 50,
    "reading": true
  }'

# Mendapatkan semua buku
curl http://localhost:9000/books

# Mendapatkan buku berdasarkan ID
curl http://localhost:9000/books/{bookId}
```
