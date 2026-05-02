# 🎮 Duo Dash Destiny
### CO-OP TEMPLE ADVENTURE

> A web-based local co-op platformer game where two players must work together to escape ancient temples — built entirely in a single HTML file.

![Game Preview](https://img.shields.io/badge/Platform-Web%20Browser-blue?style=flat-square)
![Language](https://img.shields.io/badge/Language-Vanilla%20JavaScript-yellow?style=flat-square)
![Rendering](https://img.shields.io/badge/Rendering-HTML5%20Canvas-orange?style=flat-square)
![Backend](https://img.shields.io/badge/Backend-Supabase-green?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

---

## 📖 Daftar Isi

- [Tentang Game](#-tentang-game)
- [Cara Bermain](#-cara-bermain)
- [Fitur Utama](#-fitur-utama)
- [Level & Stage](#-level--stage)
- [Sistem Poin & Grading](#-sistem-poin--grading)
- [Global Leaderboard](#-global-leaderboard)
- [Character Selection](#-character-selection)
- [Team Account System](#-team-account-system)
- [Teknologi & Arsitektur](#-teknologi--arsitektur)
- [Teknik Game Development](#-teknik-game-development)
- [Setup & Instalasi](#-setup--instalasi)
- [Database Schema](#-database-schema)
- [Struktur File](#-struktur-file)

---

## 🏛️ Tentang Game

**Duo Dash Destiny** adalah *web-based local co-op platformer* yang terinspirasi dari [Pico Park](https://picoparkgame.com/). Game ini dimainkan oleh **dua orang dalam satu perangkat secara bersamaan**, dengan masing-masing pemain menggunakan bagian keyboard yang berbeda.

Inti dari game ini adalah **kerja sama tim** — setiap level dirancang agar **tidak bisa diselesaikan sendirian**. Pemain harus berkomunikasi, berkoordinasi, dan saling membantu untuk memecahkan puzzle platform di setiap levelnya.

Game ini dibangun sepenuhnya dalam **satu file HTML** menggunakan vanilla JavaScript dan HTML5 Canvas API — tanpa framework game, tanpa library eksternal untuk gameplay.

---

## 🕹️ Cara Bermain

| | Player 1 — Mochi 🐱 | Player 2 — Biscuit 🐶 |
|---|---|---|
| **Lompat** | `W` | `↑` |
| **Gerak Kiri** | `A` | `←` |
| **Gerak Kanan** | `D` | `→` |

### Mekanik Khusus

**Boost Jump** — Diam di tempat, biarkan partner melompat ke atasmu, lalu lompat lagi — partner akan melesat ke atas! Digunakan untuk mencapai platform yang terlalu tinggi untuk lompatan biasa.

---

## ✨ Fitur Utama

### 🎮 Local Co-op Real-Time
Dua pemain bermain secara bersamaan di satu perangkat. Kedua pemain harus berada di **EXIT** secara bersamaan untuk menyelesaikan level — tidak bisa satu pemain menunggu yang lain.

### 🏆 Global Online Leaderboard
Skor terhubung ke database Supabase secara real-time. Leaderboard tersedia dalam tiga kategori:
- **Per Level** — Best score tiap level secara individual
- **Per Stage** — Total best score semua level dalam satu stage
- **All Combined** — Total semua best score dari seluruh 10 level

Sistem hanya menyimpan **skor tertinggi** tiap tim — bukan setiap run.

### ⏱️ Sistem Poin & Timer
Live timer berjalan di HUD selama gameplay. Skor dihitung berdasarkan formula hybrid yang mempertimbangkan kecepatan dan akurasi.

### 👥 Team Account System
Sistem akun berbasis Tim — karena game ini dimainkan berpasangan. Tersedia mode Guest untuk main tanpa akun (skor tidak masuk leaderboard).

### 🎭 Character Selection
Sebelum setiap sesi, kedua pemain memilih karakter masing-masing dari 6 pilihan. Setiap karakter memiliki skin yang digambar dari scratch di canvas dengan anatomi unik.

### 💾 Per-Account Progress
Progress unlock level dan best score tersimpan secara terpisah per akun — tidak bocor antar akun atau ke mode Guest.

---

## 🗺️ Level & Stage

### ⭐ Stage 1 — Temple Ruins (Level 1–5)
Memperkenalkan mekanik dasar game secara bertahap.

| Level | Nama | Mekanik |
|---|---|---|
| 1 | Hello, Temple! | Intro sederhana — daki tangga masing-masing |
| 2 | Push & Pull | Pressure Plates & Gates |
| 3 | Lift Me Up! | Boost Jump |
| 4 | The Vault | Keys & Doors |
| 5 | Temple Finale | Kombinasi semua mekanik Stage 1 |

### ⚠️ Stage 2 — Underground Ruins (Level 6–10)
Menambah kompleksitas dengan mekanik baru dan lubang mematikan.

| Level | Nama | Mekanik |
|---|---|---|
| 6 | Mind the Gap | Launch Pads + lubang mematikan |
| 7 | Island Hopping | Scrolling world — 4 pulau terpisah |
| 8 | Bridge Builder | Ghost Bridge (hanya ada saat plate ditekan) |
| 9 | Key Across the Void | Keys & Chasms — kunci di pulau terisolasi |
| 10 | Grand Canyon | Stage 2 Finale — semua mekanik digabung |

### Mekanik Gameplay

| Mekanik | Deskripsi |
|---|---|
| **Pressure Plate** | Harus diinjak untuk membuka Gate — lepas dan Gate menutup |
| **Gate** | Penghalang yang terbuka saat Plate aktif |
| **Key & Door** | Kunci diambil oleh pemain, digunakan untuk membuka Door |
| **Boost Zone** | Area di mana pemain bisa melakukan Boost Jump |
| **Launch Pad** | Platform khusus yang memberi super jump saat diinjak |
| **Ghost Bridge** | Jembatan tak kasat mata yang muncul hanya saat Plate ditekan |
| **Lubang/Chasm** | Area mematikan — jatuh ke sini akan respawn di posisi awal |

---

## 🏅 Sistem Poin & Grading

### Formula

```
Total Poin = Base Points + Speed Bonus - Fall Penalty
```

| Komponen | Nilai | Keterangan |
|---|---|---|
| **Base Points** | 1,000 | Didapat dari setiap level yang berhasil diselesaikan |
| **Speed Bonus** | max 500 | Berkurang 5 poin per detik. Makin cepat = makin tinggi |
| **Fall Penalty** | -150 per jatuh | Dikurangi setiap kali pemain jatuh ke lubang/chasm |

**Contoh:** Selesaikan level dalam 30 detik dengan 1 kali jatuh:
```
1000 + (500 - 150) - 150 = 1,200 poin
```

### Star Threshold

| Bintang | Poin |
|---|---|
| ★★★ | ≥ 900 poin |
| ★★ | 600 – 899 poin |
| ★ | < 600 poin |

Badge **✨ New Best!** muncul di layar Level Complete saat rekor pribadi dipecahkan.

---

## 🌐 Global Leaderboard

Leaderboard terhubung ke **Supabase** secara real-time dan dapat diakses dari Main Menu.

### Tampilan Leaderboard

**Per Level** — Pilih level (1–10), tampilkan ranking berdasarkan poin tertinggi. Menampilkan: Rank, Team Name, Stars, Time, Points.

**Per Stage** — Pilih Stage 1 atau Stage 2, tampilkan total best score dari semua level dalam stage tersebut. Menampilkan: Rank, Team Name, Levels Done, Total Points.

**All Combined** — Total best score dari semua 10 level. Menampilkan: Rank, Team Name, Levels Done, Total Points.

### Aturan Leaderboard
- Hanya **skor tertinggi** tiap tim per level yang dihitung
- Guest tidak masuk leaderboard
- Leaderboard di-fetch langsung dari Supabase setiap kali dibuka

---

## 🎭 Character Selection

Setiap pemain memilih karakter sebelum bermain. Pilihan tersimpan di `localStorage` dan bertahan antar sesi.

### Player 1

| Karakter | Hewan | Warna | Ciri Khas |
|---|---|---|---|
| 🐱 **Mochi** | Kucing | Merah-coral | Telinga lancip, kumis panjang, ekor bergradasi |
| 🦊 **Blaze** | Rubah | Oranye | Telinga sangat lancip, pipi putih khas rubah, ekor besar berbulu |
| 🐺 **Frost** | Serigala | Biru es | Moncong angular, tanda salju di dahi, ekor tebal gelap |

### Player 2

| Karakter | Hewan | Warna | Ciri Khas |
|---|---|---|---|
| 🐶 **Biscuit** | Anjing | Teal | Telinga floppy panjang, hidung kotak, tag kalung |
| 🐻 **Coco** | Beruang | Amber coklat | Telinga bulat kecil, moncong lebar, tetesan madu di dahi |
| 🐯 **Spark** | Harimau | Kuning emas | Loreng hitam di seluruh tubuh, moncong putih, kumis tebal |

Setiap skin digambar dari scratch menggunakan **Canvas 2D API** — bukan sekadar ganti warna, melainkan anatomi hewan yang benar-benar berbeda.

---

## 👥 Team Account System

### Cara Daftar / Login
1. Klik **Login** di badge Guest pada Main Menu
2. Pilih tab **Register** untuk akun baru atau **Login** untuk akun yang sudah ada
3. Masukkan **Team Name** dan **Password**
4. Klik tombol aksi

### Keamanan
- Password di-hash menggunakan **SHA-256** via Web Crypto API sebelum dikirim ke Supabase
- Tidak ada password plaintext yang tersimpan di database
- Session tersimpan di `localStorage` dan otomatis di-restore saat halaman dibuka kembali

### Progress Isolation
- Setiap akun tim memiliki progress tersendiri (`localStorage` key: `duoDash_team_{uuid}`)
- Mode Guest memiliki progress terpisah (`duoDash_guest`)
- Logout otomatis mereset tampilan ke progress Guest
- Login ke akun lain otomatis memuat progress akun tersebut

---

## 🛠️ Teknologi & Arsitektur

### Tech Stack

| Layer | Teknologi |
|---|---|
| **Rendering** | HTML5 Canvas 2D API |
| **Language** | Vanilla JavaScript (ES6+) |
| **Styling** | CSS3 — tanpa framework |
| **Backend / DB** | Supabase (PostgreSQL + REST API) |
| **Authentication** | Custom + Web Crypto API (SHA-256) |
| **Storage** | localStorage — progress & session |
| **Deployment** | Single HTML file — portable, no build step |

### Arsitektur

```
duo-dash-destiny.html
├── <style>          CSS — semua styling dalam satu blok
├── <body>           HTML screens (sTitle, sAuth, sCharSelect, 
│                    sGame, sComplete, sWin, sControls, 
│                    sLevels, sPause, sLeaderboard)
└── <script>
    ├── LEVELS[]         Data definisi semua 10 level
    ├── CHARS{}          Data karakter (nama, emoji, warna)
    ├── Game Engine
    │   ├── Game Loop    requestAnimationFrame + delta time
    │   ├── Player       Class dengan physics, collision, rendering
    │   ├── buildSolids  Mengumpulkan semua solid objects per frame
    │   ├── resolveVsSolids  AABB collision vs platforms/gates/walls
    │   ├── resolveVsPartner Collision & push antar player
    │   ├── drawPlayer   Per-character canvas rendering
    │   └── Particle System  Efek visual lightweight
    ├── Scoring System
    │   ├── calcPoints   Formula Base + Speed - Fall
    │   ├── Timer        Pause-aware live timer
    │   └── saveProgress Per-account localStorage
    ├── Auth System
    │   ├── hashPassword Web Crypto SHA-256
    │   ├── handleAuth   Register / Login flow
    │   └── updateAuthUI Badge state management
    └── Supabase Integration
        ├── sbFetch      REST API wrapper
        ├── submitScore  POST ke level_scores
        └── loadLeaderboard Fetch & render per mode
```

---

## ⚙️ Teknik Game Development

### 1. HTML5 Canvas Rendering
Seluruh gameplay dirender di atas `<canvas>` menggunakan Canvas 2D API. Semua objek game — karakter, platform, gate, kunci, partikel — digambar langsung setiap frame. Tidak ada DOM manipulation untuk objek game.

### 2. Game Loop dengan Delta Time
```javascript
function loop(ts) {
  const dt = Math.min((ts - lastTime) || 16, 50); // cap delta time
  lastTime = ts;
  Object.keys(justPressed).forEach(k => justPressed[k] = false);
  update(dt);
  draw();
  requestAnimationFrame(loop);
}
```
Delta time capping mencegah physics explosion saat tab tidak aktif atau terjadi frame drop ekstrem.

### 3. Camera / Viewport System
Canvas dikunci di resolusi **900×508 piksel**, tapi "dunia" level bisa jauh lebih lebar (hingga 1800px). Setiap frame, `cameraX` dihitung dari rata-rata posisi kedua pemain, lalu semua objek digambar dengan offset `x - cameraX`.

```javascript
// Camera follows the midpoint of both players
const targetCam = (p1.cx + p2.cx) / 2 - GW / 2;
cameraX += (targetCam - cameraX) * 0.08; // smooth lerp
```

Teknik ini identik dengan yang digunakan di game 2D klasik — dunia yang bergerak, bukan kamera yang bergerak.

### 4. AABB Collision Detection
Physics collision menggunakan **Axis-Aligned Bounding Box**. Resolusi dipisahkan per sumbu (X dan Y) untuk mencegah *corner sticking*.

```javascript
function resolveVsSolids(pl, solids) {
  // Move X, resolve X
  pl.x += pl.vx;
  for (const s of solids) { /* resolve horizontal */ }
  // Move Y, resolve Y
  pl.y += pl.vy;
  for (const s of solids) { /* resolve vertical + set onGround */ }
}
```

### 5. Player-to-Player Push dengan Wall Awareness
Ketika satu pemain mendorong pemain lain, sistem terlebih dahulu mengecek apakah pemain yang akan didorong sudah menempel di wall — jika ya, mover dihentikan total untuk mencegah wall penetration.

```javascript
const otherHasWallRight = solids.some(s =>
  ov(other.x + 1, other.y, other.w, other.h, s.x, s.y, s.w, s.h)
);
if (otherHasWallRight) {
  mover.x = other.x - mover.w;
  mover.vx = 0; // stop mover, don't push other through wall
}
```

### 6. Coyote Time & Jump Buffer
Dua teknik *game feel* klasik:

- **Coyote Time** — Grace period ~6 frame setelah meninggalkan tepi platform di mana jump masih bisa dilakukan. Mencegah rasa frustrasi saat lompat di ujung platform.
- **Jump Buffer** — Input jump disimpan selama ~8 frame. Jump yang ditekan sedikit sebelum mendarat tetap terdaftar.

```javascript
if (isJ && !pl.wasJump) pl.jumpBuffer = 8;
if (pl.onGround) pl.coyote = 6;

if (pl.jumpBuffer > 0 && pl.coyote > 0) {
  pl.vy = JFORCE; // execute jump
  pl.coyote = 0;
  pl.jumpBuffer = 0;
}
```

### 7. Particle System
Efek visual diimplementasikan dengan lightweight particle array:
```javascript
// Each particle: { x, y, vx, vy, life, maxLife, color, size, type }
function spawnGfx(x, y, type, color, scale) { /* ... */ }
function updateParticles() { /* update position, fade, remove dead */ }
function drawParticles() { /* render each particle */ }
```

### 8. Web Crypto API untuk Password Hashing
```javascript
async function hashPassword(password) {
  const encoder = new TextEncoder();
  const data = encoder.encode(password);
  const hashBuffer = await crypto.subtle.digest('SHA-256', data);
  const hashArray = Array.from(new Uint8Array(hashBuffer));
  return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
}
```
Password di-hash di browser sebelum dikirim ke Supabase — tidak ada plaintext password yang pernah meninggalkan device.

---

## 🚀 Setup & Instalasi

### Prasyarat
- Browser modern (Chrome, Firefox, Edge, Safari)
- Akun [Supabase](https://supabase.com) (gratis)
- Koneksi internet (untuk leaderboard & auth)

### Langkah Setup

**1. Clone / Download**
```bash
git clone https://github.com/username/duo-dash-destiny.git
cd duo-dash-destiny
```

**2. Buat Project Supabase**
- Daftar di [supabase.com](https://supabase.com)
- Buat project baru
- Catat **Project URL** dan **anon public key** dari Settings → API

**3. Setup Database**

Jalankan SQL berikut di **Supabase SQL Editor**:

```sql
-- Tabel tim (akun)
CREATE TABLE teams (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  team_name text UNIQUE NOT NULL,
  password_hash text NOT NULL,
  created_at timestamptz DEFAULT now()
);

-- Tabel scores per level
CREATE TABLE level_scores (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id uuid REFERENCES teams(id) ON DELETE CASCADE,
  level_number int NOT NULL,
  points int NOT NULL,
  time_seconds float NOT NULL,
  falls int NOT NULL,
  stars int NOT NULL,
  played_at timestamptz DEFAULT now()
);

-- Index untuk query leaderboard
CREATE INDEX ON level_scores(level_number, points DESC);
CREATE INDEX ON level_scores(team_id);

-- Row Level Security
ALTER TABLE teams ENABLE ROW LEVEL SECURITY;
ALTER TABLE level_scores ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Public read scores" ON level_scores FOR SELECT USING (true);
CREATE POLICY "Public read teams"  ON teams        FOR SELECT USING (true);
CREATE POLICY "Insert scores"      ON level_scores FOR INSERT WITH CHECK (true);
CREATE POLICY "Insert teams"       ON teams        FOR INSERT WITH CHECK (true);
```

**4. Konfigurasi Credentials**

Buka `duo-dash-destiny.html`, cari dan ganti:

```javascript
const SUPABASE_URL = 'https://YOUR_PROJECT_ID.supabase.co';
const SUPABASE_KEY = 'YOUR_ANON_PUBLIC_KEY';
```

**5. Jalankan**

Buka file HTML langsung di browser — tidak perlu server, tidak perlu build step.

```bash
open duo-dash-destiny.html
# atau drag & drop ke browser
```

---

## 🗄️ Database Schema

```
teams
├── id            uuid PRIMARY KEY
├── team_name     text UNIQUE NOT NULL
├── password_hash text NOT NULL  (SHA-256)
└── created_at    timestamptz

level_scores
├── id            uuid PRIMARY KEY
├── team_id       uuid → teams.id
├── level_number  int  (1–10)
├── points        int
├── time_seconds  float
├── falls         int
├── stars         int  (1–3)
└── played_at     timestamptz
```

### Query Leaderboard

```sql
-- Per Level: best score per team untuk level tertentu
SELECT t.team_name, MAX(ls.points) as best_points, 
       MIN(ls.time_seconds) as best_time, MAX(ls.stars) as stars
FROM level_scores ls
JOIN teams t ON t.id = ls.team_id
WHERE ls.level_number = 1
GROUP BY t.team_name
ORDER BY best_points DESC;

-- All Combined: total best score semua level
SELECT t.team_name, SUM(best.max_pts) as total_points, COUNT(*) as levels_done
FROM teams t
JOIN (
  SELECT team_id, level_number, MAX(points) as max_pts
  FROM level_scores
  GROUP BY team_id, level_number
) best ON best.team_id = t.id
GROUP BY t.team_name
ORDER BY total_points DESC;
```

---

## 📁 Struktur File

```
duo-dash-destiny/
├── duo-dash-stable.html    # Seluruh game dalam satu file
├── duo-dash-v01...         # Versi versi sebelumnya
├── vercel.json             # Koneksi ke Vercel
└── README.md               # Dokumentasi ini
```

Karena game dibangun dalam satu file, tidak ada build process, dependency management, atau konfigurasi tambahan yang diperlukan selain mengisi Supabase credentials.

---

## 🎮 Inspirasi

Game ini terinspirasi dari **Pico Park** oleh TECOPARK — sebuah co-op puzzle platformer yang mengharuskan semua pemain bergerak dan berpikir bersama. Duo Dash Destiny mengadaptasi konsep tersebut untuk dimainkan oleh tepat dua orang di satu perangkat, dengan tambahan fitur online leaderboard dan character selection.

---

## 📝 Lisensi

MIT License — bebas digunakan, dimodifikasi, dan didistribusikan.

---

<div align="center">

**Dibuat dengan passion oleh Faizal Kurniawan**

*"Two players. One destiny."*

</div>
