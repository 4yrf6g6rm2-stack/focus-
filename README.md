<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tactical Gaming VR Helmet</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
        background: #0a0a12;
        color: #fff;
        overflow: hidden;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
    }

    /* Subtle animated glow */
    body::before {
        content: '';
        position: absolute;
        inset: 0;
        background: radial-gradient(circle at center, rgba(255,0,60,0.08), transparent 70%);
        animation: pulseGlow 6s infinite;
        z-index: 0;
    }

    @keyframes pulseGlow {
        0%, 100% { opacity: 0.3; }
        50% { opacity: 0.8; }
    }

    .container {
        width: 100%;
        max-width: 1200px;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 30px;
        padding: 20px;
        position: relative;
        z-index: 1;
    }

    .helmet-container {
        position: relative;
        width: 600px;
        height: 400px;
        perspective: 1000px;
    }

    .helmet {
        position: relative;
        width: 100%;
        height: 100%;
        transform-style: preserve-3d;
        transition: transform 0.5s ease;
    }

    .helmet-face {
        position: absolute;
        width: 100%;
        height: 100%;
        background: linear-gradient(135deg, #1a1a2a 0%, #0a0a12 100%);
        border-radius: 20px;
        overflow: hidden;
        box-shadow: 0 0 30px rgba(255, 0, 60, 0.5);
        border: 2px solid #ff003c;
    }

    .helmet-face::before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background:
            radial-gradient(circle at 30% 40%, rgba(255, 0, 60, 0.1) 0%, transparent 50%),
            radial-gradient(circle at 70% 60%, rgba(0, 229, 255, 0.1) 0%, transparent 50%);
        z-index: 1;
    }

    .helmet-lens {
        position: absolute;
        top: 25%;
        left: 10%;
        width: 80%;
        height: 50%;
        background: linear-gradient(135deg, rgba(0, 20, 40, 0.8) 0%, rgba(0, 40, 80, 0.9) 100%);
        border-radius: 10px;
        border: 2px solid #00e5ff;
        z-index: 2;
        overflow: hidden;
        box-shadow: 0 0 20px rgba(0, 229, 255, 0.5);
    }

    .hud-display {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        padding: 20px;
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-template-rows: 50px 1fr 50px;
        gap: 15px;
        z-index: 3;
    }

    .hud-header, .hud-footer {
        grid-column: 1 / 3;
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid rgba(0, 229, 255, 0.5);
        padding-bottom: 10px;
    }

    .hud-footer {
        border-bottom: none;
        border-top: 1px solid rgba(0, 229, 255, 0.5);
        padding-top: 10px;
    }

    .network-status, .battery, .help-prompt {
        display: flex;
        align-items: center;
        gap: 8px;
        font-size: 14px;
        color: #00e5ff;
    }

    .battery i { color: #00ff88; }

    .time {
        font-size: 18px;
        font-weight: bold;
    }

    .hud-left, .hud-right {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }

    .system-info, .threat-radar {
        background: rgba(0, 20, 40, 0.6);
        border-radius: 8px;
        padding: 10px;
    }

    .system-info { border-left: 3px solid #ff003c; }
    .threat-radar { border: 1px solid #00e5ff; align-items: center; display: flex; flex-direction: column; }

    .system-title { font-size: 12px; color: #00e5ff; margin-bottom: 5px; }
    .system-value { font-size: 16px; font-weight: bold; }

    .radar {
        width: 120px;
        height: 120px;
        border-radius: 50%;
        border: 2px solid #00e5ff;
        position: relative;
        margin: 10px 0;
    }

    .radar-sweep {
        position: absolute;
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background: conic-gradient(transparent, #00e5ff, transparent);
        animation: sweep 4s linear infinite;
    }

    @keyframes sweep { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }

    .radar-dot {
        position: absolute;
        width: 6px;
        height: 6px;
        background: #ff003c;
        border-radius: 50%;
        transform: translate(-50%, -50%);
        animation: blink 1.5s infinite alternate;
    }

    @keyframes blink {
        0% { opacity: 1; transform: scale(1); }
        100% { opacity: 0.4; transform: scale(1.3); }
    }

    .branding { display: flex; align-items: center; gap: 15px; }
    .rog-brand { font-size: 18px; font-weight: 900; background: linear-gradient(to right, #ff003c, #ff8a00); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
    .atomic-brand { font-size: 14px; color: #00e5ff; }

    .helmet-body, .helmet-top, .helmet-bottom, .vent {
        position: absolute;
    }

    .helmet-body { top: 0; left: 0; width: 100%; height: 100%; z-index: 1; }
    .helmet-top {
        top: 0; left: 10%; width: 80%; height: 20%;
        background: linear-gradient(to bottom, #2a2a3a 0%, #1a1a2a 100%);
        border-radius: 10px 10px 0 0;
        border: 1px solid #ff003c; border-bottom: none;
    }

    .helmet-bottom {
        bottom: 0; left: 15%; width: 70%; height: 15%;
        background: linear-gradient(to top, #2a2a3a 0%, #1a1a2a 100%);
        border-radius: 0 0 10px 10px;
        border: 1px solid #ff003c; border-top: none;
    }

    .vent {
        width: 80%; height: 8px;
        background: repeating-linear-gradient(90deg, #ff003c, #ff003c 5px, transparent 5px, transparent 10px);
        bottom: 25%; left: 10%; opacity: 0.7;
    }

    .specs-panel {
        display: flex;
        gap: 30px;
        background: rgba(20, 20, 30, 0.7);
        border-radius: 10px;
        padding: 20px;
        border: 1px solid #ff003c;
        box-shadow: 0 0 15px rgba(255, 0, 60, 0.3);
        flex-wrap: wrap;
        justify-content: center;
    }

    .spec-item { display: flex; flex-direction: column; align-items: center; gap: 8px; }
    .spec-icon { font-size: 24px; color: #00e5ff; }
    .spec-label { font-size: 14px; color: #aaa; }
    .spec-value { font-size: 18px; font-weight: bold; }

    .scan-line {
        position: absolute;
        top: 0; left: 0; width: 100%; height: 100%;
        background: linear-gradient(to bottom, transparent 50%, rgba(0, 255, 255, 0.03) 50%);
        background-size: 100% 4px;
        z-index: 4;
        pointer-events: none;
        animation: scan 4s linear infinite;
    }

    @keyframes scan {
        0% { transform: translateY(-100%); }
        100% { transform: translateY(100%); }
    }

    .logo {
        position: absolute;
        bottom: 20px;
        right: 20px;
        font-size: 12px;
        color: #00e5ff;
        z-index: 5;
    }

    /* Mobile optimization */
    @media (max-width: 768px) {
        .helmet-container { width: 90%; height: auto; }
        .helmet-lens { height: 40%; }
        .specs-panel { gap: 15px; }
    }
</style>
</head>
<body>
<div class="container">
    <div class="helmet-container">
        <div class="helmet">
            <div class="helmet-face">
                <div class="helmet-body">
                    <div class="helmet-top"></div>
                    <div class="helmet-bottom"></div>
                    <div class="vent"></div>
                </div>
                <div class="helmet-lens">
                    <div class="hud-display">
                        <div class="hud-header">
                            <div class="network-status"><i class="fas fa-signal"></i><span>Verizon 5G</span></div>
                            <div class="time">3:02 AM</div>
                            <div class="battery"><i class="fas fa-battery-half"></i><span>52%</span></div>
                        </div>

                        <div class="hud-left">
                            <div class="system-info">
                                <div class="system-title">SYSTEM STATUS</div>
                                <div class="system-value">OPERATIONAL</div>
                            </div>
                            <div class="system-info">
                                <div class="system-title">THREAT LEVEL</div>
                                <div class="system-value">ELEVATED</div>
                            </div>
                            <div class="system-info">
                                <div class="system-title">MISSION TIME</div>
                                <div class="system-value">04:32:17</div>
                            </div>
                        </div>

                        <div class="hud-right">
                            <div class="threat-radar">
                                <div class="system-title">THREAT RADAR</div>
                                <div class="radar">
                                    <div class="radar-sweep"></div>
                                </div>
                                <div class="system-value radar-count">2 THREATS DETECTED</div>
                            </div>
                        </div>

                        <div class="hud-footer">
                            <div class="help-prompt"><i class="fas fa-question-circle"></i><span>Need help?</span></div>
                            <div class="branding">
                                <div class="rog-brand">ROG STRIX G18</div>
                                <div class="atomic-brand">ATOMIC DEFENSE HUD</div>
                            </div>
                        </div>
                    </div>
                    <div class="scan-line"></div>
                </div>
                <div class="logo">rog.asus.com | atomicdefense.com</div>
            </div>
        </div>
    </div>

    <div class="specs-panel">
        <div class="spec-item"><div class="spec-icon"><i class="fas fa-microchip"></i></div><div class="spec-label">PROCESSOR</div><div class="spec-value">Intel i9-14900K</div></div>
        <div class="spec-item"><div class="spec-icon"><i class="fas fa-memory"></i></div><div class="spec-label">MEMORY</div><div class="spec-value">64GB DDR5</div></div>
        <div class="spec-item"><div class="spec-icon"><i class="fas fa-vr-cardboard"></i></div><div class="spec-label">DISPLAY</div><div class="spec-value">4K OLED x2</div></div>
        <div class="spec-item"><div class="spec-icon"><i class="fas fa-satellite-dish"></i></div><div class="spec-label">CONNECTIVITY</div><div class="spec-value">5G + Wi-Fi 6E</div></div>
    </div>
</div>

<script>
    // Time updater
    function updateTime() {
        const now = new Date();
        const timeString = now.toLocaleTimeString('en-US', { hour12: true, hour: 'numeric', minute: '2-digit' });
        document.querySelector('.time').textContent = timeString;
    }
    setInterval(updateTime, 1000);
    updateTime();

    // Helmet motion
    const helmet = document.querySelector('.helmet');
    helmet.addEventListener('mousemove', (e) => {
        const xAxis = (window.innerWidth / 2 - e.pageX) / 25;
        const yAxis = (window.innerHeight / 2 - e.pageY) / 25;
        helmet.style.transform = `rotateY(${xAxis}deg) rotateX(${yAxis}deg)`;
    });
    helmet.addEventListener('mouseenter', () => helmet.style.transition = 'none');
    helmet.addEventListener('mouseleave', () => {
        helmet.style.transition = 'transform 0.5s ease';
        helmet.style.transform = 'rotateY(0deg) rotateX(0deg)';
    });

    // Simulate battery drain
    let batteryLevel = 52;
    setInterval(() => {
        if (batteryLevel > 5) {
            batteryLevel -= 0.1;
            document.querySelector('.battery span').textContent = Math.round(batteryLevel) + '%';
            const batteryIcon = document.querySelector('.battery i');
            if (batteryLevel < 20) { batteryIcon.className = 'fas fa-battery-quarter'; batteryIcon.style.color = '#ff003c'; }
            else if (batteryLevel < 50) { batteryIcon.className = 'fas fa-battery-half'; batteryIcon.style.color = '#ffcc00'; }
        }
    }, 10000);

    // Dynamic radar threats
    function updateThreats() {
        const radar = document.querySelector('.radar');
        const countDisplay = document.querySelector('.radar-count');
        const threatCount = Math.floor(Math.random() * 4) + 1;
        countDisplay.textContent = `${threatCount} THREAT${threatCount > 1 ? 'S' : ''} DETECTED`;
        radar.querySelectorAll('.radar-dot').forEach(dot => dot.remove());
        for (let i = 0; i < threatCount; i++) {
            const dot = document.createElement('div');
            dot.className = 'radar-dot';
            dot.style.top = `${Math.random() * 80 + 10}%`;
            dot.style.left = `${Math.random() * 80 + 10}%`;
            radar.appendChild(dot);
        }
    }
    setInterval(updateThreats, 5000);
    updateThreats();
</script>
</body>
</html>
