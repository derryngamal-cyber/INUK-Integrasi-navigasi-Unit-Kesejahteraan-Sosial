<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="theme-color" content="#07111f" />
  <title>INUK — Integrasi Navigasi Unit Kesejahteraan Sosial</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>

  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <style>
    :root {
      --bg: #050b14;
      --panel: rgba(12, 25, 42, .72);
      --border: rgba(76, 201, 240, .18);
      --cyan: #22d3ee;
      --violet: #8b5cf6;
      --green: #34d399;
      --yellow: #fbbf24;
      --red: #fb7185;
    }

    * {
      box-sizing: border-box;
      scrollbar-width: thin;
      scrollbar-color: #164e63 #050b14;
    }

    body {
      margin: 0;
      background:
        radial-gradient(circle at 10% 10%, rgba(34,211,238,.08), transparent 25%),
        radial-gradient(circle at 90% 10%, rgba(139,92,246,.09), transparent 28%),
        radial-gradient(circle at 50% 100%, rgba(14,165,233,.06), transparent 30%),
        var(--bg);
      color: #e5f4ff;
      min-height: 100vh;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    .glass {
      background: linear-gradient(145deg, rgba(15,32,52,.82), rgba(7,18,31,.72));
      border: 1px solid var(--border);
      box-shadow:
        0 15px 50px rgba(0,0,0,.25),
        inset 0 1px 0 rgba(255,255,255,.025);
      backdrop-filter: blur(18px);
      -webkit-backdrop-filter: blur(18px);
    }

    .neon {
      box-shadow:
        0 0 0 1px rgba(34,211,238,.12),
        0 0 35px rgba(34,211,238,.06);
    }

    .nav-item {
      transition: all .25s ease;
    }

    .nav-item:hover,
    .nav-item.active {
      background: rgba(34,211,238,.10);
      border-color: rgba(34,211,238,.25);
      color: #67e8f9;
      transform: translateX(3px);
    }

    .card-hover {
      transition: transform .25s ease, border-color .25s ease, box-shadow .25s ease;
    }

    .card-hover:hover {
      transform: translateY(-3px);
      border-color: rgba(34,211,238,.3);
      box-shadow: 0 15px 45px rgba(0,0,0,.25);
    }

    .gradient-text {
      background: linear-gradient(90deg, #67e8f9, #a78bfa);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    .input {
      width: 100%;
      background: rgba(3,10,18,.72);
      border: 1px solid rgba(148,163,184,.16);
      color: #e5f4ff;
      border-radius: 14px;
      padding: 11px 14px;
      outline: none;
      transition: .2s;
    }

    .input:focus {
      border-color: rgba(34,211,238,.55);
      box-shadow: 0 0 0 3px rgba(34,211,238,.07);
    }

    select.input option {
      background: #0b1727;
      color: white;
    }

    .btn {
      border-radius: 13px;
      padding: 10px 15px;
      font-weight: 700;
      transition: .2s;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }

    .btn-primary {
      color: #02131b;
      background: linear-gradient(135deg,#67e8f9,#22d3ee);
    }

    .btn-primary:hover {
      transform: translateY(-1px);
      box-shadow: 0 8px 25px rgba(34,211,238,.18);
    }

    .btn-secondary {
      background: rgba(30,41,59,.65);
      border: 1px solid rgba(148,163,184,.15);
      color: #cbd5e1;
    }

    .btn-secondary:hover {
      border-color: rgba(34,211,238,.3);
    }

    .badge {
      display: inline-flex;
      align-items: center;
      gap: 5px;
      border-radius: 999px;
      padding: 5px 9px;
      font-size: 11px;
      font-weight: 800;
    }

    .badge-green {
      color: #6ee7b7;
      background: rgba(16,185,129,.10);
      border: 1px solid rgba(52,211,153,.18);
    }

    .badge-yellow {
      color: #fde68a;
      background: rgba(245,158,11,.10);
      border: 1px solid rgba(251,191,36,.18);
    }

    .badge-red {
      color: #fda4af;
      background: rgba(244,63,94,.10);
      border: 1px solid rgba(251,113,133,.18);
    }

    .badge-blue {
      color: #67e8f9;
      background: rgba(34,211,238,.10);
      border: 1px solid rgba(34,211,238,.18);
    }

    .sidebar {
      transition: transform .3s ease;
    }

    @media(max-width: 767px) {
      .sidebar {
        position: fixed;
        z-index: 50;
        transform: translateX(-105%);
      }

      .sidebar.open {
        transform: translateX(0);
      }

      .mobile-overlay {
        display: none;
      }

      .mobile-overlay.show {
        display: block;
      }
    }

    .pulse-dot {
      width: 8px;
      height: 8px;
      border-radius: 999px;
      background: #34d399;
      box-shadow: 0 0 0 5px rgba(52,211,153,.08), 0 0 16px rgba(52,211,153,.4);
    }

    .table-scroll {
      overflow-x: auto;
    }

    .table-scroll::-webkit-scrollbar {
      height: 5px;
    }

    .table-scroll::-webkit-scrollbar-thumb {
      background: #164e63;
      border-radius: 999px;
    }

    .modal {
      display: none;
    }

    .modal.show {
      display: flex;
    }
  </style>
</head>

<body>

  <!-- MOBILE OVERLAY -->
  <div id="mobileOverlay"
       class="mobile-overlay fixed inset-0 bg-black/60 z-40"
       onclick="closeSidebar()"></div>

  <!-- APP SHELL -->
  <div class="min-h-screen flex">

    <!-- SIDEBAR -->
    <aside id="sidebar"
           class="sidebar w-[270px] shrink-0 border-r border-white/5 bg-[#07111f]/95 backdrop-blur-xl min-h-screen md:relative md:translate-x-0">

      <div class="p-5 border-b border-white/5">
        <div class="flex items-center gap-3">
          <div class="w-11 h-11 rounded-2xl bg-gradient-to-br from-cyan-300 to-violet-500 flex items-center justify-center text-slate-950 font-black shadow-lg shadow-cyan-500/10">
            IN
          </div>
          <div>
            <div class="font-black tracking-wide text-lg">INUK</div>
            <div class="text-[10px] text-slate-400">DINAS SOSIAL MABAR</div>
          </div>
        </div>
      </div>

      <!-- ROLE SWITCHER -->
      <div class="p-4">
        <div class="text-[10px] uppercase tracking-widest text-slate-500 mb-2">
          Mode Pengguna
        </div>

        <select id="roleSelect" class="input text-sm" onchange="changeRole(this.value)">
          <option value="admin">Administrator</option>
          <option value="lks">LKS / Lembaga</option>
          <option value="tksk">TKSK / Petugas Lapangan</option>
        </select>
      </div>

      <!-- NAVIGATION -->
      <nav class="px-3 space-y-1" id="mainNav">

        <button class="nav-item active w-full flex items-center gap-3 px-4 py-3 rounded-xl border border-transparent text-left"
                data-page="dashboard" onclick="navigate('dashboard')">
          <i data-lucide="layout-dashboard" class="w-5 h-5"></i>
          <span>Dashboard</span>
        </button>

        <button class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-xl border border-transparent text-left"
                data-page="lks" onclick="navigate('lks')">
          <i data-lucide="building-2" class="w-5 h-5"></i>
          <span>Data LKS</span>
        </button>

        <button class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-xl border border-transparent text-left"
                data-page="lk3" onclick="navigate('lk3')">
          <i data-lucide="users-round" class="w-5 h-5"></i>
          <span>Data LK3</span>
        </button>

        <button class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-xl border border-transparent text-left"
                data-page="psm" onclick="navigate('psm')">
          <i data-lucide="heart-handshake" class="w-5 h-5"></i>
          <span>Data PSM</span>
        </button>

        <button class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-xl border border-transparent text-left"
                data-page="tksk" onclick="navigate('tksk')">
          <i data-lucide="map-pin" class="w-5 h-5"></i>
          <span>Data TKSK</span>
        </button>

        <button class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-xl border border-transparent text-left"
                data-page="verifikasi" onclick="navigate('verifikasi')">
          <i data-lucide="shield-check" class="w-5 h-5"></i>
          <span>Verifikasi</span>
          <span id="pendingBadge"
                class="ml-auto hidden text-[10px] px-2 py-1 rounded-full bg-rose-500/15 text-rose-300">0</span>
        </button>

        <button class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-xl border border-transparent text-left"
                data-page="laporan" onclick="navigate('laporan')">
          <i data-lucide="clipboard-list" class="w-5 h-5"></i>
          <span>Laporan Lapangan</span>
        </button>
      </nav>

      <!-- SIDEBAR FOOTER -->
      <div class="absolute bottom-0 left-0 right-0 p-4">
        <div class="glass rounded-2xl p-4">
          <div class="flex items-center gap-3">
            <div class="pulse-dot"></div>
            <div>
              <div class="text-xs font-bold">Sistem Online</div>
              <div class="text-[10px] text-slate-500">INUK v1.0 • 2026</div>
            </div>
          </div>
        </div>
      </div>
    </aside>

    <!-- MAIN -->
    <main class="flex-1 min-w-0">

      <!-- TOPBAR -->
      <header class="sticky top-0 z-30 glass border-x-0 border-t-0">
        <div class="h-[72px] px-4 sm:px-6 lg:px-8 flex items-center justify-between gap-4">

          <div class="flex items-center gap-3">
            <button onclick="openSidebar()"
                    class="md:hidden btn btn-secondary !p-2.5">
              <i data-lucide="menu"></i>
            </button>

            <div>
              <div class="text-xs text-slate-500">Integrasi Navigasi Unit Kesejahteraan Sosial</div>
              <h1 id="pageTitle" class="text-lg sm:text-xl font-black">Dashboard</h1>
            </div>
          </div>

          <div class="flex items-center gap-2">

            <button onclick="showNotifications()"
                    class="relative btn btn-secondary !p-2.5">
              <i data-lucide="bell" class="w-5 h-5"></i>
              <span id="notificationDot"
                    class="hidden absolute top-1 right-1 w-2 h-2 rounded-full bg-rose-400"></span>
            </button>

            <div class="hidden sm:flex items-center gap-3 pl-2">
              <div class="w-9 h-9 rounded-full bg-gradient-to-br from-cyan-300 to-violet-500 flex items-center justify-center text-slate-950 font-bold">
                <span id="avatarLetter">A</span>
              </div>
              <div>
                <div id="userName" class="text-sm font-bold">Administrator</div>
                <div id="userRole" class="text-[10px] text-slate-500">DINAS SOSIAL</div>
              </div>
            </div>
          </div>
        </div>
      </header>

      <!-- CONTENT -->
      <section id="appContent" class="p-4 sm:p-6 lg:p-8 max-w-[1920px] mx-auto">
      </section>

    </main>
  </div>


  <!-- MODAL -->
  <div id="modal"
       class="modal fixed inset-0 z-[100] items-center justify-center p-4 bg-black/70 backdrop-blur-sm">
    <div id="modalContent"
         class="glass rounded-3xl w-full max-w-2xl max-h-[90vh] overflow-y-auto p-5 sm:p-7">
    </div>
  </div>


<script>
/* ============================================================
   INUK APPLICATION
   Integrasi Navigasi Unit Kesejahteraan Sosial
   Dinas Sosial Kabupaten Manggarai Barat
   ============================================================ */

"use strict";

/* ------------------------------------------------------------
   DATA AWAL
   ------------------------------------------------------------ */

const INITIAL_LKS = [
  {
    id: 1,
    name: "St Damian Binongko",
    address: "Jl. Binongko, RT 002 RW 002",
    district: "Komodo",
    status: "Valid",
    documentStatus: "Disetujui",
    licenseExpiry: "2027-08-20",
    phone: "-",
    category: "LKS"
  },
  {
    id: 2,
    name: "Kongregasi Kkottongnae Indonesia",
    address: "Jalan Mangga Golek, RT/RW 006/002, Gang Matahari, Cowang Ndereng, Desa Batu Cermin",
    district: "Komodo",
    status: "Akan Habis",
    documentStatus: "Dalam Verifikasi",
    licenseExpiry: "2026-09-20",
    phone: "-",
    category: "LKS"
  },
  {
    id: 3,
    name: "MISSIONARIES OF THE POOR SACRED HEART HOME",
    address: "Jln. Matahari Cowang Dereng RT 006 RW 02, Desa Batu Cermin",
    district: "Komodo",
    status: "Valid",
    documentStatus: "Disetujui",
    licenseExpiry: "2027-04-12",
    phone: "-",
    category: "LKS"
  },
  {
    id: 4,
    name: "LKSA ABDI KASIH",
    address: "Jl. Pasar Lembor Malawatar",
    district: "Lembor",
    status: "Valid",
    documentStatus: "Disetujui",
    licenseExpiry: "2027-12-15",
    phone: "-",
    category: "LKS"
  },
  {
    id: 5,
    name: "Perkumpulan Rumah Pekerti Inklusi",
    address: "Golokoe, RT 014 RW 004, Kelurahan Wae Kelambu",
    district: "Komodo",
    status: "Valid",
    documentStatus: "Disetujui",
    licenseExpiry: "2028-01-30",
    phone: "-",
    category: "LKS"
  },
  {
    id: 6,
    name: "RUMAH SINGGAH SANTA THERESIA",
    address: "JL. FRANS SALES LEGA, RT 005/RT 014, Kelurahan Wae Kelambu",
    district: "Komodo",
    status: "Valid",
    documentStatus: "Disetujui",
    licenseExpiry: "2027-06-11",
    phone: "-",
    category: "LKS"
  },
  {
    id: 7,
    name: "Pastoral Sosial Orang Berkebutuhan Khusus (PSOBK)",
    address: "Reweng, RT 004/RW 001, Desa Lendong",
    district: "Lembor Selatan",
    status: "Tidak Aktif",
    documentStatus: "Ditolak",
    licenseExpiry: "2025-11-20",
    phone: "-",
    category: "LKS"
  },
  {
    id: 8,
    name: "Karya Murni",
    address: "Jln. Gorontalo, Desa Gorontalo",
    district: "Komodo",
    status: "Valid",
    documentStatus: "Disetujui",
    licenseExpiry: "2027-10-02",
    phone: "-",
    category: "LKS"
  }
];

/* Data kosong sesuai permintaan user */
const EMPTY_DATA = {
  lk3: [],
  psm: [],
  tksk: []
};

/* ------------------------------------------------------------
   LOCAL STORAGE
   ------------------------------------------------------------ */

const STORAGE_KEY = "inuk_manggarai_barat_v1";

function loadState() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY);

    if (!saved) {
      const initial = {
        lks: INITIAL_LKS,
        lk3: EMPTY_DATA.lk3,
        psm: EMPTY_DATA.psm,
        tksk: EMPTY_DATA.tksk,
        reports: [],
        verificationLogs: [],
        notifications: []
      };

      localStorage.setItem(STORAGE_KEY, JSON.stringify(initial));
      return initial;
    }

    const parsed = JSON.parse(saved);

    return {
      lks: Array.isArray(parsed.lks) ? parsed.lks : INITIAL_LKS,
      lk3: Array.isArray(parsed.lk3) ? parsed.lk3 : [],
      psm: Array.isArray(parsed.psm) ? parsed.psm : [],
      tksk: Array.isArray(parsed.tksk) ? parsed.tksk : [],
      reports: Array.isArray(parsed.reports) ? parsed.reports : [],
      verificationLogs: Array.isArray(parsed.verificationLogs) ? parsed.verificationLogs : [],
      notifications: Array.isArray(parsed.notifications) ? parsed.notifications : []
    };

  } catch (error) {
    console.error("Gagal membaca localStorage:", error);
    showToast("Gagal membaca penyimpanan lokal.", "error");

    return {
      lks: [...INITIAL_LKS],
      lk3: [],
      psm: [],
      tksk: [],
      reports: [],
      verificationLogs: [],
      notifications: []
    };
  }
}

let state = loadState();

function saveState() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
    return true;
  } catch (error) {
    console.error("Gagal menyimpan data:", error);
    showToast("Data gagal disimpan.", "error");
    return false;
  }
}

/* ------------------------------------------------------------
   GLOBAL STATE
   ------------------------------------------------------------ */

let currentRole = "admin";
let currentPage = "dashboard";
let chartInstance = null;

/* ------------------------------------------------------------
   UTILITIES
   ------------------------------------------------------------ */

function escapeHTML(value) {
  return String(value ?? "")
    .replaceAll("&", "&amp;")
    .replaceAll("<", "&lt;")
    .replaceAll(">", "&gt;")
    .replaceAll('"', "&quot;")
    .replaceAll("'", "&#039;");
}

function formatDate(date) {
  try {
    return new Intl.DateTimeFormat("id-ID", {
      day: "2-digit",
      month: "short",
      year: "numeric"
    }).format(new Date(date));
  } catch {
    return "-";
  }
}

function daysUntil(date) {
  const target = new Date(date);
  const now = new Date();

  target.setHours(0,0,0,0);
  now.setHours(0,0,0,0);

  return Math.ceil((target - now) / 86400000);
}

function statusBadge(status) {
  const map = {
    "Valid": "badge-green",
    "Akan Habis": "badge-yellow",
    "Tidak Aktif": "badge-red",
    "Disetujui": "badge-green",
    "Dalam Verifikasi": "badge-yellow",
    "Ditolak": "badge-red",
    "Belum Diverifikasi": "badge-blue"
  };

  return `<span class="badge ${map[status] || "badge-blue"}">${escapeHTML(status)}</span>`;
}

function showToast(message, type = "success") {
  const toast = document.createElement("div");

  const color = type === "error"
    ? "border-rose-400/30 text-rose-200"
    : "border-cyan-400/30 text-cyan-100";

  toast.className =
    `fixed bottom-5 right-5 z-[200] glass ${color} border rounded-2xl px-4 py-3 text-sm font-semibold shadow-2xl max-w-sm`;

  toast.innerHTML = `
    <div class="flex items-center gap-3">
      <i data-lucide="${type === "error" ? "circle-alert" : "circle-check"}" class="w-5 h-5"></i>
      <span>${escapeHTML(message)}</span>
    </div>
  `;

  document.body.appendChild(toast);
  lucide.createIcons();

  setTimeout(() => {
    toast.remove();
  }, 3200);
}

/* ------------------------------------------------------------
   ROLE MANAGEMENT
   ------------------------------------------------------------ */

function changeRole(role) {
  currentRole = role;

  const roles = {
    admin: {
      name: "Administrator",
      role: "DINAS SOSIAL",
      letter: "A"
    },
    lks: {
      name: "Pengguna LKS",
      role: "LEMBAGA KESEJAHTERAAN SOSIAL",
      letter: "L"
    },
    tksk: {
      name: "Petugas TKSK",
      role: "PETUGAS LAPANGAN",
      letter: "T"
    }
  };

  const selected = roles[role] || roles.admin;

  document.getElementById("userName").textContent = selected.name;
  document.getElementById("userRole").textContent = selected.role;
  document.getElementById("avatarLetter").textContent = selected.letter;

  navigate(role === "admin" ? "dashboard" : role === "lks" ? "lks" : "tksk");

  showToast(`Mode pengguna: ${selected.name}`);
}

/* ------------------------------------------------------------
   NAVIGATION
   ------------------------------------------------------------ */

function navigate(page) {
  currentPage = page;

  document.querySelectorAll(".nav-item").forEach(btn => {
    btn.classList.toggle("active", btn.dataset.page === page);
  });

  const titles = {
    dashboard: "Dashboard",
    lks: "Data LKS",
    lk3: "Data LK3",
    psm: "Data PSM",
    tksk: "Data TKSK",
    verifikasi: "Verifikasi Data & Dokumen",
    laporan: "Laporan Peninjauan Lapangan"
  };

  document.getElementById("pageTitle").textContent = titles[page] || "INUK";

  renderPage();
  closeSidebar();
}

function renderPage() {
  const content = document.getElementById("appContent");

  try {
    switch(currentPage) {
      case "dashboard":
        content.innerHTML = renderDashboard();
        setTimeout(renderChart, 50);
        break;

      case "lks":
        content.innerHTML = renderLKS();
        break;

      case "lk3":
        content.innerHTML = renderEmptyModule("LK3", "Lembaga Kesejahteraan Keluarga");
        break;

      case "psm":
        content.innerHTML = renderEmptyModule("PSM", "Pekerja Sosial Masyarakat");
        break;

      case "tksk":
        content.innerHTML = renderTKSK();
        break;

      case "verifikasi":
        content.innerHTML = renderVerification();
        break;

      case "laporan":
        content.innerHTML = renderReports();
        break;

      default:
        content.innerHTML = renderDashboard();
    }

    lucide.createIcons();

  } catch (error) {
    console.error(error);

    content.innerHTML = `
      <div class="glass rounded-3xl p-8 text-center">
        <i data-lucide="triangle-alert" class="w-12 h-12 mx-auto text-rose-300"></i>
        <h2 class="text-xl font-black mt-4">Terjadi Kesalahan</h2>
        <p class="text-slate-400 mt-2">Halaman tidak dapat ditampilkan.</p>
      </div>
    `;

    lucide.createIcons();
  }

  updatePendingBadge();
  updateNotifications();
}

/* ------------------------------------------------------------
   DASHBOARD
   ------------------------------------------------------------ */

function renderDashboard() {
  const totalLKS = state.lks.length;

  const valid = state.lks.filter(x => x.status === "Valid").length;
  const expiring = state.lks.filter(x => x.status === "Akan Habis").length;
  const inactive = state.lks.filter(x => x.status === "Tidak Aktif").length;

  const pending = state.lks.filter(x => x.documentStatus === "Dalam Verifikasi").length;

  return `
    <div class="space-y-6">

      <!-- HERO -->
      <div class="glass neon rounded-3xl p-5 sm:p-7 overflow-hidden relative">
        <div class="absolute -right-20 -top-20 w-64 h-64 rounded-full bg-cyan-400/10 blur-3xl"></div>
        <div class="absolute right-20 bottom-0 w-48 h-48 rounded-full bg-violet-500/10 blur-3xl"></div>

        <div class="relative">
          <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-5">
            <div>
              <div class="flex items-center gap-2 text-cyan-300 text-xs font-bold uppercase tracking-widest">
                <span class="pulse-dot"></span>
                Sistem Terintegrasi
              </div>

              <h2 class="text-2xl sm:text-3xl lg:text-4xl font-black mt-3">
                Pusat Data <span class="gradient-text">PSKS</span>
              </h2>

              <p class="text-slate-400 max-w-2xl mt-2 text-sm sm:text-base">
                Integrasi data LKS, LK3, PSM dan TKSK
                Kabupaten Manggarai Barat dalam satu platform.
              </p>
            </div>

            <button onclick="openLKSForm()" class="btn btn-primary">
              <i data-lucide="plus" class="w-4 h-4"></i>
              Tambah LKS
            </button>
          </div>
        </div>
      </div>

      <!-- STATISTICS -->
      <div class="grid grid-cols-2 xl:grid-cols-4 gap-3 sm:gap-5">

        ${statCard("Total LKS", totalLKS, "building-2", "Data terdaftar", "cyan")}
        ${statCard("Izin Valid", valid, "badge-check", "Operasional aktif", "green")}
        ${statCard("Akan Habis", expiring, "clock-3", "Perlu perhatian", "yellow")}
        ${statCard("Tidak Aktif", inactive, "circle-off", "Perlu pembaruan", "red")}

      </div>

      <!-- CHART + ALERT -->
      <div class="grid lg:grid-cols-3 gap-5">

        <div class="glass rounded-3xl p-5 lg:col-span-2">
          <div class="flex items-center justify-between mb-5">
            <div>
              <h3 class="font-black">Rekapitulasi PSKS</h3>
              <p class="text-xs text-slate-500 mt-1">Data berdasarkan kategori</p>
            </div>
            <span class="badge badge-blue">LIVE DATA</span>
          </div>

          <div class="h-[280px]">
            <canvas id="psksChart"></canvas>
          </div>
        </div>

        <div class="glass rounded-3xl p-5">
          <div class="flex items-center justify-between">
            <div>
              <h3 class="font-black">Peringatan</h3>
              <p class="text-xs text-slate-500 mt-1">Status izin LKS</p>
            </div>
            <i data-lucide="bell-ring" class="w-5 h-5 text-yellow-300"></i>
          </div>

          <div class="mt-5 space-y-3">

            ${state.lks.filter(x => x.status === "Akan Habis").length
              ? state.lks.filter(x => x.status === "Akan Habis").map(x => `
                <div class="rounded-2xl bg-yellow-400/5 border border-yellow-400/10 p-3">
                  <div class="flex gap-3">
                    <div class="w-9 h-9 rounded-xl bg-yellow-400/10 flex items-center justify-center">
                      <i data-lucide="clock-3" class="w-4 h-4 text-yellow-300"></i>
                    </div>
                    <div class="min-w-0">
                      <div class="font-bold text-sm truncate">${escapeHTML(x.name)}</div>
                      <div class="text-xs text-yellow-200/70 mt-1">
                        Izin berakhir ${formatDate(x.licenseExpiry)}
                      </div>
                    </div>
                  </div>
                </div>
              `).join("")
              : `
                <div class="text-center py-8 text-slate-500 text-sm">
                  Tidak ada izin yang mendekati kadaluarsa.
                </div>
              `
            }

          </div>
        </div>

      </div>

      <!-- MAP -->
      <div class="glass rounded-3xl overflow-hidden">
        <div class="p-5 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div>
            <h3 class="font-black flex items-center gap-2">
              <i data-lucide="map" class="w-5 h-5 text-cyan-300"></i>
              Peta Sebaran PSKS
            </h3>
            <p class="text-xs text-slate-500 mt-1">
              Kabupaten Manggarai Barat
            </p>
          </div>

          <button onclick="getCurrentLocation()" class="btn btn-secondary text-xs">
            <i data-lucide="crosshair" class="w-4 h-4"></i>
            Deteksi Lokasi Saya
          </button>
        </div>

        <div id="mapPreview"
             class="h-[300px] relative overflow-hidden bg-[#061321]">

          <div class="absolute inset-0 opacity-30"
               style="
                 background-image:
                 linear-gradient(rgba(34,211,238,.12) 1px, transparent 1px),
                 linear-gradient(90deg, rgba(34,211,238,.12) 1px, transparent 1px);
                 background-size: 35px 35px;
               ">
          </div>

          <div class="absolute inset-0 flex items-center justify-center">
            <div class="text-center">
              <div class="w-20 h-20 mx-auto rounded-full border border-cyan-300/20 bg-cyan-300/5 flex items-center justify-center">
                <i data-lucide="map-pin" class="w-9 h-9 text-cyan-300"></i>
              </div>
              <div class="font-black mt-4">Manggarai Barat</div>
              <div class="text-xs text-slate-500 mt-1">
                ${totalLKS} titik LKS terdaftar
              </div>
            </div>
          </div>

          ${[
            [20,32],[34,52],[52,27],[67,47],[78,34],[58,68],[84,70],[40,75]
          ].map((p,i)=>`
            <div class="absolute w-3 h-3 rounded-full bg-cyan-300 shadow-[0_0_18px_rgba(34,211,238,.8)]"
                 style="left:${p[0]}%;top:${p[1]}%"
                 title="${escapeHTML(state.lks[i]?.name || 'LKS')}">
            </div>
          `).join("")}

        </div>
      </div>

      <!-- LATEST LKS -->
      <div class="glass rounded-3xl p-5">
        <div class="flex items-center justify-between mb-5">
          <div>
            <h3 class="font-black">LKS Terdaftar</h3>
            <p class="text-xs text-slate-500 mt-1">Ringkasan data terbaru</p>
          </div>
          <button onclick="navigate('lks')" class="text-xs text-cyan-300 hover:text-cyan-200">
            Lihat semua →
          </button>
        </div>

        <div class="space-y-2">
          ${state.lks.slice(0,5).map(x => `
            <div class="card-hover rounded-2xl border border-white/5 bg-white/[.015] p-3 flex items-center gap-3">
              <div class="w-10 h-10 rounded-xl bg-cyan-400/10 flex items-center justify-center shrink-0">
                <i data-lucide="building-2" class="w-5 h-5 text-cyan-300"></i>
              </div>

              <div class="min-w-0 flex-1">
                <div class="font-bold text-sm truncate">${escapeHTML(x.name)}</div>
                <div class="text-xs text-slate-500 truncate">
                  ${escapeHTML(x.district)} • ${escapeHTML(x.address)}
                </div>
              </div>

              ${statusBadge(x.status)}
            </div>
          `).join("")}
        </div>
      </div>

    </div>
  `;
}

function statCard(title, value, icon, subtitle, color) {
  const colors = {
    cyan: "text-cyan-300 bg-cyan-400/10",
    green: "text-emerald-300 bg-emerald-400/10",
    yellow: "text-yellow-300 bg-yellow-400/10",
    red: "text-rose-300 bg-rose-400/10"
  };

  return `
    <div class="glass card-hover rounded-3xl p-4 sm:p-5">
      <div class="flex items-start justify-between gap-3">
        <div>
          <div class="text-xs text-slate-500">${title}</div>
          <div class="text-2xl sm:text-3xl font-black mt-2">${value}</div>
          <div class="text-[10px] text-slate-500 mt-1">${subtitle}</div>
        </div>

        <div class="w-10 h-10 rounded-xl ${colors[color]} flex items-center justify-center">
          <i data-lucide="${icon}" class="w-5 h-5"></i>
        </div>
      </div>
    </div>
  `;
}

function renderChart() {
  const canvas = document.getElementById("psksChart");

  if (!canvas) return;

  try {
    if (chartInstance) {
      chartInstance.destroy();
    }

    chartInstance = new Chart(canvas, {
      type: "doughnut",
      data: {
        labels: ["LKS", "LK3", "PSM", "TKSK"],
        datasets: [{
          data: [
            state.lks.length,
            state.lk3.length,
            state.psm.length,
            state.tksk.length
          ],
          borderWidth: 0
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            position: "bottom",
            labels: {
              color: "#94a3b8",
              padding: 20,
              usePointStyle: true
            }
          }
        }
      }
    });

  } catch (error) {
    console.error("Chart error:", error);
  }
}

/* ------------------------------------------------------------
   LKS
   ------------------------------------------------------------ */

function renderLKS() {
  const query = window.lksSearch || "";
  const district = window.lksDistrict || "Semua";

  const districts = [...new Set(state.lks.map(x => x.district))];

  const filtered = state.lks.filter(x => {
    const matchesQuery =
      x.name.toLowerCase().includes(query.toLowerCase()) ||
      x.address.toLowerCase().includes(query.toLowerCase());

    const matchesDistrict =
      district === "Semua" || x.district === district;

    return matchesQuery && matchesDistrict;
  });

  return `
    <div class="space-y-5">

      <div class="flex flex-col lg:flex-row lg:items-end justify-between gap-4">
        <div>
          <div class="text-xs uppercase tracking-widest text-cyan-300 font-bold">
            PSKS • LKS
          </div>
          <h2 class="text-2xl sm:text-3xl font-black mt-2">Data Lembaga Kesejahteraan Sosial</h2>
          <p class="text-slate-500 text-sm mt-1">
            Kelola data lembaga dan status legalitas operasional.
          </p>
        </div>

        <button onclick="openLKSForm()" class="btn btn-primary">
          <i data-lucide="plus" class="w-4 h-4"></i>
          Registrasi LKS
        </button>
      </div>

      <!-- FILTER -->
      <div class="glass rounded-3xl p-4">
        <div class="grid md:grid-cols-3 gap-3">

          <div class="md:col-span-2 relative">
            <i data-lucide="search"
               class="absolute left-4 top-1/2 -translate-y-1/2 w-4 h-4 text-slate-500"></i>

            <input id="lksSearchInput"
                   value="${escapeHTML(query)}"
                   oninput="filterLKS()"
                   class="input pl-11"
                   placeholder="Cari nama LKS atau alamat..." />
          </div>

          <select id="districtFilter"
                  class="input"
                  onchange="filterLKS()">
            <option value="Semua">Semua Kecamatan</option>
            ${districts.map(d => `
              <option value="${escapeHTML(d)}" ${district === d ? "selected" : ""}>
                ${escapeHTML(d)}
              </option>
            `).join("")}
          </select>

        </div>
      </div>

      <!-- TABLE -->
      <div class="glass rounded-3xl overflow-hidden">

        <div class="p-4 sm:p-5 border-b border-white/5 flex items-center justify-between">
          <div>
            <div class="font-black">${filtered.length} LKS</div>
            <div class="text-xs text-slate-500">Data Kabupaten Manggarai Barat</div>
          </div>

          <div class="text-xs text-slate-500">
            ${state.lks.length} total
          </div>
        </div>

        <div class="table-scroll">
          <table class="w-full min-w-[900px]">
            <thead>
              <tr class="text-left text-[10px] uppercase tracking-widest text-slate-500 border-b border-white/5">
                <th class="p-4">Lembaga</th>
                <th class="p-4">Wilayah</th>
                <th class="p-4">Izin</th>
                <th class="p-4">Dokumen</th>
                <th class="p-4">Masa Berlaku</th>
                <th class="p-4 text-right">Aksi</th>
              </tr>
            </thead>

            <tbody>
              ${filtered.map(x => `
                <tr class="border-b border-white/5 hover:bg-white/[.02] transition">

                  <td class="p-4">
                    <div class="flex gap-3 items-center">
                      <div class="w-10 h-10 rounded-xl bg-cyan-400/10 flex items-center justify-center shrink-0">
                        <i data-lucide="building-2" class="w-5 h-5 text-cyan-300"></i>
                      </div>
                      <div>
                        <div class="font-bold text-sm max-w-[320px]">${escapeHTML(x.name)}</div>
                        <div class="text-xs text-slate-500 max-w-[320px] truncate">
                          ${escapeHTML(x.address)}
                        </div>
                      </div>
                    </div>
                  </td>

                  <td class="p-4">
                    <span class="text-sm">${escapeHTML(x.district)}</span>
                  </td>

                  <td class="p-4">
                    ${statusBadge(x.status)}
                  </td>

                  <td class="p-4">
                    ${statusBadge(x.documentStatus)}
                  </td>

                  <td class="p-4">
                    <div class="text-sm">${formatDate(x.licenseExpiry)}</div>
                    <div class="text-[10px] text-slate-500">
                      ${daysUntil(x.licenseExpiry) < 0
                        ? "Sudah berakhir"
                        : `${daysUntil(x.licenseExpiry)} hari lagi`}
                    </div>
                  </td>

                  <td class="p-4 text-right">
                    <div class="flex justify-end gap-2">
                      <button onclick="viewLKS(${x.id})"
                              class="btn btn-secondary !p-2">
                        <i data-lucide="eye" class="w-4 h-4"></i>
                      </button>

                      ${currentRole === "admin" ? `
                        <button onclick="openEditLKS(${x.id})"
                                class="btn btn-secondary !p-2">
                          <i data-lucide="pencil" class="w-4 h-4"></i>
                        </button>
                      ` : ""}
                    </div>
                  </td>

                </tr>
              `).join("")}

              ${filtered.length === 0 ? `
                <tr>
                  <td colspan="6" class="p-12 text-center text-slate-500">
                    Tidak ada data ditemukan.
                  </td>
                </tr>
              ` : ""}
            </tbody>
          </table>
        </div>
      </div>
    </div>
  `;
}

function filterLKS() {
  window.lksSearch = document.getElementById("lksSearchInput")?.value || "";
  window.lksDistrict = document.getElementById("districtFilter")?.value || "Semua";
  renderPage();
}

/* ------------------------------------------------------------
   FORM LKS
   ------------------------------------------------------------ */

function openLKSForm(id = null) {
  const item = id ? state.lks.find(x => x.id === id) : null;

  document.getElementById("modalContent").innerHTML = `
    <div class="flex items-center justify-between mb-6">
      <div>
        <div class="text-xs uppercase tracking-widest text-cyan-300 font-bold">
          ${item ? "EDIT DATA" : "SELF SERVICE"}
        </div>
        <h2 class="text-2xl font-black mt-1">
          ${item ? "Perbarui Data LKS" : "Registrasi LKS"}
        </h2>
      </div>

      <button onclick="closeModal()" class="btn btn-secondary !p-2">
        <i data-lucide="x"></i>
      </button>
    </div>

    <form onsubmit="saveLKS(event, ${item ? item.id : "null"})" class="space-y-4">

      <div>
        <label class="text-xs text-slate-400">Nama Lembaga</label>
        <input id="formName"
               required
               class="input mt-2"
               value="${escapeHTML(item?.name || "")}"
               placeholder="Nama LKS" />
      </div>

      <div>
        <label class="text-xs text-slate-400">Alamat</label>
        <textarea id="formAddress"
                  required
                  rows="3"
                  class="input mt-2"
                  placeholder="Alamat lengkap">${escapeHTML(item?.address || "")}</textarea>
      </div>

      <div class="grid sm:grid-cols-2 gap-4">

        <div>
          <label class="text-xs text-slate-400">Kecamatan</label>
          <select id="formDistrict" class="input mt-2">
            ${["Komodo","Lembor","Lembor Selatan","Sano Nggoang","Boleng","Welak","Kuwus","Kuwus Barat","Macang Pacar","Mbeliling","Ndoso"].map(d => `
              <option ${item?.district === d ? "selected" : ""}>${d}</option>
            `).join("")}
          </select>
        </div>

        <div>
          <label class="text-xs text-slate-400">Masa Berlaku Izin</label>
          <input id="formExpiry"
                 type="date"
                 required
                 class="input mt-2"
                 value="${item?.licenseExpiry || ""}" />
        </div>

      </div>

      <div>
        <label class="text-xs text-slate-400">Nomor Telepon</label>
        <input id="formPhone"
               class="input mt-2"
               value="${escapeHTML(item?.phone || "")}"
               placeholder="Nomor kontak" />
      </div>

      <div class="glass rounded-2xl p-4">
        <div class="flex gap-3">
          <i data-lucide="info" class="w-5 h-5 text-cyan-300 shrink-0"></i>
          <div class="text-xs text-slate-400 leading-relaxed">
            Dokumen legalitas dapat ditambahkan pada modul verifikasi.
            Status dokumen baru akan ditandai sebagai
            <strong class="text-yellow-300">Dalam Verifikasi</strong>.
          </div>
        </div>
      </div>

      <div class="flex gap-3 pt-2">
        <button type="button" onclick="closeModal()" class="btn btn-secondary flex-1">
          Batal
        </button>

        <button type="submit" class="btn btn-primary flex-1">
          <i data-lucide="save" class="w-4 h-4"></i>
          Simpan
        </button>
      </div>

    </form>
  `;

  document.getElementById("modal").classList.add("show");
  lucide.createIcons();
}

function openEditLKS(id) {
  openLKSForm(id);
}

function saveLKS(event, id) {
  event.preventDefault();

  try {
    const name = document.getElementById("formName").value.trim();
    const address = document.getElementById("formAddress").value.trim();
    const district = document.getElementById("formDistrict").value;
    const expiry = document.getElementById("formExpiry").value;
    const phone = document.getElementById("formPhone").value.trim();

    if (!name || !address || !expiry) {
      showToast("Lengkapi data wajib.", "error");
      return;
    }

    if (id) {
      const index = state.lks.findIndex(x => x.id === id);

      if (index === -1) {
        showToast("Data LKS tidak ditemukan.", "error");
        return;
      }

      state.lks[index] = {
        ...state.lks[index],
        name,
        address,
        district,
        licenseExpiry: expiry,
        phone
      };

      state.verificationLogs.unshift({
        id: Date.now(),
        lksId: id,
        action: "Data LKS diperbarui",
        actor: currentRole,
        timestamp: new Date().toISOString()
      });

      showToast("Data LKS berhasil diperbarui.");

    } else {

      const newId = Date.now();

      state.lks.push({
        id: newId,
        name,
        address,
        district,
        licenseExpiry: expiry,
        phone,
        status: "Akan Habis",
        documentStatus: "Dalam Verifikasi",
        category: "LKS"
      });

      state.verificationLogs.unshift({
        id: Date.now(),
        lksId: newId,
        action: "LKS baru didaftarkan",
        actor: currentRole,
        timestamp: new Date().toISOString()
      });

      showToast("LKS berhasil didaftarkan.");
    }

    saveState();
    closeModal();
    renderPage();

  } catch (error) {
    console.error(error);
    showToast("Terjadi kesalahan saat menyimpan LKS.", "error");
  }
}

/* ------------------------------------------------------------
   VIEW LKS
   ------------------------------------------------------------ */

function viewLKS(id) {
  const x = state.lks.find(item => item.id === id);

  if (!x) {
    showToast("Data tidak ditemukan.", "error");
    return;
  }

  document.getElementById("modalContent").innerHTML = `
    <div class="flex justify-between items-start gap-4">
      <div>
        <div class="w-14 h-14 rounded-2xl bg-cyan-400/10 flex items-center justify-center mb-4">
          <i data-lucide="building-2" class="w-7 h-7 text-cyan-300"></i>
        </div>

        <h2 class="text-2xl font-black">${escapeHTML(x.name)}</h2>
        <p class="text-slate-500 text-sm mt-1">${escapeHTML(x.category)}</p>
      </div>

      <button onclick="closeModal()" class="btn btn-secondary !p-2">
        <i data-lucide="x"></i>
      </button>
    </div>

    <div class="grid sm:grid-cols-2 gap-3 mt-7">

      ${detailBox("map-pin", "Kecamatan", x.district)}
      ${detailBox("map", "Alamat", x.address)}
      ${detailBox("shield-check", "Status Izin", statusBadge(x.status))}
      ${detailBox("file-check", "Status Dokumen", statusBadge(x.documentStatus))}
      ${detailBox("calendar", "Masa Berlaku", formatDate(x.licenseExpiry))}
      ${detailBox("phone", "Kontak", x.phone || "-")}

    </div>

    <div class="mt-5 flex gap-3">
      ${currentRole === "admin" ? `
        <button onclick="closeModal(); openEditLKS(${x.id})"
                class="btn btn-primary flex-1">
          <i data-lucide="pencil"></i>
          Edit
        </button>
      ` : ""}

      <button onclick="closeModal()" class="btn btn-secondary flex-1">
        Tutup
      </button>
    </div>
  `;

  document.getElementById("modal").classList.add("show");
  lucide.createIcons();
}

function detailBox(icon, label, value) {
  return `
    <div class="rounded-2xl bg-white/[.025] border border-white/5 p-4">
      <div class="flex items-center gap-2 text-xs text-slate-500">
        <i data-lucide="${icon}" class="w-4 h-4"></i>
        ${label}
      </div>
      <div class="text-sm font-bold mt-2">${value}</div>
    </div>
  `;
}

/* ------------------------------------------------------------
   EMPTY MODULES
   ------------------------------------------------------------ */

function renderEmptyModule(type, fullName) {
  return `
    <div class="space-y-5">

      <div>
        <div class="text-xs uppercase tracking-widest text-violet-300 font-bold">
          PSKS • ${type}
        </div>

        <h2 class="text-2xl sm:text-3xl font-black mt-2">
          Data ${escapeHTML(type)}
        </h2>

        <p class="text-slate-500 text-sm mt-1">
          ${escapeHTML(fullName)}
        </p>
      </div>

      <div class="glass rounded-3xl p-8 sm:p-14 text-center">

        <div class="w-20 h-20 mx-auto rounded-3xl bg-violet-400/10 flex items-center justify-center">
          <i data-lucide="${type === "PSM" ? "heart-handshake" : "users-round"}"
             class="w-9 h-9 text-violet-300"></i>
        </div>

        <h3 class="text-xl font-black mt-5">
          Belum Ada Data
        </h3>

        <p class="text-slate-500 text-sm max-w-md mx-auto mt-2">
          Data ${escapeHTML(type)} belum ditambahkan.
          Modul sudah disiapkan dan dapat digunakan ketika data tersedia.
        </p>

        <button onclick="showToast('Form ${type} siap dikembangkan saat data tersedia.')"
                class="btn btn-secondary mt-6">
          <i data-lucide="plus"></i>
          Tambah Data
        </button>

      </div>
    </div>
  `;
}

/* ------------------------------------------------------------
   VERIFICATION
   ------------------------------------------------------------ */

function renderVerification() {
  const pending = state.lks.filter(x =>
    x.documentStatus === "Dalam Verifikasi"
  );

  return `
    <div class="space-y-5">

      <div>
        <div class="text-xs uppercase tracking-widest text-yellow-300 font-bold">
          ADMINISTRATOR
        </div>

        <h2 class="text-2xl sm:text-3xl font-black mt-2">
          Verifikasi Data & Dokumen
        </h2>

        <p class="text-slate-500 text-sm mt-1">
          Validasi dokumen legalitas yang diajukan LKS.
        </p>
      </div>

      <div class="grid sm:grid-cols-3 gap-4">

        ${statCard("Menunggu", pending.length, "clock-3", "Perlu diperiksa", "yellow")}
        ${statCard("Disetujui", state.lks.filter(x => x.documentStatus === "Disetujui").length, "circle-check", "Dokumen valid", "green")}
        ${statCard("Ditolak", state.lks.filter(x => x.documentStatus === "Ditolak").length, "circle-x", "Perlu perbaikan", "red")}

      </div>

      <div class="glass rounded-3xl overflow-hidden">

        <div class="p-5 border-b border-white/5">
          <h3 class="font-black">Antrean Verifikasi</h3>
        </div>

        <div class="p-4 space-y-3">

          ${pending.length ? pending.map(x => `
            <div class="rounded-2xl border border-white/5 bg-white/[.02] p-4">
              <div class="flex flex-col lg:flex-row lg:items-center gap-4">

                <div class="w-12 h-12 rounded-2xl bg-yellow-400/10 flex items-center justify-center shrink-0">
                  <i data-lucide="file-clock" class="text-yellow-300"></i>
                </div>

                <div class="flex-1 min-w-0">
                  <div class="font-black">${escapeHTML(x.name)}</div>
                  <div class="text-xs text-slate-500 mt-1">
                    ${escapeHTML(x.address)}
                  </div>

                  <div class="flex flex-wrap gap-2 mt-3">
                    ${statusBadge(x.documentStatus)}
                    <span class="badge badge-blue">
                      <i data-lucide="map-pin" class="w-3 h-3"></i>
                      ${escapeHTML(x.district)}
                    </span>
                  </div>
                </div>

                <div class="flex gap-2">
                  <button onclick="verifyLKS(${x.id}, 'Disetujui')"
                          class="btn btn-primary text-xs">
                    <i data-lucide="check"></i>
                    Setujui
                  </button>

                  <button onclick="verifyLKS(${x.id}, 'Ditolak')"
                          class="btn btn-secondary text-xs">
                    <i data-lucide="x"></i>
                    Tolak
                  </button>
                </div>

              </div>
            </div>
          `).join("") : `
            <div class="text-center py-14">
              <div class="w-16 h-16 mx-auto rounded-2xl bg-emerald-400/10 flex items-center justify-center">
                <i data-lucide="circle-check" class="w-8 h-8 text-emerald-300"></i>
              </div>
              <h3 class="font-black mt-4">Tidak Ada Antrean</h3>
              <p class="text-sm text-slate-500 mt-1">
                Semua dokumen sudah diperiksa.
              </p>
            </div>
          `}

        </div>
      </div>
    </div>
  `;
}

function verifyLKS(id, status) {
  try {
    const item = state.lks.find(x => x.id === id);

    if (!item) {
      showToast("LKS tidak ditemukan.", "error");
      return;
    }

    item.documentStatus = status;

    if (status === "Disetujui") {
      item.status = "Valid";
    }

    if (status === "Ditolak") {
      item.status = "Tidak Aktif";
    }

    state.verificationLogs.unshift({
      id: Date.now(),
      lksId: id,
      action: `Dokumen ${status}`,
      actor: "Administrator",
      timestamp: new Date().toISOString()
    });

    saveState();

    showToast(`Dokumen ${item.name} ${status.toLowerCase()}.`);
    renderPage();

  } catch (error) {
    console.error(error);
    showToast("Gagal melakukan verifikasi.", "error");
  }
}

/* ------------------------------------------------------------
   TKSK
   ------------------------------------------------------------ */

function renderTKSK() {
  return `
    <div class="space-y-5">

      <div class="flex flex-col sm:flex-row sm:items-end justify-between gap-4">
        <div>
          <div class="text-xs uppercase tracking-widest text-cyan-300 font-bold">
            PETUGAS LAPANGAN
          </div>

          <h2 class="text-2xl sm:text-3xl font-black mt-2">
            Data TKSK
          </h2>

          <p class="text-slate-500 text-sm mt-1">
            Tenaga Kesejahteraan Sosial Kecamatan.
          </p>
        </div>

        <button onclick="openReportForm()" class="btn btn-primary">
          <i data-lucide="clipboard-plus"></i>
          Laporan Lapangan
        </button>
      </div>

      <div class="grid lg:grid-cols-2 gap-5">

        <div class="glass rounded-3xl p-6">
          <div class="w-14 h-14 rounded-2xl bg-cyan-400/10 flex items-center justify-center">
            <i data-lucide="map-pin-check" class="w-7 h-7 text-cyan-300"></i>
          </div>

          <h3 class="text-xl font-black mt-5">Geo-Tagging GPS</h3>

          <p class="text-sm text-slate-500 mt-2">
            Ambil koordinat lokasi peninjauan secara otomatis menggunakan GPS perangkat.
          </p>

          <div id="gpsStatus"
               class="rounded-2xl border border-white/5 bg-white/[.02] p-4 mt-5">
            <div class="text-xs text-slate-500">Koordinat terakhir</div>
            <div class="font-mono text-sm mt-2 text-cyan-300">
              Belum tersedia
            </div>
          </div>

          <button onclick="getCurrentLocation()"
                  class="btn btn-primary w-full mt-4">
            <i data-lucide="crosshair"></i>
            Ambil Lokasi GPS
          </button>
        </div>

        <div class="glass rounded-3xl p-6">
          <div class="w-14 h-14 rounded-2xl bg-violet-400/10 flex items-center justify-center">
            <i data-lucide="clipboard-list" class="w-7 h-7 text-violet-300"></i>
          </div>

          <h3 class="text-xl font-black mt-5">Laporan Peninjauan</h3>

          <p class="text-sm text-slate-500 mt-2">
            Buat laporan kondisi LKS berdasarkan hasil kunjungan lapangan.
          </p>

          <div class="grid grid-cols-2 gap-3 mt-5">
            <div class="rounded-2xl bg-white/[.025] border border-white/5 p-4">
              <div class="text-xs text-slate-500">Total Laporan</div>
              <div class="text-2xl font-black mt-1">${state.reports.length}</div>
            </div>

            <div class="rounded-2xl bg-white/[.025] border border-white/5 p-4">
              <div class="text-xs text-slate-500">LKS</div>
              <div class="text-2xl font-black mt-1">${state.lks.length}</div>
            </div>
          </div>

          <button onclick="navigate('laporan')"
                  class="btn btn-secondary w-full mt-4">
            Lihat Riwayat Laporan
          </button>
        </div>

      </div>

      <!-- LIST TKSK -->
      <div class="glass rounded-3xl p-5">
        <h3 class="font-black">Daftar TKSK</h3>
        <p class="text-xs text-slate-500 mt-1">
          Data TKSK akan ditambahkan kemudian.
        </p>

        <div class="py-12 text-center">
          <i data-lucide="users" class="w-10 h-10 mx-auto text-slate-600"></i>
          <p class="text-sm text-slate-500 mt-3">Belum ada data TKSK.</p>
        </div>
      </div>

    </div>
  `;
}

/* ------------------------------------------------------------
   GEOLOCATION
   ------------------------------------------------------------ */

function getCurrentLocation() {
  if (!navigator.geolocation) {
    showToast("Browser tidak mendukung GPS.", "error");
    return;
  }

  showToast("Meminta izin lokasi GPS...");

  navigator.geolocation.getCurrentPosition(
    position => {
      const lat = position.coords.latitude;
      const lng = position.coords.longitude;
      const accuracy = Math.round(position.coords.accuracy);

      window.currentGPS = {
        latitude: lat,
        longitude: lng,
        accuracy
      };

      const gpsStatus = document.getElementById("gpsStatus");

      if (gpsStatus) {
        gpsStatus.innerHTML = `
          <div class="text-xs text-slate-500">Lokasi berhasil ditemukan</div>

          <div class="grid grid-cols-2 gap-3 mt-3">
            <div>
              <div class="text-[10px] text-slate-500">Latitude</div>
              <div class="font-mono text-sm text-cyan-300">${lat.toFixed(6)}</div>
            </div>

            <div>
              <div class="text-[10px] text-slate-500">Longitude</div>
              <div class="font-mono text-sm text-cyan-300">${lng.toFixed(6)}</div>
            </div>
          </div>

          <div class="text-[10px] text-slate-500 mt-3">
            Akurasi ±${accuracy} meter
          </div>
        `;
      }

      showToast("Lokasi GPS berhasil diperoleh.");

    },
    error => {
      console.error(error);

      const messages = {
        1: "Izin lokasi ditolak.",
        2: "Lokasi tidak tersedia.",
        3: "Permintaan lokasi terlalu lama."
      };

      showToast(messages[error.code] || "GPS gagal digunakan.", "error");
    },
    {
      enableHighAccuracy: true,
      timeout: 15000,
      maximumAge: 0
    }
  );
}

/* ------------------------------------------------------------
   REPORT FORM
   ------------------------------------------------------------ */

function openReportForm() {

  document.getElementById("modalContent").innerHTML = `
    <div class="flex items-center justify-between mb-6">
      <div>
        <div class="text-xs uppercase tracking-widest text-cyan-300 font-bold">
          TKSK
        </div>
        <h2 class="text-2xl font-black mt-1">
          Laporan Peninjauan Lapangan
        </h2>
      </div>

      <button onclick="closeModal()" class="btn btn-secondary !p-2">
        <i data-lucide="x"></i>
      </button>
    </div>

    <form onsubmit="saveReport(event)" class="space-y-4">

      <div>
        <label class="text-xs text-slate-400">Pilih LKS</label>
        <select id="reportLKS" required class="input mt-2">
          <option value="">-- Pilih LKS --</option>
          ${state.lks.map(x => `
            <option value="${x.id}">
              ${escapeHTML(x.name)}
            </option>
          `).join("")}
        </select>
      </div>

      <div>
        <label class="text-xs text-slate-400">Kondisi / Hasil Peninjauan</label>
        <textarea id="reportDescription"
                  required
                  rows="5"
                  class="input mt-2"
                  placeholder="Tuliskan hasil peninjauan lapangan..."></textarea>
      </div>

      <div class="glass rounded-2xl p-4">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-xl bg-cyan-400/10 flex items-center justify-center">
            <i data-lucide="map-pin" class="text-cyan-300"></i>
          </div>

          <div class="flex-1">
            <div class="font-bold text-sm">Geo-Tagging</div>
            <div id="reportGPS" class="text-xs text-slate-500 mt-1">
              ${window.currentGPS
                ? `${window.currentGPS.latitude.toFixed(6)}, ${window.currentGPS.longitude.toFixed(6)}`
                : "Belum mengambil lokasi"}
            </div>
          </div>

          <button type="button"
                  onclick="getCurrentLocation()"
                  class="btn btn-secondary text-xs">
            Ambil GPS
          </button>
        </div>
      </div>

      <div class="flex gap-3 pt-2">
        <button type="button"
                onclick="closeModal()"
                class="btn btn-secondary flex-1">
          Batal
        </button>

        <button type="submit" class="btn btn-primary flex-1">
          <i data-lucide="send"></i>
          Simpan Laporan
        </button>
      </div>

    </form>
  `;

  document.getElementById("modal").classList.add("show");
  lucide.createIcons();
}

function saveReport(event) {
  event.preventDefault();

  try {
    const lksId = Number(document.getElementById("reportLKS").value);
    const description = document.getElementById("reportDescription").value.trim();

    if (!lksId || !description) {
      showToast("Lengkapi laporan.", "error");
      return;
    }

    const lks = state.lks.find(x => x.id === lksId);

    if (!lks) {
      showToast("LKS tidak ditemukan.", "error");
      return;
    }

    state.reports.unshift({
      id: Date.now(),
      lksId,
      lksName: lks.name,
      description,
      gps: window.currentGPS || null,
      createdAt: new Date().toISOString(),
      officer: "TKSK"
    });

    saveState();
    closeModal();

    showToast("Laporan berhasil disimpan.");
    navigate("laporan");

  } catch (error) {
    console.error(error);
    showToast("Gagal menyimpan laporan.", "error");
  }
}

/* ------------------------------------------------------------
   REPORT LIST
   ------------------------------------------------------------ */

function renderReports() {

  return `
    <div class="space-y-5">

      <div class="flex flex-col sm:flex-row sm:items-end justify-between gap-4">
        <div>
          <div class="text-xs uppercase tracking-widest text-violet-300 font-bold">
            TKSK
          </div>

          <h2 class="text-2xl sm:text-3xl font-black mt-2">
            Laporan Peninjauan
          </h2>

          <p class="text-slate-500 text-sm mt-1">
            Riwayat laporan lapangan dan koordinat lokasi.
          </p>
        </div>

        <button onclick="openReportForm()" class="btn btn-primary">
          <i data-lucide="plus"></i>
          Buat Laporan
        </button>
      </div>

      <div class="space-y-3">

        ${state.reports.length ? state.reports.map(report => `
          <div class="glass rounded-3xl p-5">

            <div class="flex flex-col lg:flex-row gap-5">

              <div class="w-12 h-12 rounded-2xl bg-violet-400/10 flex items-center justify-center shrink-0">
                <i data-lucide="clipboard-check" class="text-violet-300"></i>
              </div>

              <div class="flex-1">
                <div class="flex flex-wrap gap-2 items-center">
                  <h3 class="font-black">${escapeHTML(report.lksName)}</h3>
                  <span class="badge badge-blue">
                    ${formatDate(report.createdAt)}
                  </span>
                </div>

                <p class="text-sm text-slate-400 mt-3 leading-relaxed">
                  ${escapeHTML(report.description)}
                </p>

                <div class="mt-4 flex flex-wrap gap-3">
                  ${report.gps ? `
                    <span class="badge badge-green">
                      <i data-lucide="map-pin" class="w-3 h-3"></i>
                      ${report.gps.latitude.toFixed(6)}, ${report.gps.longitude.toFixed(6)}
                    </span>

                    <span class="badge badge-blue">
                      Akurasi ±${report.gps.accuracy} m
                    </span>
                  ` : `
                    <span class="badge badge-red">
                      Tanpa koordinat GPS
                    </span>
                  `}
                </div>
              </div>

            </div>

          </div>
        `).join("") : `
          <div class="glass rounded-3xl p-14 text-center">
            <i data-lucide="clipboard-x" class="w-10 h-10 mx-auto text-slate-600"></i>
            <h3 class="font-black mt-4">Belum Ada Laporan</h3>
            <p class="text-sm text-slate-500 mt-1">
              Laporan peninjauan TKSK akan muncul di sini.
            </p>
          </div>
        `}

      </div>

    </div>
  `;
}

/* ------------------------------------------------------------
   NOTIFICATIONS
   ------------------------------------------------------------ */

function updateNotifications() {
  const expiring = state.lks.filter(x => {
    const days = daysUntil(x.licenseExpiry);
    return days >= 0 && days <= 90;
  });

  document.getElementById("notificationDot")
    ?.classList.toggle("hidden", expiring.length === 0);
}

function showNotifications() {
  const expiring = state.lks.filter(x => {
    const days = daysUntil(x.licenseExpiry);
    return days >= 0 && days <= 90;
  });

  document.getElementById("modalContent").innerHTML = `
    <div class="flex items-center justify-between mb-6">
      <div>
        <div class="text-xs uppercase tracking-widest text-yellow-300 font-bold">
          NOTIFIKASI
        </div>
        <h2 class="text-2xl font-black mt-1">Peringatan Sistem</h2>
      </div>

      <button onclick="closeModal()" class="btn btn-secondary !p-2">
        <i data-lucide="x"></i>
      </button>
    </div>

    ${expiring.length ? expiring.map(x => `
      <div class="rounded-2xl bg-yellow-400/5 border border-yellow-400/10 p-4 mb-3">
        <div class="flex gap-3">
          <i data-lucide="triangle-alert" class="w-5 h-5 text-yellow-300 shrink-0"></i>
          <div>
            <div class="font-bold text-sm">${escapeHTML(x.name)}</div>
            <div class="text-xs text-yellow-200/70 mt-1">
              Izin operasional akan berakhir
              ${formatDate(x.licenseExpiry)}
              (${daysUntil(x.licenseExpiry)} hari lagi).
            </div>
          </div>
        </div>
      </div>
    `).join("") : `
      <div class="text-center py-10">
        <i data-lucide="bell-off" class="w-10 h-10 mx-auto text-slate-600"></i>
        <p class="text-sm text-slate-500 mt-3">
          Tidak ada peringatan saat ini.
        </p>
      </div>
    `}
  `;

  document.getElementById("modal").classList.add("show");
  lucide.createIcons();
}

/* ------------------------------------------------------------
   PENDING BADGE
   ------------------------------------------------------------ */

function updatePendingBadge() {
  const count = state.lks.filter(x =>
    x.documentStatus === "Dalam Verifikasi"
  ).length;

  const badge = document.getElementById("pendingBadge");

  if (!badge) return;

  badge.textContent = count;
  badge.classList.toggle("hidden", count === 0);
}

/* ------------------------------------------------------------
   MODAL
   ------------------------------------------------------------ */

function closeModal() {
  document.getElementById("modal").classList.remove("show");
}

document.getElementById("modal").addEventListener("click", function(e) {
  if (e.target === this) {
    closeModal();
  }
});

/* ------------------------------------------------------------
   MOBILE SIDEBAR
   ------------------------------------------------------------ */

function openSidebar() {
  document.getElementById("sidebar").classList.add("open");
  document.getElementById("mobileOverlay").classList.add("show");
}

function closeSidebar() {
  document.getElementById("sidebar").classList.remove("open");
  document.getElementById("mobileOverlay").classList.remove("show");
}

/* ------------------------------------------------------------
   AUTO CHECK LICENSE EXPIRATION
   ------------------------------------------------------------ */

function checkLicenseExpiration() {
  try {
    state.lks.forEach(lks => {
      const days = daysUntil(lks.licenseExpiry);

      if (days < 0) {
        lks.status = "Tidak Aktif";
      } else if (days <= 90 && lks.documentStatus === "Disetujui") {
        lks.status = "Akan Habis";
      } else if (lks.documentStatus === "Disetujui") {
        lks.status = "Valid";
      }
    });

    saveState();

  } catch (error) {
    console.error("License check error:", error);
  }
}

/* ------------------------------------------------------------
   INITIALIZATION
   ------------------------------------------------------------ */

document.addEventListener("DOMContentLoaded", () => {

  try {
    checkLicenseExpiration();
    navigate("dashboard");
    lucide.createIcons();
    updateNotifications();
    updatePendingBadge();

  } catch (error) {
    console.error("Initialization error:", error);
    showToast("Aplikasi gagal diinisialisasi.", "error");
  }

});

/* ------------------------------------------------------------
   KEYBOARD SHORTCUT
   ------------------------------------------------------------ */

document.addEventListener("keydown", event => {

  if (event.key === "Escape") {
    closeModal();
    closeSidebar();
  }

});

/* ------------------------------------------------------------
   AUTO REFRESH STATUS
   ------------------------------------------------------------ */

setInterval(() => {
  try {
    checkLicenseExpiration();
    updateNotifications();
    updatePendingBadge();

    if (currentPage === "dashboard") {
      renderPage();
    }

  } catch (error) {
    console.error("Auto refresh error:", error);
  }
}, 60000);

</script>

</body>
</html>
