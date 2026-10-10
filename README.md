# 🖤 Monochrome Birthday Website — Special Editorial Edition

Website ucapan ulang tahun romantis, personal, dan elegan bernuansa **Editorial Fashion Magazine Monokrom (Hitam Putih)**.

Dibuat menggunakan **Next.js 14 (App Router)**, **TypeScript**, **Tailwind CSS**, **Framer Motion**, **Lenis**, dan **Prisma ORM**.

---

## 📂 Struktur Folder Proyek

```text
monochrome-birthday/
├── .env                          # File variabel lingkungan lokal
├── .env.example                  # Template variabel lingkungan
├── .gitignore                    # File yang diabaikan oleh Git
├── package.json                  # Dependensi dan script proyek
├── tsconfig.json                 # Konfigurasi TypeScript
├── tailwind.config.ts            # Konfigurasi tema monokrom & tipografi
├── postcss.config.mjs            # Konfigurasi PostCSS
├── next.config.mjs               # Konfigurasi Next.js
├── prisma/
│   └── schema.prisma             # Skema database ucapan (SQLite / Postgres)
├── public/
│   ├── audio/                    # Tempat menyimpan file lagu .mp3
│   │   └── README.md
│   └── images/                   # Tempat menyimpan foto-foto pribadi
│       └── README.md
└── src/
    ├── app/
    │   ├── admin/
    │   │   └── page.tsx          # Halaman moderasi & hapus ucapan (/admin)
    │   ├── api/
    │   │   ├── admin/login/
    │   │   │   └── route.ts      # API verifikasi password admin
    │   │   └── messages/
    │   │       ├── [id]/
    │   │       │   └── route.ts  # API DELETE ucapan
    │   │       └── route.ts      # API GET & POST ucapan (dengan rate-limiting)
    │   ├── favicon.ico
    │   ├── globals.css           # Styling dasar, animasi api lilin, & 3D transform
    │   ├── layout.tsx            # Font Google, OpenGraph SEO WhatsApp, Cursor
    │   └── page.tsx              # Halaman utama (Single Page Scroll)
    ├── components/
    │   ├── Cursor.tsx            # Custom cursor lingkaran putih difference blend-mode
    │   ├── GrainOverlay.tsx      # Efek tekstur film grain / noise halus
    │   ├── IntroScreen.tsx       # Loading & intro pembuka "Something special..."
    │   ├── MusicPlayer.tsx       # Floating audio player sudut layar
    │   ├── PinLock.tsx           # Layar proteksi PIN / tanggal jadian (opsional)
    │   ├── SmoothScroll.tsx      # Lenis smooth scroll provider
    │   └── sections/
    │       ├── CountdownSection.tsx    # Hitung hari bersama & countdown ultah
    │       ├── FooterSection.tsx       # Footer editorial & tombol back-to-top
    │       ├── GallerySection.tsx      # Grid masonry grayscale & lightbox foto
    │       ├── GuestMessageSection.tsx # Form ucapan & database feed
    │       ├── HeroSection.tsx         # Headline nama pacar bergaya majalah
    │       ├── LoveLetterSection.tsx   # Amplop digital & efek surat ketik
    │       ├── MakeAWishSection.tsx    # Tiup lilin & confetti monokrom
    │       ├── ReasonsSection.tsx      # 10 kartu 3D flip "Why I Love You"
    │       └── StorySection.tsx        # Timeline perjalanan momen berharga
    ├── config/
    │   └── content.ts            # PUSAT SEMUA KONTEN, TEKS, FOTO, & TANGGAL
    └── lib/
        ├── prisma.ts             # Singleton Prisma client
        └── utils.ts              # Utility classmerge & format tanggal Indonesia
```

---

## 🚀 Langkah Instalasi & Menjalankan di Lokal (Untuk Pemula)

### 1. Prasyarat
Pastikan komputermu sudah terpasang **Node.js** (versi 18.18 atau lebih baru). Jika belum punya, download di [nodejs.org](https://nodejs.org).

### 2. Buka Terminal / CMD di Folder Proyek
Masuk ke folder `monochrome-birthday`:
```bash
cd "C:\Users\ZZNSRA\.gemini\antigravity\scratch\monochrome-birthday"
```

### 3. Install Semua Dependensi
Jalankan perintah berikut:
```bash
npm install
```

### 4. Siapkan Database Lokal (Prisma)
Jalankan perintah ini untuk membuat database SQLite lokal:
```bash
npx prisma db push
```

### 5. Jalankan Server Development
```bash
npm run dev
```

Buka browsermu di: **[http://localhost:3000](http://localhost:3000)** 🎉

---

## ✍️ Cara Mengganti Konten (Teks, Tanggal, Foto, dan Musik)

Semua konten dipusatkan di **satu file**:
👉 `src/config/content.ts`

### 1. Mengubah Data Personal & Tanggal
Buka `src/config/content.ts` di VS Code / editor teks, lalu ubah:
```ts
personal: {
  girlfriendName: "Nama Lengkap Pacar",
  girlfriendNickname: "Panggilan Pacar",
  myRoleName: "Nama Kamu",
  birthdayDate: "2002-11-20",       // Format: YYYY-MM-DD
  anniversaryDate: "2022-04-15",    // Format: YYYY-MM-DD (tanggal jadian)
  turningAge: 22,                   // Usia yang dirayakan
}
```

### 2. Mengubah Foto Sendiri
1. Taruh file foto kamu di folder `public/images/` (misal: `foto-1.jpg`, `foto-2.jpg`, dst).
2. Di `src/config/content.ts`, ganti URL gambarnya menjadi:
   ```ts
   image: "/images/foto-1.jpg",
   ```

### 3. Mengubah Lagu Favorit
1. Taruh file MP3 di folder `public/audio/` (misal: `lagu-kita.mp3`).
2. Di `src/config/content.ts`, ubah bagian `audio`:
   ```ts
   audio: {
     title: "Judul Lagu — Nama Penyanyi",
     src: "/audio/lagu-kita.mp3",
     autoplayOnEnter: true,
   }
   ```

### 4. Mengaktifkan Kunci PIN / Passcode (Opsional)
Jika ingin pacar harus memasukkan kode rahasia (misal tanggal jadian) sebelum membuka website:
```ts
security: {
  enablePinLock: true,              // Ubah jadi true
  pin: "15042022",                  // Masukkan PIN angka atau teks
  hint: "Ketik tanggal pertama kita jadian (DDMMYYYY)",
}
```

---

## 🛡️ Halaman Moderasi Ucapan (`/admin`)

Jika ada orang yang mengirimkan pesan iseng di buku ucapan:
1. Buka `http://localhost:3000/admin` (atau `https://namadomainkamu.vercel.app/admin`).
2. Masukkan password admin (default: `foreverwithyou` atau ubah di file `.env`).
3. Kamu bisa melihat semua ucapan dan menghapusnya dengan sekali klik tombol **HAPUS**.

---

## 🌐 Panduan Deploy ke Vercel (Gratis & Online)

Agar website ini bisa diakses secara online oleh pacar dari smartphone atau laptopnya:

### Langkah 1: Siapkan Database Online (Supabase / Neon)
Karena server Vercel bersifat *serverless*, database SQLite lokal tidak akan menyimpan data permanen di Vercel. Gunakan database PostgreSQL gratis dari [Supabase](https://supabase.com) atau [Neon.tech](https://neon.tech):
1. Buat akun di [Supabase.com](https://supabase.com).
2. Buat Project baru, lalu salin **Connection String URI** (format: `postgresql://postgres:...`).
3. Di file `prisma/schema.prisma`, ubah provider menjadi:
   ```prisma
   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }
   ```

### Langkah 2: Upload Proyek ke GitHub
1. Buat repository baru di [GitHub](https://github.com).
2. Di folder proyek:
   ```bash
   git init
   git add .
   git commit -m "feat: initial monochrome birthday website"
   git branch -M main
   git remote add origin https://github.com/username-kamu/nama-repo.git
   git push -u origin main
   ```

### Langkah 3: Hubungkan ke Vercel
1. Buka [Vercel](https://vercel.com) dan login dengan akun GitHub.
2. Klik **Add New...** -> **Project** -> Import repository GitHub kamu tadi.
3. Di menu **Environment Variables**, tambahkan:
   - `DATABASE_URL` = paste link database dari Supabase
   - `ADMIN_PASSWORD` = password admin rahasia pilihanmu
4. Klik tombol **Deploy**!
5. Tunggu sekitar 1-2 menit hingga statusnya menjadi **Ready**. Kamu akan mendapatkan domain gratis seperti:
   `https://valerie-birthday.vercel.app`

---

## 💌 Cara Membuat Link & QR Code Cantik untuk Pacar

1. Salin link Vercel yang sudah online (contoh: `https://valerie-birthday.vercel.app`).
2. Kunjungi situs pembuat QR Code monokrom gratis seperti [qr-code-generator.com](https://www.qr-code-generator.com) atau [me-qr.com](https://me-qr.com).
3. Pilih desain QR Code minimalis hitam putih tanpa frame ramai.
4. Download gambar QR Code tersebut, lalu:
   - Cetak pada kartu ucapan fisik / amplop surat nyata.
   - Atau kirimkan link langsung lewat WhatsApp / DM Instagram (preview OpenGraph akan otomatis muncul rapi dengan foto dan ucapan estetik!).
