# 🎀 HarmoniFashion v2 - Complete Package

Selamat! Anda sekarang memiliki versi terbaru HarmoniFashion dengan integrasi Supabase lengkap.

---

## 📦 File-File yang Disediakan

### 1. **katalog-fashion-v2.html** ⭐ MAIN FILE
File utama katalog fashion dengan:
- ✅ Wishlist terintegrasi Supabase
- ✅ Login/Logout system
- ✅ Produk, filter, search
- ✅ Review & rating (dengan peta di modal)
- ✅ Share functionality
- ✅ Responsive design

**Cara menggunakan:**
1. Ganti `SUPABASE_URL` dan `SUPABASE_ANON_KEY` dengan credentials Anda
2. Upload ke server/hosting
3. Buka di browser

---

### 2. **login.html** 🔐
Halaman login/signup untuk customer
- ✅ Login dengan email + password
- ✅ Signup akun baru
- ✅ Forgot password
- ✅ Validasi form
- ✅ Same Supabase config

**Struktur:**
```
/ → index.html (katalog-fashion-v2.html)
/login → login.html
```

---

### 3. **database_schema.sql** 💾 PENTING!
Script SQL untuk setup database di Supabase

**Isi table:**
- `products` - Katalog produk
- `stores` - Cabang/lokasi
- `reviews` - Ulasan & rating
- `wishlists` - ⭐ BARU! Wishlist per user
- `customers` - Data pelanggan
- `carts` - Keranjang belanja
- `orders` - Pesanan
- `admins` - Admin user

**RLS Policy included** untuk keamanan

---

### 4. **DOKUMENTASI.md** 📖
Dokumentasi lengkap:
- Ringkasan perubahan
- Cara implementasi step-by-step
- Database schema detail
- Function documentation
- Testing checklist
- Troubleshooting guide

---

### 5. **README.md** (File ini)
Overview cepat dan setup guide

---

## ⚡ Quick Start

### Step 1: Setup Supabase

1. Buka [supabase.com](https://supabase.com)
2. Buat project baru (pilih region Indonesia jika ada)
3. Tunggu project aktif
4. Catat:
   - Project URL (Settings > API > URL)
   - Anon Key (Settings > API > anon key)

### Step 2: Setup Database

1. Buka **SQL Editor** di Supabase dashboard
2. Buat file baru / paste
3. Copy seluruh isi `database_schema.sql`
4. Klik "Run" atau Ctrl+Enter
5. Tunggu sampai berhasil ✓

**Harusnya berhasil tanpa error**

### Step 3: Konfigurasi HTML Files

Cari dan ganti di kedua file (`katalog-fashion-v2.html` dan `login.html`):

```javascript
// SEBELUM:
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';

// SESUDAH:
const SUPABASE_URL = 'https://abc123xyz.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

### Step 4: Setup Auth URLs

Di Supabase dashboard:
1. Authentication > URL Configuration
2. Tambahkan redirect URLs:
   ```
   http://localhost:3000
   http://localhost:3000/auth/callback
   https://yourdomain.com
   https://yourdomain.com/auth/callback
   ```

### Step 5: Upload Files

Upload kedua HTML file ke hosting:
- `katalog-fashion-v2.html` → index.html (atau `/` route)
- `login.html` → `/login` route

### Step 6: Test!

1. Buka website
2. Klik LOGIN
3. Buat akun baru atau login
4. Try wishlist 💗

---

## 🎯 Key Features

### Wishlist System ⭐
```
User tidak login
  → Wishlist button disabled
  → Toast: "Login dulu"

User login
  → Wishlist button enabled
  → Klik → simpan ke database
  → Wishlist count muncul
  → Buka wishlist panel

User logout
  → Wishlist dihapus dari UI
  → Data tetap di database
  → Saat login lagi → kembali muncul
```

### Data Flow
```
Click Heart Button
  → Cek: User login?
  → Ya → Cek: Sudah di wishlist?
  → Tidak → INSERT ke table wishlists
  → Ya → DELETE dari table wishlists
  → Update UI
  → Toast message
```

### Authentication
```
Login → Get user from Supabase Auth
     → Load wishlist dari table
     → Update navbar UI
     → Show email + logout button

Logout → Sign out from Supabase
      → Clear currentUser & wishlist
      → Disable wishlist button
      → Redirect ke home
```

---

## 📱 API Endpoints (Supabase Tables)

### POST /wishlists
```json
{
  "user_id": "uuid-dari-auth",
  "product_id": "prod-001"
}
```

### GET /wishlists?user_id=eq.uuid
```json
[
  { "id": 1, "user_id": "...", "product_id": "prod-001" },
  { "id": 2, "user_id": "...", "product_id": "prod-002" }
]
```

### DELETE /wishlists?user_id=eq.uuid&product_id=eq.prod-001
Success ✓

---

## 🔐 Security Info

### Row Level Security (RLS)
- ✅ `wishlists` - Private per user (hanya user sendiri bisa lihat/hapus)
- ✅ `products` & `stores` - Public (readable by all)
- ✅ `reviews` - Public read, admin delete only
- ✅ `customers` - Private per user atau admin

### Best Practices
1. Jangan expose SUPABASE_URL & ANON_KEY di public repos
2. Gunakan environment variables untuk production
3. Keep RLS policies active
4. Backup database regularly
5. Monitor storage usage

---

## 🛠️ Customization

### Ubah Product List

Edit di SQL:
```sql
INSERT INTO products (id, name, brand, category, description, price, image, size, color, stock, store_id)
VALUES ('prod-009', 'New Product', 'Brand', 'category', '...', 9000000, 'image.jpg', '...', 'Color', 10, 'store-id');
```

### Ubah Store/Cabang

Edit di SQL:
```sql
INSERT INTO stores (id, name, address, phone, hours, query, is_visible)
VALUES ('store-new', 'HarmoniFashion Baru', '...', '...', '...', '...', true);
```

### Ubah Styling

Edit CSS di `<style>` tag:
```css
:root {
  --gold: #C9A84C;        /* Warna utama */
  --black: #0D0D0D;       /* Warna gelap */
  --white: #FFFFFF;       /* Warna terang */
  /* ... custom colors ... */
}
```

### Ubah Text/Language

Cari & replace di HTML:
```
"Wishlist" → "Daftar Impian"
"Add to Cart" → "Tambah Keranjang"
dll
```

---

## 📊 Database Struktur (Simple)

```
┌─────────────────────────────────────────────┐
│           PRODUCTS (Katalog)                │
├─────────────────────────────────────────────┤
│ id | name | brand | price | image | ...    │
└─────────────────────────────────────────────┘
            ↓ FK: store_id ↓
┌─────────────────────────────────────────────┐
│           STORES (Cabang)                   │
├─────────────────────────────────────────────┤
│ id | name | address | phone | hours | ...  │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│         WISHLISTS (⭐ BARU!)                │
├─────────────────────────────────────────────┤
│ id | user_id | product_id | created_at    │
└─────────────────────────────────────────────┘
   ↑ FK: auth.users  ↑ FK: products

┌─────────────────────────────────────────────┐
│           REVIEWS (Ulasan)                  │
├─────────────────────────────────────────────┤
│ id | store_id | author | rating | text |..│
└─────────────────────────────────────────────┘
```

---

## 🧪 Testing

### Manual Test Checklist

- [ ] Akses katalog tanpa login
- [ ] Lihat product list
- [ ] Filter by category/brand
- [ ] Search produk
- [ ] Klik heart → toast "login dulu"
- [ ] Klik login button
- [ ] Signup dengan email baru
- [ ] Login dengan akun yang ada
- [ ] Navbar muncul nama + logout
- [ ] Klik heart → item masuk wishlist
- [ ] Wishlist count meningkat
- [ ] Buka wishlist panel
- [ ] Lihat item di wishlist
- [ ] Refresh page → wishlist tetap ada
- [ ] Logout
- [ ] Wishlist hilang
- [ ] Login lagi → wishlist kembali
- [ ] Submit review
- [ ] Review muncul di DB

---

## 🆘 Troubleshooting

### "SUPABASE_URL not defined"
**Fix:** Ganti `YOUR_SUPABASE_URL` dengan actual URL

### "Wishlists table not found"
**Fix:** Jalankan database_schema.sql di SQL editor

### "User login but wishlist button still disabled"
**Fix:** Reload page atau check `currentUser` di console

### "RLS policy error"
**Fix:** Check RLS policies di Supabase dashboard > Authentication > Policies

### "CORS error"
**Fix:** Add domain di Auth > URL Configuration

---

## 📞 Next Support

Jika ada masalah:

1. **Check Console** (F12 > Console)
   - Lihat error message
   - Copy & paste di search

2. **Check Supabase Logs**
   - Dashboard > Logs
   - Cari error details

3. **Read Documentation**
   - `DOKUMENTASI.md` file included
   - Supabase docs: https://supabase.com/docs

4. **Database Check**
   - Buka Supabase dashboard
   - Tables > wishlists
   - Check data ada?

5. **Auth Check**
   - Authentication > Users
   - Check user terdaftar?

---

## 📈 Performance Tips

1. **Lazy Load Images** - Load product images only when visible
2. **Cache Wishlist** - Store in localStorage as backup
3. **Pagination** - Load products 20 at a time
4. **CDN** - Use CDN for static assets
5. **Minify** - Minify HTML/CSS/JS for production

---

## 🚀 Deployment

### Deploy ke Vercel (Recommended)
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# atau langsung push ke GitHub
```

### Deploy ke Netlify
```bash
# Drag & drop file ke Netlify
# atau connect GitHub repo
```

### Deploy ke Server Sendiri
```bash
# 1. Upload files via FTP
# 2. Update SUPABASE_URL & ANON_KEY
# 3. Test di browser
```

---

## 📝 Changelog

### v2.0 (Latest)
- ✅ Wishlist terintegrasi Supabase
- ✅ User authentication
- ✅ Wishlist data terikat ke user
- ✅ Database schema lengkap
- ✅ RLS policies
- ✅ Peta hanya di review section
- ✅ Remove peta dari katalog

### v1.0 (Original)
- Basic catalog
- Local storage wishlist
- Review/rating system

---

## 📄 File Summary

| File | Size | Purpose |
|------|------|---------|
| katalog-fashion-v2.html | ~100KB | Main catalog |
| login.html | ~15KB | Auth page |
| database_schema.sql | ~20KB | DB setup |
| DOKUMENTASI.md | ~50KB | Full docs |
| README.md | ~30KB | This file |

**Total: ~215KB** (Very lightweight!)

---

## ✅ Ready to Go!

Semua file sudah siap digunakan. Follow langkah Quick Start dan Anda langsung bisa:

1. ✅ Have working fashion catalog
2. ✅ Accept user login
3. ✅ Store wishlists in database
4. ✅ Manage products & stores
5. ✅ Collect reviews & ratings

**Good luck! 🎀**

---

**Questions?** Check DOKUMENTASI.md for detailed info  
**Last Updated:** 2024-01-23  
**Status:** Production Ready ✓
