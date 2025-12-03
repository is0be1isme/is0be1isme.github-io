<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Liquid Flow</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #050f14; /* Deep dark teal background */
        }
        canvas {
            display: block;
            filter: blur(30px); 
        }
        
        .content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: white;
            font-family: sans-serif;
            font-size: 2rem;
            font-weight: 100;
            letter-spacing: 4px;
            pointer-events: none;
            text-shadow: 0 0 20px rgba(0,0,0,0.5);
            mix-blend-mode: overlay;
        }
    </style>
</head>
<body>

    <div class="content">LIQUID FLOW</div>
    <canvas id="liquidCanvas"></canvas>

    <script>
        const canvas = document.getElementById('liquidCanvas');
        const ctx = canvas.getContext('2d');

        // resize canvas to fill screen
        let width, height;
        function resize() {
            width = window.innerWidth;
            height = window.innerHeight;
            canvas.width = width;
            canvas.height = height;
        }
        window.addEventListener('resize', resize);
        resize();

        const colors = [
            '#FF4D4D', // Red
            '#C70039', // Deep Red
            '#4DFF91', // Green
            '#00E396', // Bright Green
            '#FF4DA6', // Pink
            '#FF007F', // Deep Pink
            '#4DFFFF', // Teal
            '#00B5B5'  // Deep Teal
        ];

        class Orb {
            constructor() {
                this.init();
            }

            init() {
            
                this.radius = (Math.random() * 300) + 150;
                
                this.x = Math.random() * width;
                this.y = Math.random() * height;
                
                this.vx = (Math.random() - 0.5) * 1.5;
                this.vy = (Math.random() - 0.5) * 1.5;
                
                this.color = colors[Math.floor(Math.random() * colors.length)];
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;

                if (this.x < -100 || this.x > width + 100) this.vx *= -1;
                if (this.y < -100 || this.y > height + 100) this.vy *= -1;
            }

            draw() {
                const gradient = ctx.createRadialGradient(
                    this.x, this.y, 0, 
                    this.x, this.y, this.radius
                );

                gradient.addColorStop(0, this.color);
                gradient.addColorStop(1, 'rgba(0,0,0,0)');

                ctx.fillStyle = gradient;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                ctx.fill();
            }
        }

        const orbs = [];
        const orbCount = 12;
        for (let i = 0; i < orbCount; i++) {
            orbs.push(new Orb());
        }

        function animate() {
            ctx.clearRect(0, 0, width, height);
            ctx.globalCompositeOperation = 'screen'; 

            orbs.forEach(orb => {
                orb.update();
                orb.draw();
            });

            requestAnimationFrame(animate);
        }

        animate();
    </script>
</body>
</html>
