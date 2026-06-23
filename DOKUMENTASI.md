# Dokumentasi HarmoniFashion v2 - Update dengan Wishlist Supabase

## 📋 Ringkasan Perubahan

Versi terbaru ini menggabungkan semua fitur dengan perubahan signifikan:

### ✅ Fitur yang Diimplementasikan

1. **Wishlist Terintegrasi Supabase**
   - Wishlist disimpan di database (bukan local storage)
   - Data wishlist terikat ke akun customer
   - Wishlist otomatis dihapus saat logout
   - Hanya user yang login bisa menggunakan wishlist

2. **Autentikasi Customer**
   - Login/Logout dengan Supabase Auth
   - Tombol logout muncul setelah login
   - Greeting dengan nama email user
   - Session management otomatis

3. **Peta Dihapus dari Katalog**
   - Tombol "Tampilkan Peta" sudah dihilangkan
   - Peta hanya ada di tab ulasan/rating (dalam modal produk)
   - Lebih fokus pada katalog produk

4. **Database Schema Lengkap**
   - Table untuk products, stores, reviews
   - Table wishlist (baru) dengan user_id
   - Table customers, orders, carts
   - Row Level Security (RLS) untuk keamanan

---

## 🚀 Cara Implementasi

### Step 1: Setup Supabase Project

1. Buka [supabase.com](https://supabase.com) dan buat project baru
2. Setelah project dibuat, catat:
   - **Project URL** (dari Settings > API)
   - **Anon Public Key** (dari Settings > API)

### Step 2: Database Setup

1. Buka **SQL Editor** di Supabase
2. Copy seluruh isi file `database_schema.sql`
3. Paste dan eksekusi query
4. Tunggu semua table terbuat ✓

### Step 3: Konfigurasi HTML

Di file `katalog-fashion-v2.html`, cari baris:

```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

Ganti dengan:
```javascript
const SUPABASE_URL = 'https://your-project.supabase.co';
const SUPABASE_ANON_KEY = 'your-anon-key-here';
```

### Step 4: Setup Autentikasi

Di Supabase:
1. Pergi ke **Authentication > Providers**
2. Pastikan **Email** sudah enabled
3. Pergi ke **Authentication > URL Configuration**
4. Tambahkan redirect URL:
   - `http://localhost:3000` (untuk development)
   - `https://yourdomain.com` (untuk production)

### Step 5: Upload File HTML

Upload file `katalog-fashion-v2.html` ke server/hosting Anda

---

## 🔑 Fitur Wishlist - Detail

### Cara Kerja Wishlist

```javascript
// 1. User harus login terlebih dahulu
if (!currentUser) {
  showToast('Silakan login untuk menggunakan wishlist.');
  return;
}

// 2. Data wishlist disimpan di table 'wishlists'
// dengan user_id + product_id

// 3. Saat user logout, wishlist otomatis hilang dari UI
// dan tidak disimpan di local storage

// 4. Saat user login kembali, wishlist di-load dari database
await loadWishlist(); // fetch dari table wishlists
```

### Tabel Wishlists Structure

```sql
CREATE TABLE wishlists (
  id BIGINT PRIMARY KEY,
  user_id UUID NOT NULL,           -- User yang login
  product_id TEXT NOT NULL,         -- ID Produk
  created_at TIMESTAMP,
  
  UNIQUE(user_id, product_id)      -- 1 produk cuma bisa 1x di wishlist
);
```

### Button Wishlist Behavior

| State | Tampilan | Action |
|-------|----------|--------|
| **Not Logged In** | Button disabled, "♡" | Klik → Toast: "Login dulu" |
| **Logged In, Not Wishlisted** | "♡" putih, border abu | Klik → Tambah ke wishlist |
| **Logged In, Wishlisted** | "♡" merah, bg merah gelap | Klik → Hapus dari wishlist |

---

## 📍 Peta Review/Rating

Peta hanya tampil di **modal produk > tab Ulasan**

**Implementasi Peta di Reviews:**

```html
<!-- Di dalam modal-reviews section -->
<div id="reviewMapContainer" style="height: 300px; margin-top: 20px;"></div>

<!-- Script untuk load peta -->
<script>
  // Gunakan Google Maps API atau Leaflet
  // Initialize map ketika modal dibuka
  // Center ke store location (dari database)
</script>
```

Lokasi tersimpan di tabel `stores` dengan kolom `query` untuk Google Maps search.

---

## 🗄️ Database Tables Overview

### 1. **products**
```sql
- id (TEXT) - Product ID
- name, brand, category - Info produk
- price - Harga dalam Rupiah
- image - URL gambar
- stock - Stok barang
- store_id - FK ke table stores
```

### 2. **stores**
```sql
- id (TEXT) - Store ID
- name, address, phone, hours - Info cabang
- query - Query untuk Google Maps
- social - JSON: {ig, fb, wa, tt}
- is_visible - Boolean untuk show/hide
```

### 3. **wishlists** ⭐ BARU
```sql
- id (BIGINT) - Primary key
- user_id (UUID) - FK ke auth.users
- product_id (TEXT) - FK ke products
- UNIQUE(user_id, product_id)
```

### 4. **reviews**
```sql
- id (BIGINT) - Auto increment
- store_id (TEXT) - FK ke stores
- author - Nama pereview
- rating - 1-5 bintang
- text - Komentar
- date - Tanggal review
```

### 5. **customers**
```sql
- id (UUID) - FK ke auth.users
- full_name, email, phone
- address, city, postal_code
- avatar_url - Foto profil
```

### 6. **orders & order_items**
```sql
- orders: id, user_id, order_number, total_price, status, created_at
- order_items: id, order_id, product_id, quantity, price_at_order
```

---

## 🔒 Row Level Security (RLS)

Semua table sudah punya RLS policy:

| Table | Policy |
|-------|--------|
| **products** | Readable by all (public) |
| **stores** | Readable all, but admins see hidden stores |
| **reviews** | Readable all, deletable by admin only |
| **wishlists** | Private per user (SELECT/INSERT/DELETE own only) |
| **carts** | Private per user |
| **orders** | Private per user or admin |
| **customers** | Private per user or admin |

---

## 🔐 Authentication Flow

### Login Process
```
User Click "LOGIN" 
  → Redirect ke auth page 
  → Supabase Auth UI
  → Email + Password
  → Callback ke /auth/callback
  → Set currentUser
  → Load wishlist from DB
  → Update UI
```

### Logout Process
```
User Click "LOGOUT"
  → sb.auth.signOut()
  → Clear currentUser
  → Clear currentWishlist
  → Disable wishlist button
  → Close wishlist panel
  → Redirect ke home
```

---

## 📱 Front-End Functions

### Wishlist Functions

```javascript
// Load wishlist dari database
async loadWishlist() {
  if (!currentUser) return;
  const { data } = await sb
    .from('wishlists')
    .select('product_id')
    .eq('user_id', currentUser.id);
  currentWishlist = data.map(w => w.product_id);
}

// Tambah ke wishlist
async addToWishlist(productId) {
  await sb.from('wishlists').insert({
    user_id: currentUser.id,
    product_id: productId
  });
  currentWishlist.push(productId);
  updateWishlistCount();
}

// Hapus dari wishlist
async removeFromWishlist(productId) {
  await sb.from('wishlists')
    .delete()
    .eq('user_id', currentUser.id)
    .eq('product_id', productId);
  currentWishlist = currentWishlist.filter(id => id !== productId);
  updateWishlistCount();
}

// Update UI wishlist count
function updateWishlistCount() {
  document.getElementById('wishlistCount').textContent = currentWishlist.length;
}
```

### Auth Functions

```javascript
// Cek user saat init
async initAuth() {
  const { data: { user } } = await sb.auth.getUser();
  currentUser = user;
  updateAuthUI();
}

// Update UI berdasarkan login state
function updateAuthUI() {
  if (currentUser) {
    // Tampilkan nama user + logout button
    // Enable wishlist button
  } else {
    // Tampilkan login button
    // Disable wishlist button
  }
}

// Logout
async logout() {
  await sb.auth.signOut();
  currentUser = null;
  currentWishlist = [];
  updateAuthUI();
}
```

---

## 🎨 UI Changes

### Navbar Changes
- ✅ Login button muncul saat belum login
- ✅ User email + logout button muncul saat login
- ✅ Wishlist button di-enable hanya saat login
- ✅ Wishlist count badge (merah) muncul saat ada item

### Wishlist Panel Changes
- ✅ Tampilkan pesan login jika belum login
- ✅ Tampilkan daftar wishlist dari database
- ✅ Button remove per item
- ✅ Button view untuk lihat detail produk

### Product Cards Changes
- ✅ Wishlist button di card (♡)
- ✅ Tombol "Tampilkan Peta" dihilangkan
- ✅ Wishlist button disabled jika belum login

### Modal Produk Changes
- ✅ Wishlist button di dalam modal
- ✅ Tab ulasan menampilkan peta (jika diimplement)
- ✅ Form submit review langsung ke database

---

## 🧪 Testing Checklist

### Unit Testing
- [ ] User bisa signup dengan email baru
- [ ] User bisa login dengan email + password
- [ ] Wishlist button disabled saat belum login
- [ ] Klik wishlist → tambah ke DB
- [ ] Wishlist count meningkat
- [ ] Refresh page → wishlist masih ada
- [ ] Logout → wishlist hilang
- [ ] Login lagi → wishlist kembali

### Integration Testing
- [ ] Form review bisa submit
- [ ] Rating tampil di database
- [ ] Search produk berfungsi
- [ ] Filter kategori/brand/harga berfungsi
- [ ] Share produk ke WhatsApp/FB
- [ ] Add to cart (jika di-implement)

### Security Testing
- [ ] Non-login user tidak bisa akses wishlist API
- [ ] User tidak bisa hapus wishlist user lain
- [ ] RLS policies properly enforce
- [ ] SQL injection prevention

---

## 🐛 Troubleshooting

### Wishlist tidak load
**Solusi:**
```javascript
// Cek console error
// Verify SUPABASE_URL dan SUPABASE_ANON_KEY benar
// Cek RLS policy pada table wishlists
// Pastikan user sudah login (currentUser !== null)
```

### RLS error saat insert wishlist
**Solusi:**
```sql
-- Check existing policies
SELECT * FROM pg_policies WHERE tablename = 'wishlists';

-- Drop dan recreate policy
DROP POLICY IF EXISTS "Users can insert own wishlist" ON wishlists;
CREATE POLICY "Users can insert own wishlist" ON wishlists 
FOR INSERT WITH CHECK (auth.uid() = user_id);
```

### Login redirect tidak working
**Solusi:**
- Pastikan URL callback sudah ditambah di Auth > URL Configuration
- Buat auth callback handler di `/auth/callback` atau `/`

### Peta tidak muncul di review
**Solusi:**
- Tambahkan Google Maps API key
- Atau gunakan Leaflet (free, open source)
- Inject map ke element dengan id `reviewMapContainer`

---

## 📖 Next Steps / Future Implementation

1. **Payment Gateway** - Stripe/Midtrans
2. **Admin Dashboard** - Manage products, stores, reviews
3. **Order Management** - Track orders, shipping
4. **Email Notifications** - Order confirmation, shipping update
5. **Product Recommendations** - Based on wishlist/purchase history
6. **Image Upload** - To Supabase Storage
7. **Dark Mode** - Complete implementation
8. **Mobile App** - React Native / Flutter

---

## 💬 Support & Contact

Untuk questions atau issues:
- Check console.log untuk error details
- Review Supabase documentation: https://supabase.com/docs
- Monitor database di Supabase dashboard

---

**Version:** 2.0  
**Last Updated:** 2024-01-23  
**Status:** Ready for Production  
