<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Inderash | Next-Gen Dev Portfolio</title>
    <!-- Google Fonts & Font Awesome -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700;14..32,800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Anime.js for smooth animations -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
    <!-- Chart.js for skill radar / dynamic graphs -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: radial-gradient(circle at 10% 30%, #0a0f1e, #03050b);
            font-family: 'Inter', sans-serif;
            color: #eef5ff;
            line-height: 1.5;
            overflow-x: hidden;
        }

        /* animated grain texture overlay */
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='1' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
            pointer-events: none;
            z-index: 1;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 2rem 2rem 4rem;
            position: relative;
            z-index: 2;
        }

        /* custom glassmorphic cards */
        .glass-card {
            background: rgba(15, 25, 45, 0.55);
            backdrop-filter: blur(12px);
            border-radius: 2rem;
            border: 1px solid rgba(102, 126, 234, 0.35);
            box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.5);
            transition: all 0.3s ease;
        }

        .glass-card:hover {
            border-color: rgba(118, 75, 162, 0.7);
            box-shadow: 0 25px 40px -14px rgba(102, 126, 234, 0.3);
        }

        /* gradient text */
        .gradient-text {
            background: linear-gradient(135deg, #a5b4fc, #c084fc, #f0abfc);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            font-weight: 800;
        }

        /* header wave animation */
        .wave-header {
            position: relative;
            overflow: hidden;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.8rem;
            margin: 2.5rem 0;
        }

        .skill-badge {
            background: rgba(45, 55, 85, 0.6);
            border-radius: 2rem;
            padding: 0.4rem 1rem;
            font-size: 0.85rem;
            font-weight: 500;
            backdrop-filter: blur(4px);
            transition: all 0.2s;
        }

        .skill-badge i {
            margin-right: 8px;
            color: #a78bfa;
        }

        .project-card {
            background: rgba(12, 20, 35, 0.7);
            border-left: 4px solid #667eea;
            padding: 1.2rem;
            border-radius: 1.2rem;
            transition: transform 0.2s, background 0.2s;
        }

        .project-card:hover {
            transform: translateY(-5px);
            background: rgba(25, 35, 60, 0.85);
            border-left-color: #c084fc;
        }

        .progress-bar-bg {
            background: #1e293b;
            border-radius: 12px;
            overflow: hidden;
            height: 12px;
        }

        .progress-fill {
            background: linear-gradient(90deg, #667eea, #c084fc);
            width: 0%;
            height: 100%;
            border-radius: 12px;
        }

        .trophy-grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 1.2rem;
        }

        .trophy-item {
            background: #0f172ad9;
            padding: 0.6rem 1.2rem;
            border-radius: 40px;
            font-size: 0.9rem;
            font-weight: 500;
            backdrop-filter: blur(4px);
            border: 1px solid #334155;
        }

        .btn-outline-glow {
            border: 1px solid #8b5cf6;
            background: transparent;
            padding: 0.6rem 1.5rem;
            border-radius: 40px;
            font-weight: 600;
            transition: all 0.2s;
            color: #c4b5fd;
        }

        .btn-outline-glow:hover {
            background: #8b5cf620;
            box-shadow: 0 0 12px #a78bfa;
            color: white;
        }

        @keyframes float {
            0% { transform: translateY(0px); }
            100% { transform: translateY(-8px); }
        }

        .floating-icon {
            animation: float 3s ease-in-out infinite alternate;
        }

        footer {
            text-align: center;
            margin-top: 4rem;
        }

        canvas#skillRadar {
            max-height: 260px;
            width: 100%;
        }

        .contribution-graph-placeholder {
            background: #050a15;
            border-radius: 1.5rem;
            padding: 1rem;
            text-align: center;
        }

        @media (max-width: 700px) {
            .container {
                padding: 1rem;
            }
            h1 {
                font-size: 2.2rem;
            }
        }
    </style>
</head>
<body>

<div class="container">
    <!-- Enhanced Hero with dynamic wave -->
    <div class="wave-header glass-card" style="padding: 2rem 1.5rem; margin-bottom: 1rem; text-align: center; position: relative; overflow: hidden;">
        <div style="position: absolute; top: -50%; left: -20%; width: 140%; height: 200%; background: radial-gradient(ellipse at 30% 40%, rgba(102,126,234,0.2), transparent); pointer-events: none;"></div>
        <div class="floating-icon" style="margin-bottom: 1rem;">
            <i class="fas fa-code" style="font-size: 3rem; background: linear-gradient(145deg, #b794f4, #6b46c0); background-clip: text; -webkit-background-clip: text; color: transparent;"></i>
        </div>
        <h1 style="font-size: 4rem; font-weight: 800; letter-spacing: -0.02em;">INDERASH<span style="font-weight: 400; font-size: 2rem;">.</span></h1>
        <div style="height: 8px;"></div>
        <p style="font-size: 1.2rem; max-width: 700px; margin: 0 auto; color: #cbd5e6;">
            <i class="fas fa-rocket"></i> Full Stack Architect &nbsp;|&nbsp; 
            <i class="fas fa-brain"></i> AI Enthusiast &nbsp;|&nbsp;
            <i class="fab fa-react"></i> MERN Maverick
        </p>
        <div style="margin-top: 1rem;">
            <span class="skill-badge"><i class="fas fa-map-marker-alt"></i> B.Sc CS & Data Analytics</span>
            <span class="skill-badge"><i class="fas fa-lightbulb"></i> Real-world builder</span>
        </div>
        <div style="margin-top: 1.2rem;">
            <span id="dynamic-typing" style="font-weight: 500; background: #0f172a80; padding: 0.3rem 1.2rem; border-radius: 40px; font-size: 1rem;"></span>
        </div>
    </div>

    <!-- About me + Connect section in modern flex -->
    <div style="display: flex; flex-wrap: wrap; gap: 1.5rem; justify-content: space-between; margin: 2rem 0;">
        <div class="glass-card" style="flex: 2; padding: 1.8rem;">
            <h3><i class="fas fa-user-astronaut gradient-text"></i> <span class="gradient-text">About Me</span></h3>
            <p style="margin-top: 0.8rem;">🎓 B.Sc Computer Science & Data Analytics Student · 💻 Passionate about Full Stack & AI · 🤖 Currently building <strong>CampusFinder AI</strong> & MERN ecosystems. I transform ideas into robust digital solutions with clean code and user-first mindset.</p>
            <div style="margin-top: 1rem; display: flex; flex-wrap: wrap; gap: 0.6rem;">
                <span class="skill-badge"><i class="fab fa-python"></i> Python 3.x</span>
                <span class="skill-badge"><i class="fab fa-js"></i> Modern JS</span>
                <span class="skill-badge"><i class="fab fa-react"></i> React 18+</span>
                <span class="skill-badge"><i class="fas fa-database"></i> SQL/NoSQL</span>
            </div>
        </div>
        <div class="glass-card" style="flex: 1.2; padding: 1.8rem; text-align: center;">
            <h3><i class="fas fa-hand-peace"></i> Connect</h3>
            <div style="display: flex; justify-content: center; gap: 1.2rem; margin-top: 1rem; flex-wrap: wrap;">
                <a href="https://github.com/inderash18" target="_blank" style="color: #e2e8f0; font-size: 2rem; transition: 0.2s;"><i class="fab fa-github"></i></a>
                <a href="#" style="color: #e2e8f0; font-size: 2rem; transition: 0.2s;"><i class="fab fa-linkedin"></i></a>
                <a href="#" style="color: #e2e8f0; font-size: 2rem; transition: 0.2s;"><i class="fas fa-globe"></i></a>
                <a href="#" style="color: #e2e8f0; font-size: 2rem; transition: 0.2s;"><i class="fab fa-twitter"></i></a>
            </div>
            <div class="btn-outline-glow" style="margin-top: 1.2rem; display: inline-block; cursor: default;">
                <i class="far fa-envelope"></i> inderash@dev.me
            </div>
        </div>
    </div>

    <!-- Tech Stack Master Section with Icons Grid -->
    <div class="glass-card" style="padding: 1.5rem; margin: 1rem 0;">
        <h3 class="gradient-text" style="font-size: 1.7rem;"><i class="fas fa-cogs"></i>  Tech Armory</h3>
        <div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 1.5rem; margin-top: 1rem;">
            <img src="https://skillicons.dev/icons?i=html,css,js,react,nodejs,python,flask,mongodb,mysql,sqlite,java,git,github,vscode,tailwind,bootstrap,linux" alt="skills" style="max-width: 100%; filter: drop-shadow(0 0 5px #667eea70);"/>
        </div>
    </div>

    <!-- Featured Projects + Learning Radar + Progress -->
    <div class="stats-grid">
        <div class="glass-card" style="padding: 1.5rem;">
            <h3><i class="fas fa-star-of-life gradient-text"></i> Signature Projects</h3>
            <div style="margin-top: 1rem; display: grid; gap: 1rem;">
                <div class="project-card"><i class="fas fa-robot" style="color: #a78bfa;"></i> <strong>🤖 CampusFinder AI</strong> — AI Powered Lost & Found System</div>
                <div class="project-card"><i class="fas fa-bus" style="color: #a78bfa;"></i> <strong>🚌 College Bus Tracking System</strong> — Real-Time GPS Tracking</div>
                <div class="project-card"><i class="fas fa-university" style="color: #a78bfa;"></i> <strong>🏫 College Portal System</strong> — Role-Based Dashboard</div>
                <div class="project-card"><i class="fas fa-chart-line" style="color: #a78bfa;"></i> <strong>📊 AI Sentiment Analyzer</strong> — Student Feedback Analysis</div>
                <div class="project-card"><i class="fas fa-fingerprint" style="color: #a78bfa;"></i> <strong>🗳️ Digital Voting Machine</strong> — Fingerprint Verification</div>
            </div>
        </div>
        <div class="glass-card" style="padding: 1.5rem;">
            <h3><i class="fas fa-chart-simple gradient-text"></i> Learning Progress (MERN Focus)</h3>
            <div style="margin-top: 1rem;">
                <div><span>Frontend Dev</span><span style="float: right;">85%</span><div class="progress-bar-bg mt-1"><div class="progress-fill" style="width:85%"></div></div></div>
                <div class="mt-2"><span>JavaScript</span><span style="float: right;">75%</span><div class="progress-bar-bg"><div class="progress-fill" style="width:75%"></div></div></div>
                <div class="mt-2"><span>React.js</span><span style="float: right;">50%</span><div class="progress-bar-bg"><div class="progress-fill" style="width:50%"></div></div></div>
                <div class="mt-2"><span>Backend (Node/Express)</span><span style="float: right;">70%</span><div class="progress-bar-bg"><div class="progress-fill" style="width:70%"></div></div></div>
                <div class="mt-2"><span>Python</span><span style="float: right;">90%</span><div class="progress-bar-bg"><div class="progress-fill" style="width:90%"></div></div></div>
                <div class="mt-2"><span>SQL</span><span style="float: right;">85%</span><div class="progress-bar-bg"><div class="progress-fill" style="width:85%"></div></div></div>
                <div class="mt-2"><span>MongoDB</span><span style="float: right;">50%</span><div class="progress-bar-bg"><div class="progress-fill" style="width:50%"></div></div></div>
                <div class="mt-2"><span>MERN Stack Integration</span><span style="float: right;">40%</span><div class="progress-bar-bg"><div class="progress-fill" style="width:40%"></div></div></div>
            </div>
        </div>
        <div class="glass-card" style="padding: 1.5rem;">
            <h3><i class="fas fa-chart-pie gradient-text"></i> Skill Radar</h3>
            <canvas id="skillRadar" width="300" height="250" style="width:100%; max-height:230px;"></canvas>
        </div>
    </div>

    <!-- GitHub Stats Animated & Trophies Fusion -->
    <div style="display: flex; flex-wrap: wrap; gap: 1.5rem; margin: 2rem 0;">
        <div class="glass-card" style="flex: 1; padding: 1.5rem; text-align: center;">
            <h3><i class="fab fa-github-alt gradient-text"></i> GitHub Pulse</h3>
            <div style="display: flex; justify-content: space-around; flex-wrap: wrap; gap: 1rem; margin-top: 1rem;">
                <div><i class="fas fa-code-branch"></i> <strong id="repo-count">12</strong><br>Repos</div>
                <div><i class="fas fa-star"></i> <strong>148</strong><br>Stars equivalent</div>
                <div><i class="fas fa-users"></i> <strong>9</strong><br>Contributors vibe</div>
            </div>
            <div style="margin-top: 1rem;">
                <img src="https://github-readme-stats.vercel.app/api?username=inderash18&show_icons=true&theme=tokyonight&hide_border=true&bg_color=00000000&text_color=cbd5e6&icon_color=8b5cf6" width="100%" alt="stats">
            </div>
        </div>
        <div class="glass-card" style="flex: 1; padding: 1.5rem; text-align: center;">
            <h3><i class="fas fa-trophy gradient-text"></i> Achievements</h3>
            <div class="trophy-grid" style="margin: 1rem 0;">
                <span class="trophy-item"><i class="fas fa-medal"></i> PR Pioneer</span>
                <span class="trophy-item"><i class="fas fa-brain"></i> AI Hackathon</span>
                <span class="trophy-item"><i class="fas fa-database"></i> DB Architect</span>
                <span class="trophy-item"><i class="fas fa-fire"></i> 6+ Projects Live</span>
                <span class="trophy-item"><i class="fas fa-code"></i> Clean Code Addict</span>
            </div>
            <img src="https://github-profile-trophy.vercel.app/?username=inderash18&theme=tokyonight&no-frame=true&margin-w=8&row=1&column=4" style="max-width: 100%; border-radius: 1rem; margin-top: 0.8rem;" alt="trophies">
        </div>
    </div>

    <!-- Contribution Graph Alternative: interactive heatmap style -->
    <div class="glass-card" style="padding: 1.5rem; margin: 1rem 0;">
        <h3><i class="fas fa-chart-line gradient-text"></i> Contribution Chronicle</h3>
        <div class="contribution-graph-placeholder" style="margin-top: 1rem;">
            <img src="https://github-readme-activity-graph.vercel.app/graph?username=inderash18&theme=tokyo-night&hide_border=true&bg_color=050a15&color=a78bfa&line=667eea&point=c084fc" width="100%" alt="contribution-graph" style="border-radius: 1rem;">
        </div>
    </div>

    <!-- profile views & extra flair -->
    <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; margin-top: 2rem; gap: 1rem;">
        <div class="glass-card" style="padding: 0.7rem 1.5rem;">
            <i class="fas fa-eye"></i> PROFILE EXPLORATIONS: <strong id="viewCounter">2,847</strong> <span style="font-size:0.8rem;">+ interstellar</span>
        </div>
        <div class="glass-card" style="padding: 0.7rem 1.5rem;">
            <i class="fas fa-mug-hot"></i> "Code. Build. Innovate."
        </div>
        <div>
            <i class="fas fa-calendar-alt"></i> 2026 — pushing boundaries
        </div>
    </div>

    <footer>
        <p style="opacity: 0.7;">⭐ Crafted with Next-Gen Aesthetics & MERN Spirit ⭐</p>
        <div style="margin-top: 1rem;">
            <img src="https://capsule-render.vercel.app/api?type=waving&height=80&section=footer&color=0:667eea,100:764ba2&fontColor=ffffff" width="100%" style="border-radius: 20px;">
        </div>
    </footer>
</div>

<script>
    // Dynamic typing effect 
    const phrases = [
        "Full Stack Developer 🚀",
        "AI Project Builder 🤖",
        "MERN Stack Learner 💻",
        "Real‑World Problem Solver 🎯"
    ];
    let idx = 0;
    let charIdx = 0;
    let currentText = "";
    let isDeleting = false;
    const typingSpan = document.getElementById("dynamic-typing");

    function typeEffect() {
        const fullText = phrases[idx];
        if (!isDeleting) {
            currentText = fullText.substring(0, charIdx + 1);
            charIdx++;
            if (charIdx === fullText.length) {
                isDeleting = true;
                setTimeout(() => {}, 1500);
            }
        } else {
            currentText = fullText.substring(0, charIdx - 1);
            charIdx--;
            if (charIdx === 0) {
                isDeleting = false;
                idx = (idx + 1) % phrases.length;
            }
        }
        typingSpan.textContent = currentText;
        let speed = isDeleting ? 60 : 100;
        if (!isDeleting && charIdx === fullText.length) speed = 1800;
        if (isDeleting && charIdx === 0) speed = 350;
        setTimeout(typeEffect, speed);
    }
    typeEffect();

    // Skill Radar Chart (Interactive)
    const ctx = document.getElementById('skillRadar').getContext('2d');
    new Chart(ctx, {
        type: 'radar',
        data: {
            labels: ['React', 'Node.js', 'Python', 'MongoDB', 'SQL', 'JS/TS', 'UI/UX Sense'],
            datasets: [{
                label: 'Proficiency %',
                data: [50, 68, 90, 48, 85, 76, 72],
                backgroundColor: 'rgba(102, 126, 234, 0.35)',
                borderColor: '#c084fc',
                borderWidth: 2,
                pointBackgroundColor: '#a78bfa',
                pointBorderColor: '#fff',
                pointRadius: 4,
                pointHoverRadius: 6,
                tension: 0.1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: true,
            scales: {
                r: {
                    beginAtZero: true,
                    max: 100,
                    ticks: { stepSize: 20, color: '#b9c7dd', backdropColor: 'transparent' },
                    grid: { color: '#2d3a5e', circular: true },
                    angleLines: { color: '#2d3a5e' },
                    pointLabels: { color: '#cbd5e1', font: { size: 11 } }
                }
            },
            plugins: { legend: { labels: { color: '#e2e8f0', font: { weight: 'bold' } } } }
        }
    });

    // Animate progress bars on load
    const progressFills = document.querySelectorAll('.progress-fill');
    progressFills.forEach(fill => {
        const width = fill.style.width;
        fill.style.width = '0%';
        setTimeout(() => {
            fill.style.width = width;
            fill.style.transition = 'width 0.9s cubic-bezier(0.2, 0.9, 0.4, 1.1)';
        }, 100);
    });

    // Counter animation for view count
    const viewSpan = document.getElementById('viewCounter');
    let currentViews = 2847;
    function animateViews() {
        let target = currentViews + Math.floor(Math.random() * 17);
        anime({
            targets: { val: currentViews },
            val: target,
            duration: 1400,
            easing: 'easeOutQuad',
            update: function(anim) {
                viewSpan.innerText = Math.floor(anim.animations[0].currentValue);
            },
            complete: () => { currentViews = target; setTimeout(animateViews, 30000); }
        });
    }
    animateViews();

    // Smooth floating effect on icons & hover animations on cards
    const cards = document.querySelectorAll('.glass-card, .project-card');
    cards.forEach(card => {
        card.addEventListener('mouseenter', (e) => {
            card.style.transition = 'transform 0.2s, border-color 0.2s, box-shadow 0.2s';
            card.style.transform = 'translateY(-3px)';
        });
        card.addEventListener('mouseleave', () => {
            card.style.transform = 'translateY(0px)';
        });
    });

    // Simulate GitHub repo count animation (just for visual flair)
    const repoElem = document.getElementById('repo-count');
    let repoCount = 12;
    setInterval(() => {
        let delta = Math.floor(Math.random() * 3);
        repoCount = Math.min(25, repoCount + delta);
        anime({
            targets: { val: parseInt(repoElem.innerText) },
            val: repoCount,
            duration: 600,
            easing: 'spring',
            update: (anim) => { repoElem.innerText = Math.floor(anim.animations[0].currentValue); }
        });
    }, 24000);

    // dynamic background particle effect (light)
    const style = document.createElement('style');
    style.textContent = ` .mt-1 { margin-top: 0.5rem; } .mt-2 { margin-top: 0.9rem; } `;
    document.head.appendChild(style);
</script>

</body>
</html>
