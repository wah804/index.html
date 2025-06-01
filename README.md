<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>n8n Assistant</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700&family=Rajdhani:wght@300;400;500;600;700&display=swap');
        
        :root {
            --primary-color: #00c8ff;
            --secondary-color: #1a1a2e;
            --accent-color: #ff3e3e;
            --text-color: #e0e0e0;
            --border-color: #30445a;
            --shadow-color: rgba(0, 200, 255, 0.3);
            --widget-width: 380px;
            --widget-height: 520px;
            --header-height: 60px;
            --border-radius: 2px;
            --glow-effect: 0 0 10px var(--primary-color), 0 0 20px rgba(0, 200, 255, 0.4);
            --hologram-overlay: repeating-linear-gradient(
                rgba(0, 200, 255, 0.03) 2px,
                transparent 4px,
                rgba(0, 200, 255, 0.02) 6px
            );
        }

        body {
            font-family: 'Rajdhani', sans-serif;
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #0a0e17;
            color: var(--text-color);
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(0, 200, 255, 0.03) 0%, transparent 20%),
                radial-gradient(circle at 90% 80%, rgba(255, 62, 62, 0.03) 0%, transparent 20%),
                linear-gradient(to bottom, #0a0e17 0%, #141e30 100%);
            overflow: hidden;
        }

        #assistant-container {
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        #assistant-widget {
            position: absolute;
            width: var(--widget-width);
            background: rgba(20, 30, 48, 0.85);
            border-radius: var(--border-radius);
            box-shadow: 0 0 15px var(--shadow-color);
            z-index: 10000;
            transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
            overflow: hidden;
            max-height: var(--widget-height);
            display: flex;
            flex-direction: column;
            border: 1px solid var(--primary-color);
            backdrop-filter: blur(5px);
        }
        
        #assistant-widget::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-image: var(--hologram-overlay);
            pointer-events: none;
            opacity: 0.5;
            z-index: -1;
        }
        
        #widget-header {
            background: linear-gradient(90deg, var(--secondary-color) 0%, rgba(0, 200, 255, 0.2) 100%);
            color: var(--primary-color);
            padding: 0 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            cursor: move;
            height: var(--header-height);
            flex-shrink: 0;
            border-bottom: 1px solid var(--primary-color);
            position: relative;
        }
        
        #widget-header::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 2px;
            background: var(--primary-color);
            box-shadow: var(--glow-effect);
        }
        
        #widget-title {
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            margin: 0;
            font-size: 18px;
            text-transform: uppercase;
            letter-spacing: 2px;
            text-shadow: 0 0 5px var(--primary-color);
            display: flex;
            align-items: center;
        }
        
        #widget-title::before {
            content: '//';
            margin-right: 8px;
            color: var(--accent-color);
            font-size: 16px;
        }
        
        #widget-controls {
            display: flex;
            gap: 10px;
        }
        
        .widget-button {
            background: none;
            border: 1px solid var(--primary-color);
            color: var(--primary-color);
            cursor: pointer;
            font-size: 16px;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 2px;
            transition: all 0.2s;
            position: relative;
            overflow: hidden;
        }
        
        .widget-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--primary-color);
            opacity: 0;
            transition: opacity 0.2s;
            z-index: -1;
        }
        
        .widget-button:hover {
            color: var(--secondary-color);
            box-shadow: 0 0 5px var(--primary-color);
        }
        
        .widget-button:hover::before {
            opacity: 1;
        }
        
        #widget-body {
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 15px;
            flex-grow: 1;
            background: linear-gradient(135deg, rgba(20, 30, 48, 0.9) 0%, rgba(30, 40, 60, 0.8) 100%);
            position: relative;
        }
        
        #widget-body::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(0, 200, 255, 0.05) 0%, transparent 20%),
                radial-gradient(circle at 90% 80%, rgba(255, 62, 62, 0.05) 0%, transparent 20%);
            pointer-events: none;
        }
        
        #question-input {
            width: 100%;
            padding: 12px;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            box-sizing: border-box;
            resize: vertical;
            min-height: 80px;
            font-family: 'Rajdhani', sans-serif;
            font-size: 16px;
            background-color: rgba(10, 15, 25, 0.7);
            color: var(--text-color);
            transition: all 0.3s;
            position: relative;
        }
        
        #question-input:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 5px var(--primary-color);
        }
        
        #action-select {
            width: 100%;
            padding: 12px;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            box-sizing: border-box;
            font-family: 'Rajdhani', sans-serif;
            font-size: 16px;
            background-color: rgba(10, 15, 25, 0.7);
            color: var(--text-color);
            appearance: none;
            position: relative;
            transition: all 0.3s;
        }
        
        #action-select:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 5px var(--primary-color);
        }
        
        .select-wrapper {
            position: relative;
            width: 100%;
        }
        
        .select-wrapper::after {
            content: '▼';
            position: absolute;
            right: 15px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--primary-color);
            pointer-events: none;
            font-size: 12px;
        }
        
        #ask-button {
            background: linear-gradient(90deg, var(--primary-color) 0%, rgba(0, 150, 255, 0.8) 100%);
            color: var(--secondary-color);
            border: none;
            padding: 12px 15px;
            border-radius: var(--border-radius);
            cursor: pointer;
            width: 100%;
            font-weight: bold;
            font-size: 16px;
            font-family: 'Orbitron', sans-serif;
            letter-spacing: 1px;
            text-transform: uppercase;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }
        
        #ask-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent 0%, rgba(255, 255, 255, 0.3) 50%, transparent 100%);
            transition: all 0.6s;
        }
        
        #ask-button:hover {
            box-shadow: 0 0 10px var(--primary-color);
        }
        
        #ask-button:hover::before {
            left: 100%;
        }
        
        #response-area {
            margin-top: 5px;
            padding: 15px;
            background: rgba(10, 15, 25, 0.7);
            border-radius: var(--border-radius);
            white-space: pre-wrap;
            overflow-y: auto;
            flex-grow: 1;
            font-size: 16px;
            line-height: 1.6;
            border: 1px solid var(--border-color);
            position: relative;
        }
        
        #response-area::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-image: var(--hologram-overlay);
            pointer-events: none;
            opacity: 0.3;
            z-index: -1;
        }
        
        .minimized {
            width: 70px !important;
            height: 70px !important;
            border-radius: 50% !important;
            overflow: hidden;
            box-shadow: 0 0 15px var(--primary-color) !important;
        }
        
        .minimized #widget-body {
            display: none;
        }
        
        .minimized #widget-title {
            display: none;
        }
        
        .minimized #widget-header {
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            background: radial-gradient(circle, var(--primary-color) 0%, var(--secondary-color) 100%);
            border: none;
        }
        
        .minimized #minimize-button {
            display: none;
        }
        
        .minimized #maximize-button {
            display: block !important;
            border: none;
            background: none;
        }
        
        .minimized .widget-button {
            border: none;
        }
        
        #maximize-button {
            display: none;
        }

        .loading {
            display: inline-block;
            width: 24px;
            height: 24px;
            border: 3px solid rgba(0, 200, 255, 0.3);
            border-radius: 50%;
            border-top-color: var(--primary-color);
            animation: spin 1s ease-in-out infinite;
            box-shadow: 0 0 5px var(--primary-color);
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        #loading-indicator {
            display: none;
            margin-right: 10px;
        }
        
        /* HUD elements */
        .hud-corner {
            position: absolute;
            width: 30px;
            height: 30px;
            pointer-events: none;
        }
        
        .hud-corner-tl {
            top: 0;
            left: 0;
            border-top: 2px solid var(--primary-color);
            border-left: 2px solid var(--primary-color);
        }
        
        .hud-corner-tr {
            top: 0;
            right: 0;
            border-top: 2px solid var(--primary-color);
            border-right: 2px solid var(--primary-color);
        }
        
        .hud-corner-bl {
            bottom: 0;
            left: 0;
            border-bottom: 2px solid var(--primary-color);
            border-left: 2px solid var(--primary-color);
        }
        
        .hud-corner-br {
            bottom: 0;
            right: 0;
            border-bottom: 2px solid var(--primary-color);
            border-right: 2px solid var(--primary-color);
        }
        
        /* Scanning effect */
        .scan-line {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: var(--primary-color);
            opacity: 0.7;
            box-shadow: 0 0 8px var(--primary-color);
            animation: scan 3s linear infinite;
            z-index: 1000;
            pointer-events: none;
        }
        
        @keyframes scan {
            0% { top: 0; }
            100% { top: 100%; }
        }
        
        /* Circular HUD elements */
        .hud-circle {
            position: absolute;
            width: 150px;
            height: 150px;
            border-radius: 50%;
            border: 1px solid var(--primary-color);
            opacity: 0.2;
            pointer-events: none;
        }
        
        .hud-circle-1 {
            top: -75px;
            right: -75px;
        }
        
        .hud-circle-2 {
            bottom: -100px;
            left: -50px;
        }
        
        /* Animated tech background */
        .tech-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(to right, rgba(0, 200, 255, 0.05) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(0, 200, 255, 0.05) 1px, transparent 1px);
            background-size: 20px 20px;
            pointer-events: none;
            z-index: -1;
            animation: gridMove 30s linear infinite;
        }
        
        @keyframes gridMove {
            0% { background-position: 0 0; }
            100% { background-position: 20px 20px; }
        }
        
        /* Glowing dots */
        .glow-dot {
            position: absolute;
            width: 4px;
            height: 4px;
            background: var(--primary-color);
            border-radius: 50%;
            box-shadow: 0 0 5px var(--primary-color);
            opacity: 0.7;
            animation: pulse 3s infinite;
        }
        
        .glow-dot-1 {
            top: 20px;
            right: 30px;
            animation-delay: 0s;
        }
        
        .glow-dot-2 {
            top: 40px;
            right: 30px;
            animation-delay: 1s;
        }
        
        .glow-dot-3 {
            top: 60px;
            right: 30px;
            animation-delay: 2s;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }
        
        /* Status indicators */
        .status-indicator {
            position: absolute;
            bottom: 15px;
            right: 15px;
            display: flex;
            gap: 5px;
            pointer-events: none;
        }
        
        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--accent-color);
            animation: statusBlink 2s infinite;
        }
        
        .status-dot:nth-child(2) {
            animation-delay: 0.5s;
        }
        
        .status-dot:nth-child(3) {
            animation-delay: 1s;
        }
        
        @keyframes statusBlink {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        /* Mobile responsiveness */
        @media (max-width: 600px) {
            :root {
                --widget-width: 90vw;
                --widget-height: 80vh;
            }
            
            #assistant-widget {
                position: fixed;
                top: 10vh;
                left: 5vw;
            }
        }

        /* Dark mode support - already dark by default */
        @media (prefers-color-scheme: dark) {
            /* Already optimized for dark mode */
        }
    </style>
</head>
<body>
    <div class="tech-bg"></div>
    <div id="assistant-container">
        <div id="assistant-widget">
            <div class="scan-line"></div>
            <div class="hud-corner hud-corner-tl"></div>
            <div class="hud-corner hud-corner-tr"></div>
            <div class="hud-corner hud-corner-bl"></div>
            <div class="hud-corner hud-corner-br"></div>
            <div class="hud-circle hud-circle-1"></div>
            <div class="hud-circle hud-circle-2"></div>
            
            <div id="widget-header">
                <h3 id="widget-title">n8n Assistant</h3>
                <div class="glow-dot glow-dot-1"></div>
                <div class="glow-dot glow-dot-2"></div>
                <div class="glow-dot glow-dot-3"></div>
                <div id="widget-controls">
                    <div id="loading-indicator" class="loading"></div>
                    <button id="minimize-button" class="widget-button" title="Minimize">−</button>
                    <button id="maximize-button" class="widget-button" title="Maximize">+</button>
                    <button id="close-button" class="widget-button" title="Close">×</button>
                </div>
            </div>
            <div id="widget-body">
                <div class="select-wrapper">
                    <select id="action-select" aria-label="Select action type">
                        <option value="help">Help with workflow creation</option>
                        <option value="debug">Debug existing workflows</option>
                        <option value="analyze">Analyze workflow performance</option>
                        <option value="general">General n8n questions</option>
                    </select>
                </div>
                <textarea id="question-input" placeholder="Ask me anything about n8n..." aria-label="Your question"></textarea>
                <button id="ask-button">Initiate Query</button>
                <div id="response-area" aria-live="polite">Systems online. Ready to assist with n8n workflows.</div>
                <div class="status-indicator">
                    <div class="status-dot"></div>
                    <div class="status-dot"></div>
                    <div class="status-dot"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Configuration - REPLACE WITH YOUR ACTUAL WEBHOOK URL
        const WEBHOOK_URL = 'https://known-cow-yearly.ngrok-free.app/webhook/ai-assistant';
        
        // DOM Elements
        const widget = document.getElementById('assistant-widget');
        const minimizeButton = document.getElementById('minimize-button');
        const maximizeButton = document.getElementById('maximize-button');
        const closeButton = document.getElementById('close-button');
        const askButton = document.getElementById('ask-button');
        const questionInput = document.getElementById('question-input');
        const actionSelect = document.getElementById('action-select');
        const responseArea = document.getElementById('response-area');
        const loadingIndicator = document.getElementById('loading-indicator');
        
        // Add typewriter effect
        function typeWriter(element, text, speed = 30) {
            let i = 0;
            element.textContent = '';
            function type() {
                if (i < text.length) {
                    element.textContent += text.charAt(i);
                    i++;
                    setTimeout(type, speed);
                }
            }
            type();
        }
        
        // Widget position persistence
        let widgetPosition = localStorage.getItem('n8nAssistantPosition');
        if (widgetPosition) {
            try {
                const position = JSON.parse(widgetPosition);
                widget.style.left = position.left;
                widget.style.top = position.top;
                widget.style.right = 'auto';
                widget.style.bottom = 'auto';
            } catch (e) {
                console.error('Error parsing saved position', e);
            }
        }
        
        // Widget state persistence
        const widgetState = localStorage.getItem('n8nAssistantState');
        if (widgetState === 'minimized') {
            widget.classList.add('minimized');
        }
        
        // Minimize/Maximize functionality
        minimizeButton.addEventListener('click', () => {
            widget.classList.add('minimized');
            localStorage.setItem('n8nAssistantState', 'minimized');
        });
        
        maximizeButton.addEventListener('click', () => {
            widget.classList.remove('minimized');
            localStorage.setItem('n8nAssistantState', 'maximized');
        });
        
        // Close functionality
        closeButton.addEventListener('click', () => {
            widget.style.display = 'none';
        });
        
        // Make widget draggable
        const header = document.getElementById('widget-header');
        let isDragging = false;
        let offsetX, offsetY;
        
        header.addEventListener('mousedown', (e) => {
            isDragging = true;
            offsetX = e.clientX - widget.getBoundingClientRect().left;
            offsetY = e.clientY - widget.getBoundingClientRect().top;
        });
        
        document.addEventListener('mousemove', (e) => {
            if (isDragging) {
                const left = (e.clientX - offsetX) + 'px';
                const top = (e.clientY - offsetY) + 'px';
                
                widget.style.left = left;
                widget.style.top = top;
                widget.style.right = 'auto';
                widget.style.bottom = 'auto';
                
                // Save position
                localStorage.setItem('n8nAssistantPosition', JSON.stringify({ left, top }));
            }
        });
        
        document.addEventListener('mouseup', () => {
            isDragging = false;
        });
        
        // Touch support for mobile devices
        header.addEventListener('touchstart', (e) => {
            isDragging = true;
            offsetX = e.touches[0].clientX - widget.getBoundingClientRect().left;
            offsetY = e.touches[0].clientY - widget.getBoundingClientRect().top;
        });
        
        document.addEventListener('touchmove', (e) => {
            if (isDragging) {
                const left = (e.touches[0].clientX - offsetX) + 'px';
                const top = (e.touches[0].clientY - offsetY) + 'px';
                
                widget.style.left = left;
                widget.style.top = top;
                widget.style.right = 'auto';
                widget.style.bottom = 'auto';
                
                // Save position
                localStorage.setItem('n8nAssistantPosition', JSON.stringify({ left, top }));
            }
        });
        
        document.addEventListener('touchend', () => {
            isDragging = false;
        });
        
        // Ask functionality
        askButton.addEventListener('click', async () => {
            const question = questionInput.value.trim();
            const action = actionSelect.value;
            
            if (!question) return;
            
            // Show loading indicator
            loadingIndicator.style.display = 'inline-block';
            askButton.disabled = true;
            responseArea.textContent = "Processing query...";
            
            // Add scanning animation
            const scanLine = document.querySelector('.scan-line');
            scanLine.style.animation = 'scan 1.5s linear infinite';
            
            try {
                const response = await fetch(WEBHOOK_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        action: action,
                        question: question
                    })
                });
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const data = await response.json();
                // Use typewriter effect for response
                typeWriter(responseArea, data.response || "I received a response but couldn't find the answer. Please try again.");
            } catch (error) {
                typeWriter(responseArea, "Error: " + error.message + "\n\nPlease check that your n8n workflow is active and the webhook URL is correct.");
            } finally {
                // Hide loading indicator
                loadingIndicator.style.display = 'none';
                askButton.disabled = false;
                
                // Reset scanning animation
                setTimeout(() => {
                    scanLine.style.animation = 'scan 3s linear infinite';
                }, 1500);
            }
        });
        
        // Allow Enter key to submit
        questionInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                askButton.click();
            }
        });
        
        // History functionality
        let history = JSON.parse(localStorage.getItem('n8nAssistantHistory') || '[]');
        
        function saveToHistory(question, action, response) {
            history.unshift({
                question,
                action,
                response,
                timestamp: new Date().toISOString()
            });
            
            // Limit history to 10 items
            if (history.length > 10) {
                history = history.slice(0, 10);
            }
            
            localStorage.setItem('n8nAssistantHistory', JSON.stringify(history));
        }
        
        // Save responses to history
        askButton.addEventListener('click', () => {
            const question = questionInput.value.trim();
            const action = actionSelect.value;
            
            // Wait for response to be populated
            setTimeout(() => {
                const response = responseArea.textContent;
                if (response && response !== "Processing query...") {
                    saveToHistory(question, action, response);
                }
            }, 1000);
        });
        
        // Add random tech particles
        function createParticles() {
            const container = document.getElementById('assistant-container');
            for (let i = 0; i < 20; i++) {
                const particle = document.createElement('div');
                particle.classList.add('tech-particle');
                particle.style.position = 'absolute';
                particle.style.width = Math.random() * 3 + 'px';
                particle.style.height = Math.random() * 3 + 'px';
                particle.style.backgroundColor = 'rgba(0, 200, 255, ' + (Math.random() * 0.5 + 0.2) + ')';
                particle.style.borderRadius = '50%';
                particle.style.left = Math.random() * 100 + 'vw';
                particle.style.top = Math.random() * 100 + 'vh';
                particle.style.boxShadow = '0 0 ' + (Math.random() * 5 + 2) + 'px rgba(0, 200, 255, 0.7)';
                particle.style.pointerEvents = 'none';
                
                // Animation
                particle.style.animation = 'float ' + (Math.random() * 10 + 5) + 's linear infinite';
                container.appendChild(particle);
            }
        }
        
        // Add floating animation
        const style = document.createElement('style');
        style.textContent = `
            @keyframes float {
                0% { transform: translateY(0) translateX(0); }
                25% { transform: translateY(-10px) translateX(10px); }
                50% { transform: translateY(-20px) translateX(0); }
                75% { transform: translateY(-10px) translateX(-10px); }
                100% { transform: translateY(0) translateX(0); }
            }
        `;
        document.head.appendChild(style);
        
        // Create particles on load
        window.addEventListener('load', createParticles);
        
        // Initialize with typewriter effect
        window.addEventListener('load', () => {
            typeWriter(responseArea, "Systems online. Ready to assist with n8n workflows.");
        });
    </script>
</body>
</html>
