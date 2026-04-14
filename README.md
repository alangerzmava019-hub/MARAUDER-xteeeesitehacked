<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>MARAUDER | SECURE CHAT</title>
    <script type="importmap">
        {
            "imports": {
                "firebase/app": "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js",
                "firebase/auth": "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js",
                "firebase/database": "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js"
            }
        }
    </script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Share Tech Mono', 'Courier New', monospace;
        }
        body {
            background: #000000;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            position: relative;
        }
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: repeating-linear-gradient(0deg, rgba(0,255,102,0.03) 0px, rgba(0,255,102,0.03) 2px, transparent 2px, transparent 6px);
            pointer-events: none;
            z-index: 10;
        }
        .app-container {
            width: 100%;
            max-width: 700px;
            background: #0a0a0a;
            border: 2px solid #00ff66;
            border-radius: 24px;
            box-shadow: 0 0 30px rgba(0,255,102,0.3);
            overflow: hidden;
            position: relative;
            z-index: 20;
        }
        .hacker-header {
            background: #000000;
            border-bottom: 1px solid #00ff66;
            padding: 16px 20px;
            text-align: center;
        }
        .glitch-title {
            font-size: 1.8rem;
            font-weight: bold;
            color: #00ff66;
            text-shadow: 0 0 8px #00ff66;
            letter-spacing: 4px;
            min-height: 70px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .status-bar {
            display: flex;
            justify-content: space-between;
            font-size: 0.6rem;
            color: #00aa44;
            margin-top: 8px;
            border-top: 1px dashed #00ff6633;
            padding-top: 6px;
        }
        .content {
            padding: 20px;
            min-height: 500px;
        }
        .terminal-input {
            background: #0a0a0a;
            border: 1px solid #00ff66;
            color: #00ff66;
            padding: 12px 14px;
            border-radius: 12px;
            font-size: 13px;
            width: 100%;
            outline: none;
            font-family: monospace;
        }
        .terminal-input:focus {
            box-shadow: 0 0 12px #00ff66;
        }
        .btn {
            background: #00ff66;
            border: none;
            padding: 12px 18px;
            border-radius: 12px;
            font-size: 13px;
            font-weight: bold;
            color: #000000;
            cursor: pointer;
            transition: all 0.2s;
            text-align: center;
            width: 100%;
            font-family: monospace;
        }
        .btn:hover {
            background: #00cc55;
            box-shadow: 0 0 15px #00ff66;
            transform: scale(0.98);
        }
        .btn-secondary {
            background: #111111;
            border: 1px solid #00ff66;
            color: #00ff66;
        }
        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }
        .captcha-container {
            background: #050505;
            border: 1px solid #00ff66;
            border-radius: 16px;
            padding: 14px;
            margin-bottom: 16px;
        }
        .floating-word {
            font-size: 1.8rem;
            letter-spacing: 8px;
            text-align: center;
            color: #00ff66;
            font-weight: bold;
            background: #000000;
            padding: 16px;
            border-radius: 12px;
            margin-bottom: 12px;
            animation: float 2s ease-in-out infinite;
        }
        @keyframes float {
            0%,100% { transform: translateY(0px); letter-spacing: 8px; }
            50% { transform: translateY(-3px); letter-spacing: 10px; }
        }
        .target-word {
            text-align: center;
            font-size: 1.2rem;
            letter-spacing: 6px;
            color: #00ff66;
            background: #000000;
            padding: 12px;
            border-radius: 12px;
            margin-bottom: 12px;
            font-weight: bold;
        }
        .puzzle-area {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin: 15px 0;
            flex-wrap: wrap;
        }
        .puzzle-slot {
            background: #0a0a0a;
            border: 2px dashed #00ff66;
            border-radius: 12px;
            padding: 12px 16px;
            min-width: 60px;
            text-align: center;
            color: #00ff66;
            cursor: pointer;
            font-size: 1.2rem;
            font-weight: bold;
        }
        .puzzle-slot:hover {
            background: #00ff6611;
            border-style: solid;
        }
        .puzzle-piece {
            background: #00ff66;
            color: #000000;
            padding: 12px 16px;
            border-radius: 12px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.2rem;
        }
        .puzzle-piece:hover {
            background: #00cc55;
            transform: scale(0.96);
        }
        .auth-form {
            margin-top: 16px;
            padding-top: 12px;
            border-top: 1px solid #00ff6633;
            animation: fadeIn 0.5s ease;
        }
        .success-msg {
            color: #00ff66;
            text-align: center;
            padding: 12px;
            margin-bottom: 16px;
            background: #00ff6611;
            border-radius: 12px;
            border: 1px solid #00ff66;
        }
        .hidden { display: none !important; }
        .room-card {
            background: #050505;
            border-radius: 16px;
            padding: 16px;
            margin-bottom: 16px;
            border: 1px solid #00ff66;
        }
        .room-code {
            font-size: 28px;
            font-weight: bold;
            font-family: monospace;
            letter-spacing: 4px;
            color: #00ff66;
            text-align: center;
            margin: 10px 0;
            word-break: break-all;
        }
        .status-text {
            font-size: 13px;
            padding: 10px;
            border-radius: 12px;
            text-align: center;
            margin: 10px 0;
        }
        .waiting {
            background: rgba(255,193,7,0.15);
            color: #ffc107;
            border: 1px solid rgba(255,193,7,0.3);
        }
        .connected {
            background: rgba(76,175,80,0.15);
            color: #4caf50;
            border: 1px solid rgba(76,175,80,0.3);
        }
        .device-badge {
            font-size: 10px;
            color: #00aa66;
            background: #0a0a0a;
            padding: 2px 8px;
            border-radius: 20px;
            margin-left: 8px;
        }
        .chat-messages {
            height: 280px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 8px;
            padding: 8px;
            background: #010101;
            border-radius: 16px;
            border: 1px solid #00ff6633;
            margin-bottom: 12px;
        }
        .message {
            display: flex;
            flex-direction: column;
            max-width: 85%;
            animation: fadeIn 0.2s ease;
        }
        .message.own {
            align-items: flex-end;
            align-self: flex-end;
        }
        .message-bubble {
            background: #0c0c0c;
            border: 1px solid #00ff66;
            padding: 8px 12px;
            border-radius: 16px;
            border-bottom-left-radius: 4px;
        }
        .message.own .message-bubble {
            background: #0a1f12;
            border-bottom-right-radius: 4px;
            border-bottom-left-radius: 16px;
        }
        .message-text {
            font-size: 12px;
            color: #ccffcc;
            word-break: break-word;
        }
        .message-meta {
            font-size: 8px;
            color: #00aa66;
            margin-top: 3px;
            margin-left: 6px;
            margin-right: 6px;
        }
        .input-row {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }
        .input-row input {
            flex: 1;
            min-width: 120px;
        }
        .back-btn {
            background: #220000;
            border-color: #ff6666;
            color: #ff6666;
            margin-bottom: 12px;
            width: auto;
            padding: 8px 16px;
        }
        .players-info {
            font-size: 12px;
            margin-top: 10px;
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 8px;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(5px); }
            to { opacity: 1; transform: translateY(0); }
        }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #0a0a0a; }
        ::-webkit-scrollbar-thumb { background: #00ff66; border-radius: 10px; }
        .error-msg { color: #ff6666; font-size: 12px; margin-top: 8px; text-align: center; }
        .info-msg { color: #00aa66; font-size: 12px; margin-top: 8px; text-align: center; }
    </style>
</head>
<body>
<div class="app-container" id="appRoot">
    <div class="hacker-header">
        <div class="glitch-title" id="animatedTitle">MARAUDER</div>
        <div class="status-bar">
            <span>[ SECURE ]</span>
            <span>[ AES-GCM ]</span>
            <span>[ 1v1 ROOMS ]</span>
        </div>
    </div>
    <div class="content" id="contentArea"></div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
    import { getDatabase, ref, set, onValue, push, off, get, remove, update } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

    // ========== FIREBASE CONFIG (ЗАМЕНИТЕ НА ВАШУ) ==========
    const firebaseConfig = {
        apiKey: "AIzaSyB5F5R5tL9zEf7X9k8wJq3Vp0yLmN2qR6sT",
        authDomain: "marauder-arena-demo.firebaseapp.com",
        projectId: "marauder-arena-demo",
        storageBucket: "marauder-arena-demo.appspot.com",
        messagingSenderId: "123456789012",
        appId: "1:123456789012:web:demo123456",
        databaseURL: "https://marauder-arena-demo-default-rtdb.firebaseio.com"
    };
    
    // ========== ГЛОБАЛЬНЫЕ ПЕРЕМЕННЫЕ ==========
    let app, auth, db;
    let firebaseUser = null;
    let currentUserId = null;
    let currentNickname = null;
    let currentRoomId = null;
    let currentIsHost = false;
    let cryptoKey = null;
    let roomUnsubscribe = null;
    let messagesUnsubscribe = null;
    let titleAnimationTimer = null;
    let titleTimeout = null;
    
    // ========== АНИМАЦИЯ ЗАГОЛОВКА ==========
    const titlePhrases = ["MARAUDER", "SECURE CHAT", "AES-GCM", "1v1 ENCRYPTED"];
    let phraseIdx = 0, charPos = 0, isDeleting = false;
    const titleEl = document.getElementById("animatedTitle");
    
    function stopTitleAnimation() {
        if (titleAnimationTimer) clearTimeout(titleAnimationTimer);
        if (titleTimeout) clearTimeout(titleTimeout);
        titleAnimationTimer = null;
        titleTimeout = null;
    }
    
    function animateTitle() {
        if (!titleEl) return;
        const full = titlePhrases[phraseIdx];
        if (!isDeleting && charPos < full.length) {
            charPos++;
            titleEl.innerText = full.slice(0, charPos) + "▌";
            titleAnimationTimer = setTimeout(animateTitle, 100);
        } else if (!isDeleting && charPos === full.length) {
            titleEl.innerText = full;
            titleTimeout = setTimeout(() => { isDeleting = true; animateTitle(); }, 3000);
        } else if (isDeleting && charPos > 0) {
            charPos--;
            titleEl.innerText = full.slice(0, charPos) + "█";
            titleAnimationTimer = setTimeout(animateTitle, 50);
        } else if (isDeleting && charPos === 0) {
            isDeleting = false;
            phraseIdx = (phraseIdx + 1) % titlePhrases.length;
            animateTitle();
        }
    }
    
    // ========== КРИПТОГРАФИЯ AES-GCM ==========
    async function generateRoomKey() {
        return await crypto.subtle.generateKey(
            { name: "AES-GCM", length: 256 },
            true,
            ["encrypt", "decrypt"]
        );
    }
    
    async function exportKey(key) {
        const raw = await crypto.subtle.exportKey("raw", key);
        return btoa(String.fromCharCode(...new Uint8Array(raw)));
    }
    
    async function importKey(base64Key) {
        if (!base64Key) throw new Error("No key provided");
        const raw = Uint8Array.from(atob(base64Key), c => c.charCodeAt(0));
        return await crypto.subtle.importKey(
            "raw",
            raw,
            { name: "AES-GCM", length: 256 },
            true,
            ["encrypt", "decrypt"]
        );
    }
    
    function base64ToBuffer(base64) {
        if (!base64) return null;
        try {
            const binary = atob(base64);
            const bytes = new Uint8Array(binary.length);
            for (let i = 0; i < binary.length; i++) {
                bytes[i] = binary.charCodeAt(i);
            }
            return bytes;
        } catch (e) {
            console.warn("base64 decode failed:", e);
            return null;
        }
    }
    
    function bufferToBase64(buffer) {
        const bytes = new Uint8Array(buffer);
        let binary = '';
        for (let i = 0; i < bytes.length; i++) {
            binary += String.fromCharCode(bytes[i]);
        }
        return btoa(binary);
    }
    
    async function encryptMessage(key, text) {
        const iv = crypto.getRandomValues(new Uint8Array(12));
        const encoder = new TextEncoder();
        const encrypted = await crypto.subtle.encrypt(
            { name: "AES-GCM", iv: iv },
            key,
            encoder.encode(text)
        );
        return {
            iv: bufferToBase64(iv),
            data: bufferToBase64(encrypted)
        };
    }
    
    async function decryptMessage(key, ivBase64, dataBase64) {
        if (!key || !ivBase64 || !dataBase64) return null;
        try {
            const iv = base64ToBuffer(ivBase64);
            const data = base64ToBuffer(dataBase64);
            if (!iv || !data) return null;
            const decrypted = await crypto.subtle.decrypt(
                { name: "AES-GCM", iv: iv },
                key,
                data
            );
            return new TextDecoder().decode(decrypted);
        } catch (e) {
            console.warn("Decrypt failed:", e);
            return null;
        }
    }
    
    // ========== ВСПОМОГАТЕЛЬНЫЕ ФУНКЦИИ ==========
    function getDeviceInfo() {
        const ua = navigator.userAgent;
        if (/iPhone|iPad|iPod/i.test(ua)) return "📱 iOS";
        if (/Android/i.test(ua)) return "📱 Android";
        if (/Windows/i.test(ua)) return "💻 Windows";
        if (/Mac/i.test(ua)) return "🍎 Mac";
        return "💻 PC";
    }
    
    function generateRoomId() {
        return Math.random().toString(36).substring(2, 10).toUpperCase();
    }
    
    function isValidRoomId(id) {
        return /^[A-Z0-9]{8}$/.test(id);
    }
    
    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, m => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;' }[m]));
    }
    
    function showError(msg) {
        const errDiv = document.getElementById('globalError');
        if (errDiv) errDiv.innerText = msg;
        else console.error(msg);
    }
    
    function clearError() {
        const errDiv = document.getElementById('globalError');
        if (errDiv) errDiv.innerText = '';
    }
    
    // ========== FIREBASE ROOM ФУНКЦИИ (БЕЗ runTransaction) ==========
    async function createRoom(nickname, device) {
        const roomId = generateRoomId();
        const roomKey = await generateRoomKey();
        const exportedKey = await exportKey(roomKey);
        
        const roomData = {
            roomId: roomId,
            status: "waiting",
            host: {
                nickname: nickname,
                userId: currentUserId,
                device: device,
                joinedAt: Date.now()
            },
            guest: null,
            createdAt: Date.now(),
            cryptoKey: exportedKey,
            users: {
                [currentUserId]: {
                    nickname: nickname,
                    isHost: true,
                    joinedAt: Date.now(),
                    device: device
                }
            }
        };
        
        const roomRef = ref(db, `rooms/${roomId}`);
        await set(roomRef, roomData);
        
        return { success: true, roomId: roomId, cryptoKey: roomKey };
    }
    
    async function joinRoom(roomId, nickname, device) {
        if (!isValidRoomId(roomId)) {
            throw new Error("Invalid room ID (8 chars, A-Z0-9)");
        }
        
        const roomRef = ref(db, `rooms/${roomId}`);
        const snapshot = await get(roomRef);
        
        if (!snapshot.exists()) {
            throw new Error("ROOM NOT FOUND");
        }
        
        const room = snapshot.val();
        
        if (room.status === "playing") {
            throw new Error("ROOM IS FULL");
        }
        
        if (room.guest !== null && room.guest.userId !== currentUserId) {
            throw new Error("ROOM ALREADY HAS GUEST");
        }
        
        // Обновляем комнату
        const updatedRoom = {
            ...room,
            guest: {
                nickname: nickname,
                userId: currentUserId,
                device: device,
                joinedAt: Date.now()
            },
            status: "playing",
            users: {
                ...room.users,
                [currentUserId]: {
                    nickname: nickname,
                    isHost: false,
                    joinedAt: Date.now(),
                    device: device
                }
            }
        };
        
        await set(roomRef, updatedRoom);
        
        const cryptoKeyImported = await importKey(room.cryptoKey);
        return { success: true, roomId: roomId, cryptoKey: cryptoKeyImported };
    }
    
    async function sendEncryptedMessage(roomId, text) {
        if (!text.trim() || !cryptoKey) return false;
        
        try {
            const encrypted = await encryptMessage(cryptoKey, text);
            const messagesRef = ref(db, `rooms/${roomId}/messages`);
            await push(messagesRef, {
                encrypted: true,
                iv: encrypted.iv,
                data: encrypted.data,
                sender: currentNickname,
                timestamp: Date.now()
            });
            return true;
        } catch (e) {
            console.error("Send message error:", e);
            return false;
        }
    }
    
    async function leaveRoom() {
        if (roomUnsubscribe) {
            off(roomUnsubscribe);
            roomUnsubscribe = null;
        }
        if (messagesUnsubscribe) {
            off(messagesUnsubscribe);
            messagesUnsubscribe = null;
        }
        
        if (!currentRoomId) return;
        
        const roomRef = ref(db, `rooms/${currentRoomId}`);
        const snapshot = await get(roomRef);
        
        if (!snapshot.exists()) {
            currentRoomId = null;
            currentIsHost = false;
            cryptoKey = null;
            return;
        }
        
        const room = snapshot.val();
        
        if (currentIsHost) {
            // Хост удаляет комнату
            await remove(roomRef);
        } else {
            // Гость покидает комнату
            const updatedRoom = {
                ...room,
                guest: null,
                status: "waiting"
            };
            
            if (updatedRoom.users && updatedRoom.users[currentUserId]) {
                delete updatedRoom.users[currentUserId];
            }
            
            await set(roomRef, updatedRoom);
        }
        
        cryptoKey = null;
        currentRoomId = null;
        currentIsHost = false;
    }
    
    // ========== UI ОТРИСОВКА ==========
    const contentArea = document.getElementById('contentArea');
    let activeCaptchaWord = "";
    let activePuzzleTarget = "";
    let activePuzzleState = [];
    let activePuzzleRemaining = [];
    
    const captchaWords = ["ROOT", "HACKER", "CYBER", "CRYPTO", "SHELL", "BYTE", "ZERO", "ALPHA", "OMEGA", "GHOST"];
    
    function renderPuzzleUI() {
        const slots = document.getElementById('puzzleSlots');
        const pieces = document.getElementById('puzzlePieces');
        if (!slots || !pieces) return;
        
        slots.innerHTML = activePuzzleState.map((v, i) => 
            `<div class="puzzle-slot" data-slot="${i}">${v !== null ? escapeHtml(v) : "___"}</div>`
        ).join('');
        
        pieces.innerHTML = activePuzzleRemaining.map(p => 
            `<div class="puzzle-piece" data-piece="${escapeHtml(p)}">${escapeHtml(p)}</div>`
        ).join('');
        
        document.querySelectorAll('.puzzle-slot').forEach(slot => {
            slot.onclick = () => {
                const idx = parseInt(slot.dataset.slot);
                if (activePuzzleState[idx] !== null) {
                    activePuzzleRemaining.push(activePuzzleState[idx]);
                    activePuzzleState[idx] = null;
                    renderPuzzleUI();
                }
            };
        });
        
        document.querySelectorAll('.puzzle-piece').forEach(piece => {
            piece.onclick = () => {
                const val = piece.dataset.piece;
                const empty = activePuzzleState.findIndex(s => s === null);
                if (empty !== -1) {
                    activePuzzleState[empty] = val;
                    const idx = activePuzzleRemaining.indexOf(val);
                    if (idx !== -1) activePuzzleRemaining.splice(idx, 1);
                    renderPuzzleUI();
                }
            };
        });
    }
    
    function renderAuthScreen() {
        const captchaWord = captchaWords[Math.floor(Math.random() * captchaWords.length)];
        const shuffled = captchaWord.split('').sort(() => Math.random() - 0.5).join('');
        activeCaptchaWord = captchaWord;
        
        const targetWord = captchaWords[Math.floor(Math.random() * captchaWords.length)];
        const letters = targetWord.split('');
        const shuffledLetters = [...letters].sort(() => Math.random() - 0.5);
        activePuzzleTarget = targetWord;
        activePuzzleState = new Array(targetWord.length).fill(null);
        activePuzzleRemaining = [...shuffledLetters];
        
        contentArea.innerHTML = `
            <div id="globalError" class="error-msg"></div>
            <div id="captchasContainer">
                <div class="captcha-container">
                    <div class="floating-word">${escapeHtml(shuffled)}</div>
                    <input type="text" id="captchaInput" class="terminal-input" placeholder=">_ ENTER DECODED WORD" autocomplete="off">
                </div>
                <div class="captcha-container">
                    <div class="target-word">TARGET: ${escapeHtml(targetWord)}</div>
                    <div class="puzzle-area" id="puzzleSlots"></div>
                    <div class="puzzle-area" id="puzzlePieces"></div>
                    <div style="font-size:10px; color:#00aa66; text-align:center;">Click letter → empty slot</div>
                </div>
            </div>
            <button id="verifyBtn" class="btn">✓ VERIFY & ENTER ✓</button>
            <div id="authFormContainer" style="display: none;"></div>
        `;
        renderPuzzleUI();
        
        document.getElementById('verifyBtn').onclick = () => {
            const ans = document.getElementById('captchaInput').value.trim().toUpperCase();
            if (ans !== activeCaptchaWord) {
                showError(`❌ Wrong! Expected: ${activeCaptchaWord}`);
                return;
            }
            const puzzleWord = activePuzzleState.join('');
            if (puzzleWord !== activePuzzleTarget) {
                showError(`❌ Wrong puzzle! Need: ${activePuzzleTarget}`);
                return;
            }
            
            document.getElementById('captchasContainer').style.opacity = '0';
            document.getElementById('verifyBtn').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('captchasContainer').classList.add('hidden');
                document.getElementById('verifyBtn').classList.add('hidden');
                document.getElementById('authFormContainer').style.display = 'block';
                document.getElementById('authFormContainer').innerHTML = `
                    <div class="success-msg">✓ ACCESS GRANTED. WELCOME TO MARAUDER ✓</div>
                    <div class="auth-form">
                        <input type="text" id="nicknameInput" class="terminal-input" placeholder="YOUR NICKNAME" value="ne0n_r1der" autocomplete="off">
                        <input type="text" id="roomIdInput" class="terminal-input" placeholder="ROOM ID (to join existing)" autocomplete="off">
                        <button id="createBtn" class="btn" style="margin-bottom: 10px;">⚡ CREATE NEW ROOM ⚡</button>
                        <button id="joinBtn" class="btn btn-secondary">🔗 JOIN ROOM BY ID</button>
                        <div id="authError" class="error-msg"></div>
                    </div>
                `;
                
                let actionInProgress = false;
                
                document.getElementById('createBtn').onclick = async () => {
                    if (actionInProgress) return;
                    const nick = document.getElementById('nicknameInput').value.trim();
                    if (!nick) { showError("Enter nickname"); return; }
                    actionInProgress = true;
                    const btn = document.getElementById('createBtn');
                    btn.disabled = true;
                    btn.textContent = "⏳ CREATING...";
                    clearError();
                    
                    try {
                        const res = await createRoom(nick, getDeviceInfo());
                        currentNickname = nick;
                        currentRoomId = res.roomId;
                        currentIsHost = true;
                        cryptoKey = res.cryptoKey;
                        await enterChatRoom();
                    } catch (e) {
                        showError("Create error: " + e.message);
                        btn.disabled = false;
                        btn.textContent = "⚡ CREATE NEW ROOM";
                    } finally {
                        actionInProgress = false;
                    }
                };
                
                document.getElementById('joinBtn').onclick = async () => {
                    if (actionInProgress) return;
                    const nick = document.getElementById('nicknameInput').value.trim();
                    const roomId = document.getElementById('roomIdInput').value.trim().toUpperCase();
                    if (!nick) { showError("Enter nickname"); return; }
                    if (!roomId) { showError("Enter room ID"); return; }
                    actionInProgress = true;
                    const btn = document.getElementById('joinBtn');
                    btn.disabled = true;
                    btn.textContent = "⏳ JOINING...";
                    clearError();
                    
                    try {
                        const res = await joinRoom(roomId, nick, getDeviceInfo());
                        currentNickname = nick;
                        currentRoomId = roomId;
                        currentIsHost = false;
                        cryptoKey = res.cryptoKey;
                        await enterChatRoom();
                    } catch (e) {
                        showError(e.message);
                        btn.disabled = false;
                        btn.textContent = "🔗 JOIN ROOM BY ID";
                    } finally {
                        actionInProgress = false;
                    }
                };
            }, 300);
        };
    }
    
    async function enterChatRoom() {
        if (!currentRoomId || !cryptoKey) return;
        
        contentArea.innerHTML = `
            <div id="globalError" class="error-msg"></div>
            <button id="leaveBtn" class="btn back-btn">← LEAVE ROOM</button>
            <div class="room-card">
                <div style="text-align:center; font-size:12px; color:#00aa66;">ROOM ID</div>
                <div class="room-code">${escapeHtml(currentRoomId)}</div>
                <div id="statusContainer">
                    <div class="status-text waiting" id="statusMsg">⏳ Waiting for player...</div>
                </div>
                <div class="players-info">
                    <span>👑 HOST: <span id="hostName">...</span><span id="hostDevice" class="device-badge"></span></span>
                    <span>🕹️ GUEST: <span id="guestName">...</span><span id="guestDevice" class="device-badge"></span></span>
                </div>
            </div>
            <div class="chat-messages" id="messagesList">
                <div style="text-align:center; color:#00aa66; padding:20px;">💬 No messages yet</div>
            </div>
            <div class="input-row">
                <input type="text" id="msgInput" class="terminal-input" placeholder=">_ TYPE MESSAGE..." autocomplete="off">
                <button id="sendBtn" class="btn">→</button>
            </div>
        `;
        
        const messagesDiv = document.getElementById('messagesList');
        const statusMsg = document.getElementById('statusMsg');
        const statusContainer = document.getElementById('statusContainer');
        const hostNameSpan = document.getElementById('hostName');
        const hostDeviceSpan = document.getElementById('hostDevice');
        const guestNameSpan = document.getElementById('guestName');
        const guestDeviceSpan = document.getElementById('guestDevice');
        
        // Room listener
        const roomRef = ref(db, `rooms/${currentRoomId}`);
        if (roomUnsubscribe) off(roomUnsubscribe);
        roomUnsubscribe = onValue(roomRef, (snap) => {
            if (!snap.exists()) {
                showError("Room closed by host");
                exitToMenu();
                return;
            }
            const room = snap.val();
            
            if (room.host) {
                hostNameSpan.innerText = escapeHtml(room.host.nickname);
                hostDeviceSpan.innerText = room.host.device || '';
            }
            
            if (room.guest) {
                guestNameSpan.innerText = escapeHtml(room.guest.nickname);
                guestDeviceSpan.innerText = room.guest.device || '';
            } else {
                guestNameSpan.innerText = '—';
                guestDeviceSpan.innerText = '';
            }
            
            if (room.status === "playing" && room.guest) {
                statusMsg.innerText = "✅ CONNECTED - Chat ready";
                statusContainer.className = "status-text connected";
            } else {
                statusMsg.innerText = "⏳ Waiting for player to join...";
                statusContainer.className = "status-text waiting";
            }
        });
        
        // Messages listener
        const messagesRef = ref(db, `rooms/${currentRoomId}/messages`);
        if (messagesUnsubscribe) off(messagesUnsubscribe);
        messagesUnsubscribe = onValue(messagesRef, async (snap) => {
            messagesDiv.innerHTML = '';
            const msgs = [];
            snap.forEach(child => msgs.push({ id: child.key, ...child.val() }));
            msgs.sort((a, b) => (a.timestamp || 0) - (b.timestamp || 0));
            
            if (msgs.length === 0) {
                messagesDiv.innerHTML = '<div style="text-align:center; color:#00aa66; padding:20px;">💬 No messages yet</div>';
                return;
            }
            
            for (const msg of msgs) {
                let displayText = "[ENCRYPTED]";
                
                if (msg.encrypted && msg.iv && msg.data) {
                    const decrypted = await decryptMessage(cryptoKey, msg.iv, msg.data);
                    if (decrypted) displayText = decrypted;
                } else if (msg.text) {
                    displayText = msg.text;
                }
                
                const isOwn = msg.sender === currentNickname;
                const div = document.createElement('div');
                div.className = `message ${isOwn ? 'own' : ''}`;
                div.innerHTML = `
                    <div class="message-bubble">
                        <div class="message-text">${escapeHtml(displayText)}</div>
                    </div>
                    <div class="message-meta">${escapeHtml(msg.sender)} • ${new Date(msg.timestamp).toLocaleTimeString()}</div>
                `;
                messagesDiv.appendChild(div);
            }
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        });
        
        // Send message
        const msgInput = document.getElementById('msgInput');
        const sendBtn = document.getElementById('sendBtn');
        
        const sendMessage = async () => {
            const text = msgInput.value.trim();
            if (text) {
                const success = await sendEncryptedMessage(currentRoomId, text);
                if (success) msgInput.value = '';
            }
        };
        
        sendBtn.onclick = sendMessage;
        msgInput.onkeypress = (e) => {
            if (e.key === 'Enter') sendMessage();
        };
        
        // Leave button
        document.getElementById('leaveBtn').onclick = async () => {
            await leaveRoom();
            exitToMenu();
        };
    }
    
    function exitToMenu() {
        stopTitleAnimation();
        cryptoKey = null;
        currentRoomId = null;
        currentIsHost = false;
        renderAuthScreen();
        animateTitle();
    }
    
    // ========== ИНИЦИАЛИЗАЦИЯ ==========
    async function initFirebase() {
        try {
            app = initializeApp(firebaseConfig);
            auth = getAuth(app);
            db = getDatabase(app);
            
            await signInAnonymously(auth);
            
            onAuthStateChanged(auth, (user) => {
                if (user) {
                    firebaseUser = user;
                    currentUserId = user.uid;
                    renderAuthScreen();
                    animateTitle();
                } else {
                    contentArea.innerHTML = `<div style="color:#ff6666;text-align:center;padding:20px;">
                        ⚠️ Authentication failed. Please refresh the page.<br>
                        Make sure Anonymous Auth is enabled in Firebase Console.
                    </div>`;
                }
            });
        } catch (e) {
            console.error("Firebase init error:", e);
            contentArea.innerHTML = `<div style="color:#ff6666;text-align:center;padding:20px;">
                ⚠️ Firebase configuration error.<br><br>
                Please check:<br>
                1. Enable Anonymous Auth in Firebase Console<br>
                2. Add your domain to Authorized Domains<br>
                3. Verify databaseURL in config
            </div>`;
        }
    }
    
    initFirebase();
</script>
</body>
</html>
