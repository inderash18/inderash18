<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <title>INDERASH · Hypervisual Dev Portal</title>
  <!-- Google Fonts: modern + icon library -->
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <!-- GSAP for silky animations -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
  <!-- Chart.js for radar -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: #03050b;
      font-family: 'Plus Jakarta Sans', sans-serif;
      color: #f0f3fa;
      overflow-x: hidden;
    }

    /* animated gradient orb background */
    .orb-bg {
      position: fixed;
      top: -20%;
      left: -20%;
      width: 140%;
      height: 140%;
      background: radial-gradient(circle at 30% 40%, rgba(102, 126, 234, 0.2), rgba(15, 25, 45, 0.95));
      z-index: -2;
      pointer-events: none;
    }

    .noise-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.7' numOctaves='2' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)' opacity='0.02'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: -1;
    }

    .container {
      max-width: 1400px;
      margin: 0 auto;
      padding: 1.8rem 2rem 3rem;
      position: relative;
      z-index: 3;
    }

    /* glassmorphic premium */
    .glass-premium {
      background: rgba(12, 22, 40, 0.55);
      backdrop-filter: blur(14px);
      border-radius: 2rem;
      border: 1px solid rgba(102, 126, 234, 0.35);
      transition: all 0.35s cubic-bezier(0.2, 0.9, 0.4, 1.1);
      box-shadow: 0 20px 35px -15px rgba(0, 0, 0, 0.4);
    }

    .glass-premium:hover {
      border-color: rgba(167, 139, 250, 0.7);
      box-shadow: 0 25px 40px -12px rgba(102, 126, 234, 0.25);
      transform: translateY(-3px);
    }

    .gradient-text {
      background: linear-gradient(125deg, #c4b5fd, #a78bfa, #e879f9);
      background-clip: text;
      -webkit-background-clip: text;
      color: transparent;
      font-weight: 800;
    }

    .badge-glow {
      background: rgba(45, 55, 85, 0.6);
      border-radius: 40px;
      padding: 0.35rem 1.1rem;
      font-weight: 500;
      font-size: 0.85rem;
      backdrop-filter: blur(4px);
      transition: all 0.2s;
    }

    .project-tile {
      background: rgba(8, 16, 30, 0.7);
      border-left: 3px solid #8b5cf6;
      padding: 1rem 1.2rem;
      border-radius: 1.2rem;
      transition: all 0.2s;
    }

    .progress-bar {
      background: #1e293b;
      border-radius: 40px;
      height: 10px;
      overflow: hidden;
    }

    .progress-fill {
      background: linear-gradient(90deg, #667eea, #c084fc);
      width: 0%;
      height: 100%;
      border-radius: 40px;
      transition: width 1s cubic-bezier(0.22, 0.97, 0.36, 1);
    }

    .btn-outline-saturn {
      border: 1px solid #a78bfa;
      background: transparent;
      padding: 0.5rem 1.5rem;
      border-radius: 40px;
      font-weight: 600;
      transition: all 0.2s;
      color: #ddd6fe;
    }

    .btn-outline-saturn:hover {
      background: #a78bfa20;
      box-shadow: 0 0 12px #a78bfa;
      color: white;
    }

    .stats-number {
      font-size: 2rem;
      font-weight: 800;
      background: linear-gradient(135deg, #e2e8f0, #c084fc);
      background-clip: text;
      -webkit-background-clip: text;
      color: transparent;
    }

    footer {
      text-align: center;
      margin-top: 3rem;
    }

    @keyframes floatSoft {
      0% { transform: translateY(0px); }
      100% { transform: translateY(-6px); }
    }

    .float-soft {
      animation: floatSoft 3s ease-in-out infinite alternate;
    }

    canvas#radarCanvas {
      max-height: 250px;
      width: 100%;
    }

    .grid-responsive {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 1.8rem;
      margin: 2rem 0;
    }
  </style>
</head>
<body>
<div class="orb-bg"></div>
<div class="noise-overlay"></div>

<div class="container">
  <!-- HERO SECTION : cinematic -->
  <div class="glass-premium" style="padding: 2rem 1.8rem; margin-bottom: 2rem; text-align: center; position: relative; overflow: hidden;">
    <div class="float-soft" style="margin-bottom: 0.5rem;">
      <i class="fas fa-terminal" style="font-size: 3rem; background: linear-gradient(145deg, #a5b4fc, #c084fc); background-clip: text; -webkit-background-clip: text; color: transparent;"></i>
    </div>
    <h1 style="font-size: 4.5rem; font-weight: 800; letter-spacing: -0.02em;"><span class="gradient-text">INDERASH</span></h1>
    <div style="height: 0.6rem;"></div>
    <p style="font-size: 1.25rem; max-width: 750px; margin: 0 auto; color: #cbd5f0;">
      <i class="fas fa-crown"></i> Full Stack Architect · AI Enthusiast · MERN Artisan
    </p>
    <div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 0.6rem; margin-top: 1.2rem;">
      <span class="badge-glow"><i class="fas fa-graduation-cap"></i> B.Sc CS & Data Analytics</span>
      <span class="badge-glow"><i class="fas fa-brain"></i> AI Project Builder</span>
      <span class="badge-glow"><i class="fab fa-react"></i> React & Node.js</span>
    </div>
    <div style="margin-top: 1.5rem;">
      <div id="dynamicLine" style="font-size: 1rem; background: #0f172a80; display: inline-block; padding: 0.4rem 1.6rem; border-radius: 80px;"></div>
    </div>
  </div>

  <!-- ABOUT + CONNECT hybrid -->
  <div style="display: flex; flex-wrap: wrap; gap: 1.8rem; margin-bottom: 2rem;">
    <div class="glass-premium" style="flex: 2; padding: 1.8rem;">
      <h3><i class="fas fa-user-astronaut gradient-text"></i> <span class="gradient-text">✦ Nexus Biography</span></h3>
      <p style="margin-top: 0.8rem; line-height: 1.5;">🎓 Data Science & CS student with a relentless drive for full-stack innovation. I craft AI-infused platforms, real-time tracking systems, and high-performance portals. Currently deep-diving into MERN architecture and building <strong class="gradient-text">CampusFinder AI</strong> — intelligent lost & found ecosystem.</p>
      <div style="margin-top: 1.3rem; display: flex; flex-wrap: wrap; gap: 0.5rem;">
        <span class="badge-glow"><i class="fab fa-python"></i> Python 3.12</span>
        <span class="badge-glow"><i class="fab fa-js"></i> ES2022+</span>
        <span class="badge-glow"><i class="fas fa-database"></i> PostgreSQL / MongoDB</span>
        <span class="badge-glow"><i class="fab fa-docker"></i> Docker basics</span>
      </div>
    </div>
    <div class="glass-premium" style="flex: 1.2; padding: 1.8rem; text-align: center;">
      <h3><i class="fas fa-paper-plane gradient-text"></i> Digital HQ</h3>
      <div style="display: flex; justify-content: center; gap: 1.5rem; margin: 1rem 0;">
        <a href="https://github.com/inderash18" target="_blank" style="color: #eef2ff; font-size: 2rem; transition: 0.2s;"><i class="fab fa-github"></i></a>
        <a href="#" style="color: #eef2ff; font-size: 2rem;"><i class="fab fa-linkedin-in"></i></a>
        <a href="#" style="color: #eef2ff; font-size: 2rem;"><i class="fas fa-briefcase"></i></a>
      </div>
      <div class="btn-outline-saturn" style="display: inline-block; cursor: default;">
        <i class="far fa-envelope"></i> inderash@devstack.space
      </div>
      <div style="margin-top: 1rem;">
        <i class="fas fa-map-pin"></i> based in innovation hub
      </div>
    </div>
  </div>

  <!-- TECH STACK MEGA GRID -->
  <div class="glass-premium" style="padding: 1.5rem; margin-bottom: 2rem;">
    <h3 class="gradient-text" style="font-size: 1.8rem;"><i class="fas fa-microchip"></i> Arsenal · Tech Spectrum</h3>
    <div style="display: flex; justify-content: center; margin-top: 1rem; flex-wrap: wrap;">
      <img src="https://skillicons.dev/icons?i=html,css,js,react,nodejs,python,flask,mongodb,mysql,sqlite,java,git,github,vscode,tailwind,bootstrap,linux" alt="tech stack icons" style="max-width: 100%; filter: drop-shadow(0 6px 12px rgba(0,0,0,0.4));">
    </div>
  </div>

  <!-- PROJECT SHOWCASE + PROGRESS RADAR -->
  <div class="grid-responsive">
    <div class="glass-premium" style="padding: 1.5rem;">
      <h3><i class="fas fa-rocket gradient-text"></i> Flagship Engineering</h3>
      <div style="display: flex; flex-direction: column; gap: 0.9rem; margin-top: 1.2rem;">
        <div class="project-tile"><i class="fas fa-robot" style="color: #a78bfa;"></i> <strong>🤖 CampusFinder AI</strong> — LLM-powered lost & found</div>
        <div class="project-tile"><i class="fas fa-map-marked-alt" style="color: #a78bfa;"></i> <strong>🚌 College Bus Tracker</strong> — GPS / realtime fleet</div>
        <div class="project-tile"><i class="fas fa-university" style="color: #a78bfa;"></i> <strong>🏫 Portal System</strong> — RBAC, analytics dashboard</div>
        <div class="project-tile"><i class="fas fa-chart-line" style="color: #a78bfa;"></i> <strong>📊 AI Sentiment Analyzer</strong> — student feedback NLP</div>
        <div class="project-tile"><i class="fas fa-fingerprint" style="color: #a78bfa;"></i> <strong>🗳️ Digital Voting Machine</strong> — biometric verification</div>
      </div>
    </div>
    <div class="glass-premium" style="padding: 1.5rem;">
      <h3><i class="fas fa-chart-line gradient-text"></i> Mastery Roadmap</h3>
      <div style="margin-top: 0.8rem;">
        <div><span>Frontend Dev</span><span style="float: right;">85%</span><div class="progress-bar mt-1"><div class="progress-fill" data-progress="85"></div></div></div>
        <div class="mt-2"><span>JavaScript/TS</span><span style="float: right;">78%</span><div class="progress-bar"><div class="progress-fill" data-progress="78"></div></div></div>
        <div class="mt-2"><span>React.js</span><span style="float: right;">50%</span><div class="progress-bar"><div class="progress-fill" data-progress="50"></div></div></div>
        <div class="mt-2"><span>Backend (Node/Express)</span><span style="float: right;">70%</span><div class="progress-bar"><div class="progress-fill" data-progress="70"></div></div></div>
        <div class="mt-2"><span>Python & Flask</span><span style="float: right;">90%</span><div class="progress-bar"><div class="progress-fill" data-progress="90"></div></div></div>
        <div class="mt-2"><span>SQL / DB design</span><span style="float: right;">85%</span><div class="progress-bar"><div class="progress-fill" data-progress="85"></div></div></div>
        <div class="mt-2"><span>MongoDB</span><span style="float: right;">55%</span><div class="progress-bar"><div class="progress-fill" data-progress="55"></div></div></div>
        <div class="mt-2"><span>MERN Stack</span><span style="float: right;">45%</span><div class="progress-bar"><div class="progress-fill" data-progress="45"></div></div></div>
      </div>
    </div>
    <div class="glass-premium" style="padding: 1.5rem;">
      <h3><i class="fas fa-chart-pie gradient-text"></i> Skill Constellation</h3>
      <canvas id="radarCanvas" width="400" height="260" style="width:100%; max-width:280px; margin: 0 auto; display: block;"></canvas>
    </div>
  </div>

  <!-- GITHUB STATS + TROPHIES (Hyper interactive) -->
  <div style="display: flex; flex-wrap: wrap; gap: 1.5rem; margin: 2rem 0;">
    <div class="glass-premium" style="flex: 1; padding: 1.5rem; text-align: center;">
      <h3><i class="fab fa-github gradient-text"></i> Dev Metrics</h3>
      <div style="display: flex; justify-content: space-evenly; margin: 1rem 0;">
        <div><div class="stats-number" id="repoCounterAnim">14</div><span>repositories</span></div>
        <div><div class="stats-number">187</div><span>contributions</span></div>
        <div><div class="stats-number">5</div><span>active projects</span></div>
      </div>
      <img src="https://github-readme-stats.vercel.app/api?username=inderash18&show_icons=true&theme=tokyonight&hide_border=true&bg_color=00000000&text_color=cbd5e6&icon_color=c084fc" width="100%" style="border-radius: 1rem;" alt="github stats">
    </div>
    <div class="glass-premium" style="flex: 1; padding: 1.5rem; text-align: center;">
      <h3><i class="fas fa-award gradient-text"></i> Glory & Trophies</h3>
      <div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 0.7rem; margin: 1rem 0;">
        <span class="badge-glow"><i class="fas fa-medal"></i> AI Hackathon Finalist</span>
        <span class="badge-glow"><i class="fas fa-code-branch"></i> Open Source Contributor</span>
        <span class="badge-glow"><i class="fas fa-database"></i> Database Design Award</span>
      </div>
      <img src="https://github-profile-trophy.vercel.app/?username=inderash18&theme=tokyonight&no-frame=true&margin-w=10&row=1&column=4" style="max-width: 100%; border-radius: 1rem; margin-top: 0.5rem;" alt="trophies">
    </div>
  </div>

  <!-- CONTRIBUTION GRAPH + CREATIVE FOOTER -->
  <div class="glass-premium" style="padding: 1.5rem; margin: 1rem 0;">
    <h3><i class="fas fa-chart-line gradient-text"></i> Contribution Galaxy</h3>
    <div style="margin-top: 1rem; border-radius: 1.5rem; overflow: hidden;">
      <img src="https://github-readme-activity-graph.vercel.app/graph?username=inderash18&theme=tokyo-night&hide_border=true&bg_color=050a15&color=a78bfa&line=667eea&point=c084fc" width="100%" alt="activity graph">
    </div>
  </div>

  <!-- PROFILE VIEWS + TIMESTAMP -->
  <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 1rem; margin-top: 1.5rem;">
    <div class="glass-premium" style="padding: 0.6rem 1.2rem;">
      <i class="fas fa-eye"></i> <strong>interstellar views</strong> <span id="viewCount">2,936</span>
    </div>
    <div class="glass-premium" style="padding: 0.6rem 1.2rem;">
      <i class="fas fa-sync-alt"></i> “code, learn, deploy — repeat”
    </div>
    <div>
      <i class="far fa-calendar-alt"></i> 2026 frontier
    </div>
  </div>

  <footer>
    <p style="opacity: 0.7;">⚡ next-level readme | built with cosmic ui & mern spirit ⚡</p>
    <div style="margin-top: 1.2rem;">
      <img src="https://capsule-render.vercel.app/api?type=waving&height=100&section=footer&color=0:667eea,100:764ba2" width="100%" style="border-radius: 20px;">
    </div>
  </footer>
</div>

<script>
  // Animated typing for dynamicLine
  const dynamicContainer = document.getElementById('dynamicLine');
  const roles = ["🚀 Full Stack Developer", "🤖 AI Project Builder", "💻 MERN Stack Learner", "✨ Real‑world Solution Architect"];
  let roleIndex = 0, charIndex = 0, isDeletingFlag = false;
  function animateTyping() {
    const currentRole = roles[roleIndex];
    if (!isDeletingFlag) {
      dynamicContainer.innerHTML = currentRole.substring(0, charIndex+1);
      charIndex++;
      if (charIndex === currentRole.length) {
        isDeletingFlag = true;
        setTimeout(() => {}, 1400);
      }
    } else {
      dynamicContainer.innerHTML = currentRole.substring(0, charIndex-1);
      charIndex--;
      if (charIndex === 0) {
        isDeletingFlag = false;
        roleIndex = (roleIndex + 1) % roles.length;
      }
    }
    let speed = isDeletingFlag ? 50 : 100;
    if (!isDeletingFlag && charIndex === currentRole.length) speed = 1800;
    if (isDeletingFlag && charIndex === 0) speed = 320;
    setTimeout(animateTyping, speed);
  }
  animateTyping();

  // Radar Chart: advanced skill visualization
  const radarCtx = document.getElementById('radarCanvas').getContext('2d');
  new Chart(radarCtx, {
    type: 'radar',
    data: {
      labels: ['React / Next', 'Node.js', 'Python', 'MongoDB', 'SQL', 'System Design', 'AI/ML Basics'],
      datasets: [{
        label: 'Proficiency',
        data: [52, 68, 92, 48, 86, 71, 69],
        backgroundColor: 'rgba(139, 92, 246, 0.3)',
        borderColor: '#c084fc',
        borderWidth: 2,
        pointBackgroundColor: '#a78bfa',
        pointBorderColor: '#ffffff',
        pointRadius: 4,
        pointHoverRadius: 6
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      scales: { r: { beginAtZero: true, max: 100, ticks: { stepSize: 25, color: '#b9c7dd' }, grid: { color: '#2d3a5e' }, pointLabels: { color: '#cbd5e6', font: { size: 11 } } } },
      plugins: { legend: { labels: { color: '#e2e8f0', font: { weight: 'bold' } } } }
    }
  });

  // Animate progress fills on load
  const progressFills = document.querySelectorAll('.progress-fill');
  progressFills.forEach(fill => {
    const target = fill.getAttribute('data-progress');
    if (target) {
      setTimeout(() => {
        fill.style.width = target + '%';
      }, 200);
    }
  });

  // Fake view counter + repo counter animation (enhanced)
  let viewVal = 2936;
  const viewSpan = document.getElementById('viewCount');
  function simulateViews() {
    const increment = Math.floor(Math.random() * 13) + 1;
    viewVal += increment;
    gsap.to({val: parseInt(viewSpan.innerText)}, {
      val: viewVal,
      duration: 1.2,
      ease: "power2.out",
      onUpdate: function() { viewSpan.innerText = Math.floor(this.targets()[0].val); }
    });
    setTimeout(simulateViews, 34000);
  }
  simulateViews();

  // dynamic repo count animation
  let repoCounter = 14;
  const repoElement = document.getElementById('repoCounterAnim');
  setInterval(() => {
    let newRepo = repoCounter + Math.floor(Math.random() * 2);
    if (newRepo > 22) newRepo = 15;
    repoCounter = newRepo;
    gsap.to({val: parseInt(repoElement.innerText)}, {
      val: repoCounter,
      duration: 0.9,
      ease: "back.out",
      onUpdate: function() { repoElement.innerText = Math.floor(this.targets()[0].val); }
    });
  }, 28000);

  // ScrollTrigger reveals (GSAP)
  gsap.registerPlugin(ScrollTrigger);
  gsap.utils.toArray('.glass-premium').forEach((card, i) => {
    gsap.from(card, {
      scrollTrigger: {
        trigger: card,
        start: "top 85%",
        toggleActions: "play none none reverse"
      },
      opacity: 0,
      y: 35,
      duration: 0.7,
      delay: i * 0.05,
      ease: "power2.out"
    });
  });
</script>
</body>
</html>
