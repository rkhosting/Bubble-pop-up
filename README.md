<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cosmic Bubble Pop</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            font-family: 'Orbitron', monospace;
            overflow: hidden;
            background: linear-gradient(135deg, #1a0033 0%, #330066 25%, #4d0099 50%, #6600cc 75%, #8533ff 100%);
            color: white;
            height: 100vh;
        }

        /* Animated cosmic background */
        .cosmic-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(circle at 20% 20%, rgba(255, 215, 0, 0.3) 0%, transparent 50%),
                radial-gradient(circle at 80% 80%, rgba(138, 43, 226, 0.4) 0%, transparent 50%),
                radial-gradient(circle at 40% 60%, rgba(75, 0, 130, 0.3) 0%, transparent 50%);
            z-index: -2;
        }

        /* Floating particles */
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .particle {
            position: absolute;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.8), rgba(255, 215, 0, 0.4));
            border-radius: 50%;
            animation: float 8s infinite linear;
        }

        @keyframes float {
            0% {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            90% {
                opacity: 1;
            }
            100% {
                transform: translateY(-100px) rotate(360deg);
                opacity: 0;
            }
        }

        /* Game UI */
        .game-ui {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            text-align: center;
            z-index: 100;
        }

        .target-color {
            font-size: 24px;
            font-weight: 700;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.8);
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }

        .color-indicator {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            border: 2px solid rgba(255, 255, 255, 0.8);
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
            animation: glow 2s ease-in-out infinite alternate;
        }

        @keyframes glow {
            from { box-shadow: 0 0 15px rgba(255, 255, 255, 0.5); }
            to { box-shadow: 0 0 25px rgba(255, 255, 255, 0.9); }
        }

        .score {
            position: fixed;
            top: 20px;
            right: 30px;
            font-size: 28px;
            font-weight: 700;
            text-shadow: 0 0 15px rgba(255, 255, 255, 0.8);
            z-index: 100;
        }

        /* Bubbles */
        .bubble {
            position: absolute;
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 
                inset -10px -10px 0 rgba(255, 255, 255, 0.2),
                inset 10px 10px 0 rgba(0, 0, 0, 0.1),
                0 0 20px rgba(255, 255, 255, 0.3);
            animation: rise 8s linear infinite;
            transition: all 0.3s ease;
        }

        .bubble:hover {
            transform: scale(1.1);
            box-shadow: 
                inset -10px -10px 0 rgba(255, 255, 255, 0.3),
                inset 10px 10px 0 rgba(0, 0, 0, 0.1),
                0 0 30px rgba(255, 255, 255, 0.5);
        }

        @keyframes rise {
            from {
                bottom: -100px;
                opacity: 0;
            }
            5% {
                opacity: 0.8;
            }
            95% {
                opacity: 0.8;
            }
            to {
                bottom: 100vh;
                opacity: 0;
            }
        }

        /* Pop effect */
        .pop-effect {
            position: absolute;
            pointer-events: none;
            z-index: 50;
        }

        .sparkle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: radial-gradient(circle, #fff, #ffd700);
            border-radius: 50%;
            animation: sparkle 0.6s ease-out forwards;
        }

        @keyframes sparkle {
            0% {
                transform: scale(1) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: scale(0) rotate(180deg);
                opacity: 0;
            }
        }

        /* Feedback messages */
        .feedback {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 36px;
            font-weight: 900;
            text-shadow: 0 0 20px currentColor;
            z-index: 200;
            animation: feedbackPop 1s ease-out forwards;
            pointer-events: none;
        }

        @keyframes feedbackPop {
            0% {
                transform: translate(-50%, -50%) scale(0);
                opacity: 0;
            }
            30% {
                transform: translate(-50%, -50%) scale(1.2);
                opacity: 1;
            }
            100% {
                transform: translate(-50%, -50%) scale(1);
                opacity: 0;
            }
        }

        /* Game Over Screen */
        .game-over {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            backdrop-filter: blur(10px);
        }

        .game-over-content {
            text-align: center;
            background: linear-gradient(135deg, rgba(138, 43, 226, 0.3), rgba(75, 0, 130, 0.3));
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 0 50px rgba(255, 255, 255, 0.2);
        }

        .game-over h2 {
            font-size: 48px;
            font-weight: 900;
            color: #ff4444;
            text-shadow: 0 0 20px rgba(255, 68, 68, 0.8);
            margin-bottom: 20px;
        }

        .final-score {
            font-size: 36px;
            font-weight: 700;
            color: #ffd700;
            text-shadow: 0 0 15px rgba(255, 215, 0, 0.8);
            margin-bottom: 30px;
        }

        .restart-btn {
            font-family: 'Orbitron', monospace;
            font-size: 24px;
            font-weight: 700;
            padding: 15px 30px;
            background: linear-gradient(135deg, #4488ff, #8844ff);
            color: white;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
            box-shadow: 0 0 20px rgba(68, 136, 255, 0.5);
            transition: all 0.3s ease;
        }

        .correct { color: #00ff88; }
        .wrong { color: #ff4444; }

        /* Responsive design */
        @media (max-width: 768px) {
            .target-color {
                font-size: 20px;
            }
            .score {
                font-size: 24px;
                top: 15px;
                right: 20px;
            }
            .color-indicator {
                width: 25px;
                height: 25px;
            }
        }
    </style>
</head>
<body>
    <!-- Cosmic background with particles -->
    <div class="cosmic-bg"></div>
    <div class="particles" id="particles"></div>

    <!-- Game UI -->
    <div class="game-ui">
        <div class="target-color">
            Pop the <span id="target-color-text">BLUE</span> Bubble
            <div class="color-indicator" id="color-indicator"></div>
        </div>
    </div>

    <div class="score">
        Score: <span id="score">0</span>
    </div>

    <!-- Game Over Screen -->
    <div class="game-over" id="game-over">
        <div class="game-over-content">
            <h2>Game Over!</h2>
            <div class="final-score">Final Score: <span id="final-score">0</span></div>
            <button class="restart-btn" onclick="restartGame()">Play Again</button>
        </div>
    </div>

    <script>
        // Game state
        let score = 0;
        let targetColor = 'blue';
        let bubblesPopped = 0;
        let gameActive = true;
        let bubbleSpeed = 8; // Starting speed in seconds
        let spawnInterval = 1500; // Starting spawn interval in ms
        let bubbleGenerationTimer;
        const colorsNeededToChange = 5; // Change target color every 5 correct pops
        const speedIncrement = 0.2; // Speed increase per correct pop
        const spawnDecrement = 50; // Spawn interval decrease per correct pop

        // Bubble colors with their CSS values
        const bubbleColors = {
            red: '#ff4444',
            blue: '#4488ff',
            green: '#44ff44',
            yellow: '#ffff44',
            pink: '#ff44ff',
            purple: '#8844ff',
            orange: '#ff8844',
            cyan: '#44ffff'
        };

        // DOM elements
        const scoreElement = document.getElementById('score');
        const targetColorText = document.getElementById('target-color-text');
        const colorIndicator = document.getElementById('color-indicator');
        const particlesContainer = document.getElementById('particles');
        const gameOverScreen = document.getElementById('game-over');
        const finalScoreElement = document.getElementById('final-score');

        // Initialize game
        function init() {
            resetGameState();
            setTargetColor();
            createFloatingParticles();
            startBubbleGeneration();
        }

        // Reset game state
        function resetGameState() {
            score = 0;
            bubblesPopped = 0;
            gameActive = true;
            bubbleSpeed = 8;
            spawnInterval = 1500;
            scoreElement.textContent = '0';
            gameOverScreen.style.display = 'none';
            
            // Clear existing bubbles
            document.querySelectorAll('.bubble').forEach(bubble => bubble.remove());
            
            // Clear existing timers
            if (bubbleGenerationTimer) {
                clearInterval(bubbleGenerationTimer);
            }
        }

        // Create floating background particles
        function createFloatingParticles() {
            for (let i = 0; i < 15; i++) {
                setTimeout(() => createParticle(), i * 1000);
            }
        }

        function createParticle() {
            const particle = document.createElement('div');
            particle.className = 'particle';
            
            const size = Math.random() * 4 + 2;
            particle.style.width = size + 'px';
            particle.style.height = size + 'px';
            particle.style.left = Math.random() * window.innerWidth + 'px';
            particle.style.animationDuration = (Math.random() * 6 + 6) + 's';
            particle.style.animationDelay = Math.random() * 2 + 's';
            
            particlesContainer.appendChild(particle);
            
            // Remove particle after animation and create new one
            setTimeout(() => {
                particle.remove();
                createParticle();
            }, 8000);
        }

        // Set new target color
        function setTargetColor() {
            const colors = Object.keys(bubbleColors);
            targetColor = colors[Math.floor(Math.random() * colors.length)];
            targetColorText.textContent = targetColor.toUpperCase();
            colorIndicator.style.backgroundColor = bubbleColors[targetColor];
        }

        // Create a bubble
        function createBubble() {
            if (!gameActive) return;
            
            const bubble = document.createElement('div');
            bubble.className = 'bubble';
            
            // Random properties
            const size = Math.random() * 60 + 40; // 40-100px
            const colors = Object.keys(bubbleColors);
            const bubbleColor = colors[Math.floor(Math.random() * colors.length)];
            const leftPosition = Math.random() * (window.innerWidth - size);
            const animationDuration = bubbleSpeed; // Use current game speed
            
            // Set bubble properties
            bubble.style.width = size + 'px';
            bubble.style.height = size + 'px';
            bubble.style.left = leftPosition + 'px';
            bubble.style.backgroundColor = bubbleColors[bubbleColor];
            bubble.style.animationDuration = animationDuration + 's';
            bubble.dataset.color = bubbleColor;
            
            // Add click handler
            bubble.addEventListener('click', () => handleBubblePop(bubble));
            
            document.body.appendChild(bubble);
            
            // Remove bubble after animation
            setTimeout(() => {
                if (bubble.parentNode) {
                    bubble.remove();
                }
            }, animationDuration * 1000);
        }

        // Handle bubble pop
        function handleBubblePop(bubble) {
            if (!gameActive) return;
            
            const bubbleColor = bubble.dataset.color;
            const rect = bubble.getBoundingClientRect();
            const x = rect.left + rect.width / 2;
            const y = rect.top + rect.height / 2;
            
            // Create pop effect
            createPopEffect(x, y);
            
            // Remove bubble immediately
            bubble.remove();
            
            // Check if correct color
            if (bubbleColor === targetColor) {
                score += 10;
                bubblesPopped++;
                showFeedback('Great!', 'correct');
                
                // Increase difficulty - make bubbles faster and spawn more frequently
                bubbleSpeed = Math.max(3, bubbleSpeed - speedIncrement); // Minimum 3 seconds
                spawnInterval = Math.max(800, spawnInterval - spawnDecrement); // Minimum 800ms
                
                // Restart bubble generation with new speed
                restartBubbleGeneration();
                
                // Change target color after certain number of correct pops
                if (bubblesPopped % colorsNeededToChange === 0) {
                    setTimeout(() => setTargetColor(), 500);
                }
            } else {
                // Wrong color - end game
                gameActive = false;
                endGame();
            }
            
            // Update score display
            scoreElement.textContent = score;
        }

        // Create pop effect with sparkles
        function createPopEffect(x, y) {
            const popEffect = document.createElement('div');
            popEffect.className = 'pop-effect';
            popEffect.style.left = x + 'px';
            popEffect.style.top = y + 'px';
            
            // Create sparkles
            for (let i = 0; i < 8; i++) {
                const sparkle = document.createElement('div');
                sparkle.className = 'sparkle';
                
                const angle = (i / 8) * Math.PI * 2;
                const distance = 30 + Math.random() * 20;
                const sparkleX = Math.cos(angle) * distance;
                const sparkleY = Math.sin(angle) * distance;
                
                sparkle.style.left = sparkleX + 'px';
                sparkle.style.top = sparkleY + 'px';
                sparkle.style.animationDelay = (Math.random() * 0.2) + 's';
                
                popEffect.appendChild(sparkle);
            }
            
            document.body.appendChild(popEffect);
            
            // Remove effect after animation
            setTimeout(() => {
                popEffect.remove();
            }, 600);
        }

        // Show feedback message
        function showFeedback(message, type) {
            const feedback = document.createElement('div');
            feedback.className = `feedback ${type}`;
            feedback.textContent = message;
            
            document.body.appendChild(feedback);
            
            setTimeout(() => {
                feedback.remove();
            }, 1000);
        }

        // Start continuous bubble generation
        function startBubbleGeneration() {
            bubbleGenerationTimer = setInterval(() => {
                createBubble();
            }, spawnInterval);
        }

        // Restart bubble generation with new speed
        function restartBubbleGeneration() {
            if (bubbleGenerationTimer) {
                clearInterval(bubbleGenerationTimer);
            }
            startBubbleGeneration();
        }

        // End game
        function endGame() {
            gameActive = false;
            if (bubbleGenerationTimer) {
                clearInterval(bubbleGenerationTimer);
            }
            
            // Remove remaining bubbles
            document.querySelectorAll('.bubble').forEach(bubble => bubble.remove());
            
            // Show game over screen
            finalScoreElement.textContent = score;
            gameOverScreen.style.display = 'flex';
        }

        // Restart game
        function restartGame() {
            init();
        }

        // Handle window resize
        window.addEventListener('resize', () => {
            // Remove existing bubbles that might be out of bounds
            document.querySelectorAll('.bubble').forEach(bubble => {
                const rect = bubble.getBoundingClientRect();
                if (rect.left < 0 || rect.left > window.innerWidth) {
                    bubble.remove();
                }
            });
        });

        // Start the game when page loads
        window.addEventListener('load', init);

        // Prevent context menu on long press (mobile)
        document.addEventListener('contextmenu', e => e.preventDefault());

        // Add some initial bubbles for immediate gameplay
        setTimeout(() => {
            for (let i = 0; i < 3; i++) {
                setTimeout(() => createBubble(), i * 800);
            }
        }, 1000);
    </script>
</body>
</html>
