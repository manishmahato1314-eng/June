<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Birthday Countdown! 🎂</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background: linear-gradient(135deg, #ffe5ec, #ffc2d1);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        .card-container {
            background: white;
            padding: 2.5rem;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            text-align: center;
            max-width: 480px;
            width: 90%;
            z-index: 10;
            transition: transform 0.3s ease;
        }

        h1 {
            color: #ff477e;
            font-size: 2.3rem;
            margin-bottom: 1rem;
            font-family: 'Arial Black', Gadget, sans-serif;
        }

        p {
            color: #6c757d;
            font-size: 1.05rem;
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }

        /* Countdown Timer Styling */
        .countdown-container {
            display: flex;
            justify-content: space-around;
            margin-bottom: 2rem;
            background: #fff0f3;
            padding: 15px;
            border-radius: 12px;
            border: 2px dashed #ffb5a7;
        }

        .time-box {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 22%;
        }

        .time-num {
            font-size: 1.8rem;
            font-weight: bold;
            color: #ff0a54;
            font-family: monospace;
        }

        .time-label {
            font-size: 0.75rem;
            text-transform: uppercase;
            color: #a3a3a3;
            letter-spacing: 1px;
            margin-top: 2px;
        }

        /* Birthday Cake Styling */
        .cake-container {
            position: relative;
            width: 120px;
            height: 120px;
            margin: 0 auto 1.5rem auto;
        }

        .cake {
            position: absolute;
            bottom: 0;
            width: 120px;
            height: 60px;
            background: #ffb5a7;
            border-radius: 10px 10px 0 0;
            box-shadow: inset 0 -10px 0 #f8ad9d;
        }

        .cake::before {
            content: '';
            position: absolute;
            top: 15px;
            left: 0;
            width: 120px;
            height: 12px;
            background: #fcd5ce;
        }

        .candle {
            position: absolute;
            bottom: 60px;
            left: 53px;
            width: 14px;
            height: 35px;
            background: repeating-linear-gradient(45deg, #fff, #fff 5px, #ff477e 5px, #ff477e 10px);
            border-radius: 3px 3px 0 0;
        }

        .flame {
            position: absolute;
            bottom: 95px;
            left: 55px;
            width: 10px;
            height: 18px;
            background: #ffb703;
            border-radius: 50% 50% 20% 20%;
            box-shadow: 0 0 10px #ffb703;
            animation: flicker 0.6s infinite alternate;
        }

        @keyframes flicker {
            0% { transform: scale(1); }
            100% { transform: scale(1.1) rotate(3deg); opacity: 0.9; }
        }

        /* Surprise Message Styling */
        .hidden-message {
            display: none;
            margin-top: 1rem;
            color: #ff0a54;
            font-weight: bold;
            font-size: 1.4rem;
            animation: fadeIn 1s ease forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Confetti Canvas */
        #confetti-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }
    </style>
</head>
<body>

    <canvas id="confetti-canvas"></canvas>

    <div class="card-container">
        <div class="cake-container">
            <div class="flame" id="flame"></div>
            <div class="candle"></div>
            <div class="cake"></div>
        </div>
        
        <h1 id="greeting">Counting Down... ⏳</h1>
        <p id="message">Great things come to those who wait! The countdown to June 13th is officially on.</p>
        
        <div class="countdown-container" id="countdown-box">
            <div class="time-box">
                <span class="time-num" id="days">00</span>
                <span class="time-label">Days</span>
            </div>
            <div class="time-box">
                <span class="time-num" id="hours">00</span>
                <span class="time-label">Hours</span>
            </div>
            <div class="time-box">
                <span class="time-num" id="minutes">00</span>
                <span class="time-label">Mins</span>
            </div>
            <div class="time-box">
                <span class="time-num" id="seconds">00</span>
                <span class="time-label">Secs</span>
            </div>
        </div>
        
        <div class="hidden-message" id="surprise-text">🥳 HAPPY BIRTHDAY! 🎉<br>❤️ Have the best day ever! ❤️</div>
    </div>

    <script>
        // Set target date: June 13, 2027 at midnight
        const targetDate = new Date("June 13, 2027 00:00:00").getTime();

        // Update the countdown every single second
        const countdownInterval = setInterval(function() {
            const now = new Date().getTime();
            const difference = targetDate - now;

            // Calculations for days, hours, minutes and seconds
            const d = Math.floor(difference / (1000 * 60 * 60 * 24));
            const h = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const m = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
            const s = Math.floor((difference % (1000 * 60)) / 1000);

            // Display results, adding leading zeros if number is under 10
            document.getElementById("days").innerText = d < 10 ? "0" + d : d;
            document.getElementById("hours").innerText = h < 10 ? "0" + h : h;
            document.getElementById("minutes").innerText = m < 10 ? "0" + m : m;
            document.getElementById("seconds").innerText = s < 10 ? "0" + s : s;

            // When countdown hits zero
            if (difference <= 0) {
                clearInterval(countdownInterval);
                triggerBirthdayParty();
            }
        }, 1000);

        function triggerBirthdayParty() {
            // Update Text headers
            document.getElementById('greeting').innerText = "Happy Birthday! 🎉";
            document.getElementById('message').innerText = "The wait is over! Wishing you a gorgeous day filled with love, laughter, and endless pieces of cake.";
            
            // Hide timer & blow out candle
            document.getElementById('countdown-box').style.display = 'none';
            document.getElementById('flame').style.display = 'none';
            
            // Reveal secret birthday text
            document.getElementById('surprise-text').style.display = 'block';
            
            // Fire up confetti
            initConfetti();
            animate();
        }

        // --- Confetti Engine ---
        const canvas = document.getElementById('confetti-canvas');
        const ctx = canvas.getContext('2d');
        let confetti = [];

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        class ConfettiParticle {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height - canvas.height;
                this.size = Math.random() * 8 + 5;
                this.color = `hsl(${Math.random() * 360}, 100%, 65%)`;
                this.speedX = Math.random() * 3 - 1.5;
                this.speedY = Math.random() * 3 + 2;
                this.rotation = Math.random() * 360;
                this.rotationSpeed = Math.random() * 4 - 2;
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;
                this.rotation += this.rotationSpeed;
                if (this.y > canvas.height) {
                    this.y = -20;
                    this.x = Math.random() * canvas.width;
                }
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate(this.rotation * Math.PI / 180);
                ctx.fillStyle = this.color;
                ctx.fillRect(-this.size/2, -this.size/2, this.size, this.size);
                ctx.restore();
            }
        }

        function initConfetti() {
            for (let i = 0; i < 120; i++) {
                confetti.push(new ConfettiParticle());
            }
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            confetti.forEach(particle => {
                particle.update();
                particle.draw();
            });
            requestAnimationFrame(animate);
        }
    </script>
</body>
</html>
