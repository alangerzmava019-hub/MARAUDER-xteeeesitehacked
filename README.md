async function initFirebase() {
    console.log("🔥 Firebase init started");
    
    // Защита от повторной инициализации
    if (app && auth && db) {
        console.log("⚠️ Firebase already initialized, skipping...");
        return;
    }
    
    try {
        console.log("📡 Initializing app with config:", {
            projectId: firebaseConfig.projectId,
            authDomain: firebaseConfig.authDomain,
            hasDatabaseURL: !!firebaseConfig.databaseURL
        });
        
        app = initializeApp(firebaseConfig);
        console.log("✅ App initialized successfully");
        
        auth = getAuth(app);
        db = getDatabase(app);
        console.log("✅ Auth and Database services obtained");
        
        console.log("🔐 Attempting anonymous sign in...");
        const userCredential = await signInAnonymously(auth);
        console.log("✅ Anonymous login success, UID:", userCredential.user.uid);
        
        // Настройка обработчика состояния аутентификации
        onAuthStateChanged(auth, (user) => {
            if (user) {
                firebaseUser = user;
                currentUserId = user.uid;
                console.log("👤 User authenticated - UID:", currentUserId);
                console.log("🕐 Auth timestamp:", new Date(user.metadata.lastSignInTime));
                
                // Проверка доступа к базе данных
                const testRef = ref(db, '.info/connected');
                onValue(testRef, (snap) => {
                    console.log("🌐 Database connection status:", snap.val() ? "CONNECTED" : "DISCONNECTED");
                }, { onlyOnce: true });
                
                renderAuthScreen();
                animateTitle();
            } else {
                console.error("❌ No user after signInAnonymously - this should not happen");
                showAuthError();
            }
        });
        
    } catch (e) {
        console.error("❌ Firebase init failed with error:", e);
        console.error("Error code:", e.code);
        console.error("Error message:", e.message);
        
        let userMessage = "";
        
        // Расшифровка ошибок Firebase
        switch (e.code) {
            case "auth/operation-not-allowed":
                userMessage = "Anonymous Auth is disabled. Please enable it in Firebase Console → Authentication → Sign-in methods.";
                break;
            case "auth/network-request-failed":
                userMessage = "Network error. Check your internet connection and Firebase security rules.";
                break;
            case "auth/configuration-not-found":
                userMessage = "Firebase configuration error. Check your apiKey and projectId.";
                break;
            case "auth/internal-error":
                userMessage = "Internal auth error. Check if Realtime Database is enabled.";
                break;
            default:
                userMessage = `Firebase error: ${e.message}`;
        }
        
        showFirebaseError(userMessage);
    }
}

function showAuthError() {
    const contentArea = document.getElementById('contentArea');
    if (contentArea) {
        contentArea.innerHTML = `
            <div style="color:#ff6666;text-align:center;padding:20px;">
                <div style="font-size:18px; margin-bottom:16px;">⚠️ AUTHENTICATION FAILED</div>
                <div style="font-size:13px; margin-bottom:12px;">Please check:</div>
                <div style="font-size:12px; text-align:left; max-width:300px; margin:0 auto;">
                    1. Anonymous Auth is ENABLED in Firebase Console<br>
                    2. Your domain is in Authorized Domains list<br>
                    3. Realtime Database rules allow read/write
                </div>
                <button onclick="location.reload()" style="margin-top:20px; background:#00ff66; border:none; padding:10px 20px; border-radius:8px; cursor:pointer;">⟳ REFRESH</button>
            </div>
        `;
    }
}

function showFirebaseError(message) {
    const contentArea = document.getElementById('contentArea');
    if (contentArea) {
        contentArea.innerHTML = `
            <div style="color:#ff6666;text-align:center;padding:20px;">
                <div style="font-size:18px; margin-bottom:16px;">🔥 FIREBASE ERROR</div>
                <div style="font-size:12px; background:#1a0000; padding:12px; border-radius:8px; border:1px solid #ff6666; margin-bottom:16px;">
                    ${escapeHtml(message)}
                </div>
                <div style="font-size:11px; color:#00aa66;">Check console for details</div>
                <button onclick="location.reload()" style="margin-top:20px; background:#00ff66; border:none; padding:10px 20px; border-radius:8px; cursor:pointer;">⟳ RETRY</button>
            </div>
        `;
    }
}
