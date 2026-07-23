<script setup>
import { ref, onMounted, computed, onUnmounted } from 'vue';
import cvUrl from './assets/CV Moh. Syaeful Effendi.pdf';
import cert1 from './assets/Certificates/Working With Data.jpg';
import cert2 from './assets/Certificates/Advanced MySQL Topics.jpg';
import cert3 from './assets/Certificates/Database Engineer Capstone.jpg';
import cert4 from './assets/Certificates/Web Design Wireframe to Prototypes.jpg';
import cert5 from './assets/Certificates/Web Design Strategy and Information.jpg';
import cert6 from './assets/Certificates/User Experience Design.jpg';
import bahasakuLogo from './assets/project/Logo Bahasaku.jpg';
import cuaninLogo from './assets/project/Cuanin.jpg';


const certificatesData = [
  { title: 'Working with Data', issuer: 'Meta', link: 'https://www.coursera.org/account/accomplishments/verify/UGO69O6KSPUP', image: cert1 },
  { title: 'Advanced MySQL Topics', issuer: 'Meta', link: 'https://www.coursera.org/account/accomplishments/verify/EHTZ5CBNCS26', image: cert2 },
  { title: 'Database Engineer Capstone', issuer: 'Meta', link: 'https://www.coursera.org/account/accomplishments/verify/69JGARHF70K3', image: cert3 },
  { title: 'Web Design: Wireframes to Prototypes', issuer: 'California Institute of the Arts', link: 'https://www.coursera.org/account/accomplishments/verify/BZ9B4HWYN4ZZ', image: cert4 },
  { title: 'Web Design: Strategy and Information Architecture', issuer: 'California Institute of the Arts', link: 'https://www.coursera.org/account/accomplishments/verify/622NIJZXNWBK', image: cert5 },
  { title: 'User experience design', issuer: 'University of Cambridge', link: 'https://www.coursera.org/account/accomplishments/verify/O4IQ1YC66R31', image: cert6 }
];

// Language State & Translations
const lang = ref('EN');
const translations = {
  ID: {
    nav: { home: 'Home', about: 'About', experience: 'Experience', projects: 'Projects', contact: 'Contact' },
    hero: { role: 'Fullstack Developer', title: 'HELLO, I AM', subtitle: 'Membangun aplikasi web fungsional, interaktif, dan berpusat pada pengguna.', viewProjects: 'Lihat Proyek', contactMe: 'Hubungi Saya' },
    about: { title: 'SAYA <span class="bg-blue-500 text-white px-3 py-1 sm:px-4 sm:py-1.5 rounded-lg sm:rounded-xl inline-block mb-2 sm:mb-3">SYAEFUL EFFENDI</span>,<br/> <span class="bg-emerald-500 text-white px-3 py-1 sm:px-4 sm:py-1.5 rounded-lg sm:rounded-xl inline-block mt-1 sm:mt-2">FULLSTACK DEVELOPER</span>', desc: 'Sebagai seorang <span class="font-semibold text-gray-800">Software Engineer</span> dan akademisi yang berfokus pada <span class="font-semibold text-gray-800">Full-stack Development</span>, saya memiliki dedikasi tinggi dalam membangun ekosistem digital yang mulus, stabil, dan interaktif.<br/><br/>Berbekal pengalaman dalam pengembangan web, kecerdasan buatan, dan Internet of Things, saya menikmati tantangan merancang sistem yang skalabel—mulai dari antarmuka pengguna hingga logika backend—untuk memecahkan masalah di dunia nyata.', downloadCV: 'Unduh CV' },
    experience: { title: 'Pengalaman Kerja', role1: 'Asisten Dosen Pemrograman Web', location1: 'Universitas', desc1: 'Mengajar mahasiswa tingkat bawah dan mempersiapkan materi modul pembelajaran.' },
    projects: { title: 'Proyek Unggulan', desc: 'Pilihan proyek terbaru yang menunjukkan keahlian saya di berbagai bidang.', viewDetails: 'Lihat Detail', visitProject: 'Kunjungi Proyek' },
    contact: { title: "Mari bekerja", titleBlue: " bersama!", subtitle: "Jangan ragu untuk menghubungi saya untuk kolaborasi, proyek, atau sekadar menyapa.", name: 'Nama', message: 'Pesan', send: 'Kirim Pesan', certificates: 'Sertifikat', certSub: 'Kumpulan pencapaian dan kursus profesional yang telah saya selesaikan.' }
  },
  EN: {
    nav: { home: 'Home', about: 'About', experience: 'Experience', projects: 'Projects', contact: 'Contact' },
    hero: { role: 'Fullstack Developer', title: 'HELLO, I AM', subtitle: 'Building functional, interactive, and user-centric web applications.', viewProjects: 'View Projects', contactMe: 'Contact Me' },
    about: { title: 'I AM <span class="bg-blue-500 text-white px-3 py-1 sm:px-4 sm:py-1.5 rounded-lg sm:rounded-xl inline-block mb-2 sm:mb-3">SYAEFUL EFFENDI</span>,<br/> <span class="bg-emerald-500 text-white px-3 py-1 sm:px-4 sm:py-1.5 rounded-lg sm:rounded-xl inline-block mt-1 sm:mt-2">FULLSTACK DEVELOPER</span>', desc: 'As a <span class="font-semibold text-gray-800">Software Engineer</span> and academic focusing on <span class="font-semibold text-gray-800">Full-stack Development</span>, I am highly dedicated to building seamless, stable, and interactive digital ecosystems.<br/><br/>Equipped with experience in web development, artificial intelligence, and the Internet of Things, I enjoy the challenge of designing scalable systems—from user interfaces to backend logic—to solve real-world problems.', downloadCV: 'Download CV' },
    experience: { title: 'Work Experience', role1: 'Web Programming Teaching Assistant', location1: 'University', desc1: 'Teaching junior students and preparing learning module materials.' },
    projects: { title: 'Featured Works', desc: 'A selection of recent projects showcasing my expertise in various domains.', viewDetails: 'View Details', visitProject: 'Visit Project' },
    contact: { title: "Let's work", titleBlue: " together!", subtitle: "Feel free to reach out for collaborations, project inquiries, or just a friendly hello.", name: 'Name', message: 'Message', send: 'Send Message', certificates: 'Certificates', certSub: 'A collection of achievements and professional courses I have completed.' }
  }
};
const t = computed(() => translations[lang.value]);

// Preloader State
const greetings = ["Halo", "Hello", "Hallo", "Привет", "こんにちは", "안녕하세요"];
const currentGreeting = ref(greetings[0]);
const preloaderActive = ref(true);
const slideUp = ref(false);
const showMainContent = ref(false);

// 3D Tilt State
const tiltCard = ref(null);
const tiltX = ref(0);
const tiltY = ref(0);

// Mobile Menu State
const isMobileMenuOpen = ref(false);
const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};

const handleMouseMove = (e) => {
  if (!tiltCard.value || window.innerWidth < 1024) return; // Disable tilt on mobile for better performance
  const rect = tiltCard.value.getBoundingClientRect();
  const x = e.clientX - rect.left; 
  const y = e.clientY - rect.top;  
  
  const centerX = rect.width / 2;
  const centerY = rect.height / 2;
  
  const rotateX = ((y - centerY) / centerY) * -15;
  const rotateY = ((x - centerX) / centerX) * 15;
  
  tiltX.value = rotateX;
  tiltY.value = rotateY;
};

const handleMouseLeave = () => {
  tiltX.value = 0;
  tiltY.value = 0;
};

const tiltStyle = computed(() => ({
  transform: `perspective(1000px) rotateX(${tiltX.value}deg) rotateY(${tiltY.value}deg) scale3d(1.02, 1.02, 1.02)`,
  transition: tiltX.value === 0 && tiltY.value === 0 ? 'transform 0.5s ease' : 'transform 0.1s ease-out'
}));

// Tech Stack Data
const skillsRow1 = [
  { name: 'Tailwind CSS', icon: 'tailwind' },
  { name: 'PostgreSQL', icon: 'postgresql' },
  { name: 'Next.js', icon: 'nextjs' },
  { name: 'Laravel', icon: 'laravel' },
  { name: 'ReactJS', icon: 'react' },
  { name: 'VueJS', icon: 'vue' },
  { name: 'HTML5', icon: 'html' },
  { name: 'CSS3', icon: 'css' }
];

const skillsRow2 = [
  { name: 'Figma', icon: 'figma' },
  { name: 'Git', icon: 'git' },
  { name: 'PHP', icon: 'php' },
  { name: 'MySQL', icon: 'mysql' },
  { name: 'TypeScript', icon: 'ts' },
  { name: 'Node.js', icon: 'nodejs' },
  { name: 'Python', icon: 'python' },
  { name: 'Docker', icon: 'docker' }
];

const marquee1 = [...skillsRow1, ...skillsRow1, ...skillsRow1, ...skillsRow1, ...skillsRow1];
const marquee2 = [...skillsRow2, ...skillsRow2, ...skillsRow2, ...skillsRow2, ...skillsRow2];

// Projects Data & Modal
const projects = computed(() => {
  if (lang.value === 'ID') {
    return [
      { 
        title: "Bahasaku: Sistem Penerjemah Bahasa Isyarat", 
        desc: "CV real-time menggunakan MediaPipe dan arsitektur LSTM.", 
        fullDesc: "Sistem cerdas yang mampu menerjemahkan bahasa isyarat secara real-time. Menggunakan MediaPipe untuk deteksi pose dan tangan, serta LSTM untuk pengenalan pola gerakan berurutan.",
        link: "https://bahasaku.tifpsdku.com/",
        image: bahasakuLogo
      },
      { 
        title: "Bahasaku Mobile", 
        desc: "Aplikasi mobile penerjemah bahasa isyarat.", 
        fullDesc: "Versi mobile dari sistem penerjemah bahasa isyarat Bahasaku, memungkinkan pengguna untuk menterjemahkan gerakan tangan secara real-time langsung melalui smartphone.",
        link: "https://github.com/SyaefulEffendi/Bahasaku-Mobile-V1",
        image: bahasakuLogo
      },
      { 
        title: "Cuanin", 
        desc: "Layanan pembuatan website terjangkau untuk UMKM.", 
        fullDesc: "Sistem ini menjual jasa pembuatan website dengan harga terjangkau guna membantu UMKM memiliki website sendiri, serta terintegrasi dengan metode pembayaran QR Code (Midtrans).",
        link: "https://github.com/SyaefulEffendi/cuanin",
        image: null
      }
    ];
  } else {
    return [
      { 
        title: "Bahasaku: Sign Language Translator System", 
        desc: "Real-time CV using MediaPipe and LSTM architecture.", 
        fullDesc: "An intelligent system capable of translating sign language in real-time. Uses MediaPipe for pose and hand detection, and LSTM for sequential movement pattern recognition.",
        link: "https://bahasaku.tifpsdku.com/",
        image: bahasakuLogo
      },
      { 
        title: "Bahasaku Mobile", 
        desc: "Mobile sign language translator application.", 
        fullDesc: "The mobile version of the Bahasaku sign language translation system, allowing users to translate hand gestures in real-time directly through their smartphones.",
        link: "https://github.com/SyaefulEffendi/Bahasaku-Mobile-V1",
        image: bahasakuLogo
      },
      { 
        title: "Cuanin", 
        desc: "Affordable website creation service for MSMEs.", 
        fullDesc: "This system provides affordable website creation services to help MSMEs own their websites, integrated with QR Code payment methods via Midtrans.",
        link: "https://github.com/SyaefulEffendi/cuanin",
        image: cuaninLogo
      }
    ];
  }
});

const contactName = ref('');
const contactEmail = ref('');
const contactMessage = ref('');

const sendMessage = () => {
  if (!contactName.value || !contactEmail.value || !contactMessage.value) return;
  const subject = encodeURIComponent(`Pesan dari ${contactName.value} via Portfolio`);
  const body = encodeURIComponent(`Dari: ${contactName.value}\nEmail: ${contactEmail.value}\n\nPesan:\n${contactMessage.value}`);
  
  const gmailUrl = `https://mail.google.com/mail/?view=cm&fs=1&to=mohsyaefuleffendi@gmail.com&su=${subject}&body=${body}`;
  window.open(gmailUrl, '_blank');
};

const modalOpen = ref(false);
const selectedProject = ref(null);
const certModalOpen = ref(false);
const selectedCert = ref(null);

const openModal = (project) => {
  selectedProject.value = project;
  modalOpen.value = true;
  document.body.style.overflow = 'hidden';
};

const openCertModal = (cert) => {
  selectedCert.value = cert;
  certModalOpen.value = true;
  document.body.style.overflow = 'hidden';
};

const closeModal = () => {
  modalOpen.value = false;
  selectedProject.value = null;
  if (!certModalOpen.value) document.body.style.overflow = '';
};

const closeCertModal = () => {
  certModalOpen.value = false;
  selectedCert.value = null;
  if (!modalOpen.value) document.body.style.overflow = '';
};

// Lifecycle Hooks
const activeSection = ref('home');
let observer = null;

onMounted(() => {
  // Intersection Observer for active section
  const observerOptions = {
    root: null,
    rootMargin: '-50% 0px -50% 0px',
    threshold: 0
  };

  observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        if (entry.target.id === 'skills' || entry.target.id === 'certificates') {
          activeSection.value = 'about';
        } else {
          activeSection.value = entry.target.id;
        }
      }
    });
  }, observerOptions);

  const sections = document.querySelectorAll('section[id]');
  sections.forEach(section => {
    observer.observe(section);
  });

  let count = 0;
  const interval = setInterval(() => {
    count++;
    if (count < greetings.length) {
      currentGreeting.value = greetings[count];
    } else {
      clearInterval(interval);
      slideUp.value = true;
      showMainContent.value = true;
      setTimeout(() => {
        preloaderActive.value = false;
      }, 700); 
    }
  }, 500);
});

onUnmounted(() => {
  if (observer) observer.disconnect();
});
</script>

<template>
  <div class="font-sans antialiased bg-white selection:bg-blue-200 overflow-x-hidden">
    <!-- Preloader -->
    <div v-if="preloaderActive" 
         :class="['fixed inset-0 z-[100] flex items-center justify-center bg-white transition-transform duration-700 ease-in-out', slideUp ? '-translate-y-full' : '']">
      <h1 class="text-3xl sm:text-5xl font-bold text-gray-900 tracking-tight">{{ currentGreeting }}</h1>
    </div>

    <!-- Main Content -->
    <div v-show="showMainContent" class="text-gray-800">
      
      <!-- Navbar -->
      <nav class="fixed top-4 sm:top-6 left-1/2 -translate-x-1/2 z-40 bg-white/90 backdrop-blur-md shadow-sm rounded-full px-2.5 py-2 border border-gray-100 w-[95%] sm:w-auto max-w-5xl flex justify-between items-center transition-all duration-300">
        <!-- Logo -->
        <div class="font-black text-xl text-gray-900 px-4 tracking-tighter">SE<span class="text-blue-600">.</span></div>
        
        <!-- Desktop Menu -->
        <ul class="hidden lg:flex items-center gap-1 mx-4 text-sm font-semibold tracking-wide">
          <li>
            <a href="#home" :class="['flex items-center gap-2 px-4 py-2 rounded-full transition-all', activeSection === 'home' ? 'bg-blue-500 text-white shadow-md' : 'text-gray-600 hover:bg-gray-100']">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"></path></svg>
              {{ t.nav.home }}
            </a>
          </li>
          <li>
            <a href="#about" :class="['flex items-center gap-2 px-4 py-2 rounded-full transition-all', activeSection === 'about' ? 'bg-blue-500 text-white shadow-md' : 'text-gray-600 hover:bg-gray-100']">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
              {{ t.nav.about }}
            </a>
          </li>
          <li>
            <a href="#experience" :class="['flex items-center gap-2 px-4 py-2 rounded-full transition-all', activeSection === 'experience' ? 'bg-blue-500 text-white shadow-md' : 'text-gray-600 hover:bg-gray-100']">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 13.255A23.931 23.931 0 0112 15c-3.183 0-6.22-.62-9-1.745M16 6V4a2 2 0 00-2-2h-4a2 2 0 00-2 2v2m4 6h.01M5 20h14a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
              {{ t.nav.experience }}
            </a>
          </li>
          <li>
            <a href="#projects" :class="['flex items-center gap-2 px-4 py-2 rounded-full transition-all', activeSection === 'projects' ? 'bg-blue-500 text-white shadow-md' : 'text-gray-600 hover:bg-gray-100']">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-6l-2-2H5a2 2 0 00-2 2z"></path></svg>
              {{ t.nav.projects }}
            </a>
          </li>
          <li>
            <a href="#contact" :class="['flex items-center gap-2 px-4 py-2 rounded-full transition-all', activeSection === 'contact' ? 'bg-blue-500 text-white shadow-md' : 'text-gray-600 hover:bg-gray-100']">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
              {{ t.nav.contact }}
            </a>
          </li>
        </ul>

        <div class="flex items-center gap-2 pr-1">
          <!-- Lang Toggle -->
          <button @click="lang = lang === 'EN' ? 'ID' : 'EN'" class="flex items-center gap-1.5 px-3 py-1.5 rounded-full bg-gray-100/80 hover:bg-gray-200 text-gray-700 text-xs sm:text-sm font-bold transition-colors">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3.055 11H5a2 2 0 012 2v1a2 2 0 002 2 2 2 0 012 2v2.945M8 3.935V5.5A2.5 2.5 0 0010.5 8h.5a2 2 0 012 2 2 2 0 104 0 2 2 0 012-2h1.064M15 20.488V18a2 2 0 012-2h3.064M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
            {{ lang }}
          </button>
          
          <!-- Mobile Menu Toggle Button -->
          <button @click="toggleMobileMenu" class="lg:hidden p-2 text-gray-700 bg-gray-100 rounded-full hover:bg-gray-200 focus:outline-none transition-colors">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path v-if="!isMobileMenuOpen" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
              <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
          </button>
        </div>
      </nav>

      <!-- Mobile Dropdown Menu -->
      <transition name="slide-fade">
        <div v-if="isMobileMenuOpen" class="fixed top-20 left-1/2 -translate-x-1/2 z-30 w-[90%] bg-white/95 backdrop-blur-xl shadow-xl rounded-3xl border border-gray-100 p-4 flex flex-col gap-2 lg:hidden">
          <a @click="toggleMobileMenu" href="#home" :class="['flex items-center gap-3 px-4 py-3 rounded-2xl font-semibold transition-colors', activeSection === 'home' ? 'bg-blue-500 text-white' : 'text-gray-700 hover:bg-gray-50']">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"></path></svg>
            {{ t.nav.home }}
          </a>
          <a @click="toggleMobileMenu" href="#about" :class="['flex items-center gap-3 px-4 py-3 rounded-2xl font-semibold transition-colors', activeSection === 'about' ? 'bg-blue-500 text-white' : 'text-gray-700 hover:bg-gray-50']">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
            {{ t.nav.about }}
          </a>
          <a @click="toggleMobileMenu" href="#experience" :class="['flex items-center gap-3 px-4 py-3 rounded-2xl font-semibold transition-colors', activeSection === 'experience' ? 'bg-blue-500 text-white' : 'text-gray-700 hover:bg-gray-50']">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 13.255A23.931 23.931 0 0112 15c-3.183 0-6.22-.62-9-1.745M16 6V4a2 2 0 00-2-2h-4a2 2 0 00-2 2v2m4 6h.01M5 20h14a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
            {{ t.nav.experience }}
          </a>
          <a @click="toggleMobileMenu" href="#projects" :class="['flex items-center gap-3 px-4 py-3 rounded-2xl font-semibold transition-colors', activeSection === 'projects' ? 'bg-blue-500 text-white' : 'text-gray-700 hover:bg-gray-50']">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-6l-2-2H5a2 2 0 00-2 2z"></path></svg>
            {{ t.nav.projects }}
          </a>
          <a @click="toggleMobileMenu" href="#contact" :class="['flex items-center gap-3 px-4 py-3 rounded-2xl font-semibold transition-colors', activeSection === 'contact' ? 'bg-blue-500 text-white' : 'text-gray-700 hover:bg-gray-50']">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
            {{ t.nav.contact }}
          </a>
        </div>
      </transition>

      <!-- Hero Section -->
      <section id="home" class="min-h-screen flex items-center pt-32 pb-16 px-6 sm:px-8 max-w-7xl mx-auto overflow-hidden">
        <div class="grid lg:grid-cols-2 gap-12 lg:gap-16 items-center w-full">
          <div class="space-y-6 sm:space-y-8 text-center lg:text-left order-2 lg:order-1 flex flex-col items-center lg:items-start">
            <h1 class="text-4xl sm:text-5xl md:text-7xl font-extrabold leading-tight text-gray-900 w-full">
              {{ t.hero.title }} <br/>
              <span class="text-transparent bg-clip-text bg-gradient-to-r from-blue-600 to-indigo-600">Moh. Syaeful Effendi.</span>
            </h1>
            <div class="inline-block px-4 py-1.5 bg-blue-50 border border-blue-100 text-blue-700 rounded-full text-xs sm:text-sm font-bold tracking-wide uppercase shadow-sm">
              {{ t.hero.role }}
            </div>
            <p class="text-lg sm:text-xl text-gray-500 font-light max-w-lg leading-relaxed">
              {{ t.hero.subtitle }}
            </p>
            <div class="flex flex-col sm:flex-row flex-wrap gap-4 pt-2 w-full justify-center lg:justify-start">
              <a href="#projects" class="px-8 py-3.5 sm:py-4 bg-blue-600 text-white rounded-full font-semibold hover:bg-blue-700 hover:shadow-lg hover:shadow-blue-200 transition-all active:scale-95 text-center">{{ t.hero.viewProjects }}</a>
              <a href="#contact" class="px-8 py-3.5 sm:py-4 border-2 border-gray-200 text-gray-800 rounded-full font-semibold hover:border-gray-300 hover:bg-gray-50 transition-all active:scale-95 text-center">{{ t.hero.contactMe }}</a>
            </div>
            <div class="flex gap-5 pt-6 justify-center lg:justify-start">
              <!-- GitHub -->
              <a href="https://github.com/SyaefulEffendi" class="w-10 h-10 sm:w-12 sm:h-12 flex items-center justify-center rounded-full bg-gray-50 border border-gray-200 hover:bg-gray-100 transition-colors text-gray-700">
                <svg class="w-5 h-5 sm:w-6 sm:h-6" fill="currentColor" viewBox="0 0 24 24"><path fill-rule="evenodd" clip-rule="evenodd" d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.166 6.839 9.489.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.34-3.369-1.34-.454-1.156-1.11-1.463-1.11-1.463-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.831.092-.646.35-1.086.636-1.336-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0112 6.836c.85.004 1.705.114 2.504.336 1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.203 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.743 0 .267.18.578.688.48C19.138 20.161 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
              </a>
              <!-- Instagram -->
              <a href="https://www.instagram.com/syaefuleffendi/" class="w-10 h-10 sm:w-12 sm:h-12 flex items-center justify-center rounded-full bg-gray-50 border border-gray-200 hover:bg-gray-100 transition-colors text-gray-700">
                <svg class="w-5 h-5 sm:w-6 sm:h-6" fill="currentColor" viewBox="0 0 24 24"><path fill-rule="evenodd" clip-rule="evenodd" d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
              </a>
              <!-- LinkedIn -->
              <a href="https://www.linkedin.com/in/moh-syaeful-effendi-b664a3315/" class="w-10 h-10 sm:w-12 sm:h-12 flex items-center justify-center rounded-full bg-gray-50 border border-gray-200 hover:bg-gray-100 transition-colors text-gray-700">
                <svg class="w-5 h-5 sm:w-6 sm:h-6" fill="currentColor" viewBox="0 0 24 24"><path fill-rule="evenodd" clip-rule="evenodd" d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/></svg>
              </a>
            </div>
          </div>
          
          <div class="relative flex justify-center perspective-1000 items-center h-full w-full order-1 lg:order-2"
               ref="tiltCard" 
               @mousemove="handleMouseMove" 
               @mouseleave="handleMouseLeave">
            <div class="w-full max-w-[16rem] sm:max-w-xs lg:w-80 aspect-[3/4] sm:h-[28rem] bg-gray-100 rounded-3xl shadow-xl lg:shadow-2xl overflow-hidden border-4 sm:border-8 border-white/50 transform-style-3d relative transition-transform duration-300" :style="tiltStyle">
              <img src="./assets/Foto1.jpg" alt="Syaeful Effendi" class="w-full h-full object-cover" />
            </div>
          </div>
        </div>
      </section>

      <!-- About Section -->
      <section id="about" class="py-20 md:py-32 px-6 sm:px-8 bg-gray-50 overflow-hidden">
        <div class="max-w-6xl mx-auto grid lg:grid-cols-2 gap-12 lg:gap-16 items-center">
          <div class="order-2 lg:order-1 flex justify-center w-full">
            <div class="w-full max-w-[16rem] sm:max-w-sm md:max-w-md aspect-square bg-blue-100 rounded-[2rem] sm:rounded-[3rem] p-3 sm:p-4 rotate-3 hover:rotate-0 transition-transform duration-500">
               <div class="w-full h-full bg-white rounded-[1.5rem] sm:rounded-[2.5rem] flex items-center justify-center text-blue-300 overflow-hidden shadow-sm">
                 <img src="./assets/Foto2.jpg" alt="About Image" class="w-full h-full object-cover">
               </div>
            </div>
          </div>
          <div class="order-1 lg:order-2 space-y-6 sm:space-y-8 text-center lg:text-left">
            <h2 class="text-3xl sm:text-4xl md:text-5xl font-extrabold text-gray-900 leading-tight tracking-tight uppercase" v-html="t.about.title">
            </h2>
            <div class="border-l-4 border-purple-500 pl-4 sm:pl-6 text-base sm:text-lg text-gray-600 font-medium leading-relaxed" v-html="t.about.desc">
            </div>
            <a :href="cvUrl" download="CV Moh. Syaeful Effendi.pdf" class="flex items-center justify-center gap-3 px-6 py-3 sm:px-8 sm:py-4 bg-[#1A202C] text-white rounded-full font-bold hover:bg-gray-800 hover:shadow-lg transition-all active:scale-95 w-full sm:w-auto uppercase tracking-wide text-sm sm:text-base">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
              {{ t.about.downloadCV }}
            </a>
          </div>
        </div>
      </section>

      <!-- Tech Skills -->
      <section id="skills" class="py-16 sm:py-24 overflow-hidden bg-white flex flex-col gap-6 sm:gap-10">
        <div class="text-center mb-6 sm:mb-10">
          <h2 class="text-3xl sm:text-4xl md:text-5xl font-extrabold tracking-tight">
            <span class="text-gray-900">Tech </span><span class="text-blue-600">Skills.</span>
          </h2>
        </div>

        <div class="marquee-container py-2 sm:py-4">
          <div class="marquee marquee-left flex gap-5 sm:gap-8 items-center px-4">
            <div v-for="(skill, index) in marquee1" :key="'l'+index" class="flex items-center gap-3 sm:gap-4 bg-white px-5 py-3 sm:px-6 sm:py-4 rounded-full shadow-sm border border-gray-100 hover:shadow-md transition-shadow shrink-0 cursor-default">
              <img :src="`https://skillicons.dev/icons?i=${skill.icon}`" :alt="skill.name" class="w-6 h-6 sm:w-8 sm:h-8" />
              <span class="font-bold text-gray-800 text-sm sm:text-base">{{ skill.name }}</span>
            </div>
          </div>
        </div>

        <div class="marquee-container py-2 sm:py-4">
          <div class="marquee marquee-right flex gap-5 sm:gap-8 items-center px-4">
            <div v-for="(skill, index) in marquee2" :key="'r'+index" class="flex items-center gap-3 sm:gap-4 bg-white px-5 py-3 sm:px-6 sm:py-4 rounded-full shadow-sm border border-gray-100 hover:shadow-md transition-shadow shrink-0 cursor-default">
              <img :src="`https://skillicons.dev/icons?i=${skill.icon}`" :alt="skill.name" class="w-6 h-6 sm:w-8 sm:h-8" />
              <span class="font-bold text-gray-800 text-sm sm:text-base">{{ skill.name }}</span>
            </div>
          </div>
        </div>
      </section>

      <!-- Certificates Section -->
      <section id="certificates" class="py-20 md:py-32 px-6 sm:px-8 max-w-7xl mx-auto bg-gray-50 rounded-[2rem] sm:rounded-[3rem] mb-12">
        <div class="text-center mb-10 sm:mb-16">
          <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900 mb-4">{{ t.contact.certificates }}</h2>
          <p class="text-gray-500 font-light max-w-xl mx-auto text-sm sm:text-base">{{ t.contact.certSub }}</p>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 sm:gap-8">
          <div v-for="(cert, index) in certificatesData" :key="index" @click="openCertModal(cert)" class="group bg-white p-3 sm:p-4 rounded-2xl shadow-sm border border-gray-100 hover:shadow-xl hover:border-blue-200 transition-all cursor-pointer flex flex-col items-center">
            <div class="w-full aspect-[4/3] bg-gray-50 rounded-xl overflow-hidden relative flex items-center justify-center mb-4 group-hover:scale-[1.02] transition-transform duration-300 shadow-sm">
              <img :src="cert.image" :alt="cert.title" class="w-full h-full object-cover" />
            </div>
            <h3 class="font-bold text-gray-800 text-center text-sm sm:text-base line-clamp-2 leading-snug px-2">{{ cert.title }}</h3>
          </div>
        </div>
      </section>

      <!-- Work Experience -->
      <section id="experience" class="py-20 md:py-32 px-6 sm:px-8 bg-white">
        <div class="max-w-4xl mx-auto">
          <div class="text-center mb-10 sm:mb-16">
            <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900">{{ t.experience.title }}</h2>
          </div>
          <div class="relative border-l-2 border-blue-100 ml-3 sm:ml-4 md:ml-0 md:pl-0 md:mx-auto md:w-3/4 space-y-10 sm:space-y-12">
            <!-- Timeline Item -->
            <div class="ml-6 sm:ml-8 md:ml-12 relative group">
              <div class="absolute -left-9 sm:-left-[2.35rem] md:-left-[3.25rem] mt-1.5 w-4 h-4 sm:w-5 sm:h-5 rounded-full bg-blue-500 ring-4 ring-blue-50 group-hover:ring-blue-100 transition-all"></div>
              <div class="bg-gray-50 p-5 sm:p-6 rounded-2xl border border-gray-100 group-hover:shadow-md transition-shadow">
                <h3 class="text-xl sm:text-2xl font-bold text-gray-900 mb-1">{{ t.experience.role1 }}</h3>
                <span class="text-xs sm:text-sm text-blue-600 font-bold tracking-wide uppercase mb-3 sm:mb-4 inline-block bg-blue-50 px-3 py-1 rounded-full">{{ t.experience.location1 }}</span>
                <p class="text-sm sm:text-base text-gray-600 font-light leading-relaxed">{{ t.experience.desc1 }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Projects -->
      <section id="projects" class="py-20 md:py-32 px-6 sm:px-8 max-w-7xl mx-auto">
        <div class="text-center mb-10 sm:mb-16">
          <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900 mb-4">{{ t.projects.title }}</h2>
          <p class="text-gray-500 font-light max-w-xl mx-auto text-sm sm:text-base">{{ t.projects.desc }}</p>
        </div>
        <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6 sm:gap-8">
          <div v-for="(project, index) in projects" :key="index" 
               @click="openModal(project)"
               class="bg-white rounded-2xl sm:rounded-3xl p-5 sm:p-6 shadow-sm border border-gray-100 hover:shadow-xl hover:-translate-y-2 hover:border-blue-100 transition-all duration-300 cursor-pointer group flex flex-col h-full">
            <div class="aspect-video bg-gray-50 rounded-xl sm:rounded-2xl mb-4 sm:mb-6 overflow-hidden relative border border-gray-100 flex items-center justify-center">
              <img v-if="project.image" :src="project.image" :alt="project.title" class="w-full h-full object-cover" />
              <div v-else class="text-gray-300 font-bold tracking-widest text-lg">PROJECT</div>
              <div class="absolute inset-0 bg-blue-600/10 opacity-0 group-hover:opacity-100 transition-opacity flex items-center justify-center backdrop-blur-[2px] z-10">
                <span class="bg-white px-4 py-2 sm:px-5 sm:py-2.5 rounded-full text-xs sm:text-sm font-bold shadow-sm transform translate-y-4 group-hover:translate-y-0 transition-transform duration-300 text-gray-800 flex items-center gap-2">
                  {{ t.projects.viewDetails }}
                  <svg class="w-3 h-3 sm:w-4 sm:h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path></svg>
                </span>
              </div>
            </div>
            <h3 class="text-lg sm:text-xl font-bold mb-2 sm:mb-3 group-hover:text-blue-600 transition-colors text-gray-900">{{ project.title }}</h3>
            <p class="text-gray-500 text-xs sm:text-sm font-light leading-relaxed mb-4 flex-grow">{{ project.desc }}</p>
          </div>
        </div>
      </section>

      <!-- Contact -->
      <section id="contact" class="py-20 md:py-24 px-6 sm:px-8 bg-gray-900 text-white rounded-t-[2rem] sm:rounded-t-[3rem] mt-12 overflow-hidden">
        <div class="max-w-6xl mx-auto grid lg:grid-cols-2 gap-12 sm:gap-16 items-center">
          <div class="space-y-8 sm:space-y-10 text-center lg:text-left">
            <div>
              <h2 class="text-4xl sm:text-5xl font-extrabold mb-4 sm:mb-6 leading-tight">{{ t.contact.title }}<br class="hidden sm:block"/><span class="text-blue-400">{{ t.contact.titleBlue }}</span></h2>
              <p class="text-gray-400 font-light text-base sm:text-lg">{{ t.contact.subtitle }}</p>
            </div>
            <div class="space-y-4 sm:space-y-6 flex flex-col items-center lg:items-start">
              <div class="flex items-center gap-4 sm:gap-5 text-gray-300 w-full max-w-sm justify-center lg:justify-start">
                <div class="w-12 h-12 sm:w-14 sm:h-14 shrink-0 rounded-2xl bg-gray-800 flex items-center justify-center shadow-inner border border-gray-700">
                  <svg class="w-5 h-5 sm:w-6 sm:h-6 text-blue-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
                </div>
                <div class="text-left w-full">
                  <div class="text-[10px] sm:text-xs text-gray-500 font-bold uppercase tracking-wider mb-0.5 sm:mb-1">Email</div>
                  <div class="text-sm sm:text-lg font-medium break-all">mohsyaefuleffendi@gmail.com</div>
                </div>
              </div>
              <div class="flex items-center gap-4 sm:gap-5 text-gray-300 w-full max-w-sm justify-center lg:justify-start">
                <div class="w-12 h-12 sm:w-14 sm:h-14 shrink-0 rounded-2xl bg-gray-800 flex items-center justify-center shadow-inner border border-gray-700">
                  <svg class="w-5 h-5 sm:w-6 sm:h-6 text-blue-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
                </div>
                <div class="text-left w-full">
                  <div class="text-[10px] sm:text-xs text-gray-500 font-bold uppercase tracking-wider mb-0.5 sm:mb-1">Location</div>
                  <div class="text-sm sm:text-lg font-medium">Cirebon, Indonesia</div>
                </div>
              </div>
            </div>
          </div>
          <div class="bg-gray-800 p-6 sm:p-10 rounded-[2rem] sm:rounded-[2.5rem] border border-gray-700 shadow-2xl w-full max-w-lg mx-auto">
            <form class="space-y-4 sm:space-y-5" @submit.prevent="sendMessage">
              <div>
                <label class="block text-xs sm:text-sm text-gray-400 mb-1.5 sm:mb-2 font-medium">{{ t.contact.name }}</label>
                <input v-model="contactName" required type="text" class="w-full bg-gray-900 border border-gray-700 rounded-xl px-4 py-3 sm:px-5 sm:py-4 text-sm sm:text-base text-white focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 transition-all" placeholder="John Doe">
              </div>
              <div>
                <label class="block text-xs sm:text-sm text-gray-400 mb-1.5 sm:mb-2 font-medium">Email</label>
                <input v-model="contactEmail" required type="email" class="w-full bg-gray-900 border border-gray-700 rounded-xl px-4 py-3 sm:px-5 sm:py-4 text-sm sm:text-base text-white focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 transition-all" placeholder="john@example.com">
              </div>
              <div>
                <label class="block text-xs sm:text-sm text-gray-400 mb-1.5 sm:mb-2 font-medium">{{ t.contact.message }}</label>
                <textarea v-model="contactMessage" required rows="4" class="w-full bg-gray-900 border border-gray-700 rounded-xl px-4 py-3 sm:px-5 sm:py-4 text-sm sm:text-base text-white focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 transition-all resize-none" placeholder="Hello..."></textarea>
              </div>
              <button type="submit" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-bold py-3.5 sm:py-4 rounded-xl transition-all shadow-lg shadow-blue-900/50 active:scale-95 text-sm sm:text-base">
                {{ t.contact.send }}
              </button>
            </form>
          </div>
        </div>
      </section>

      <!-- Footer -->
      <footer class="bg-gray-900 text-gray-500 text-center py-6 sm:py-8 text-xs sm:text-sm border-t border-gray-800">
        <p>&copy; {{ new Date().getFullYear() }} Moh. Syaeful Effendi. All rights reserved.</p>
      </footer>

    </div>

      <!-- Project Modal -->
      <Transition name="fade">
        <div v-if="modalOpen" class="fixed inset-0 z-50 flex items-center justify-center px-4 sm:px-6">
          <div class="absolute inset-0 bg-gray-900/60 backdrop-blur-sm" @click="closeModal"></div>
          
          <div class="relative w-full max-w-3xl bg-white rounded-2xl sm:rounded-3xl shadow-2xl overflow-hidden z-10 max-h-[90vh] flex flex-col transform transition-all">
            <div class="relative w-full aspect-video bg-gray-100 flex items-center justify-center overflow-hidden">
              <img v-if="selectedProject.image" :src="selectedProject.image" :alt="selectedProject.title" class="w-full h-full object-cover" />
              <div v-else class="absolute inset-0 flex items-center justify-center text-gray-400">
                <svg class="w-16 h-16 opacity-20" fill="currentColor" viewBox="0 0 24 24"><path d="M4 4h16v16H4V4zm2 4v10h12V8H6z"></path></svg>
              </div>
              <button @click="closeModal" class="absolute top-4 right-4 p-2.5 bg-black/50 hover:bg-black text-white rounded-full transition-colors backdrop-blur-md">
                <svg class="w-5 h-5 sm:w-6 sm:h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
              </button>
            </div>
            
            <div class="p-6 sm:p-8 overflow-y-auto">
              <h3 class="text-xl sm:text-3xl font-extrabold text-gray-900 mb-4 leading-tight">{{ selectedProject.title }}</h3>
              <p class="text-base sm:text-lg text-gray-600 font-light leading-relaxed">{{ selectedProject.fullDesc }}</p>
              
              <div class="mt-8 flex flex-col sm:flex-row gap-4">
                <a :href="selectedProject.link" target="_blank" rel="noopener noreferrer" class="flex-1 flex items-center justify-center gap-2 bg-gray-900 text-white px-6 py-3.5 sm:py-4 rounded-xl font-bold hover:bg-gray-800 transition-colors shadow-lg shadow-gray-900/20">
                  {{ t.projects.visitProject }}
                  <svg class="w-4 h-4 sm:w-5 sm:h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path></svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </Transition>

      <!-- Certificate Modal -->
      <Transition name="fade">
        <div v-if="certModalOpen" class="fixed inset-0 z-50 flex items-center justify-center px-4 sm:px-6">
          <div class="absolute inset-0 bg-gray-900/60 backdrop-blur-sm" @click="closeCertModal"></div>
          
          <div class="relative w-full max-w-3xl bg-white rounded-2xl sm:rounded-3xl shadow-2xl overflow-hidden z-10 max-h-[90vh] flex flex-col transform transition-all">
            <div class="relative w-full aspect-[4/3] sm:aspect-video bg-gray-100 border-b border-gray-100 flex items-center justify-center">
              <img :src="selectedCert.image" :alt="selectedCert.title" class="w-full h-full object-contain p-2" />
              <button @click="closeCertModal" class="absolute top-4 right-4 p-2.5 bg-black/50 hover:bg-black text-white rounded-full transition-colors backdrop-blur-md">
                <svg class="w-5 h-5 sm:w-6 sm:h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
              </button>
            </div>
            
            <div class="p-6 sm:p-8 overflow-y-auto">
              <h3 class="text-xl sm:text-3xl font-extrabold text-gray-900 mb-2 leading-tight">{{ selectedCert.title }}</h3>
              <p class="text-blue-600 font-semibold mb-6 flex items-center gap-2">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"></path></svg>
                {{ selectedCert.issuer }}
              </p>
              
              <div class="mt-8 flex flex-col sm:flex-row gap-4">
                <a :href="selectedCert.link" target="_blank" rel="noopener noreferrer" class="flex-1 flex items-center justify-center gap-2 bg-blue-600 text-white px-6 py-3.5 sm:py-4 rounded-xl font-bold hover:bg-blue-700 transition-colors shadow-lg shadow-blue-600/30">
                  <span>{{ lang === 'ID' ? 'Verifikasi Kredensial' : 'Verify Credential' }}</span>
                  <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path></svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </Transition>

  </div>
</template>

<style>
html {
  scroll-behavior: smooth;
}
</style>

<style scoped>
/* Glassmorphism & 3D utility */
.perspective-1000 {
  perspective: 1000px;
}
.transform-style-3d {
  transform-style: preserve-3d;
}

/* Transitions */
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}
.slide-fade-leave-active {
  transition: all 0.2s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-10px) translateX(-50%);
  opacity: 0;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-active > div:nth-child(2),
.fade-leave-active > div:nth-child(2) {
  transition: transform 0.3s ease, opacity 0.3s ease;
}
.fade-enter-from > div:nth-child(2),
.fade-leave-to > div:nth-child(2) {
  transform: scale(0.95) translateY(10px);
  opacity: 0;
}

/* Marquee Animation */
.marquee-container {
  display: flex;
  overflow: hidden;
  user-select: none;
  width: 100vw;
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
}

.marquee {
  white-space: nowrap;
  will-change: transform;
}

.marquee-left {
  animation: scroll-left 40s linear infinite;
}

.marquee-right {
  animation: scroll-right 40s linear infinite;
}

@keyframes scroll-left {
  0% { transform: translateX(0%); }
  100% { transform: translateX(-50%); }
}

@keyframes scroll-right {
  0% { transform: translateX(-50%); }
  100% { transform: translateX(0%); }
}
</style>
