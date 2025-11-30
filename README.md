# 🛡️ Secure WordPress Theme Boilerplate & Guide

<div align="center">

![WordPress](https://img.shields.io/badge/WordPress-%23117AC9.svg?style=for-the-badge&logo=WordPress&logoColor=white)
![PHP](https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)
![Security](https://img.shields.io/badge/Security-First-red?style=for-the-badge)

**Panduan dan Starter Template untuk membuat Tema WordPress dengan standar keamanan profesional.**
*Stop making vulnerable themes. Start coding securely.*

</div>

---

## 📖 Mengapa Repo Ini Dibuat?
Banyak tutorial WordPress di luar sana hanya mengajarkan "cara agar tampilannya jadi", tapi melupakan:
1.  **Sanitization** (Membersihkan input data).
2.  **Escaping** (Membersihkan output data).
3.  **Nonce Verification** (Validasi asal request).
4.  **Direct Access Prevention** (Mencegah akses file langsung).

Repo ini adalah pondasi (boilerplate) untuk Anda yang ingin membangun tema WP yang **aman, cepat, dan standar industri**.

---

## 🔐 The Golden Rules of WP Security

### 1. Jangan Pernah Percaya Input User
Setiap data yang masuk ke database (`$_POST`, `$_GET`) wajib dibersihkan (Sanitization).

**❌ Salah:**
```php
update_post_meta($post_id, 'custom_field', $_POST['custom_field']);
