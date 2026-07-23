Bertindaklah sebagai Senior Frontend Developer. Buatkan saya kode lengkap untuk satu halaman profil/portofolio statis menggunakan Vue 3 dengan pendekatan Composition API (`<script setup>`). Halaman ini murni 1 file (App.vue), tanpa TypeScript, tanpa Vue Router, dan tanpa Pinia. 

Gunakan Tailwind CSS (melalui CDN di index.html atau asumsikan sudah terpasang) untuk styling utama, dan gunakan blok `<style scoped>` untuk animasi kustom. 

Desain keseluruhan harus modern, clean, dengan background putih/terang, dan navbar bergaya pill/glassmorphism yang sticky di atas. 

Berikut adalah spesifikasi detail untuk setiap section yang HARUS ada beserta interaksinya:

1. Pre-loader Animasi (Opening):
   - Layar putih penuh menutupi web. Di tengahnya ada teks yang berganti bahasa setiap 0.5 detik: "Halo" (Indonesia) -> "Hello" (Inggris) -> "Hallo" (Jerman) -> "Привет" (Rusia) -> "こんにちは" (Jepang) -> "안녕하세요" (Korea).
   - Setelah kata bahasa Korea selesai, layar putih ini harus memiliki animasi slide-up (menggulir ke atas) hingga menghilang dan menampilkan halaman web utama.

2. Navbar:
   - Sticky di tengah atas, berbentuk melengkung (rounded-full).
   - Menu: Home, About, Experience, Projects, Contact. (Gunakan anchor tag biasa untuk smooth scrolling ke id section).

3. Hero Section (id="home"):
   - Layout 2 kolom (kiri teks, kanan gambar).
   - Kiri: Teks "HELLO, I AM Moh. Syaeful Effendi.", dengan badge "Fullstack Developer". Logo icon (GitHub, Instagram, LinkedIn). Paragraf "Building functional, interactive, and user-centric web applications." Dua tombol: "View Projects" (warna aksen utama) dan "Contact Me" (outline).
   - Kanan: Sebuah foto (gunakan placeholder div/img). Buat interaksi efek 3D tilt: saat kursor mouse bergerak di atas foto, foto tersebut sedikit miring/bergerak mengikuti arah kursor.

4. About Section (id="about"):
   - Layout 2 kolom kebalikan hero. Kiri gambar, kanan teks.
   - Kanan: Judul highlight "I AM SYAEFUL EFFENDI, FULLSTACK DEVELOPER". 
   - Teks perkenalan: "Mahasiswa IT Semester 4 dan Asisten Dosen Pemrograman Web. Saya memiliki minat mendalam pada Full-stack Development, Artificial Intelligence (Computer Vision), Game Development, dan Internet of Things."
   - Tombol "Download CV".

5. Tech Skills (id="skills"):
   - Buat animasi infinite horizontal marquee (teks berjalan tanpa batas).
   - Baris 1 (bergerak ke kiri): React JS, Vue JS, Flask, Docker, MySQL, Tailwind.
   - Baris 2 (bergerak ke kanan): Godot Engine, Arduino IDE, YOLOv8, MediaPipe, LSTM, IoT.

6. Certificates Section (id="certificates"):
   - Layout CSS Grid, persis 3 kolom per baris (grid-cols-3).
   - Gunakan desain card sederhana untuk 6 sertifikat (gunakan placeholder box dengan rasio landscape).

7. Work Experience (id="experience"):
   - Desain list/timeline.
   - Masukkan pengalaman: "Asisten Dosen Pemrograman Web" - Mengajar mahasiswa tingkat bawah dan mempersiapkan materi modul pembelajaran.

8. Featured Works / Projects (id="projects"):
   - Layout Grid Card. Saat card diklik, munculkan modal/popup sederhana yang menampilkan detail lengkap dan link.
   - Masukkan state data project ini:
     1. "Bahasaku: Sistem Penerjemah Bahasa Isyarat" - CV real-time menggunakan MediaPipe dan arsitektur LSTM.
     2. "Smart Door Lock & Access Log" - Sistem IoT dengan RFID terintegrasi MySQL.
     3. "2D Farming Simulation" - Game simulasi pertanian (Godot Engine).

9. Contact Section (id="contact"):
   - Layout 2 kolom.
   - Kiri: Teks ajakan, Email, dan Lokasi (Madiun, Indonesia).
   - Kanan: Form kontak (Nama, Email, Pesan, tombol Submit).

Tolong tuliskan kode lengkapnya untuk `App.vue`. Pastikan logika state (untuk preloader, data project, modal, dan event listener 3D tilt) ditulis dengan rapi menggunakan `ref` dan `onMounted` di dalam `<script setup>`.