<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PLAYER 1: ANTONIO // SYSTEM INTERFACE</title>
    
    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=VT323&display=swap" rel="stylesheet">
    
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        :root {
            --neon-green: #39FF14;
            --neon-cyan: #00F3FF;
            --neon-pink: #FF00FF;
            --dark-bg: #050505;
            --panel-bg: rgba(10, 15, 10, 0.85);
            --border-color: #333;
            --font-main: 'Orbitron', sans-serif;
            --font-mono: 'VT323', monospace;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--dark-bg);
            color: var(--neon-green);
            font-family: var(--font-main);
            overflow-x: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px; /* For 3D effects */
        }

        /* --- Background Animation Canvas --- */
        #neural-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
        }

        /* --- Scanline Overlay --- */
        .scanlines {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(
                to bottom,
                rgba(255,255,255,0),
                rgba(255,255,255,0) 50%,
                rgba(0,0,0,0.2) 50%,
                rgba(0,0,0,0.2)
            );
            background-size: 100% 4px;
            pointer-events: none;
            z-index: 100;
            opacity: 0.6;
        }

        /* --- Boot Screen --- */
        #boot-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--dark-bg);
            z-index: 200;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-family: var(--font-mono);
            transition: opacity 1s ease-out;
        }

        .boot-text {
            font-size: 1.5rem;
            color: var(--neon-green);
            text-shadow: 0 0 5px var(--neon-green);
        }

        .progress-bar {
            width: 300px;
            height: 20px;
            border: 2px solid var(--neon-green);
            margin-top: 20px;
            position: relative;
        }

        .progress-fill {
            height: 100%;
            background-color: var(--neon-green);
            width: 0%;
            transition: width 0.1s linear;
            box-shadow: 0 0 10px var(--neon-green);
        }

        /* --- Main Container (HUD) --- */
        .hud-container {
            width: 90%;
            max-width: 1200px;
            max-height: 95vh;
            background: var(--panel-bg);
            border: 1px solid var(--neon-green);
            box-shadow: 0 0 20px rgba(57, 255, 20, 0.2);
            display: grid;
            grid-template-rows: auto auto 1fr auto;
            gap: 15px;
            padding: 20px;
            position: relative;
            backdrop-filter: blur(5px);
            opacity: 0; /* Hidden initially until boot finishes */
            transform: rotateX(5deg);
            transition: all 0.5s ease;
            overflow-y: auto;
        }

        /* Custom Scrollbar */
        .hud-container::-webkit-scrollbar {
            width: 8px;
        }
        .hud-container::-webkit-scrollbar-track {
            background: #000;
        }
        .hud-container::-webkit-scrollbar-thumb {
            background: var(--neon-green);
            border-radius: 4px;
        }

        /* --- Header Section --- */
        .hud-header {
            text-align: center;
            border-bottom: 1px solid var(--neon-green);
            padding-bottom: 15px;
        }

        .header-gif-container {
            width: 100%;
            height: 200px;
            overflow: hidden;
            border: 2px solid var(--neon-cyan);
            position: relative;
            margin-bottom: 15px;
        }

        .header-gif-container img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            opacity: 0.8;
        }

        .player-title {
            font-size: 2.5rem;
            text-transform: uppercase;
            letter-spacing: 4px;
            text-shadow: 2px 2px 0px var(--neon-cyan);
            margin-bottom: 10px;
            position: relative;
            display: inline-block;
        }

        .player-title::after {
            content: '';
            display: block;
            width: 100%;
            height: 2px;
            background: var(--neon-green);
            box-shadow: 0 0 10px var(--neon-green);
            margin-top: 5px;
        }

        .typing-container {
            font-family: var(--font-mono);
            font-size: 1.5rem;
            color: var(--neon-cyan);
            min-height: 1.5em;
        }

        /* --- Main Grid Layout --- */
        .hud-body {
            display: grid;
            grid-template-columns: 1fr 2fr;
            gap: 20px;
        }

        @media (max-width: 768px) {
            .hud-body {
                grid-template-columns: 1fr;
            }
        }

        /* --- Panel Styling --- */
        .panel {
            border: 1px solid var(--border-color);
            background: rgba(0,0,0,0.6);
            padding: 15px;
            position: relative;
            clip-path: polygon(
                0 0, 
                100% 0, 
                100% calc(100% - 20px), 
                calc(100% - 20px) 100%, 
                0 100%
            );
            transition: all 0.3s ease;
        }

        .panel:hover {
            border-color: var(--neon-cyan);
            box-shadow: 0 0 15px rgba(0, 243, 255, 0.2);
        }

        .panel-title {
            font-family: var(--font-mono);
            font-size: 1.8rem;
            color: #fff;
            margin-bottom: 15px;
            border-left: 5px solid var(--neon-pink);
            padding-left: 10px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        /* --- Quest Log (Terminal) --- */
        .terminal-window {
            background: #000;
            border: 1px solid var(--neon-green);
            padding: 15px;
            font-family: var(--font-mono);
            font-size: 1.2rem;
            color: #ccc;
            height: 100%;
            box-shadow: inset 0 0 20px rgba(57, 255, 20, 0.1);
        }

        .terminal-line {
            margin-bottom: 5px;
            display: block;
        }
        
        .highlight { color: var(--neon-green); }
        .warning { color: #ff5e00; }
        .question { color: var(--neon-cyan); }

        /* --- Stats / Inventory --- */
        .stat-row {
            display: flex;
            align-items: center;
            margin-bottom: 12px;
            font-family: var(--font-mono);
            font-size: 1.2rem;
        }

        .stat-label {
            width: 120px;
            color: var(--neon-cyan);
        }

        .stat-bar-bg {
            flex-grow: 1;
            height: 10px;
            background: #333;
            border: 1px solid #555;
        }

        .stat-bar-fill {
            height: 100%;
            background: var(--neon-green);
            width: 0%; /* Animate this */
            box-shadow: 0 0 5px var(--neon-green);
            position: relative;
        }

        /* --- Active Missions (Projects) --- */
        .mission-card {
            display: flex;
            gap: 15px;
            margin-bottom: 15px;
            border-bottom: 1px dashed #333;
            padding-bottom: 15px;
            align-items: center;
        }
        
        .mission-card:last-child { border-bottom: none; }

        .mission-img {
            width: 120px;
            height: 80px;
            object-fit: cover;
            border: 1px solid var(--neon-green);
            filter: grayscale(80%);
            transition: 0.3s;
        }

        .mission-card:hover .mission-img {
            filter: grayscale(0%);
            box-shadow: 0 0 10px var(--neon-green);
        }

        .mission-info h3 {
            font-size: 1.1rem;
            color: #fff;
            margin-bottom: 5px;
        }

        .mission-info p {
            font-family: var(--font-mono);
            font-size: 1rem;
            color: #aaa;
        }

        .status-badge {
            display: inline-block;
            padding: 2px 8px;
            background: var(--neon-green);
            color: #000;
            font-size: 0.8rem;
            font-weight: bold;
            margin-top: 5px;
            clip-path: polygon(10% 0, 100% 0, 100% 100%, 0% 100%);
        }

        /* --- Footer / Comms --- */
        .hud-footer {
            margin-top: 10px;
            display: flex;
            justify-content: center;
            gap: 20px;
            border-top: 1px solid var(--border-color);
            padding-top: 15px;
        }

        .comm-btn {
            background: transparent;
            border: 1px solid var(--neon-cyan);
            color: var(--neon-cyan);
            padding: 10px 20px;
            font-family: var(--font-main);
            text-decoration: none;
            text-transform: uppercase;
            letter-spacing: 2px;
            transition: 0.3s;
            position: relative;
            overflow: hidden;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .comm-btn:hover {
            background: var(--neon-cyan);
            color: #000;
            box-shadow: 0 0 20px var(--neon-cyan);
        }

        /* --- Animations --- */
        @keyframes glitch {
            0% { transform: translate(0); }
            20% { transform: translate(-2px, 2px); }
            40% { transform: translate(-2px, -2px); }
            60% { transform: translate(2px, 2px); }
            80% { transform: translate(2px, -2px); }
            100% { transform: translate(0); }
        }

        .glitch-effect:hover {
            animation: glitch 0.3s cubic-bezier(.25, .46, .45, .94) both infinite;
            color: var(--neon-pink);
        }

    </style>
</head>
<body>

    <!-- Neural Network Background -->
    <canvas id="neural-canvas"></canvas>
    
    <!-- CRT Scanlines -->
    <div class="scanlines"></div>

    <!-- Boot Sequence -->
    <div id="boot-screen">
        <div class="boot-text">INITIALIZING SYSTEM...</div>
        <div class="progress-bar">
            <div class="progress-fill" id="boot-progress"></div>
        </div>
        <div class="boot-text" id="boot-log" style="font-size: 1rem; margin-top: 10px; color: #666;">Loading core modules...</div>
    </div>

    <!-- Main HUD -->
    <div class="hud-container" id="main-interface">
        
        <!-- Header -->
        <header class="hud-header">
            <div class="header-gif-container">
                <!-- User provided Header GIF -->
                <img src="https://media.giphy.com/media/iIqmM5tTjmpOB9k5Ke/giphy.gif" alt="Header Feed">
            </div>
            <h1 class="player-title glitch-effect">👾 PLAYER 1: ANTONIO 👾</h1>
            <div class="typing-container" id="typing-text"></div>
        </header>

        <div class="hud-body">
            
            <!-- Left Column: Stats & System -->
            <div class="left-col">
                
                <section class="panel">
                    <div class="panel-title">
                        <span>SYSTEM STATUS</span>
                        <i class="fas fa-microchip"></i>
                    </div>
                    
                    <div class="stat-row">
                        <span class="stat-label">HEALTH</span>
                        <div class="stat-bar-bg"><div class="stat-bar-fill" style="width: 100%;"></div></div>
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">MANA (Java)</span>
                        <div class="stat-bar-bg"><div class="stat-bar-fill" style="width: 85%; background: var(--neon-pink);"></div></div>
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">STAMINA</span>
                        <div class="stat-bar-bg"><div class="stat-bar-fill" style="width: 90%;"></div></div>
                    </div>

                    <div style="margin-top: 20px; text-align: center;">
                         <!-- User provided Wide GIF -->
                         <img src="https://user-images.githubusercontent.com/74038190/212284100-561aa473-3905-4a80-b561-0d28506553ee.gif" 
                              style="width: 100%; border: 1px solid #333;" alt="System Visualization">
                    </div>
                </section>

                <section class="panel" style="margin-top: 20px;">
                    <div class="panel-title">
                        <span>INVENTORY</span>
                        <i class="fas fa-toolbox"></i>
                    </div>
                    <div style="font-family: var(--font-mono); font-size: 1.1rem; line-height: 1.8;">
                        <div>[LEGENDARY] Arch Linux (BTW)</div>
                        <div>[RARE] Fullstack Frameworks</div>
                        <div>[RARE] Web Security Protocols</div>
                        <div>[COMMON] Coffee Beans</div>
                    </div>
                </section>

            </div>

            <!-- Right Column: Quest Log & Missions -->
            <div class="right-col">
                
                <section class="panel">
                    <div class="panel-title">
                        <span>QUEST LOG</span>
                        <i class="fas fa-terminal"></i>
                    </div>
                    <div class="terminal-window" id="quest-log-content">
                        <!-- Content injected by JS -->
                    </div>
                </section>

                <section class="panel" style="margin-top: 20px;">
                    <div class="panel-title">
                        <span>ACTIVE MISSIONS</span>
                        <i class="fas fa-crosshairs"></i>
                    </div>

                    <div class="mission-card">
                        <img src="https://picsum.photos/seed/lms/150/100" class="mission-img" alt="LMS Project">
                        <div class="mission-info">
                            <h3>Project: Learning Management System</h3>
                            <p>Developing core architecture for educational tracking.</p>
                            <span class="status-badge">IN PROGRESS</span>
                        </div>
                    </div>

                    <div class="mission-card">
                        <img src="https://picsum.photos/seed/crypto/150/100" class="mission-img" alt="Crypto Dashboard">
                        <div class="mission-info">
                            <h3>Side Quest: Crypto Analytics</h3>
                            <p>Real-time data visualization dashboard.</p>
                            <span class="status-badge">PRIORITY</span>
                        </div>
                    </div>

                </section>

            </div>
        </div>

        <!-- Footer -->
        <footer class="hud-footer">
            <a href="mailto:antoniosulistio2004@gmail.com" class="comm-btn">
                <i class="fas fa-envelope"></i> SEND ENCRYPTED MAIL
            </a>
            <a href="https://www.linkedin.com/in/antoniosulistio" target="_blank" class="comm-btn">
                <i class="fab fa-linkedin"></i> CONNECT UPLINK
            </a>
            <a href="#" class="comm-btn" onclick="alert('Initiating download sequence...')">
                <i class="fas fa-download"></i> DOWNLOAD SOURCE
            </a>
        </footer>

    </div>

    <script>
        // --- 1. Boot Sequence Logic ---
        document.addEventListener("DOMContentLoaded", () => {
            const progressFill = document.getElementById('boot-progress');
            const bootLog = document.getElementById('boot-log');
            const bootScreen = document.getElementById('boot-screen');
            const mainInterface = document.getElementById('main-interface');
            
            const logs = [
                "Loading Kernel...",
                "Mounting File Systems...",
                "Checking Permissions...",
                "Accessing User Profile: ANTONIO...",
                "Rendering HUD..."
            ];

            let progress = 0;
            let logIndex = 0;

            const interval = setInterval(() => {
                progress += Math.random() * 5;
                if (progress > 100) progress = 100;
                
                progressFill.style.width = `${progress}%`;

                // Update logs occasionally
                if (Math.random() > 0.7 && logIndex < logs.length) {
                    bootLog.innerText = logs[logIndex];
                    logIndex++;
                }

                if (progress === 100) {
                    clearInterval(interval);
                    setTimeout(() => {
                        bootScreen.style.opacity = '0';
                        setTimeout(() => {
                            bootScreen.style.display = 'none';
                            mainInterface.style.opacity = '1';
                            startTypingEffect();
                            initQuestLog();
                        }, 1000);
                    }, 500);
                }
            }, 50);
        });

        // --- 2. Typing Effect (Header) ---
        function startTypingEffect() {
            const textElement = document.getElementById('typing-text');
            const lines = [
                "CLASS: Technomancer (Fullstack Dev)",
                "GUILD: BINUS University",
                "QUEST: Building LMS & Crypto Dashboards",
                "STATUS: Grinding Code & Where Winds Meet"
            ];
            
            let lineIndex = 0;
            let charIndex = 0;
            let currentLine = "";

            function type() {
                if (lineIndex < lines.length) {
                    if (charIndex < lines[lineIndex].length) {
                        currentLine += lines[lineIndex].charAt(charIndex);
                        textElement.innerHTML = currentLine + "<span class='cursor'>_</span>";
                        charIndex++;
                        setTimeout(type, 30 + Math.random() * 50); // Random typing speed
                    } else {
                        // Line finished
                        textElement.innerHTML = currentLine; // Remove cursor temporarily
                        setTimeout(() => {
                            currentLine = "";
                            charIndex = 0;
                            lineIndex++;
                            type();
                        }, 1000); // Wait 1s before next line
                    }
                }
            }
            type();
        }

        // --- 3. Quest Log Animation ---
        function initQuestLog() {
            const logContainer = document.getElementById('quest-log-content');
            const lines = [
                { text: "> LOADING USER_PROFILE...", class: "" },
                { text: "> [✔] CURRENT OBJECTIVE: Develop Learning Management System (LMS)", class: "highlight" },
                { text: "> [✔] SIDE QUEST: Real-time Crypto Analytics Dashboard", class: "highlight" },
                { text: "> [!] LEARNING SKILL: Advanced Web Security & Linux Ricing", class: "warning" },
                { text: "> [?] CURRENT OS: Omarchy / Arch Linux (BTW)", class: "question" },
                { text: "> [🎮] AFK ACTIVITY: Where Winds Meet", class: "" }
            ];

            lines.forEach((line, index) => {
                const div = document.createElement('div');
                div.className = `terminal-line ${line.class}`;
                div.innerText = line.text;
                div.style.opacity = '0';
                logContainer.appendChild(div);

                // Staggered fade in
                setTimeout(() => {
                    div.style.transition = 'opacity 0.5s';
                    div.style.opacity = '1';
                }, 100 * index);
            });
        }

        // --- 4. Canvas Neural Network Background ---
        const canvas = document.getElementById('neural-canvas');
        const ctx = canvas.getContext('2d');
        let width, height;
        let particles = [];

        function resize() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
        }

        window.addEventListener('resize', resize);
        resize();

        class Particle {
            constructor() {
                this.x = Math.random() * width;
                this.y = Math.random() * height;
                this.vx = (Math.random() - 0.5) * 0.5;
                this.vy = (Math.random() - 0.5) * 0.5;
                this.size = Math.random() * 2 + 1;
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;

                if (this.x < 0 || this.x > width) this.vx *= -1;
                if (this.y < 0 || this.y > height) this.vy *= -1;
            }

            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fillStyle = '#39FF14';
                ctx.fill();
            }
        }

        function initParticles() {
            particles = [];
            const particleCount = Math.floor((width * height) / 15000); // Responsive count
            for (let i = 0; i < particleCount; i++) {
                particles.push(new Particle());
            }
        }

        function animateCanvas() {
            ctx.clearRect(0, 0, width, height);
            
            // Draw connections
            for (let i = 0; i < particles.length; i++) {
                for (let j = i + 1; j < particles.length; j++) {
                    const dx = particles[i].x - particles[j].x;
                    const dy = particles[i].y - particles[j].y;
                    const distance = Math.sqrt(dx * dx + dy * dy);

                    if (distance < 100) {
                        ctx.beginPath();
                        ctx.strokeStyle = `rgba(57, 255, 20, ${1 - distance / 100})`;
                        ctx.lineWidth = 0.5;
                        ctx.moveTo(particles[i].x, particles[i].y);
                        ctx.lineTo(particles[j].x, particles[j].y);
                        ctx.stroke();
                    }
                }
            }

            // Draw particles
            particles.forEach(p => {
                p.update();
                p.draw();
            });

            requestAnimationFrame(animateCanvas);
        }

        initParticles();
        animateCanvas();

        // --- 5. Mouse Interaction for Tilt Effect (Parallax) ---
        const hud = document.getElementById('main-interface');
        document.addEventListener('mousemove', (e) => {
            const x = (window.innerWidth / 2 - e.pageX) / 50;
            const y = (window.innerHeight / 2 - e.pageY) / 50;
            hud.style.transform = `rotateY(${x}deg) rotateX(${y}deg)`;
        });

    </script>
</body>
</html>
