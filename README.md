<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SolarWinds Hybrid Cloud Observability</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #0a0f1f;
            --primary-color: #ff9900;
            --secondary-color: #00bfff;
            --text-color: #e0e0e0;
            --text-color-dark: #a0a0a0;
        }

        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .animation-container {
            width: 95%;
            max-width: 1000px;
            height: 600px;
            position: relative;
            background: radial-gradient(circle, rgba(15, 22, 43, 0.9) 0%, rgba(10, 15, 31, 0.95) 70%), url('https://www.transparenttextures.com/patterns/grid.png');
            border-radius: 20px;
            box-shadow: 0 0 50px rgba(0, 191, 255, 0.2);
            border: 1px solid rgba(0, 191, 255, 0.3);
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        .scene {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            opacity: 0;
            transition: opacity 1s ease-in-out;
            z-index: 2;
            padding: 2rem;
            box-sizing: border-box;
        }

        .scene.active {
            opacity: 1;
        }

        h1 {
            font-size: 2.5rem;
            font-weight: 700;
            margin: 0;
            text-shadow: 0 0 10px var(--secondary-color);
        }

        p {
            font-size: 1.2rem;
            font-weight: 300;
            max-width: 80%;
            color: var(--text-color-dark);
            margin-top: 1rem;
        }

        /* --- Scene 1: The Chaos --- */
        #scene1 .it-elements {
            position: relative;
            width: 600px;
            height: 300px;
            margin-top: 2rem;
        }

        .it-icon {
            position: absolute;
            display: flex;
            flex-direction: column;
            align-items: center;
            color: #ccc;
            animation: float 8s ease-in-out infinite;
        }

        .it-icon svg {
            width: 50px;
            height: 50px;
            filter: drop-shadow(0 0 5px rgba(255, 255, 255, 0.3));
        }
        
        .it-icon span {
            font-size: 0.8rem;
            margin-top: 5px;
        }

        #icon-server { top: 10%; left: 10%; animation-delay: 0s; }
        #icon-aws { top: 20%; left: 80%; animation-delay: -2s; }
        #icon-azure { top: 70%; left: 5%; animation-delay: -4s; }
        #icon-db { top: 80%; left: 75%; animation-delay: -6s; }
        #icon-app { top: 40%; left: 45%; animation-delay: -1s; }

        /* --- Scene 2: The Solution --- */
        #logo-container {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            animation: pulse 2s infinite, fadeIn 1s forwards;
        }
        #logo-container img {
             width: 120px;
             height: auto;
        }

        /* --- Scene 3: Benefits --- */
        #benefits-grid {
            display: flex;
            justify-content: space-around;
            width: 100%;
            max-width: 800px;
            margin-top: 3rem;
        }
        .benefit-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeIn-up 0.8s forwards;
        }
        .benefit-item:nth-child(1) { animation-delay: 0.5s; }
        .benefit-item:nth-child(2) { animation-delay: 1.0s; }
        .benefit-item:nth-child(3) { animation-delay: 1.5s; }
        .benefit-icon {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            border: 2px solid var(--secondary-color);
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 1rem;
            background: rgba(0, 191, 255, 0.1);
            box-shadow: 0 0 15px rgba(0, 191, 255, 0.3);
        }
        .benefit-icon svg { width: 40px; height: 40px; }
        .benefit-item h3 { font-size: 1.2rem; color: var(--text-color); margin: 0;}
        .benefit-item p { font-size: 1rem; color: var(--text-color-dark); margin-top: 0.5rem; max-width: 200px; }

        /* --- Scene 4: Final CTA --- */
        .cta-logos {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 1.5rem;
            margin-bottom: 2rem;
        }
        .main-logo {
            width: 250px;
            height: auto;
        }
        .distributor-info {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.5rem;
            border-top: 1px solid rgba(0, 191, 255, 0.3);
            padding-top: 1.5rem;
            opacity: 0;
            animation: fadeIn 1s 0.5s forwards;
        }
        .distributor-logo {
            width: 120px;
            height: auto;
        }
        .distributor-info span {
            font-size: 0.9rem;
            color: var(--text-color-dark);
            font-weight: 300;
        }


        /* --- Animations --- */
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-15px); }
            100% { transform: translateY(0px); }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes pulse {
            0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(255, 153, 0, 0.7); }
            70% { transform: scale(1); box-shadow: 0 0 0 20px rgba(255, 153, 0, 0); }
            100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(255, 153, 0, 0); }
        }

        @keyframes fadeIn-up {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            h1 { font-size: 2rem; }
            p { font-size: 1rem; }
            .animation-container { height: 90vh; }
            #benefits-grid { flex-direction: column; align-items: center; }
            .benefit-item { margin-bottom: 2rem; }
            .main-logo { width: 200px; }
            .distributor-logo { width: 100px; }
            .cta-logos { gap: 1rem; margin-bottom: 1rem; }
            .distributor-info { padding-top: 1rem; }
        }
    </style>
</head>
<body>

    <div class="animation-container">
        <canvas id="particle-canvas"></canvas>

        <!-- SCENE 1: The Problem of IT Complexity -->
        <div id="scene1" class="scene active">
            <h1>Is Your Hybrid IT a Black Box?</h1>
            <p>Struggling with fragmented tools and a lack of visibility across your complex environment?</p>
            <div class="it-elements">
                <!-- SVG Icons for IT components -->
                <div id="icon-server" class="it-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="8" rx="2" ry="2"></rect><rect x="2" y="14" width="20" height="8" rx="2" ry="2"></rect><line x1="6" y1="6" x2="6.01" y2="6"></line><line x1="6" y1="18" x2="6.01" y2="18"></line></svg>
                    <span>On-Prem</span>
                </div>
                <div id="icon-aws" class="it-icon">
                     <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17.5 19H9a7 7 0 1 1 6.71-9h1.79a4.5 4.5 0 1 1 0 9Z"></path></svg>
                    <span>Cloud</span>
                </div>
                <div id="icon-azure" class="it-icon">
                     <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17.5 19H9a7 7 0 1 1 6.71-9h1.79a4.5 4.5 0 1 1 0 9Z"></path></svg>
                    <span>Multi-Cloud</span>
                </div>
                <div id="icon-db" class="it-icon">
                     <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="9" ry="3"></ellipse><path d="M21 12c0 1.66-4 3-9 3s-9-1.34-9-3"></path><path d="M3 5v14c0 1.66 4 3 9 3s9-1.34 9-3V5"></path></svg>
                    <span>Databases</span>
                </div>
                <div id="icon-app" class="it-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect><line x1="3" y1="9" x2="21" y2="9"></line><line x1="9" y1="21" x2="9" y2="9"></line></svg>
                    <span>Applications</span>
                </div>
            </div>
        </div>

        <!-- SCENE 2: The Solution -->
        <div id="scene2" class="scene">
             <div id="logo-container">
                <img src="https://cdn.shopify.com/s/files/1/0729/3508/0249/files/Solarwinds_Logo.png?v=1758266377" alt="SolarWinds Logo">
            </div>
            <h1>Unify Your Vision.</h1>
            <p>Introducing SolarWinds® Hybrid Cloud Observability.</p>
        </div>

        <!-- SCENE 3: Key Benefits -->
        <div id="scene3" class="scene">
            <h1>Achieve Full-Stack Observability</h1>
            <p>Transform disconnected data into actionable intelligence for proactive problem resolution.</p>
            <div id="benefits-grid">
                <div class="benefit-item">
                    <div class="benefit-icon">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20V10"></path><path d="M18 20V4"></path><path d="M6 20V16"></path></svg>
                    </div>
                    <h3>Boost Performance</h3>
                    <p>Optimize applications and infrastructure across your entire environment.</p>
                </div>
                <div class="benefit-item">
                    <div class="benefit-icon">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path></svg>
                    </div>
                    <h3>Reduce Risk</h3>
                    <p>Minimize downtime with predictive analytics and rapid root-cause analysis.</p>
                </div>
                <div class="benefit-item">
                    <div class="benefit-icon">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"></path><path d="M12 5l7 7-7 7"></path></svg>
                    </div>
                    <h3>Accelerate Innovation</h3>
                    <p>Free up IT teams to focus on strategic initiatives instead of firefighting.</p>
                </div>
            </div>
        </div>
        
        <!-- SCENE 4: Final CTA -->
        <div id="scene4" class="scene">
            <div class="cta-logos">
                <img class="main-logo" src="https://cdn.shopify.com/s/files/1/0729/3508/0249/files/softech_white_logo.png?v=1758266376" alt="Softech Logo">
                <div class="distributor-info">
                    <img class="distributor-logo" src="https://cdn.shopify.com/s/files/1/0729/3508/0249/files/Solarwinds_Logo.png?v=1758266377" alt="SolarWinds Logo">
                    <span>Authorized Distributor</span>
                </div>
           </div>
            <h1>See Everything. Solve Anything.</h1>
            <p>Empower your organization with Hybrid Cloud Observability.</p>
        </div>

    </div>

    <script>
        // --- Canvas & Global Variables ---
        const canvas = document.getElementById('particle-canvas');
        const ctx = canvas.getContext('2d');
        const scenes = document.querySelectorAll('.scene');
        
        let particles = [];
        const numParticles = 100;
        let currentScene = 0;
        const sceneDuration = 6000;

        const chaosParticles = [];
        const numChaosParticles = 20;
        const iconElements = Array.from(document.querySelectorAll('.it-icon'));
        const iconPositions = [];

        // --- Particle Class ---
        class Particle {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.size = Math.random() * 2 + 1;
                this.speedX = Math.random() * 1 - 0.5;
                this.speedY = Math.random() * 1 - 0.5;
                this.color = 'rgba(0, 191, 255, 0.5)';
            }
            update() {
                this.x += this.speedX;
                this.y += this.speedY;

                if (this.size > 0.1) this.size -= 0.01;
                if (this.size <= 0.1) {
                    this.x = Math.random() * canvas.width;
                    this.y = Math.random() * canvas.height;
                    this.size = Math.random() * 2 + 1;
                    this.speedX = Math.random() * 1 - 0.5;
                    this.speedY = Math.random() * 1 - 0.5;
                }
            }
            draw() {
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
            }
        }

        // --- Chaos Particle Class ---
        class ChaosParticle {
            constructor() {
                this.reset();
            }

            reset() {
                if (iconPositions.length < 2) return;
                const startIndex = Math.floor(Math.random() * iconPositions.length);
                let endIndex;
                do {
                    endIndex = Math.floor(Math.random() * iconPositions.length);
                } while (startIndex === endIndex);

                this.start = iconPositions[startIndex];
                this.end = iconPositions[endIndex];

                this.x = this.start.x;
                this.y = this.start.y;
                
                const angle = Math.atan2(this.end.y - this.start.y, this.end.x - this.start.x);
                const speed = Math.random() * 1.5 + 0.5;
                this.vx = Math.cos(angle) * speed;
                this.vy = Math.sin(angle) * speed;
                this.size = Math.random() * 2 + 1.5;
                this.color = '#ff4136'; // Red color for chaos/alert
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;

                const dx = this.x - this.end.x;
                const dy = this.y - this.end.y;
                const dist = Math.sqrt(dx * dx + dy * dy);

                if (dist < 5) {
                    this.reset();
                }
            }

            draw() {
                ctx.fillStyle = this.color;
                ctx.shadowColor = this.color;
                ctx.shadowBlur = 8;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0; // Reset shadow blur
            }
        }

        // --- Core Functions ---
        function resizeCanvas() {
            canvas.width = canvas.parentElement.offsetWidth;
            canvas.height = canvas.parentElement.offsetHeight;
        }

        function initParticles() {
            particles = [];
            for (let i = 0; i < numParticles; i++) {
                particles.push(new Particle());
            }
        }

        function getIconPositions() {
            iconPositions.length = 0;
            const containerRect = document.querySelector('.animation-container').getBoundingClientRect();
            const itElementsRect = document.querySelector('.it-elements').getBoundingClientRect();

            iconElements.forEach(icon => {
                const iconRect = icon.getBoundingClientRect();
                const x = (iconRect.left - containerRect.left) + (iconRect.width / 2);
                const y = (iconRect.top - containerRect.top) + (iconRect.height / 2);
                iconPositions.push({ x, y });
            });
        }
        
        function initChaos() {
            getIconPositions();
            chaosParticles.length = 0;
            if(iconPositions.length > 0) {
                for (let i = 0; i < numChaosParticles; i++) {
                    chaosParticles.push(new ChaosParticle());
                }
            }
        }

        function switchScene() {
            scenes[currentScene].classList.remove('active');
            currentScene = (currentScene + 1) % scenes.length;
            scenes[currentScene].classList.add('active');
        }
        
        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            particles.forEach(p => {
                p.update();
                p.draw();
            });

            if (scenes[0].classList.contains('active') && iconPositions.length > 0) {
                chaosParticles.forEach(p => {
                    p.update();
                    p.draw();
                });
            }

            requestAnimationFrame(animateParticles);
        }
        
        // --- Event Listeners & Initialization ---
        window.addEventListener('resize', () => {
            resizeCanvas();
            initParticles();
            setTimeout(initChaos, 100);
        });

        // Initial setup
        resizeCanvas();
        initParticles();
        setTimeout(initChaos, 100);
        animateParticles();
        
        scenes[0].classList.add('active');
        setInterval(switchScene, sceneDuration);

    </script>
</body>
</html>

