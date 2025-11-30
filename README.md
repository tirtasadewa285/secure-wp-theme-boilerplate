# 🛡️ Secure WordPress Theme Boilerplate

<div align="center">

![WordPress](https://img.shields.io/badge/WordPress-6.0%2B-%23117AC9.svg?style=for-the-badge&logo=WordPress&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.0%2B-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)
![Security](https://img.shields.io/badge/Security-Hardened-red?style=for-the-badge&logo=security)

**Starter Theme WordPress profesional dengan konfigurasi keamanan tingkat lanjut (Security-First Approach).**
*Stop building vulnerable themes. Start coding securely.*

[Fitur](#-fitur-unggulan) • [Instalasi](#-instalasi--penggunaan) • [Standar Keamanan](#-standar-keamanan-wajib-baca) • [Struktur](#-struktur-folder)

</div>

---

## 📖 Mengapa Boilerplate Ini Dibuat?

Tahukah Anda? Sebagian besar kerentanan WordPress bukan berasal dari Core-nya, melainkan dari **Tema dan Plugin** yang dibuat dengan buruk.

Banyak tutorial di luar sana mengajarkan cara membuat tema agar "tampilannya bagus", tetapi melupakan aspek vital:
1.  **XSS (Cross-Site Scripting):** Karena lupa melakukan *escaping* pada output.
2.  **SQL Injection:** Karena tidak men-sanitasi input sebelum masuk database.
3.  **CSRF & Brute Force:** Karena tidak ada proteksi nonce dan eksposur API yang tidak perlu.

Repositori ini hadir sebagai **pondasi (boilerplate)** bagi developer yang ingin membangun tema WordPress yang aman, cepat, dan mematuhi standar *WordPress Coding Standards*.

---

## 🚀 Fitur Unggulan

Boilerplate ini bukan sekadar file kosong. Di dalamnya sudah tertanam fungsi keamanan otomatis:

-   ✅ **Secure HTTP Headers:** Otomatis menambahkan header anti-clickjacking (`X-Frame-Options`) dan XSS protection.
-   ✅ **Disable XML-RPC:** Menutup celah serangan DDoS dan Brute Force klasik.
-   ✅ **Version Hiding:** Menyembunyikan versi WordPress dari source code agar tidak terdeteksi bot scanner.
-   ✅ **Login Error Obfuscation:** Menyamarkan pesan error login agar hacker tidak bisa menebak username valid.
-   ✅ **Direct Access Prevention:** Semua file PHP diproteksi dari akses langsung tanpa melewati WordPress.

---

## 🛠 Instalasi & Penggunaan

### 1. Download / Clone
Masuk ke folder instalasi WordPress Anda:
```bash
cd wp-content/themes/
git clone [https://github.com/USERNAME/secure-wp-theme-boilerplate.git](https://github.com/USERNAME/secure-wp-theme-boilerplate.git) my-secure-theme


-   ✅ **Secure HTTP Headers:** Otomatis menambahkan header anti-clickjacking (`X-Frame-Options`) dan XSS protection.
-   ✅ **Disable XML-RPC:** Menutup celah serangan DDoS dan Brute Force klasik.
-   ✅ **Version Hiding:** Menyembunyikan versi WordPress dari source code agar tidak terdeteksi bot scanner.
-   ✅ **Login Error Obfuscation:** Menyamarkan pesan error login agar hacker tidak bisa menebak username valid.
-   ✅ **SVG Sanitization:** (Optional) Mengizinkan upload SVG dengan aman tanpa risiko eksekusi script.
-   ✅ **Direct Access Prevention:** Semua file PHP diproteksi dari akses langsung.

---

## 🛠 Instalasi & Penggunaan

### 1. Download / Clone
Masuk ke folder instalasi WordPress Anda:
```bash
cd wp-content/themes/
git clone [https://github.com/USERNAME/secure-wp-theme-boilerplate.git](https://github.com/USERNAME/secure-wp-theme-boilerplate.git) my-secure-theme
````

### 2\. Konfigurasi Awal

1.  Ubah nama folder `my-secure-theme` sesuai nama proyek Anda.
2.  Buka `style.css` dan edit informasi tema (Theme Name, Author, dll).
3.  (Opsional) Lakukan *Search & Replace* di seluruh file untuk mengganti text-domain `secure-theme` menjadi nama tema Anda.

### 3\. Aktivasi

Masuk ke Dashboard WordPress \> **Appearance** \> **Themes**, lalu aktifkan tema.

-----

## 🔐 Standar Keamanan (Wajib Baca\!)

Jika Anda menggunakan boilerplate ini, Anda **WAJIB** mengikuti aturan main berikut agar tema tetap aman.

### 1\. The Golden Rule: Sanitization (Input)

Jangan pernah percaya data dari user. Bersihkan semua input `$_POST`, `$_GET`, atau `$_REQUEST` sebelum diproses.

**❌ SALAH:**

```php
$title = $_POST['custom_title'];
update_post_meta($post_id, 'meta_key', $title);
```

**✅ BENAR:**

```php
if ( isset($_POST['custom_title']) ) {
    $title = sanitize_text_field( $_POST['custom_title'] );
    update_post_meta($post_id, 'meta_key', $title);
}
```

*Fungsi sanitasi lain: `sanitize_email()`, `sanitize_key()`, `absint()`.*

### 2\. The Golden Rule: Escaping (Output)

Bersihkan data saat *detik terakhir* sebelum ditampilkan ke layar (Late Escaping). Data di database bisa saja sudah terinfeksi, jadi jangan percaya database.

**❌ SALAH:**

```php
echo $variable; 
// Jika variable berisi <script>alert(1)</script>, user kena hack.
```

**✅ BENAR:**

```php
echo esc_html( $variable ); // Untuk teks biasa
echo esc_url( $link );      // Untuk URL
echo esc_attr( $class );    // Untuk atribut HTML
```

### 3\. Nonce Verification

Selalu gunakan **Nonce** saat memproses form untuk mencegah serangan CSRF (Cross-Site Request Forgery).

```php
// Di Form:
wp_nonce_field( 'my_action', 'my_nonce_name' );

// Di Pemrosesan (Functions.php):
if ( ! isset( $_POST['my_nonce_name'] ) || ! wp_verify_nonce( $_POST['my_nonce_name'], 'my_action' ) ) {
   die( 'Security check failed' );
}
```

-----

## 📂 Struktur Folder

Kami memisahkan logika keamanan agar mudah dikelola dan tidak menumpuk di `functions.php`.

```text
secure-wp-theme-boilerplate/
├── assets/                  # CSS, JS, Images, Fonts
├── inc/                     # Logika Tambahan
│   ├── security.php         # 🛡️ Core Security Functions (Headers, Hardening)
│   ├── theme-setup.php      # Konfigurasi dasar tema
│   └── template-tags.php    # Custom functions untuk template
├── template-parts/          # Potongan layout (Header, Footer content)
├── functions.php            # Main entry point (Hanya memanggil file di /inc)
├── header.php               # Header HTML (Meta tags aman)
├── footer.php               # Footer HTML
├── index.php                # Fallback template
├── style.css                # Metadata Tema
└── README.md                # Dokumentasi ini
