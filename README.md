# JWT CRUD API UAS Starter

Silakan lanjutkan pengembangan API dan perbaiki bug yang ditemukan.
# UAS Keamanan Komputer
## Identitas
- Nama: Havi Ichsan Fadillah
- NIM: 211080200014
- Kelas: Pengamanan Sistem Komputer 6/B4
- Repo GitHub: [link]

## Bagian A – Bug Fixing JWT REST API

### Bug 1: Token tetap aktif setelah logout
**Penjelasan:**  
Bug 1
Token tidak disimpan dan diverifikasi dengan benar.
Bug 2
Semua user bisa akses semua endpoint.
Bug 3
Tidak ada validasi kepemilikan data.
Bug 4 
Logout hanya di sisi client.

**Solusi:**  
Bug 1
 Simpan token aktif di database dan validasi setiap token pada setiap request.
Bug 2 
Tambahkan filter untuk memeriksa role (admin/user) sebelum mengakses endpoint.
Bug 3 
Bandingkan user_id dari token dan request. Tolak jika tidak cocok, kecuali role-nya admin.
Bug 4 
Hapus token dari database saat logout.

---

## Bagian B – Simulasi Serangan dan Solusi

### Jenis Serangan: Broken Access Control  
**Simulasi Postman:**  
Gunakan Postman atau curl dengan JSON body yang mengubah user_id.

Request Berbahaya (oleh user biasa):

http
Copy
Edit
PUT /api/users/update
Content-Type: application/json
Authorization: Bearer <valid_token>

{
  "user_id": 1,
  "role": "admin",
  "email": "admin@example.com"
}

**Solusi Implementasi:**  

Tambah filter untuk validasi bahwa user_id di request sesuai dengan ID di token JWT, kecuali jika role admin.
Langkah Pencegahan:
Buat Filter bernama OwnershipCheck:

php
Copy
Edit
$userIdFromToken = $decodedToken->uid;
$requestedId = $this->request->getPost('user_id');

if ($userIdFromToken != $requestedId && $role != 'admin') {
    return Services::response()->setStatusCode(403)->setBody('Access denied.');
}
Terapkan pada endpoint /api/users/update.

## Bagian C – Refleksi Teori & Etika

### 1. CIA Triad dalam Keamanan Informasi  

Confidentiality (Kerahasiaan): Informasi hanya diakses oleh pihak berwenang.
Integrity (Integritas): Informasi tidak diubah oleh pihak tidak sah.
Availability (Ketersediaan): Sistem dan data tersedia saat dibutuhkan oleh pengguna yang sah.

### 2. UU ITE yang relevan 

Pasal 30 Ayat 1: Setiap orang dilarang mengakses sistem elektronik milik orang lain tanpa izin.
Pasal 32 Ayat 1: Melarang mengubah, merusak, menghilangkan, atau memindahkan data elektronik tanpa hak.

### 3. Pandangan Al-Qur'an  
Ayat: QS. Al-Baqarah: 205
maksud dari ayat di atas ialah Ayat ini mencerminkan larangan membuat kerusakan dalam bentuk apapun, termasuk di dunia digital. Meretas, mencuri data, atau menyalahgunakan akses termasuk bentuk kerusakan yang dilarang.

### 4. Etika Cyber dan Kejujuran  
Kejujuran: Tidak menyembunyikan kelemahan sistem, melaporkan bug secara etis (responsible disclosure).
Amanah: Menjaga data pengguna, tidak menyalahgunakan hak akses, dan bertanggung jawab atas setiap kode yang ditulis.



