## Deskripsi Lengkap Duo Dash Destiny

---

### Overview

Duo Dash Destiny adalah web-based local co-op platformer game yang terinspirasi dari Pico Park. Dimainkan oleh dua orang dalam satu perangkat secara bersamaan, game ini berfokus pada kerja sama tim untuk memecahkan puzzle dan melewati berbagai rintangan platform di setiap levelnya. Dibangun sepenuhnya dalam satu file HTML menggunakan vanilla JavaScript dan HTML5 Canvas — tanpa framework game, tanpa library eksternal untuk gameplay.

---

### Fitur Utama

**🎮 Local Co-op Gameplay**
Dua pemain bermain dalam satu perangkat secara real-time. Player 1 (Mochi) menggunakan kontrol W/A/D, Player 2 (Biscuit) menggunakan Arrow Keys. Setiap level dirancang agar tidak bisa diselesaikan sendirian — selalu membutuhkan koordinasi dari kedua pemain.

**🏛️ 10 Level dengan 2 Stage**
Stage 1 (Temple Ruins) memperkenalkan mekanik dasar seperti Pressure Plates, Gates, Keys & Doors, dan Boost Jump. Stage 2 (Underground Ruins) menambah kompleksitas dengan Launch Pads, lubang mematikan, Ghost Bridge, dan scrolling world. Difficulty meningkat secara progresif dari level ke level.

**⏱️ Sistem Poin & Grading**
Setiap level dinilai berdasarkan formula hybrid: `Base Points (1000) + Speed Bonus (max 500, berkurang 5 per detik) - Fall Penalty (150 per jatuh)`. Hasil poin menentukan bintang yang didapat (★★★ ≥900, ★★ ≥600, ★ <600). Personal best tersimpan per level, dan badge "✨ New Best!" muncul saat rekor dipecahkan.

**🏆 Global Online Leaderboard**
Leaderboard terhubung ke Supabase secara real-time dengan tiga kategori tampilan: Per Level (best score tiap level), Per Stage (total best score per stage), dan All Combined (total seluruh level). Sistem hanya mencatat skor tertinggi tiap tim — bukan setiap run. Guest tidak masuk leaderboard.

**👥 Team Account System**
Sistem akun berbasis Tim (bukan individual) karena game ini dimainkan berpasangan. Autentikasi menggunakan Team Name + Password dengan password hashing via Web Crypto API (SHA-256) di browser. Session tersimpan di localStorage. Progress tiap akun terisolasi satu sama lain — logout otomatis mereset ke progress Guest.

**🎭 Character Selection**
Sebelum bermain, kedua pemain memilih karakter masing-masing dari 6 pilihan yang tersedia. Player 1 bisa memilih antara Mochi (kucing), Blaze (rubah), atau Frost (serigala). Player 2 bisa memilih antara Biscuit (anjing), Coco (beruang), atau Spark (harimau). Setiap karakter memiliki skin yang digambar dari scratch di canvas dengan anatomi unik — bukan sekadar ganti warna. Pilihan tersimpan di localStorage dan bertahan antar sesi.

**💾 Progress System**
Progress unlock level dan best score tersimpan secara lokal per akun menggunakan localStorage dengan key yang unik per team ID. Level unlock secara sequential — menyelesaikan satu level membuka level berikutnya.

---

### Teknologi & Teknik Game Development

**HTML5 Canvas Rendering**
Seluruh gameplay dirender di atas elemen `<canvas>` menggunakan Canvas 2D API. Tidak ada DOM manipulation untuk objek game — semua platform, karakter, gate, kunci, partikel digambar langsung ke canvas setiap frame menggunakan `requestAnimationFrame`.

**Game Loop dengan Delta Time**
Game menggunakan fixed-step game loop berbasis `requestAnimationFrame` dengan delta time capping untuk mencegah physics explosion saat tab tidak aktif atau frame drop ekstrem. Ini memastikan kecepatan gameplay konsisten di berbagai frame rate.

**Camera System / Viewport Scrolling**
Ini yang kamu maksud — teknik ini disebut **Camera/Viewport System** atau lebih spesifiknya **World-Space to Screen-Space Transform**. Canvas resolution dikunci di 900×508 piksel, tapi "dunia" level bisa jauh lebih lebar (hingga 1800px di level 10). Setiap frame, posisi `cameraX` dihitung berdasarkan rata-rata posisi kedua pemain, lalu semua objek digambar dengan offset `x - cameraX`. Hasilnya: dunia bergerak mengikuti pemain meski canvas tidak berubah ukurannya. Teknik ini identik dengan yang digunakan di game 2D klasik seperti Super Mario Bros.

**AABB Collision Detection**
Physics collision menggunakan Axis-Aligned Bounding Box — setiap objek punya rectangle hitbox, dan overlap dicek secara matematis tiap frame. Resolusi collision memisahkan sumbu horizontal dan vertikal untuk mencegah "corner sticking".

**Particle System**
Efek visual seperti debu saat mendarat, sparkle saat boost jump, dan glow saat berinteraksi dengan Launch Pad diimplementasikan dengan lightweight particle system — array objek dengan posisi, velocity, opacity, dan lifetime yang diupdate dan dirender tiap frame.

**Coyote Time & Jump Buffer**
Dua teknik game feel klasik yang diimplementasikan: Coyote Time memberi jendela ~6 frame setelah karakter meninggalkan tepi platform di mana jump masih bisa dilakukan, dan Jump Buffer menyimpan input jump selama ~8 frame sehingga jump yang ditekan sedikit sebelum mendarat tetap terdaftar. Kedua teknik ini membuat kontrol terasa responsif dan tidak frustrasi.

**Web Crypto API untuk Password Hashing**
Password tidak disimpan dalam plaintext. Sebelum dikirim ke Supabase, password di-hash menggunakan `crypto.subtle.digest('SHA-256')` yang merupakan native browser API — tanpa library kriptografi eksternal.

**Supabase sebagai Backend**
Database dan API menggunakan Supabase (PostgreSQL) dengan Row Level Security. Semua query ke Supabase dilakukan via REST API langsung dari browser menggunakan `fetch()`. Tabel yang digunakan: `teams` (akun), `level_scores` (riwayat skor untuk leaderboard).

---

### Tech Stack Ringkas

| Layer | Teknologi |
|---|---|
| Rendering | HTML5 Canvas 2D API |
| Language | Vanilla JavaScript (ES6+) |
| Styling | CSS3 (tanpa framework) |
| Backend/DB | Supabase (PostgreSQL + REST) |
| Auth | Custom + Web Crypto API (SHA-256) |
| Storage | localStorage (progress & session) |
| Hosting | Single HTML file (portable) |
