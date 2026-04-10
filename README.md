<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Earnify - Your Earning Hub</title>
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-database-compat.js"></script>
    <!-- Monetag Ad SDK -->
    <script src='https://libtl.com/sdk.js' data-zone='10846482' data-sdk='show_10846482'></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: linear-gradient(145deg, #1A2A3A 0%, #0F1A24 100%);
            font-family: 'Segoe UI', 'Poppins', system-ui, -apple-system, 'Inter', sans-serif;
            padding: 20px 16px 40px;
            min-height: 100vh;
            color: #fff;
        }

        .dashboard {
            max-width: 480px;
            margin: 0 auto;
            background: rgba(255,255,255,0.05);
            backdrop-filter: blur(2px);
            border-radius: 48px;
            padding: 24px 20px;
            box-shadow: 0 20px 35px -12px rgba(0,0,0,0.4);
            border: 1px solid rgba(255,215,0,0.2);
        }

        .earnings-header {
            text-align: center;
            margin-bottom: 28px;
            background: rgba(0,0,0,0.3);
            border-radius: 48px;
            padding: 18px 12px;
        }
        .earnings-header h3 {
            font-weight: 500;
            letter-spacing: 1px;
            font-size: 18px;
            color: #FFE484;
            text-transform: uppercase;
        }
        .total-amount {
            font-size: 56px;
            font-weight: 800;
            margin: 8px 0;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            background: linear-gradient(135deg, #FFF6C9, #FFD966);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        .coin-icon {
            display: inline-block;
            width: 48px;
            height: 48px;
            background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23FFD966"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"/><path d="M12 6c-3.31 0-6 2.69-6 6s2.69 6 6 6 6-2.69 6-6-1.79-4-4-4z"/><circle cx="12" cy="12" r="2"/></svg>') no-repeat center;
            background-size: contain;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
        }
        .options-grid {
            display: flex;
            flex-direction: column;
            gap: 18px;
            margin: 30px 0 20px;
        }
        .option-card {
            background: rgba(20, 30, 42, 0.8);
            border-radius: 32px;
            padding: 18px 22px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255,232,120,0.25);
            transition: all 0.2s ease;
            cursor: pointer;
        }
        .option-card:active { transform: scale(0.97); background: rgba(40,55,75,0.9);}
        .option-left {
            display: flex;
            align-items: center;
            gap: 18px;
        }
        .option-icon { font-size: 38px; }
        .option-text h4 { font-size: 22px; font-weight: 700; }
        .option-text p { font-size: 13px; opacity: 0.7; }
        .arrow { font-size: 28px; opacity: 0.6; }
        .help-btn {
            background: #2c3e50;
            text-align: center;
            margin-top: 15px;
            padding: 14px;
            border-radius: 40px;
            cursor: pointer;
            font-weight: bold;
        }

        .modal-overlay {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0,0,0,0.9);
            backdrop-filter: blur(8px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            visibility: hidden;
            opacity: 0;
            transition: 0.2s;
        }
        .modal-overlay.active {
            visibility: visible;
            opacity: 1;
        }
        .modal-container {
            background: linear-gradient(145deg, #1E2A36, #16212B);
            width: 90%;
            max-width: 400px;
            border-radius: 48px;
            padding: 28px 20px;
            text-align: center;
            border: 1px solid rgba(255,215,0,0.3);
            box-shadow: 0 25px 40px -12px rgba(0,0,0,0.5);
            position: relative;
        }
        .close-modal {
            position: absolute;
            top: 14px; right: 20px;
            font-size: 28px;
            cursor: pointer;
            background: rgba(255,255,255,0.15);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            transition: 0.2s;
        }
        .close-modal:hover { background: rgba(255,255,255,0.3); }
        
        .wheel-container canvas {
            background: #2d3e4e;
            border-radius: 50%;
            width: 240px;
            height: 240px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.5);
        }
        .spin-btn, .ad-button, .btn-primary {
            background: linear-gradient(135deg, #FFB347, #FF8C00);
            border: none;
            padding: 14px 28px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 60px;
            margin: 15px 0;
            color: white;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
        }
        .spin-btn:active, .ad-button:active, .btn-primary:active { transform: scale(0.96); }
        .ad-button { background: linear-gradient(135deg, #2ecc71, #27ae60); }
        .btn-primary { background: linear-gradient(135deg, #FFD966, #FFB347); color: #1e2a2f; }
        
        .scratch-card-wrapper {
            display: flex;
            justify-content: center;
            margin: 10px 0;
        }
        .premium-scratch-card {
            position: relative;
            width: 320px;
            height: 220px;
            border-radius: 28px;
            overflow: hidden;
            cursor: pointer;
            box-shadow: 0 20px 35px -8px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.2);
            transition: transform 0.2s ease;
        }
        .premium-scratch-card:active { transform: scale(0.97); }
        
        .scratch-reveal-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(145deg, #FFD966, #FFA500);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 5;
            border-radius: 28px;
        }
        .scratch-reveal-layer .big-coin {
            width: 60px;
            height: 60px;
            background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23FFD700"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"/><circle cx="12" cy="12" r="5"/></svg>') no-repeat center;
            background-size: contain;
            margin-bottom: 10px;
        }
        .scratch-reveal-layer .win-amount {
            font-size: 52px;
            font-weight: 800;
            background: linear-gradient(135deg, #fff, #FFE484);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: none;
            letter-spacing: 2px;
        }
        .scratch-reveal-layer .win-label {
            font-size: 16px;
            color: rgba(255,255,255,0.95);
            font-weight: 500;
        }
        
        .scratch-canvas-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 10;
            cursor: pointer;
            border-radius: 28px;
        }
        
        .scratch-instruction {
            margin-top: 15px;
            font-size: 13px;
            color: #aaa;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .scratch-instruction span {
            background: rgba(255,255,255,0.1);
            padding: 6px 12px;
            border-radius: 40px;
        }
        
        .claim-btn-wrapper {
            margin-top: 20px;
            animation: fadeInUp 0.4s ease;
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .redeem-fields {
            display: flex;
            flex-direction: column;
            gap: 16px;
            margin: 20px 0;
        }
        .redeem-fields input {
            background: #2c3e44;
            border: none;
            padding: 14px;
            border-radius: 40px;
            color: white;
            font-size: 16px;
            text-align: center;
        }
        .history-list {
            max-height: 300px;
            overflow-y: auto;
            text-align: left;
            margin-top: 15px;
            font-size: 13px;
        }
        .history-item {
            background: #2a3b44;
            margin: 8px 0;
            padding: 10px;
            border-radius: 24px;
        }
        .refer-box {
            background: linear-gradient(135deg, #1e3a2f, #0d261d);
            padding: 16px;
            border-radius: 30px;
            margin-top: 15px;
        }
        .toast-msg {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            background: #222;
            color: #FFD966;
            padding: 10px 24px;
            border-radius: 60px;
            z-index: 1200;
            font-weight: bold;
            display: none;
        }
        button { cursor: pointer; }
        .ad-loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 2px solid #fff;
            border-radius: 50%;
            border-top-color: transparent;
            animation: spin 0.6s linear infinite;
            margin-right: 8px;
            vertical-align: middle;
        }
        @keyframes spin { to { transform: rotate(360deg); } }
        button:disabled { opacity: 0.6; cursor: not-allowed; }
        
        .share-buttons {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            justify-content: center;
            margin: 12px 0;
        }
        .share-btn {
            padding: 10px 18px;
            font-size: 14px;
            border-radius: 40px;
            border: none;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
        }
        .share-btn:active { transform: scale(0.95); }
        .share-telegram { background: #0088cc; color: white; }
        .share-copy { background: #3498db; color: white; }
        .share-whatsapp { background: #25D366; color: white; }
        .refer-link-box {
            background: #2c3e44;
            padding: 12px;
            border-radius: 30px;
            font-size: 14px;
            word-break: break-all;
            margin-top: 10px;
            font-family: monospace;
            font-weight: bold;
            letter-spacing: 1px;
        }
        
        .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            background: #FFD700;
            position: absolute;
            animation: confettiFall 2s ease-out forwards;
            z-index: 2000;
        }
        @keyframes confettiFall {
            0% { transform: translateY(0) rotate(0deg); opacity: 1; }
            100% { transform: translateY(500px) rotate(360deg); opacity: 0; }
        }
        
        /* Account Create Modal */
        .ref-code-input {
            background: #2c3e44;
            border: 2px solid #FFD966;
            padding: 14px;
            border-radius: 40px;
            color: white;
            font-size: 18px;
            text-align: center;
            letter-spacing: 2px;
            margin: 15px 0;
        }
        .skip-btn {
            background: transparent;
            border: 1px solid #FFD966;
            color: #FFD966;
            padding: 10px 20px;
            border-radius: 40px;
            margin-top: 10px;
            cursor: pointer;
        }
        .ad-banner {
            margin-top: 20px;
            text-align: center;
        }
    </style>
</head>
<body>
<div class="dashboard">
    <div class="earnings-header">
        <h3>My Earning</h3>
        <div class="total-amount">
            <span class="coin-icon"></span>
            <span id="totalCoinsDisplay">0.0</span>
        </div>
    </div>
    <div class="options-grid">
        <div class="option-card" data-option="spin">
            <div class="option-left"><div class="option-icon">🎡</div><div class="option-text"><h4>Get Spain</h4><p>Spin & win coins up to 10</p></div></div>
            <div class="arrow">›</div>
        </div>
        <div class="option-card" data-option="scratch">
            <div class="option-left"><div class="option-icon">🎫</div><div class="option-text"><h4>Get Scratch</h4><p>Scratch & win 15-45 coins</p></div></div>
            <div class="arrow">›</div>
        </div>
        <div class="option-card" data-option="redeem">
            <div class="option-left"><div class="option-icon">💸</div><div class="option-text"><h4>Redeem Point</h4><p>Withdraw your coins (Min 10000)</p></div></div>
            <div class="arrow">›</div>
        </div>
        <div class="option-card" data-option="history">
            <div class="option-left"><div class="option-icon">📜</div><div class="option-text"><h4>Redeem History</h4><p>Transactions & Referrals</p></div></div>
            <div class="arrow">›</div>
        </div>
    </div>
    <div class="help-btn" id="helpCentreBtn">🆘 Help Centre</div>
</div>
<div id="toastMsg" class="toast-msg"></div>

<!-- Adstrra Banner -->
<div class="ad-banner" id="adBanner"></div>

<script>
    // Firebase Config
    const firebaseConfig = {
        apiKey: "AIzaSyDnju-cm2fvx8NHFbFHBdg3oqIGULjUvKI",
        authDomain: "tep-2-cdca8.firebaseapp.com",
        databaseURL: "https://tep-2-cdca8-default-rtdb.firebaseio.com",
        projectId: "tep-2-cdca8",
        storageBucket: "tep-2-cdca8.firebasestorage.app",
        messagingSenderId: "361310949782",
        appId: "1:361310949782:web:f6e83f4260519440e88db7"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    // Check if user has account
    let currentUser = null;
    let userCoins = 0;
    let userReferCode = null;
    const MIN_WITHDRAW = 10000;
    
    // Generate random refer code (6 digits)
    function generateReferCode() {
        return Math.random().toString(36).substring(2, 8).toUpperCase();
    }
    
    // Show Account Create Modal First
    function showAccountCreateModal() {
        const modalHTML = `
            <h3>🎉 Welcome to Earnify!</h3>
            <p style="margin: 10px 0;">Create your account to start earning</p>
            <input type="text" id="refCodeInput" class="ref-code-input" placeholder="Enter Refer Code (Optional)">
            <button id="createAccountBtn" class="btn-primary" style="width:100%;">✨ Create Account ✨</button>
            <button id="skipCreateBtn" class="skip-btn">Skip for now</button>
            <p style="font-size:11px; margin-top:15px; opacity:0.6;">Use refer code to get 30 bonus coins!<br>Your referrer will get 60 coins</p>
        `;
        const container = openModal(modalHTML, null, true);
        const createBtn = container.querySelector('#createAccountBtn');
        const skipBtn = container.querySelector('#skipCreateBtn');
        const refInput = container.querySelector('#refCodeInput');
        
        createBtn.onclick = () => {
            const referCode = refInput.value.trim().toUpperCase();
            createNewAccount(referCode);
        };
        
        skipBtn.onclick = () => {
            createNewAccount(null);
        };
    }
    
    // Create New Account
    function createNewAccount(referCode) {
        const userId = "user_" + Date.now() + "_" + Math.random().toString(36).substr(2, 6);
        const newReferCode = generateReferCode();
        
        let bonusCoins = 0;
        let referrerId = null;
        
        // Check if refer code is valid
        if (referCode && referCode.length >= 4) {
            db.ref(`refer_codes`).orderByChild('code').equalTo(referCode).once('value', snap => {
                if (snap.exists()) {
                    snap.forEach(child => {
                        referrerId = child.val().userId;
                    });
                    if (referrerId && referrerId !== userId) {
                        bonusCoins = 30;
                        // Give 60 coins to referrer
                        db.ref(`users/${referrerId}/coins`).once('value', s => {
                            let rc = s.val() || 0;
                            db.ref(`users/${referrerId}/coins`).set(rc + 60);
                            addHistoryForUser(referrerId, "earn", 60, "completed", "Referral bonus from new user");
                        });
                    }
                }
                finalizeAccountCreation(userId, newReferCode, bonusCoins, referrerId);
            });
        } else {
            finalizeAccountCreation(userId, newReferCode, bonusCoins, null);
        }
    }
    
    function finalizeAccountCreation(userId, referCode, bonusCoins, referredBy) {
        const userData = {
            coins: 0 + bonusCoins,
            referCode: referCode,
            referredBy: referredBy,
            createdAt: Date.now(),
            history: []
        };
        
        db.ref(`users/${userId}`).set(userData);
        db.ref(`refer_codes/${referCode}`).set({ userId: userId, code: referCode });
        
        if (bonusCoins > 0) {
            addHistoryForUser(userId, "earn", bonusCoins, "completed", "Welcome bonus from refer code");
        }
        
        localStorage.setItem('earnify_user_id', userId);
        localStorage.setItem('earnify_logged_in', 'true');
        
        currentUser = userId;
        userCoins = bonusCoins;
        userReferCode = referCode;
        
        document.getElementById("totalCoinsDisplay").innerText = userCoins.toFixed(2);
        closeModal();
        showToast(`✅ Account created! You got ${bonusCoins} bonus coins!`);
        
        // Load full user data
        loadUserData();
    }
    
    function addHistoryForUser(uid, type, amount, status, details) {
        const entry = { type, amount, timestamp: Date.now(), status, details };
        db.ref(`users/${uid}/history`).once('value', snap => {
            let arr = snap.val() || [];
            arr.unshift(entry);
            if (arr.length > 50) arr.pop();
            db.ref(`users/${uid}/history`).set(arr);
        });
    }
    
    // Check if user is logged in
    function isUserLoggedIn() {
        const savedUserId = localStorage.getItem('earnify_user_id');
        const isLoggedIn = localStorage.getItem('earnify_logged_in');
        if (savedUserId && isLoggedIn === 'true') {
            currentUser = savedUserId;
            return true;
        }
        return false;
    }
    
    let activeModal = null;
    function closeModal() { if (activeModal) activeModal.classList.remove('active'); activeModal = null; }
    
    function openModal(contentHTML, onClose = null, noClose = false) {
        if (activeModal) closeModal();
        const overlay = document.createElement('div');
        overlay.className = 'modal-overlay active';
        const container = document.createElement('div');
        container.className = 'modal-container';
        container.innerHTML = `<span class="close-modal" ${noClose ? 'style="display:none"' : ''}>&times;</span>${contentHTML}`;
        overlay.appendChild(container);
        document.body.appendChild(overlay);
        activeModal = overlay;
        if (!noClose) {
            const closeBtn = container.querySelector('.close-modal');
            closeBtn.onclick = () => { closeModal(); if (onClose) onClose(); };
        }
        overlay.onclick = (e) => { if (e.target === overlay && !noClose) { closeModal(); if (onClose) onClose(); } };
        return container;
    }
    
    function showToast(text) {
        const toast = document.getElementById("toastMsg");
        toast.innerText = text;
        toast.style.display = "block";
        setTimeout(() => toast.style.display = "none", 2500);
    }
    
    function updateCoins(newBalance, showMsg = false) {
        userCoins = newBalance;
        document.getElementById("totalCoinsDisplay").innerText = userCoins.toFixed(2);
        db.ref(`users/${currentUser}/coins`).set(userCoins);
        localStorage.setItem(`earnify_coins_${currentUser}`, userCoins);
        if (showMsg) showToast(`🎉 Balance updated! ${userCoins.toFixed(2)} coins`);
    }
    
    function loadUserData() {
        db.ref(`users/${currentUser}`).once('value').then(snap => {
            let data = snap.val();
            if (data) {
                userCoins = data.coins || 0;
                userReferCode = data.referCode;
                document.getElementById("totalCoinsDisplay").innerText = userCoins.toFixed(2);
            }
        }).catch(err => console.error("Firebase error:", err));
    }
    
    function addHistory(type, amount, status, details) {
        addHistoryForUser(currentUser, type, amount, status, details);
    }
    
    // ========== AD SYSTEM ==========
    const MONETAG_ZONE = '10846482';
    
    function showRewardedAd(onSuccess, onFailure) {
        showToast("📺 Loading ad...");
        setTimeout(() => {
            onSuccess();
        }, 2000);
    }
    
    // ========== SPIN WHEEL ==========
    function showSpinGame() {
        let spinResultValue = 0;
        const wheelSegments = [0.1, 0.5, 1, 2, 3, 5, 8, 10];
        const canvas = document.createElement('canvas');
        canvas.width = 300;
        canvas.height = 300;
        canvas.style.width = "240px";
        canvas.style.height = "240px";
        const ctx = canvas.getContext('2d');
        let spinning = false;
        let currentAngle = 0;
        
        function drawWheel() {
            const segAng = (Math.PI * 2) / wheelSegments.length;
            for (let i = 0; i < wheelSegments.length; i++) {
                const start = currentAngle + i * segAng;
                const end = start + segAng;
                ctx.beginPath();
                ctx.fillStyle = `hsl(${i * 45}, 70%, 55%)`;
                ctx.moveTo(150, 150);
                ctx.arc(150, 150, 150, start, end);
                ctx.fill();
                ctx.save();
                ctx.translate(150, 150);
                ctx.rotate(start + segAng / 2);
                ctx.fillStyle = "white";
                ctx.font = "bold 16px Poppins";
                ctx.fillText(wheelSegments[i].toFixed(1), 70, 10);
                ctx.restore();
            }
            ctx.beginPath();
            ctx.arc(150, 150, 25, 0, 2 * Math.PI);
            ctx.fillStyle = "#FFD966";
            ctx.fill();
            ctx.stroke();
        }
        drawWheel();
        
        function spinWheel() {
            if (spinning) return;
            spinning = true;
            const spinDuration = 1500;
            const spinDegrees = 360 * 8 + Math.random() * 360;
            let startTime = performance.now();
            let startAngle = currentAngle;
            function animateSpin(now) {
                let elapsed = now - startTime;
                let t = Math.min(1, elapsed / spinDuration);
                let eased = 1 - Math.pow(1 - t, 3);
                let currentDeg = spinDegrees * eased;
                currentAngle = startAngle + (currentDeg * Math.PI / 180);
                ctx.clearRect(0, 0, 300, 300);
                drawWheel();
                if (t < 1) {
                    requestAnimationFrame(animateSpin);
                } else {
                    let finalAngle = currentAngle % (2 * Math.PI);
                    let segAngle = (2 * Math.PI) / wheelSegments.length;
                    let idx = Math.floor(((2 * Math.PI - finalAngle) % (2 * Math.PI)) / segAngle);
                    if (idx >= wheelSegments.length) idx = 0;
                    spinResultValue = wheelSegments[idx];
                    spinning = false;
                    closeModal();
                    showWatchAdToClaim(spinResultValue, "spin");
                }
            }
            requestAnimationFrame(animateSpin);
        }
        
        const modalHTML = `<h3>🎡 SPIN WHEEL</h3><div class="wheel-container"></div><button class="spin-btn" id="spinActionBtn">SPIN NOW</button>`;
        const container = openModal(modalHTML);
        container.querySelector('.wheel-container').appendChild(canvas);
        container.querySelector('#spinActionBtn').onclick = () => spinWheel();
    }
    
    // ========== SCRATCH CARD ==========
    function showScratchGame() {
        let scratchWinAmount = Math.floor(Math.random() * (45 - 15 + 1) + 15);
        let isRevealed = false;
        
        const modalHTML = `
            <h3>🎫 SCRATCH CARD</h3>
            <p style="font-size:13px; opacity:0.8;">Scratch the card to reveal your prize!</p>
            <div class="scratch-card-wrapper">
                <div class="premium-scratch-card" id="scratchCard">
                    <div class="scratch-reveal-layer" id="scratchReveal" style="opacity:0;">
                        <div class="big-coin"></div>
                        <div class="win-amount">${scratchWinAmount}</div>
                        <div class="win-label">COINS</div>
                    </div>
                    <canvas class="scratch-canvas-layer" id="scratchCanvas"></canvas>
                </div>
            </div>
            <div class="scratch-instruction" id="scratchProgress">
                <span>👉 Use your finger to scratch 👈</span>
            </div>
            <div id="claimButtonContainer" style="min-height: 70px;"></div>
        `;
        
        const container = openModal(modalHTML);
        const canvas = container.querySelector('#scratchCanvas');
        const revealDiv = container.querySelector('#scratchReveal');
        const progressDiv = container.querySelector('#scratchProgress');
        const claimContainer = container.querySelector('#claimButtonContainer');
        
        canvas.width = 320;
        canvas.height = 220;
        canvas.style.width = "100%";
        canvas.style.height = "100%";
        
        const ctx = canvas.getContext('2d');
        
        function drawScratchSurface() {
            const grad = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
            grad.addColorStop(0, '#B8B8B8');
            grad.addColorStop(0.3, '#D4D4D4');
            grad.addColorStop(0.6, '#A8A8A8');
            grad.addColorStop(1, '#888888');
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = '#999';
            for (let i = 0; i < 300; i++) {
                ctx.fillRect(Math.random() * canvas.width, Math.random() * canvas.height, 2, 2);
            }
            
            ctx.font = 'bold 28px "Poppins", "Segoe UI"';
            ctx.textAlign = 'center';
            ctx.fillStyle = '#FFD700';
            ctx.fillText('SCRATCH', canvas.width/2, canvas.height/2);
            
            ctx.strokeStyle = '#FFD700';
            ctx.lineWidth = 3;
            ctx.strokeRect(5, 5, canvas.width - 10, canvas.height - 10);
        }
        drawScratchSurface();
        
        let drawing = false;
        let lastX = 0, lastY = 0;
        
        function getCoords(e) {
            const rect = canvas.getBoundingClientRect();
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            let clientX, clientY;
            if (e.touches) {
                clientX = e.touches[0].clientX;
                clientY = e.touches[0].clientY;
            } else {
                clientX = e.clientX;
                clientY = e.clientY;
            }
            let x = (clientX - rect.left) * scaleX;
            let y = (clientY - rect.top) * scaleY;
            return { x: Math.max(0, Math.min(canvas.width, x)), y: Math.max(0, Math.min(canvas.height, y)) };
        }
        
        function scratch(e) {
            if (isRevealed) return;
            e.preventDefault();
            const { x, y } = getCoords(e);
            
            ctx.globalCompositeOperation = "destination-out";
            ctx.beginPath();
            ctx.arc(x, y, 18, 0, Math.PI * 2);
            ctx.fill();
            
            if (lastX !== 0 && lastY !== 0) {
                const dist = Math.hypot(x - lastX, y - lastY);
                if (dist < 35) {
                    ctx.beginPath();
                    ctx.lineWidth = 36;
                    ctx.lineCap = 'round';
                    ctx.moveTo(lastX, lastY);
                    ctx.lineTo(x, y);
                    ctx.stroke();
                }
            }
            lastX = x;
            lastY = y;
            
            const imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            let erasedPixels = 0;
            for (let i = 3; i < imgData.data.length; i += 4) {
                if (imgData.data[i] < 50) erasedPixels++;
            }
            const percent = (erasedPixels / (canvas.width * canvas.height)) * 100;
            
            if (percent >= 28 && !isRevealed) {
                isRevealed = true;
                revealDiv.style.opacity = "1";
                progressDiv.innerHTML = '<span>🎉 CONGRATULATIONS! 🎉</span>';
                
                claimContainer.innerHTML = `
                    <div class="claim-btn-wrapper">
                        <button id="claimRewardBtn" class="ad-button" style="width:100%; padding:14px; font-size:18px;">
                            🎁 CLAIM ${scratchWinAmount} COINS 🎁
                        </button>
                        <p style="font-size:11px; margin-top:8px; opacity:0.6;">Watch a short ad to claim your prize</p>
                    </div>
                `;
                const claimBtn = claimContainer.querySelector('#claimRewardBtn');
                claimBtn.onclick = () => {
                    closeModal();
                    showWatchAdToClaim(scratchWinAmount, "scratch");
                };
            }
        }
        
        function startScratch(e) {
            drawing = true;
            lastX = 0;
            lastY = 0;
            scratch(e);
        }
        
        function endScratch() {
            drawing = false;
            lastX = 0;
            lastY = 0;
        }
        
        canvas.addEventListener('mousemove', (e) => { if (drawing) scratch(e); });
        canvas.addEventListener('mousedown', startScratch);
        canvas.addEventListener('mouseup', endScratch);
        canvas.addEventListener('touchmove', (e) => { e.preventDefault(); if (drawing) scratch(e); });
        canvas.addEventListener('touchstart', (e) => { e.preventDefault(); startScratch(e); });
        canvas.addEventListener('touchend', endScratch);
    }
    
    // ========== WATCH AD & CLAIM ==========
    function showWatchAdToClaim(amount, source) {
        const modalHTML = `
            <h3>🎉 You Won ${amount.toFixed(2)} Coins!</h3>
            <div style="font-size:48px; margin:15px 0;">💰</div>
            <p style="margin: 10px 0;">Watch an ad to claim your reward</p>
            <button id="watchAdClaimBtn" class="ad-button" style="min-width: 200px;">📺 Watch Ad & Claim</button>
        `;
        const container = openModal(modalHTML);
        const watchBtn = container.querySelector('#watchAdClaimBtn');
        
        watchBtn.onclick = () => {
            const originalText = watchBtn.innerHTML;
            watchBtn.innerHTML = '<span class="ad-loading"></span> Loading Ad...';
            watchBtn.disabled = true;
            
            showRewardedAd(
                () => {
                    let newBal = userCoins + amount;
                    updateCoins(newBal, true);
                    addHistory("earn", amount, "completed", `${source === "spin" ? "Spin wheel" : "Scratch card"} win`);
                    closeModal();
                    showToast(`✨ You earned ${amount.toFixed(2)} coins!`);
                },
                (err) => {
                    watchBtn.innerHTML = originalText;
                    watchBtn.disabled = false;
                    showToast("⚠️ Ad failed to load. Please try again.");
                }
            );
        };
    }
    
    // ========== REDEEM (Min 10000 Coins) ==========
    function showRedeem() {
        if (userCoins < MIN_WITHDRAW) {
            showToast(`⚠️ Minimum withdrawal is ${MIN_WITHDRAW} coins! You have ${userCoins.toFixed(2)} coins`);
            return;
        }
        
        const modalHTML = `
            <h3>💸 Redeem Points</h3>
            <p>Current Balance: <strong>${userCoins.toFixed(2)}</strong> Coins</p>
            <p style="color:#FFD966; font-size:12px;">Minimum withdrawal: ${MIN_WITHDRAW} coins</p>
            <div class="redeem-fields">
                <input type="text" id="binaryUID" placeholder="Your Binance UID" />
                <input type="number" id="redeemAmount" placeholder="Amount (Coins)" />
            </div>
            <button id="confirmRedeemBtn" class="btn-primary">Request Withdrawal</button>
        `;
        const container = openModal(modalHTML);
        container.querySelector('#confirmRedeemBtn').onclick = () => {
            const uid = container.querySelector('#binaryUID').value.trim();
            const amount = parseFloat(container.querySelector('#redeemAmount').value);
            if (!uid) { showToast("Enter Binance UID"); return; }
            if (isNaN(amount) || amount <= 0) { showToast("Enter valid amount"); return; }
            if (amount < MIN_WITHDRAW) { showToast(`Minimum withdrawal is ${MIN_WITHDRAW} coins!`); return; }
            if (amount > userCoins) { showToast("Insufficient balance"); return; }
            
            const request = {
                userId: currentUser,
                uid: uid,
                points: amount,
                timestamp: Date.now(),
                status: "pending",
                username: "user"
            };
            db.ref(`admin/requests`).push(request).then(() => {
                addHistory("withdraw_request", amount, "pending", `Binance UID: ${uid}`);
                showToast("✅ Withdrawal request sent!");
                closeModal();
            }).catch(() => showToast("Error sending request"));
        };
    }
    
    // ========== HISTORY & REFERRAL (With Refer Code System) ==========
    function showHistoryAndRefer() {
        const shareText = `🎁 JOIN EARNIFY & GET 30 BONUS COINS! 🎁\n\n💰 Earn coins by spinning wheel & scratching cards!\n💸 Withdraw to Binance!\n\n🔑 Use my Refer Code: ${userReferCode}\n\nDownload App: ${window.location.href}`;
        
        const modalHTML = `
            <h3>📜 History & Referrals</h3>
            <div class="refer-box">
                🤝 <strong>REFER & EARN 60 COINS</strong><br/>
                <p style="font-size:12px; margin:8px 0;">Share your refer code with friends. When they join, you get 60 coins and they get 30 coins!</p>
                <div class="share-buttons">
                    <button id="copyCodeBtn" class="share-btn share-copy">📋 Copy Code</button>
                    <button id="shareTelegramBtn" class="share-btn share-telegram">📲 Telegram</button>
                    <button id="shareWhatsAppBtn" class="share-btn share-whatsapp">💬 WhatsApp</button>
                </div>
                <div class="refer-link-box" id="referLinkDisplay">
                    🔑 YOUR REFER CODE: <span style="color:#FFD966; font-size:18px;">${userReferCode}</span>
                </div>
                <p style="font-size:10px; margin-top:10px; opacity:0.7;">⭐ Share this code - Friend gets 30 coins, You get 60 coins!</p>
            </div>
            <div id="historyContainer" class="history-list">Loading...</div>
        `;
        
        const container = openModal(modalHTML);
        
        db.ref(`users/${currentUser}/history`).once('value', snap => {
            let list = snap.val() || [];
            if (list.length === 0) {
                container.querySelector('#historyContainer').innerHTML = "<div class='history-item'>No transactions yet</div>";
            } else {
                container.querySelector('#historyContainer').innerHTML = list.slice(0, 20).map(h => 
                    `<div class='history-item'>${new Date(h.timestamp).toLocaleString()} | ${h.type === "earn" ? "➕ Earned" : "➖ Withdraw"} ${h.amount} | ${h.status}</div>`
                ).join('');
            }
        });
        
        container.querySelector('#copyCodeBtn').onclick = () => {
            navigator.clipboard.writeText(userReferCode).then(() => {
                showToast("✅ Refer code copied!");
            }).catch(() => {
                prompt("Copy this code:", userReferCode);
            });
        };
        
        container.querySelector('#shareTelegramBtn').onclick = () => {
            const tgUrl = `https://t.me/share/url?url=${encodeURIComponent(window.location.href)}&text=${encodeURIComponent(shareText)}`;
            window.open(tgUrl, '_blank');
        };
        
        container.querySelector('#shareWhatsAppBtn').onclick = () => {
            const waUrl = `https://wa.me/?text=${encodeURIComponent(shareText)}`;
            window.open(waUrl, '_blank');
        };
    }
    
    function goToHelp() { 
        window.open("https://t.me/vuy903", "_blank");
    }
    
    // Load Adstrra Banner
    function loadAdstrraBanner() {
        const bannerDiv = document.getElementById('adBanner');
        const script1 = document.createElement('script');
        script1.innerHTML = `
            atOptions = {
                'key' : '9e1f4918a385b8df12957dbcb4ec7d18',
                'format' : 'iframe',
                'height' : 50,
                'width' : 320,
                'params' : {}
            };
        `;
        const script2 = document.createElement('script');
        script2.src = "https://www.highperformanceformat.com/9e1f4918a385b8df12957dbcb4ec7d18/invoke.js";
        bannerDiv.appendChild(script1);
        bannerDiv.appendChild(script2);
    }
    
    // Event Listeners
    document.querySelectorAll('.option-card').forEach(card => {
        card.addEventListener('click', () => {
            if (!currentUser) {
                showToast("Please create account first!");
                showAccountCreateModal();
                return;
            }
            const opt = card.dataset.option;
            if (opt === 'spin') showSpinGame();
            else if (opt === 'scratch') showScratchGame();
            else if (opt === 'redeem') showRedeem();
            else if (opt === 'history') showHistoryAndRefer();
        });
    });
    document.getElementById('helpCentreBtn').onclick = goToHelp;
    
    // Initialize
    if (isUserLoggedIn()) {
        loadUserData();
    } else {
        showAccountCreateModal();
    }
    
    loadAdstrraBanner();
</script>
</body>
</html>
