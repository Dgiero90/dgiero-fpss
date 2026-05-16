<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Dgiero FPS Benchmark - Sistemini seç, oyunlardaki gerçek FPS değerini anında gör.">
  <title>Dgiero | FPS Benchmark & Sistem Karşılaştırma</title>
  <style>
    :root {
      --bg: #0b0b0b;
      --bg-2: #141414;
      --panel: rgba(255,255,255,0.06);
      --panel-strong: rgba(255,255,255,0.1);
      --text: #f5f5f5;
      --muted: #c9c9c9;
      --line: rgba(255,255,255,0.12);
      --accent: #ffffff;
      --accent-2: #9ca3af;
      --shadow: 0 20px 50px rgba(0,0,0,0.45);
      --radius: 24px;
      --radius-sm: 16px;
      --max: 1180px;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      font-family: Arial, Helvetica, sans-serif;
      background:
        radial-gradient(circle at top left, rgba(255,255,255,0.08), transparent 25%),
        radial-gradient(circle at bottom right, rgba(255,255,255,0.05), transparent 22%),
        linear-gradient(135deg, #000000, #111111 45%, #1d1d1d 100%);
      color: var(--text);
      overflow-x: hidden;
      line-height: 1.6;
    }

    a {
      color: inherit;
      text-decoration: none;
    }

    button, input, select {
      font: inherit;
    }

    .container {
      width: min(var(--max), calc(100% - 32px));
      margin: 0 auto;
    }

    .noise {
      position: fixed;
      inset: 0;
      pointer-events: none;
      opacity: 0.05;
      background-image:
        linear-gradient(rgba(255,255,255,0.2) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.2) 1px, transparent 1px);
      background-size: 36px 36px;
      mix-blend-mode: soft-light;
    }

    .topbar {
      position: sticky;
      top: 0;
      z-index: 1000;
      backdrop-filter: blur(18px);
      background: rgba(10,10,10,0.72);
      border-bottom: 1px solid var(--line);
    }

    .topbar-inner {
      height: 76px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 12px;
      font-weight: 800;
      letter-spacing: 2px;
      text-transform: uppercase;
    }

    .brand-mark {
      width: 38px;
      height: 38px;
      border-radius: 12px;
      background: linear-gradient(145deg, #ffffff, #666666);
      box-shadow: 0 10px 30px rgba(255,255,255,0.15);
    }

    .nav {
      display: flex;
      align-items: center;
      gap: 18px;
      flex-wrap: wrap;
    }

    .nav a {
      color: var(--muted);
      padding: 10px 12px;
      border-radius: 999px;
      transition: 0.25s ease;
    }

    .nav a:hover {
      color: var(--text);
      background: rgba(255,255,255,0.08);
    }

    .hero {
      position: relative;
      padding: 60px 0 42px;
      min-height: calc(100vh - 76px);
      display: grid;
      align-items: center;
    }

    .hero-grid {
      display: grid;
      grid-template-columns: 1.1fr 0.9fr;
      gap: 28px;
      align-items: center;
    }

    .eyebrow {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      padding: 8px 14px;
      border-radius: 999px;
      background: rgba(255,255,255,0.08);
      border: 1px solid var(--line);
      color: var(--muted);
      font-size: 14px;
      margin-bottom: 18px;
    }

    .hero h1 {
      font-size: clamp(2.5rem, 6vw, 4.5rem);
      line-height: 1.05;
      letter-spacing: -2px;
      margin-bottom: 18px;
      animation: fadeUp 0.8s ease both;
    }

    .hero h1 span {
      background: linear-gradient(90deg, #ffffff, #9f9f9f, #ffffff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .hero p {
      max-width: 640px;
      color: var(--muted);
      font-size: 1.06rem;
      margin-bottom: 28px;
      animation: fadeUp 0.95s ease both;
    }

    .calculator-box {
      background: rgba(255,255,255,0.05);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      padding: 24px;
      box-shadow: var(--shadow);
    }

    .calc-title {
      font-size: 1.4rem;
      margin-bottom: 16px;
      font-weight: 700;
      border-bottom: 1px solid var(--line);
      padding-bottom: 10px;
    }

    .form-group {
      margin-bottom: 16px;
    }

    .form-group label {
      display: block;
      font-size: 0.9rem;
      color: var(--muted);
      margin-bottom: 6px;
    }

    .form-select {
      width: 100%;
      background: #141414;
      color: #fff;
      border: 1px solid var(--line);
      padding: 12px;
      border-radius: 12px;
      outline: none;
      cursor: pointer;
    }

    .form-select:focus {
      border-color: #fff;
    }

    .hero-panel {
      background: linear-gradient(180deg, rgba(255,255,255,0.1), rgba(255,255,255,0.04));
      border: 1px solid var(--line);
      border-radius: 28px;
      padding: 22px;
      box-shadow: var(--shadow);
    }

    .panel-screen {
      border-radius: 20px;
      overflow: hidden;
      background: #101010;
      border: 1px solid rgba(255,255,255,0.08);
    }

    .panel-top {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 14px 16px;
      background: rgba(255,255,255,0.05);
      border-bottom: 1px solid rgba(255,255,255,0.06);
    }

    .dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: #ffffff;
      opacity: 0.9;
    }

    .panel-content {
      padding: 18px;
      display: grid;
      gap: 14px;
    }

    .fps-display-card {
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.08);
      border-radius: 18px;
      padding: 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .game-info h3 {
      font-size: 1.3rem;
      margin-bottom: 4px;
    }

    .game-info p {
      font-size: 0.85rem;
      color: var(--muted);
    }

    .fps-counter {
      font-size: 2.2rem;
      font-weight: 900;
      color: #fff;
      text-shadow: 0 0 15px rgba(255,255,255,0.3);
    }

    .fps-counter span {
      font-size: 1rem;
      font-weight: 400;
      color: var(--muted);
      margin-left: 4px;
    }

    section {
      padding: 40px 0;
    }

    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: end;
      gap: 18px;
      margin-bottom: 24px;
    }

    .section-header h2 {
      font-size: clamp(1.8rem, 4vw, 2.8rem);
      letter-spacing: -1px;
    }

    .section-header p {
      max-width: 680px;
      color: var(--muted);
    }

    .tools-bar {
      display: grid;
      grid-template-columns: 1.2fr auto;
      gap: 12px;
      margin: 18px 0 24px;
    }

    .search {
      width: 100%;
      border: 1px solid var(--line);
      background: rgba(255,255,255,0.06);
      color: var(--text);
      padding: 14px 16px;
      border-radius: 18px;
      outline: none;
    }

    .filters {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    .chip {
      border: 1px solid var(--line);
      background: rgba(255,255,255,0.04);
      color: var(--muted);
      border-radius: 999px;
      padding: 12px 14px;
      cursor: pointer;
      transition: 0.25s ease;
      user-select: none;
    }

    .chip.active, .chip:hover {
      background: rgba(255,255,255,0.12);
      color: var(--text);
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 18px;
    }

    .card {
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      padding: 22px;
      box-shadow: var(--shadow);
      transition: 0.28s ease;
      display: flex;
      flex-direction: column;
      gap: 12px;
      opacity: 0;
      transform: translateY(26px);
    }

    .card.show {
      opacity: 1;
      transform: translateY(0);
    }

    .card:hover {
      transform: translateY(-6px);
      background: var(--panel-strong);
    }

    .card-top {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      align-items: center;
    }

    .badge {
      font-size: 0.8rem;
      color: #111;
      background: #fff;
      border-radius: 999px;
      padding: 7px 10px;
      white-space: nowrap;
      font-weight: 700;
    }

    .card h3 {
      font-size: 1.2rem;
      margin-top: 4px;
    }

    .card ul {
      list-style: none;
      font-size: 0.9rem;
      color: var(--muted);
      display: grid;
      gap: 4px;
    }

    .card .fps-tag {
      margin-top: auto;
      background: rgba(255,255,255,0.08);
      padding: 8px 12px;
      border-radius: 10px;
      font-size: 0.85rem;
      display: flex;
      justify-content: space-between;
    }

    .hide {
      display: none !important;
    }

    footer {
      padding: 32px 0 44px;
      border-top: 1px solid var(--line);
      color: var(--muted);
      margin-top: 40px;
    }

    .footer-inner {
      display: flex;
      justify-content: space-between;
      gap: 18px;
      flex-wrap: wrap;
      align-items: center;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(24px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 1024px) {
      .hero-grid, .tools-bar, .grid {
        grid-template-columns: 1fr;
      }
      .nav {
        display: none;
      }
    }
  </style>
</head>
<body>
  <div class="noise"></div>

  <div class="topbar">
    <div class="container topbar-inner">
      <a class="brand" href="#home">
        <span class="brand-mark"></span>
        <span>Dgiero FPS</span>
      </a>
      <nav class="nav">
        <a href="#calculator">FPS Hesapla</a>
        <a href="#sistemler">Hazır Sistemler</a>
      </nav>
    </div>
  </div>

  <main id="home" class="hero">
    <div class="container hero-grid">
      <div>
        <div class="eyebrow">Dinamik FPS Ölçüm Paneli</div>
        <h1>Sistemini Gir, <span>FPS Değerini</span> Gör.</h1>
        <p>
          Aşağıdaki panelden işlemci, ekran kartı ve RAM bileşenlerini seçerek Valorant ve Roblox gibi popüler oyunlarda alacağın tahmini performans ve FPS değerlerini anında hesapla.
        </p>
        
        <div class="calculator-box" id="calculator">
          <div class="calc-title">Sistem Bileşenlerini Seç</div>
          
          <div class="form-group">
            <label for="cpuSelect">İşlemci (CPU)</label>
            <select id="cpuSelect" class="form-select">
              <option value="ryzen5_5600">AMD Ryzen 5 5600</option>
              <option value="ryzen7_7800x3d">AMD Ryzen 7 7800X3D</option>
              <option value="intel_i5_14400f">Intel Core i5-14400F</option>
            </select>
          </div>

          <div class="form-group">
            <label for="gpuSelect">Ekran Kartı (GPU)</label>
            <select id="gpuSelect" class="form-select">
              <option value="rtx4060">NVIDIA GeForce RTX 4060</option>
              <option value="rtx4070_super">NVIDIA GeForce RTX 4070 Super</option>
              <option value="rtx5060ti">NVIDIA GeForce RTX 5060 Ti</option>
            </select>
          </div>

          <div class="form-group">
            <label for="ramSelect">Bellek (RAM)</label>
            <select id="ramSelect" class="form-select">
              <option value="16">16 GB DDR4 / DDR5</option>
              <option value="32">32 GB DDR5</option>
            </select>
          </div>
        </div>
      </div>

      <aside class="hero-panel">
        <div class="panel-screen">
          <div class="panel-top">
            <span class="dot"></span>
            <span class="dot"></span>
            <span class="dot"></span>
            <span style="margin-left: auto; font-size: 0.8rem; color: var(--muted);">CANLI FPS MONİTÖRÜ</span>
          </div>
          <div class="panel-content">
            
            <div class="fps-display-card">
              <div class="game-info">
                <h3>Valorant</h3>
                <p>1080p • Düşük/Rekabetçi Ayarlar</p>
              </div>
              <div class="fps-counter" id="valorantFps">-- <span>FPS</span></div>
            </div>

            <div class="fps-display-card">
              <div class="game-info">
                <h3>Roblox</h3>
                <p>1080p • Grafik Seviyesi 10 (Kilit Açık)</p>
              </div>
              <div class="fps-counter" id="robloxFps">-- <span>FPS</span></div>
            </div>

            <div style="padding: 10px; font-size: 0.85rem; color: var(--muted); border-top: 1px solid var(--line); text-align: center;">
              * Değerler güncel sürücüler ve optimize sistemler baz alınarak hesaplanır. Özellikle Valorant işlemci (CPU) önbelleğine doğrudan duyarlıdır.
            </div>

          </div>
        </div>
      </aside>
    </div>
  </main>

  <section id="sistemler">
    <div class="container">
      <div class="section-header">
        <div>
          <h2>Önerilen Hazır Sistemler</h2>
          <p>Yüksek FPS hedefleri için özel olarak optimize edilmiş, akvaryum kasa tasarımlı popüler konfigürasyonlar.</p>
        </div>
      </div>

      <div class="tools-bar">
        <input id="searchInput" class="search" type="text" placeholder="Sistem adı veya parça ara... (Örn: RTX 4070, Ryzen 7)">
        <div class="filters">
          <button class="chip active" data-filter="all">Tümü</button>
          <button class="chip" data-filter="fiyat-performans">F/P Sistemler</button>
          <button class="chip" data-filter="ust-seviye">Üst Seviye</button>
        </div>
      </div>

      <div id="cards" class="grid">
        <article class="card" data-category="fiyat-performans" data-title="Dgiero Starter RTX 4060">
          <div class="card-top">
            <span class="badge">F/P Canavarı</span>
            <span>40.000 TL Bandı</span>
          </div>
          <h3>Dgiero Starter v1</h3>
          <ul>
            <li><strong>İşlemci:</strong> AMD Ryzen 5 5600</li>
            <li><strong>Ekran Kartı:</strong> RTX 4060 8GB</li>
            <li><strong>RAM:</strong> 16GB DDR4</li>
            <li><strong>Kasa:</strong> Panoramik Akvaryum (Siyah)</li>
          </ul>
          <div class="fps-tag">
            <span>Valorant: ~350 FPS</span>
            <span>Roblox: ~240 FPS</span>
          </div>
        </article>

        <article class="card" data-category="ust-seviye" data-title="Dgiero NextGen RTX 5060 Ti">
          <div class="card-top">
            <span class="badge">Yeni Nesil</span>
            <span>45.000 TL Bandı</span>
          </div>
          <h3>Dgiero NextGen v2</h3>
          <ul>
            <li><strong>İşlemci:</strong> Intel i5-14400F</li>
            <li><strong>Ekran Kartı:</strong> RTX 5060 Ti 12GB</li>
            <li><strong>RAM:</strong> 16GB DDR5</li>
            <li><strong>Kasa:</strong> Temperli Akvaryum (Beyaz)</li>
          </ul>
          <div class="fps-tag">
            <span>Valorant: ~450 FPS</span>
            <span>Roblox: ~360 FPS</span>
          </div>
        </article>

        <article class="card" data-category="ust-seviye" data-title="Dgiero Ultra Ryzen 7 7800X3D RTX 4070 Super">
          <div class="card-top">
            <span class="badge">E-Spor Premium</span>
            <span>60.000 TL+</span>
          </div>
          <h3>Dgiero Ultra X3D</h3>
          <ul>
            <li><strong>İşlemci:</strong> Ryzen 7 7800X3D</li>
            <li><strong>Ekran Kartı:</strong> RTX 4070 Super 12GB</li>
            <li><strong>RAM:</strong> 32GB DDR5 6000MHz</li>
            <li><strong>Kasa:</strong> Premium Çift Odalı Akvaryum</li>
          </ul>
          <div class="fps-tag">
            <span>Valorant: ~700+ FPS</span>
            <span>Roblox: ~500+ FPS</span>
          </div>
        </article>
      </div>
    </div>
  </section>

  <footer>
    <div class="container footer-inner">
      <div>© 2026 Dgiero FPS Benchmark</div>
      <div>Gamer odaklı donanım ve performans simülasyonu</div>
    </div>
  </footer>

  <script>
    // FPS Hesaplama Veri Tabanı Matrix'i
    const fpsMatrix = {
      // İşlemci Temel Çarpanları (Valorant işlemci ağırlıklı olduğu için önemi büyük)
      cpu: {
        ryzen5_5600: { valorant: 300, roblox: 200 },
        intel_i5_14400f: { valorant: 380, roblox: 260 },
        ryzen7_7800x3d: { valorant: 620, roblox: 450 } // X3D Önbellek farkı
      },
      // Ekran Kartı Ekstra Katkıları
      gpu: {
        rtx4060: { valorant: 40, roblox: 60 },
        rtx5060ti: { valorant: 70, roblox: 110 },
        rtx4070_super: { valorant: 110, roblox: 160 }
      },
      // RAM Çarpanı
      ram: {
        "16": 1.0,
        "32": 1.12 // Çift kanal yüksek frekans dopingi
      }
    };

    const cpuSelect = document.getElementById('cpuSelect');
    const gpuSelect = document.getElementById('gpuSelect');
    const ramSelect = document.getElementById('ramSelect');
    
    const valorantFpsText = document.getElementById('valorantFps');
    const robloxFpsText = document.getElementById('robloxFps');

    function calculateFps() {
      const selectedCpu = cpuSelect.value;
      const selectedGpu = gpuSelect.value;
      const selectedRam = ramSelect.value;

      // Temel Hesaplama
      let valFPS = fpsMatrix.cpu[selectedCpu].valorant + fpsMatrix.gpu[selectedGpu].valorant;
      let robFPS = fpsMatrix.cpu[selectedCpu].roblox + fpsMatrix.gpu[selectedGpu].roblox;

      // RAM Çarpanı Uygulama
      valFPS = Math.round(valFPS * fpsMatrix.ram[selectedRam]);
      robFPS = Math.round(robFPS * fpsMatrix.ram[selectedRam]);

      // Ekrana Yazdırma (Efektli geçiş için küçük bir animasyon hissi)
      valorantFpsText.innerHTML = `${valFPS} <span>FPS</span>`;
      robloxFpsText.innerHTML = `${robFPS} <span>FPS</span>`;
    }

    // Seçim kutuları değiştikçe tetikle
    cpuSelect.addEventListener('change', calculateFps);
    gpuSelect.addEventListener('change', calculateFps);
    ramSelect.addEventListener('change', calculateFps);

    // Sayfa ilk açıldığında varsayılanı hesapla
    calculateFps();

    // --- Arama ve Filtreleme Bölümü ---
    const cards = document.querySelectorAll('.card');
    const searchInput = document.getElementById('searchInput');
    const chips = document.querySelectorAll('.chip');
    let activeFilter = 'all';

    function revealCards() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry, index) => {
          if (entry.isIntersecting) {
            entry.target.style.transitionDelay = `${index * 60}ms`;
            entry.target.classList.add('show');
            observer.unobserve(entry.target);
          }
        });
      }, { threshold: 0.05 });

      cards.forEach(card => observer.observe(card));
    }

    function filterCards() {
      const query = searchInput.value.trim().toLowerCase();

      cards.forEach(card => {
        const title = card.dataset.title.toLowerCase();
        const category = card.dataset.category;
        const matchesText = title.includes(query);
        const matchesCategory = activeFilter === 'all' || category === activeFilter;
        card.classList.toggle('hide', !(matchesText && matchesCategory));
      });
    }

    chips.forEach(chip => {
      chip.addEventListener('click', () => {
        chips.forEach(c => c.classList.remove('active'));
        chip.classList.add('active');
        activeFilter = chip.dataset.filter;
        filterCards();
      });
    });

    searchInput.addEventListener('input', filterCards);
    revealCards();
  </script>
</body>
</html>
