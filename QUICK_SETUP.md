# ⚡ Quick Setup Guide (5 Menit)

Ikuti langkah-langkah ini untuk setup HarmoniFashion v2 dengan cepat.

---

## 🎯 5 Langkah Setup

### ✅ Step 1: Buat Supabase Project (2 menit)

1. Buka https://supabase.com
2. Klik "Create a new project"
3. Pilih:
   - Organization: (buat baru jika perlu)
   - Database password: (simpan!)
   - Region: Singapore atau Indonesia (terdekat)
4. Tunggu setup ~2 menit

**Catat credentials:**
```
Project URL: https://xxxxx.supabase.co
Anon Key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

### ✅ Step 2: Setup Database (1 menit)

1. Di Supabase dashboard, buka **SQL Editor**
2. Klik "New Query"
3. Copy seluruh isi file `database_schema.sql`
4. Paste ke SQL Editor
5. Klik tombol **Run** (⏵) atau Ctrl+Enter

**Expected:** Tidak ada error, semua table terbuat

Verify:
- Pergi ke **Table Editor**
- Harusnya ada table: products, stores, wishlists, reviews, dll

---

### ✅ Step 3: Setup Authentication (1 menit)

1. Di Supabase, pergi ke **Authentication**
2. Klik tab **Providers**
3. Cari "Email" → pastikan **Enabled** ✓
4. Pergi ke **URL Configuration**
5. Di "Redirect URLs", tambahkan:
   ```
   http://localhost:3000
   http://localhost:3000/auth/callback
   ```
   (dan domain production Anda nanti)

---

### ✅ Step 4: Konfigurasi File HTML (1 menit)

**Di file: `katalog-fashion-v2.html` (line ~386)**

Cari:
```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

Ganti dengan credentials Anda:
```javascript
const SUPABASE_URL = 'https://abc123xyz.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

**Lakukan hal sama di file: `login.html` (line ~78)**

---

### ✅ Step 5: Test! (0 menit)

1. Buka `katalog-fashion-v2.html` di browser
2. Klik tombol **LOGIN**
3. Klik **DAFTAR AKUN BARU**
4. Masukkan email & password baru
5. Klik **BUAT AKUN** → tunggu success message
6. Klik **SUDAH PUNYA AKUN?** → switch ke login
7. Login dengan email+password yang baru dibuat
8. Sekarang Anda login! ✓

**Test wishlist:**
1. Klik heart (♡) di salah satu produk
2. Product masuk ke wishlist
3. Buka wishlist panel (klik ♡ di navbar)
4. Lihat item wishlist Anda
5. Refresh page → wishlist tetap ada!
6. Logout → wishlist hilang
7. Login lagi → wishlist kembali ✓

---

## 🔗 Important Links

| Task | Link |
|------|------|
| Supabase Dashboard | https://app.supabase.com |
| View Wishlist Data | Dashboard > Table Editor > wishlists |
| View Users | Dashboard > Authentication > Users |
| Check Database | Dashboard > SQL Editor > \* or Dashboard > Logs |
| Supabase Docs | https://supabase.com/docs |

---

## 📋 Checklist

- [ ] Supabase project created
- [ ] Database schema imported (no errors)
- [ ] Email auth enabled
- [ ] URL callback configured
- [ ] SUPABASE_URL replaced in katalog-fashion-v2.html
- [ ] SUPABASE_ANON_KEY replaced in katalog-fashion-v2.html
- [ ] SUPABASE_URL replaced in login.html
- [ ] SUPABASE_ANON_KEY replaced in login.html
- [ ] Tested signup
- [ ] Tested login
- [ ] Tested wishlist add
- [ ] Tested wishlist persist (refresh)
- [ ] Tested logout → wishlist gone
- [ ] Tested login again → wishlist back

---

## 🚀 Next Steps

### Jika semuanya work:

1. **Customize Products**
   - Buka Supabase > Table Editor > products
   - Edit/tambah produk
   - Update image URL

2. **Customize Stores**
   - Table Editor > stores
   - Edit nama, alamat, lokasi, dll

3. **Add Sample Reviews**
   - Table Editor > reviews
   - Insert beberapa review sample

4. **Deploy Online**
   - Upload HTML files ke web hosting
   - Update SUPABASE_URL & ANON_KEY (prod credentials)
   - Test di live domain

---

## ⚠️ Common Issues & Fixes

### "Wishlist button tidak bisa diklik"
**→** User belum login. Klik LOGIN dulu.

### "Error: SUPABASE_URL not defined"
**→** Anda lupa ganti `YOUR_SUPABASE_URL`. Check step 4.

### "Table wishlists not found"
**→** Database schema belum di-run. Ulang step 2.

### "Can't add to wishlist"
**→** Check browser console (F12). Ada error?
   - Verify credentials benar
   - Verify RLS policy sudah applied

### "Products tidak muncul"
**→** Belum ada products di database. Insert data ke table products.

---

## 📞 Debugging

### Check Supabase Connection

Di browser console (F12):
```javascript
// Test Supabase connection
console.log('SUPABASE_URL:', SUPABASE_URL);
console.log('Client created:', sb);

// Check current user
sb.auth.getUser().then(({data}) => console.log('User:', data));

// Check wishlist
if (currentUser) {
  sb.from('wishlists').select('*').then(({data}) => console.log('Wishlist:', data));
}
```

### Check Database

Di Supabase SQL Editor:
```sql
-- Check wishlists
SELECT * FROM wishlists;

-- Check users
SELECT id, email FROM auth.users;

-- Check products
SELECT id, name, price FROM products LIMIT 5;
```

---

## 📚 File Reference

```
📦 HarmoniFashion v2/
├── katalog-fashion-v2.html      ← Main file (upload ini!)
├── login.html                    ← Auth page (upload ini!)
├── database_schema.sql           ← Run di Supabase SQL Editor
├── DOKUMENTASI.md               ← Detailed docs
├── README.md                     ← Overview
└── QUICK_SETUP.md              ← Ini! (Quick ref)
```

---

## ✨ What You Get

✅ Working fashion catalog  
✅ User authentication (signup/login/logout)  
✅ Wishlist system (saved to database!)  
✅ Product management  
✅ Review & rating system  
✅ Share functionality  
✅ Responsive design  
✅ Production-ready code  

---

## 🎉 Ready!

Anda sekarang punya:
- ✅ Online fashion store
- ✅ User accounts
- ✅ Wishlist management
- ✅ All data in database

**Selamat! Enjoy your new store 🎀**

---

**Questions?**
- Read DOKUMENTASI.md for detailed info
- Check Supabase docs: https://supabase.com/docs
- Debug via browser console (F12)

**Happy coding! 🚀**
