index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cards Zones Club - Secure Portal</title>
    <style>
        * { box-sizing: border-box; }
        body { background-color: #0d1b2a; color: #ffffff; font-family: Arial, sans-serif; margin: 0; display: flex; justify-content: center; align-items: center; min-height: 100vh; }
        .login-box { background-color: #1b263b; border: 1px solid #415a77; border-radius: 10px; padding: 25px; width: 100%; max-width: 420px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); }
        
        .club-brand {
            text-align: center;
            margin-bottom: 20px;
            padding: 12px 0;
            background-color: #1b263b;
            border-bottom: 1px solid #415a77;
            border-radius: 6px 6px 0 0;
        }
        .club-badge {
            display: inline-block;
            border: 1px solid #415a77;
            padding: 2px 8px;
            font-size: 10px;
            letter-spacing: 2px;
            color: #adb5bd;
            background-color: #0d1b2a;
            border-radius: 3px;
            margin-bottom: 6px;
            text-transform: uppercase;
        }
        .club-name {
            font-size: 21px;
            font-weight: bold;
            color: #ffffff;
            letter-spacing: 2.5px;
            text-transform: uppercase;
            text-shadow: 0 2px 4px rgba(0,0,0,0.8);
            font-family: 'Courier New', Courier, monospace;
        }

        h2 { color: #38b000; text-align: center; margin-top: 0; font-size: 20px; letter-spacing: 1px; }
        .error-box { background-color: rgba(217,4,41,0.2); border: 1px solid #d90429; color: #ff4d4d; padding: 10px; border-radius: 6px; margin-bottom: 15px; font-size: 12px; display: none; text-align: center; }
        .success-box { background-color: rgba(56,176,0,0.2); border: 1px solid #38b000; color: #38b000; padding: 10px; border-radius: 6px; margin-bottom: 15px; font-size: 12px; display: none; text-align: center; }
        
        label { font-size: 13px; color: #adb5bd; display: block; margin-top: 8px; }
        input { width: 100%; padding: 10px; margin: 5px 0 12px 0; background: #0d1b2a; border: 1px solid #415a77; color: #fff; border-radius: 4px; font-size: 13px; }
        
        .captcha-box-wrapper {
            position: relative;
            width: 100%;
            margin: 5px 0 12px 0;
            background: #0d1b2a;
            border: 1px solid #415a77;
            border-radius: 4px;
            overflow: hidden;
            text-align: center;
        }
        #captcha-canvas, #signup-captcha-canvas {
            display: block;
            width: 100%;
            height: 45px;
            background: #111a28;
        }

        .btn-submit { background-color: #38b000; color: #fff; padding: 12px; border: none; border-radius: 6px; width: 100%; font-size: 14px; font-weight: bold; cursor: pointer; }
        .btn-submit:hover { background-color: #2e9000; }
        .btn-submit:disabled { background-color: #415a77; cursor: not-allowed; }
        .link-text { text-align: center; margin-top: 15px; font-size: 12px; color: #adb5bd; }
        .link-text a { color: #38b000; text-decoration: none; font-weight: bold; cursor: pointer; }
        .hidden-section { display: none; }

        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); display: none; justify-content: center; align-items: center; z-index: 1000; }
        .modal-content { background: #1b263b; border: 1px solid #38b000; padding: 20px; border-radius: 8px; width: 90%; max-width: 400px; text-align: center; }
        .modal-content h3 { color: #38b000; margin-top: 0; }
        .privkey-display { background: #0d1b2a; padding: 10px; color: #ffb703; font-family: monospace; font-size: 12px; word-break: break-all; border: 1px solid #415a77; border-radius: 4px; margin: 15px 0; }
    </style>
</head>
<body>
<div class="login-box">
        <div class="club-brand">
            <div class="club-badge">EST. 2020</div>
            <div class="club-name">Cards Zone Club</div>
        </div>
        
        <div id="error-msg" class="error-box"></div>
        <div id="success-msg" class="success-box"></div>

        <!-- LOGIN STEP 1 -->
        <div id="login-step-1">
            <h2>Login Account</h2>
            <label>Username:</label>
            <input type="text" id="login-username" placeholder="Enter username" autocomplete="off">

            <label>Password:</label>
            <input type="password" id="login-password" placeholder="Enter password" autocomplete="current-password">

            <label>Security Captcha Verification:</label>
            <div class="captcha-box-wrapper">
                <canvas id="captcha-canvas"></canvas>
            </div>
            <input type="text" id="captcha-input" placeholder="Enter exact characters shown above" autocomplete="off">

            <button onclick="proceedToStepTwo()" class="btn-submit">Login</button>

            <div class="link-text">
                Don't have an account? <a onclick="switchView('signup')">Sign Up</a>
            </div>
        </div>

        <!-- LOGIN STEP 2: Private Key -->
        <div id="login-step-2" class="hidden-section">
            <h2>Private Key Check</h2>
            <label>36-Digit Private Key:</label>
            <input type="password" id="login-privkey" placeholder="Enter 36-digit private key" autocomplete="new-password">

            <button id="login-btn" onclick="handleLogin()" class="btn-submit">Secure Verify & Continue</button>
            
            <div class="link-text">
                <a onclick="backToLoginStep1()">Back to Login</a>
            </div>
        </div>

        <!-- SIGNUP SECTION -->
        <div id="signup-section" class="hidden-section">
            <h2>Sign Up Account</h2>
            <label>Full Name:</label>
            <input type="text" id="reg-name" placeholder="Enter your full name" autocomplete="off">

            <label>Username:</label>
            <input type="text" id="reg-username" placeholder="Choose a username" autocomplete="off">

            <label>Password:</label>
            <input type="password" id="reg-password" placeholder="Choose a password" autocomplete="new-password">

            <label>Security Captcha Verification:</label>
            <div class="captcha-box-wrapper">
                <canvas id="signup-captcha-canvas"></canvas>
            </div>
            <input type="text" id="signup-captcha-input" placeholder="Enter exact characters shown above" autocomplete="off">

            <button onclick="handleSignup()" class="btn-submit">Register Account</button>

            <div class="link-text">
                Already have an account? <a onclick="switchView('login')">Login</a>
            </div>
        </div>
    </div>

    <!-- Private Key Modal -->
    <div id="privkey-modal" class="modal-overlay">
        <div class="modal-content">
            <h3>Save Your Private Key!</h3>
            <p style="font-size: 12px; color: #adb5bd;">This 36-digit key is required for login. Keep it secure, it cannot be recovered if lost.</p>
            <div class="privkey-display" id="generated-key-box"></div>
            <button onclick="closeModalAndLogin()" class="btn-submit">I Have Saved It - Go to Login</button>
        </div>
    </div>

    <script>
        let currentCaptchaText = "";
        let signupCaptchaText = "";
        let failedAttempts = 0;
        let lockoutTimer = null;
        let tempVerifiedUser = null;
        let interactionDetected = false;

        // Track human-like mouse/touch movement to block headless bots & malware scripts
        window.addEventListener('mousemove', () => { interactionDetected = true; }, { once: true });
        window.addEventListener('keydown', () => { interactionDetected = true; }, { once: true });
function detectAdvancedBot() {
            let userAgent = navigator.userAgent.toLowerCase();
            if (navigator.webdriver  userAgent.includes('bot')  userAgent.includes('crawl')  userAgent.includes('spider')  userAgent.includes('python') || userAgent.includes('requests')) {
                return true;
            }
            // If automated script tries to submit instantly without any human mouse/keyboard interaction
            if (!interactionDetected && (navigator.maxTouchPoints === 0 && !window.chrome)) {
                // Heuristic check for non-interactive automated POST requests
                return false; // Kept flexible for real users, but strict on webdriver
            }
            return false;
        }

        function initializeDefaultUsers() {
            let users = [];
            try { users = JSON.parse(localStorage.getItem('czc_users') || '[]'); } catch(e) { users = []; }

            const defaultAccounts = [
                {
                    name: 'System Administrator',
                    username: 'admin',
                    email: 'treasurerman89@gmail.com',
                    password: 'admin201215',
                    privateKey: 'CZCADMIN9876543210abcdefABCDEF123456',
                    balance_usd: 99999.00,
                    total_deposited: 5000.00,
                    is_activated: 1,
                    role: 'Owner',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Alexander Hamilton',
                    username: 'Alexander Hamilton',
                    email: 'hellokitty11@gmail.com',
                    password: 'Alex Devil',
                    privateKey: 'VuRtuzLP48MJThDB03Mudvm5B1Tj9Naokk9s',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Williams',
                    username: 'williams',
                    email: 'williams39@gmail.com',
                    password: 'Willams123',
                    privateKey: 'CZCWILLIAMS9876543210abcdefABCDEF123',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Nexus Vendor',
                    username: 'nexus_vendor',
                    email: 'nexus@czc.club',
                    password: 'seller123_d',
                    privateKey: 'CZCGEAR9876543210abcdefABCDEF123454',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Matrix Node',
                    username: 'matrix_node',
                    email: 'matrix@czc.club',
                    password: 'seller123_c',
                    privateKey: 'CZCGEAR9876543210abcdefABCDEF123453',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Zenith Merchant',
                    username: 'zenith_merchant',
                    email: 'zenith@czc.club',
password: 'seller123_i',
                    privateKey: 'CZCGEAR9876543210abcdefABCDEF123459',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Shadow Seller',
                    username: 'shadow_seller',
                    email: 'shadow@czc.club',
                    password: 'seller123_a',
                    privateKey: 'CZCGEAR9876543210abcdefABCDEF123451',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Viper Cards',
                    username: 'viper_cards',
                    email: 'viper@czc.club',
                    password: 'seller123_b',
                    privateKey: 'CZCGEAR9876543210abcdefABCDEF123452',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Phantom Store',
                    username: 'phantom_store',
                    email: 'phantom@czc.club',
                    password: 'seller123_g',
                    privateKey: 'CZCGEAR9876543210abcdefABCDEF123457',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                },
                {
                    name: 'Apex Vendor',
                    username: 'apex_vendor',
                    email: 'apex@czc.club',
                    password: 'seller123_h',
                    privateKey: 'CZCGEAR9876543210abcdefABCDEF123458',
                    balance_usd: 0.00,
                    total_deposited: 0.00,
                    is_activated: 1,
                    role: 'Official Seller',
                    tfaEnabled: false,
                    emailVerified: true
                }
            ];

            defaultAccounts.forEach(acc => {
                let index = users.findIndex(u => u.username && u.username.toLowerCase() === acc.username.toLowerCase());
                if (index === -1) {
                    users.push({
                        ...acc,
                        activationDate: new Date().toISOString()
                    });
                } else {
                    users[index].password = acc.password;
                    users[index].privateKey = acc.privateKey;
                    users[index].email = acc.email;
                    users[index].role = acc.role;
                    users[index].is_activated = 1;
                    if (acc.username.toLowerCase() === 'admin') {
                        if (users[index].balance_usd === undefined || users[index].balance_usd < 99999) {
                            users[index].balance_usd = 99999.00;
                        }
                    } else {
                        if (users[index].balance_usd === undefined) {
                            users[index].balance_usd = 0.00;
                        }
                    }
                }
            });

            localStorage.setItem('czc_users', JSON.stringify(users));
        }

        function drawCaptcha(canvasId, text) {
            let canvas = document.getElementById(canvasId);
            if (!canvas) return;
            let ctx = canvas.getContext('2d');
            
            canvas.width = canvas.offsetWidth || 300;
            canvas.height = 45;
ctx.fillStyle = "#0d1b2a";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            for (let i = 0; i < 7; i++) {
                ctx.strokeStyle = rgba(${Math.floor(Math.random()*150+50)}, ${Math.floor(Math.random()*150+50)}, ${Math.floor(Math.random()*255)}, 0.6);
                ctx.lineWidth = Math.random() * 2 + 1;
                ctx.beginPath();
                ctx.moveTo(Math.random() * canvas.width, Math.random() * canvas.height);
                ctx.lineTo(Math.random() * canvas.width, Math.random() * canvas.height);
                ctx.stroke();
            }

            for (let i = 0; i < 40; i++) {
                ctx.fillStyle = rgba(255, 255, 255, ${Math.random() * 0.5});
                ctx.fillRect(Math.random() * canvas.width, Math.random() * canvas.height, 2, 2);
            }

            ctx.font = "bold 24px monospace";
            ctx.textBaseline = "middle";
            
            let charSpacing = canvas.width / (text.length + 1);
            for (let i = 0; i < text.length; i++) {
                ctx.save();
                let x = charSpacing * (i + 1) + (Math.random() * 10 - 5);
                let y = canvas.height / 2 + (Math.random() * 10 - 5);
                ctx.translate(x, y);
                
                let angle = (Math.random() * 0.4) - 0.2;
                ctx.rotate(angle);
                
                ctx.fillStyle = i % 2 === 0 ? "#ffb703" : "#48cae4";
                ctx.fillText(text[i], -10, 0);
                ctx.restore();
            }
        }

        function generateLoginCaptcha() {
            const chars = "ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnpqrstuvwxyz23456789";
            let captcha = "";
            for (let i = 0; i < 6; i++) {
                captcha += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            currentCaptchaText = captcha;
            drawCaptcha('captcha-canvas', currentCaptchaText);
            let input = document.getElementById('captcha-input');
            if(input) input.value = '';
        }

        function generateSignupCaptcha() {
            const chars = "ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnpqrstuvwxyz23456789";
            let captcha = "";
            for (let i = 0; i < 6; i++) {
                captcha += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            signupCaptchaText = captcha;
            drawCaptcha('signup-captcha-canvas', signupCaptchaText);
            let input = document.getElementById('signup-captcha-input');
            if(input) input.value = '';
        }

        function switchView(view) {
            document.getElementById('error-msg').style.display = 'none';
            document.getElementById('success-msg').style.display = 'none';
            
            document.getElementById('login-step-1').classList.add('hidden-section');
            document.getElementById('login-step-2').classList.add('hidden-section');
            document.getElementById('signup-section').classList.add('hidden-section');

            if (view === 'login') {
                document.getElementById('login-step-1').classList.remove('hidden-section');
                generateLoginCaptcha();
            } else if (view === 'signup') {
                document.getElementById('signup-section').classList.remove('hidden-section');
                generateSignupCaptcha();
            }
        }

        function backToLoginStep1() {
            document.getElementById('login-step-2').classList.add('hidden-section');
            document.getElementById('login-step-1').classList.remove('hidden-section');
            document.getElementById('error-msg').style.display = 'none';
            tempVerifiedUser = null;
            generateLoginCaptcha();
        }
function triggerLockout() {
            failedAttempts = 0;
            let btn = document.getElementById('login-btn');
            let errorBox = document.getElementById('error-msg');
            btn.disabled = true;
            let seconds = 30;
            
            errorBox.innerText = Too many failed attempts. Security lockout active for ${seconds}s.;
            errorBox.style.display = 'block';

            lockoutTimer = setInterval(() => {
                seconds--;
                if (seconds > 0) {
                    errorBox.innerText = Security lockout active for ${seconds}s.;
                } else {
                    clearInterval(lockoutTimer);
                    btn.disabled = false;
                    errorBox.style.display = 'none';
                    generateLoginCaptcha();
                }
            }, 1000);
        }

        function proceedToStepTwo() {
            let errorBox = document.getElementById('error-msg');
            let usernameInput = document.getElementById('login-username').value.trim();
            let passwordInput = document.getElementById('login-password').value.trim();
            let captchaInput = document.getElementById('captcha-input').value.trim();

            errorBox.style.display = 'none';

            if (detectAdvancedBot()) {
                errorBox.innerText = "Access blocked: Automated script/bot detected.";
                errorBox.style.display = 'block';
                return;
            }

            if (!usernameInput  !passwordInput  !captchaInput) {
                errorBox.innerText = "Please fill in all fields including Captcha.";
                errorBox.style.display = 'block';
                return;
            }

            if (captchaInput !== currentCaptchaText) {
                failedAttempts++;
                errorBox.innerText = "Invalid Captcha code. Captcha automatically refreshed.";
                errorBox.style.display = 'block';
                generateLoginCaptcha();
                if (failedAttempts >= 4) { triggerLockout(); }
                return;
            }

            let users = [];
            try { users = JSON.parse(localStorage.getItem('czc_users') || '[]'); } catch(e) { users = []; }

            let user = users.find(u => u.username && u.username.toLowerCase() === usernameInput.toLowerCase());

            if (!user || user.password !== passwordInput) {
                failedAttempts++;
                errorBox.innerText = "Invalid username or password. Captcha refreshed.";
                errorBox.style.display = 'block';
                generateLoginCaptcha();
                if (failedAttempts >= 4) { triggerLockout(); }
                return;
            }

            tempVerifiedUser = user;
            document.getElementById('login-step-1').classList.add('hidden-section');
            document.getElementById('login-step-2').classList.remove('hidden-section');
        }

        function handleLogin() {
            let errorBox = document.getElementById('error-msg');
            if (lockoutTimer) return;

            let privKey = document.getElementById('login-privkey').value.trim();
            errorBox.style.display = 'none';

            if (!privKey) {
                errorBox.innerText = "Private Key is required.";
                errorBox.style.display = 'block';
                return;
            }

            if (!tempVerifiedUser || tempVerifiedUser.privateKey !== privKey) {
                failedAttempts++;
                errorBox.innerText = "Invalid Private Key. Access denied.";
                errorBox.style.display = 'block';
                if (failedAttempts >= 4) { triggerLockout(); }
                return;
            }

            let userKey = tempVerifiedUser.username.toLowerCase();
            let newSessionToken = 'sess_' + Math.random().toString(36).substring(2) + Date.now().toString(36);
let activeSessions = {};
            try { activeSessions = JSON.parse(localStorage.getItem('czc_active_sessions') || '{}'); } catch(e) {}
            
            activeSessions[userKey] = { token: newSessionToken, time: Date.now() };
            localStorage.setItem('czc_active_sessions', JSON.stringify(activeSessions));
            
            localStorage.setItem('czc_local_session_token', newSessionToken);
            localStorage.setItem('czc_current_user', tempVerifiedUser.username);

            window.location.href = 'store.html';
        }

        function handleSignup() {
            let errorBox = document.getElementById('error-msg');
            let nameInput = document.getElementById('reg-name').value.trim();
            let usernameInput = document.getElementById('reg-username').value.trim();
            let passwordInput = document.getElementById('reg-password').value.trim();
            let captchaInput = document.getElementById('signup-captcha-input').value.trim();

            errorBox.style.display = 'none';

            if (detectAdvancedBot()) {
                errorBox.innerText = "Access blocked: Automated script/bot detected.";
                errorBox.style.display = 'block';
                return;
            }

            if (!nameInput  !usernameInput  !passwordInput || !captchaInput) {
                errorBox.innerText = "Please fill in all fields including Captcha.";
                errorBox.style.display = 'block';
                return;
            }

            if (captchaInput !== signupCaptchaText) {
                errorBox.innerText = "Invalid Captcha code. Captcha automatically refreshed.";
                errorBox.style.display = 'block';
                generateSignupCaptcha();
                return;
            }

            let users = [];
            try { users = JSON.parse(localStorage.getItem('czc_users') || '[]'); } catch(e) { users = []; }

            let existing = users.find(u => u.username && u.username.toLowerCase() === usernameInput.toLowerCase());
            if (existing) {
                errorBox.innerText = "Username already exists. Choose another.";
                errorBox.style.display = 'block';
                generateSignupCaptcha();
                return;
            }

            const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
            let newPrivateKey = "";
            for (let i = 0; i < 36; i++) {
                newPrivateKey += chars.charAt(Math.floor(Math.random() * chars.length));
            }

            let newUser = {
                name: nameInput,
                username: usernameInput,
                password: passwordInput,
                privateKey: newPrivateKey,
                balance_usd: 0.00,
                total_deposited: 0.00,
                is_activated: 1,
                role: 'Member',
                tfaEnabled: false,
                emailVerified: true,
                activationDate: new Date().toISOString()
            };

            users.push(newUser);
            localStorage.setItem('czc_users', JSON.stringify(users));

            document.getElementById('generated-key-box').innerText = newPrivateKey;
            document.getElementById('privkey-modal').style.display = 'flex';
        }

        function closeModalAndLogin() {
            document.getElementById('privkey-modal').style.display = 'none';
            switchView('login');
        }

        window.onload = function() {
            initializeDefaultUsers();
            generateLoginCaptcha();
        };
    </script>
</body>
</html>
sellers.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Seller Portal - 🏴‍☠️Cards Zones Club🏴‍☠️</title>
    <style>
        body { background-color: #0d1b2a; color: #ffffff; font-family: Arial, sans-serif; margin: 0; padding: 20px; display: flex; justify-content: center; align-items: center; min-height: 100vh; }
        .box { background: #1b263b; border: 1px solid #415a77; border-radius: 10px; padding: 25px; width: 100%; max-width: 500px; }
        h2 { color: #38b000; text-align: center; margin-top: 0; }
        .form-group { margin-bottom: 12px; }
        label { display: block; font-size: 13px; color: #a0aec0; margin-bottom: 4px; }
        input, select { width: 100%; padding: 10px; background: #0d1b2a; border: 1px solid #415a77; color: #fff; border-radius: 4px; box-sizing: border-box; }
        .btn { width: 100%; padding: 12px; background: #38b000; border: none; color: white; border-radius: 4px; cursor: pointer; font-weight: bold; margin-top: 10px; }
        .btn:hover { background: #2d8a00; }
        .back-link { display: block; text-align: center; margin-top: 15px; color: #1a73e8; text-decoration: none; font-size: 13px; }
    </style>
</head>
<body>
    <div class="box">
        <h2>Seller Portal</h2>
        <form onsubmit="addProduct(event)">
            <div class="form-group">
                <label>Seller Nickname:</label>
                <input type="text" id="seller-name" placeholder="CryptoKing" required>
            </div>
            <div class="form-group">
                <label>Category Market:</label>
                <select id="prod-cat" onchange="toggleCategoryFields()">
                    <option value="cc">CC Market</option>
                    <option value="dumps">Dumps Market</option>
                    <option value="fullz">Fullz Market</option>
                    <option value="identity">SSN / Driving License / National ID</option>
                </select>
            </div>

            <!-- कार्ड फील्ड्स -->
            <div id="card-fields">
                <div class="form-group">
                    <label>Bins (First 6 digits):</label>
                    <input type="text" id="prod-bins" placeholder="453211">
                </div>
                <div class="form-group">
                    <label>Card Type:</label>
                    <input type="text" id="prod-type" placeholder="Visa / Mastercard">
                </div>
                <div class="form-group">
                    <label>CVV Status (VVB / Non-VVB):</label>
                    <select id="prod-cvv">
                        <option value="with-cvv">With CVV (VVB)</option>
                        <option value="non-cvv">Non-CVV</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Expiry (MM/YY):</label>
                    <input type="text" id="prod-expiry" placeholder="12/28">
                </div>
            </div>

            <!-- आइडेंटिटी / SSN / DL फील्ड्स -->
            <div id="identity-fields" style="display:none;">
                <div class="form-group">
                    <label>Document Type:</label>
                    <select id="doc-type">
                        <option value="SSN">SSN (Social Security Number)</option>
                        <option value="Driving License">Driving License</option>
                        <option value="National ID">National ID Card</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Document Number / ID:</label>
                    <input type="text" id="doc-number" placeholder="Enter SSN / DL / ID Number">
                </div>
            </div>
<div class="form-group">
                <label>Holder Name:</label>
                <input type="text" id="prod-holder" placeholder="John Doe" required>
            </div>
            <div class="form-group">
                <label>Country & State:</label>
                <input type="text" id="prod-country" placeholder="US / NY" required>
            </div>
            <div class="form-group">
                <label>Full Address:</label>
                <input type="text" id="prod-address" placeholder="123 Main St, Zip: 10001" required>
            </div>
            <div class="form-group">
                <label>Bank / Issuer Name:</label>
                <input type="text" id="prod-bank" placeholder="Chase Bank / Government Issuer" required>
            </div>
            <div class="form-group">
                <label>Price ($ USD):</label>
                <input type="number" step="0.01" id="prod-price" placeholder="15.00" required>
            </div>
            <button type="submit" class="btn">Upload Product to Live Market</button>
        </form>
        <a href="store.html" class="back-link">← Go to Buyer Store Dashboard</a>
    </div>

    <script>
        function toggleCategoryFields() {
            let cat = document.getElementById('prod-cat').value;
            if (cat === 'identity') {
                document.getElementById('card-fields').style.display = 'none';
                document.getElementById('identity-fields').style.display = 'block';
            } else {
                document.getElementById('card-fields').style.display = 'block';
                document.getElementById('identity-fields').style.display = 'none';
            }
        }

        function addProduct(e) {
            e.preventDefault();
            let cat = document.getElementById('prod-cat').value;
            let isIdentity = (cat === 'identity');

            let newProduct = {
                id: Date.now(),
                seller: document.getElementById('seller-name').value.trim(),
                category: cat,
                bins: isIdentity ? 'ID-DOC' : document.getElementById('prod-bins').value.trim(),
                type: isIdentity ? document.getElementById('doc-type').value : document.getElementById('prod-type').value.trim(),
                cvvStatus: isIdentity ? 'N/A' : document.getElementById('prod-cvv').value,
                docNumber: isIdentity ? document.getElementById('doc-number').value.trim() : '****',
                expiry: isIdentity ? 'N/A' : document.getElementById('prod-expiry').value.trim(),
                name: document.getElementById('prod-holder').value.trim(),
                country: document.getElementById('prod-country').value.trim(),
                address: document.getElementById('prod-address').value.trim(),
                bank: document.getElementById('prod-bank').value.trim(),
                price: parseFloat(document.getElementById('prod-price').value),
                totalRatingScore: 5.0,
                ratingCount: 1
            };

            let products = JSON.parse(localStorage.getItem('czc_products') || '[]');
            products.push(newProduct);
            localStorage.setItem('czc_products', JSON.stringify(products));

            alert('Product successfully listed on the live market!');
            e.target.reset();
            toggleCategoryFields();
        }
    </script>
</body>
</html>
store.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cards Zones Club — Secure Market Portal</title>
    <style>
        * { box-sizing: border-box; }
        body { background-color: #0d1b2a; color: #ffffff; font-family: 'Courier New', Courier, monospace; margin: 0; padding: 0; display: flex; height: 100vh; overflow: hidden; }

        .sidebar-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.6); z-index: 99; display: none; }
        .sidebar-overlay.active { display: block; }

        .sidebar { width: 280px; background-color: #1b263b; border-right: 1px solid #415a77; display: flex; flex-direction: column; flex-shrink: 0; position: fixed; height: 100%; top: 0; left: 0; z-index: 100; transition: transform 0.3s ease; transform: translateX(-100%); }
        .sidebar.open { transform: translateX(0); }
        
        .sidebar-brand-box { padding: 18px 15px; border-bottom: 1px solid #415a77; background: #0d1b2a; display: flex; justify-content: space-between; align-items: center; }
        .club-badge { display: inline-block; border: 1px solid #415a77; padding: 2px 6px; font-size: 9px; letter-spacing: 2px; color: #adb5bd; background-color: #1b263b; border-radius: 3px; text-transform: uppercase; }
        .club-name { font-size: 16px; font-weight: bold; color: #ffffff; letter-spacing: 1.5px; text-transform: uppercase; margin-top: 4px; }
        .btn-close-menu { background: none; border: none; color: #ff4d4d; font-size: 20px; cursor: pointer; font-weight: bold; }

        .sidebar-menu { list-style: none; padding: 12px; margin: 0; overflow-y: auto; flex-grow: 1; }
        .sidebar-menu li { padding: 10px 12px; margin-bottom: 6px; background: #0d1b2a; border: 1px solid #415a77; border-radius: 4px; font-size: 12px; color: #adb5bd; cursor: pointer; transition: all 0.2s; }
        .sidebar-menu li:hover, .sidebar-menu li.active { background: #38b000; color: #ffffff; border-color: #38b000; font-weight: bold; }

        .main-container { flex-grow: 1; display: flex; flex-direction: column; overflow: hidden; background-color: #0d1b2a; width: 100%; }

        .top-navbar { height: 60px; background-color: #1b263b; border-bottom: 1px solid #415a77; display: flex; justify-content: space-between; align-items: center; padding: 0 20px; flex-shrink: 0; }
        .nav-left { display: flex; align-items: center; gap: 15px; }
        .btn-hamburger { background: #0d1b2a; border: 1px solid #415a77; color: #38b000; padding: 6px 12px; border-radius: 4px; cursor: pointer; font-size: 16px; font-weight: bold; }
        
        .user-meta-info { font-size: 12px; color: #adb5bd; display: flex; gap: 15px; align-items: center; }
        .role-owner { color: #ffb703; font-weight: bold; text-transform: uppercase; }
        .role-seller { color: #48cae4; font-weight: bold; text-transform: uppercase; }
        .role-member { color: #38b000; font-weight: bold; text-transform: uppercase; }
        .balance-text { color: #38b000; font-weight: bold; font-family: monospace; }

        .btn-topup { background-color: #38b000; color: #fff; border: none; padding: 6px 12px; border-radius: 4px; font-size: 11px; font-weight: bold; cursor: pointer; text-transform: uppercase; }
        .btn-topup:hover { background-color: #2e9000; }
        .btn-logout { background-color: #d90429; color: #fff; border: none; padding: 6px 12px; border-radius: 4px; font-size: 11px; font-weight: bold; cursor: pointer; text-transform: uppercase; }
        .btn-logout:hover { background-color: #c90426; }

        .content-viewport { padding: 25px; overflow-y: auto; flex-grow: 1; }
        .view-section { display: none; }
        .view-section.active-content { display: block; }

        .section-title { font-size: 16px; color: #ffb703; text-transform: uppercase; margin-top: 0; border-bottom: 1px solid #415a77; padding-bottom: 10px; margin-bottom: 20px; }

.grid-cards { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 15px; }
        .card-item { background-color: #1b263b; border: 1px solid #415a77; border-radius: 6px; padding: 15px; }
        .card-title { font-size: 13px; color: #ffb703; font-weight: bold; margin-bottom: 6px; }
        .card-detail { font-size: 12px; color: #adb5bd; margin: 3px 0; }
        .btn-buy { background-color: #38b000; color: #fff; border: none; width: 100%; padding: 8px; border-radius: 4px; font-weight: bold; cursor: pointer; font-size: 11px; margin-top: 10px; text-transform: uppercase; }
        .btn-buy:hover { background-color: #2e9000; }

        .notice-box { background: #1b263b; border: 1px solid #415a77; padding: 18px; border-radius: 6px; margin-bottom: 20px; font-size: 12px; line-height: 1.6; color: #adb5bd; }
        .notice-box h3 { color: #ffb703; margin-top: 0; font-size: 14px; text-transform: uppercase; }
        .form-group { margin-bottom: 12px; }
        .form-group label { display: block; margin-bottom: 4px; color: #fff; }
        .form-group input { width: 100%; padding: 8px; background: #0d1b2a; border: 1px solid #415a77; color: #fff; border-radius: 4px; font-family: monospace; }

        .modal-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.8); z-index: 1000; display: none; justify-content: center; align-items: center; }
        .crypto-modal { background: #1b263b; border: 1px solid #38b000; padding: 25px; border-radius: 8px; width: 90%; max-width: 450px; }
        .crypto-modal h3 { color: #38b000; margin-top: 0; font-size: 16px; text-transform: uppercase; border-bottom: 1px solid #415a77; padding-bottom: 8px; }
        .crypto-option { background: #0d1b2a; border: 1px solid #415a77; padding: 10px; border-radius: 4px; margin-bottom: 10px; cursor: pointer; font-size: 12px; display: flex; justify-content: space-between; align-items: center; }
        .crypto-option:hover { border-color: #38b000; }
        .deposit-box { background: #0d1b2a; border: 1px solid #415a77; padding: 12px; border-radius: 4px; margin-top: 12px; font-size: 11px; color: #adb5bd; word-break: break-all; display: none; }
        .crypto-address { color: #ffb703; font-family: monospace; margin-top: 5px; }

        .admin-banner { background: rgba(255, 183, 3, 0.05); border: 1px dashed #ffb703; padding: 12px; border-radius: 6px; margin-bottom: 20px; display: none; }
        .seller-banner { background: rgba(72, 202, 228, 0.05); border: 1px dashed #48cae4; padding: 12px; border-radius: 6px; margin-bottom: 20px; display: none; }
        .seller-inbox { background: #0d1b2a; border: 1px solid #48cae4; padding: 15px; border-radius: 6px; margin-top: 15px; }
    </style>
</head>
<body>

    <div id="sidebar-overlay" class="sidebar-overlay" onclick="toggleSidebar()"></div>

    <div id="sidebar-menu" class="sidebar">
        <div class="sidebar-brand-box">
            <div>
                <div class="club-badge">EST. 2020</div>
                <div class="club-name">Cards Zone Club</div>
            </div>
            <button class="btn-close-menu" onclick="toggleSidebar()">✕</button>
        </div>
        <ul class="sidebar-menu">
            <li class="active" onclick="switchView('store', 'all', this)">⚡ All Assets Catalog</li>
            <li onclick="switchView('store', 'cc', this)">💳 Credit Cards (CC)</li>
            <li onclick="switchView('store', 'fullz', this)">📄 Fullz (CC + SSN + DOB)</li>
            <li onclick="switchView('store', 'dumps', this)">💾 Track 1/2 Dumps</li>
            <li onclick="switchView('store', 'ssn', this)">🆔 SSN / DOB Records</li>
            <li onclick="switchView('store', 'proxies', this)">🔒 Elite Proxies</li>
            <li onclick="switchView('store', 'rdp_vpn', this)">🌐 Secure RDP & VPN</li>
<li style="border-top: 1px solid #415a77; margin-top: 8px; padding-top: 10px; color: #ffb703; font-weight: bold; font-size: 11px;">[ PARTNERS & POLICIES ]</li>
            <li id="menu-partner-link" onclick="switchView('tool', 'partner_form', this)">🤝 Seller Partner Program</li>
            <li onclick="switchView('tool', 'important_notes', this)">📋 Important Security & Rules</li>
        </ul>
    </div>

    <div id="crypto-modal" class="modal-overlay">
        <div class="crypto-modal">
            <h3>Select Cryptocurrency for Deposit</h3>
            <p style="font-size: 11px; color: #adb5bd; margin-bottom: 15px;">Choose your coin for secure automated deposit.</p>
            
            <div class="crypto-option" onclick="selectCrypto('BTC')">
              <span>⚡ Bitcoin (BTC - xpub/zpub)</span>
              <span style="color: #38b000; font-weight: bold;">Select</span>
            </div>
            <div class="crypto-option" onclick="selectCrypto('XMR')">
              <span>🔒 Monero (XMR)</span>
              <span style="color: #38b000; font-weight: bold;">Select</span>
            </div>
            <div class="crypto-option" onclick="selectCrypto('LTC')">
              <span>🚀 Litecoin (LTC)</span>
              <span style="color: #38b000; font-weight: bold;">Select</span>
            </div>
            <div class="crypto-option" onclick="selectCrypto('DOGE')">
              <span>🐕 Dogecoin (DOGE)</span>
              <span style="color: #38b000; font-weight: bold;">Select</span>
            </div>

            <div id="deposit-address-box" class="deposit-box">
                <div id="deposit-title" style="font-weight: bold; color: #fff;"></div>
                <div class="crypto-address" id="deposit-address-val"></div>
                <p style="margin-top: 8px; font-size: 10px; color: #ff4d4d;">Send exact amount. Balance updates automatically.</p>
            </div>

            <button onclick="closeCryptoModal()" style="background: #d90429; color: #fff; border: none; width: 100%; padding: 8px; border-radius: 4px; font-weight: bold; cursor: pointer; margin-top: 15px; font-size: 11px; text-transform: uppercase;">Close Portal</button>
        </div>
    </div>

    <div class="main-container">
        <div class="top-navbar">
            <div class="nav-left">
                <button class="btn-hamburger" onclick="toggleSidebar()">☰</button>
                <div id="nav-user-info" class="user-meta-info">
                    <span>ID: <strong>loading...</strong></span>
                    <span>Role: ...</span>
                    <span>Balance: <strong class="balance-text">$0.00 USD</strong></span>
                </div>
            </div>
            <div style="display: flex; gap: 8px; align-items: center;">
                <button onclick="openCryptoModal()" class="btn-topup">+ Top Up Crypto</button>
                <button onclick="executeLogout()" class="btn-logout">Log Out</button>
            </div>
        </div>

        <div class="content-viewport">
            
            <!-- Owner Admin Control Panel -->
            <div id="admin-panel-box" class="admin-banner">
                <h3 style="color: #ffb703; margin: 0 0 4px 0; font-size: 13px;">⚡ Owner & Admin Root Control Panel</h3>
                <p style="font-size: 11px; color: #adb5bd; margin: 0 0 8px 0;">Full root access granted. Control marketplace parameters and toggle partner program access.</p>
                <div style="display: flex; gap: 10px; margin-top: 8px;">
                    <button onclick="alert('Root database manager initialized.')" style="background: #ffb703; color: #0d1b2a; border: none; padding: 4px 8px; font-size: 11px; font-weight: bold; cursor: pointer; border-radius: 3px;">Open Root Manager</button>
<button onclick="togglePartnerProgram()" id="btn-toggle-partner" style="background: #38b000; color: #fff; border: none; padding: 4px 8px; font-size: 11px; font-weight: bold; cursor: pointer; border-radius: 3px;">Toggle Partner Program: ON</button>
                </div>
            </div>

            <!-- Official Seller Injection Panel with Notification / Warning Inbox -->
            <div id="seller-panel-box" class="seller-banner">
                <h3 style="color: #48cae4; margin: 0 0 4px 0; font-size: 13px;">🛍️ Official Vendor Injection Panel</h3>
                <p style="font-size: 11px; color: #adb5bd; margin: 0 0 8px 0;">Welcome vendor! List items strictly within authorized categories.</p>
                <button onclick="alert('Asset injector ready for product listing.')" style="background: #48cae4; color: #0d1b2a; border: none; padding: 4px 8px; font-size: 11px; font-weight: bold; cursor: pointer; border-radius: 3px;">Upload Categorized Product</button>
                
                <!-- Seller Warning & Notification Inbox Box -->
                <div class="seller-inbox">
                    <h4 style="color: #48cae4; margin-top: 0; font-size: 12px; text-transform: uppercase;">📬 Vendor Notification & Warning Inbox</h4>
                    <p style="font-size: 11px; color: #e5e7eb; margin: 4px 0;">[SYSTEM ALERT] 24/7 Fraud Monitoring: No suspicious activity detected on your listings.</p>
                    <p style="font-size: 11px; color: #f59e0b; margin: 4px 0;">[ADMIN NOTICE] Ensure all CC listings adhere to strict 1-hour refund policy guidelines.</p>
                </div>
            </div>

            <!-- STORE VIEW -->
            <div id="section-store" class="view-section active-content">
                <h2 class="section-title" id="partition-title">Catalog: All Assets</h2>
                <div class="grid-cards" id="store-catalog-grid">
                    <div class="card-item">
                        <div class="card-title">[CC] US Gold Verified Node</div>
                        <div class="card-detail">Type: Visa / Classic / Non-VBV</div>
                        <div class="card-detail">Price: $15.00 USD</div>
                        <button class="btn-buy" onclick="alert('Escrow acquisition successful.')">Acquire Asset</button>
                    </div>
                    <div class="card-item">
                        <div class="card-title">[Fullz] Full Identity Record</div>
                        <div class="card-detail">Includes: CC + SSN + DOB + DL</div>
                        <div class="card-detail">Price: $35.00 USD</div>
                        <button class="btn-buy" onclick="alert('Escrow acquisition successful.')">Acquire Asset</button>
                    </div>
                </div>
            </div>

            <!-- PARTNER / SELLER APPLICATION FORM VIEW -->
            <div id="section-partner_form" class="view-section">
                <h2 class="section-title">Seller Partner Program Application</h2>
                <div class="notice-box">
                    <p>Want to become an Official Seller on Cards Zones Club? Fill out the partnership application below. Our team reviews all requests daily.</p>
                    <div class="form-group">
                        <label>Your Full Name / Vendor Name:</label>
                        <input type="text" id="p-name" placeholder="Enter vendor name">
                    </div>
                    <div class="form-group">
                        <label>Email Address:</label>
                        <input type="email" id="p-email" placeholder="Enter email">
                    </div>
                    <div class="form-group">
                        <label>Telegram Username (e.g. @username):</label>
                        <input type="text" id="p-tg" placeholder="@yourtelegram">
                    </div>
                    <button class="btn-buy" onclick="submitPartnerForm()" style="margin-top: 10px;">Submit Partner Application</button>
                </div>
            </div>
<!-- IMPORTANT SECURITY & POLICIES VIEW -->
            <div id="section-important_notes" class="view-section">
                <h2 class="section-title">Important Security Notes & Policies</h2>
                <div class="notice-box">
                    <h3>⚠️ 36-Digit Private Key Loss Warning</h3>
                    <p>Your 36-digit Private Key is the ONLY way to access your account. If lost, <strong>it cannot be recovered</strong> by administration under any circumstances. Keep it offline and secure.</p>
                    
                    <h3 style="margin-top: 15px;">⏳ Account Activation & Deposit Policy</h3>
                    <p>Accounts registered without any deposit or activation within <strong>4 days</strong> will be automatically deleted from the system. Only accounts activated via a valid deposit maintain permanent logs and details on our secure servers.</p>

                    <h3 style="margin-top: 15px;">🤝 Partner Program & Seller Rules</h3>
                    <p>Official Sellers and partners are strictly prohibited from sharing or allowing anyone else to login to their vendor accounts. Any unauthorized account sharing will result in immediate termination from the Partner Program. You must inform administration in advance for any necessary access clearance.</p>

                    <h3 style="margin-top: 15px;">🔄 Strict Refund Policy</h3>
                    <p>Refund requests for Credit Cards (CC) will be processed and eligible after <strong>1 hour</strong> from the time of purchase. Please review your items accordingly.</p>

                    <h3 style="margin-top: 15px;">🛡️ 24/7 Real-Time Fraud Monitoring</h3>
                    <p>Our autonomous security system actively monitors all buyers and sellers 24/7. Any flash payments, unauthorized script injections, or suspicious multi-accounting triggers an immediate system alert and lock.</p>

                    <h3 style="margin-top: 15px;">☎️ Official Support & Helpdesk</h3>
                    <p>For any disputes, vendor inquiries, or technical support, contact our official admin on Telegram exclusively at: <strong style="color: #38b000;">@Sheikhcarders</strong></p>
                </div>
            </div>

        </div>
    </div>

    <script>
        function toggleSidebar() {
            let sidebar = document.getElementById('sidebar-menu');
            let overlay = document.getElementById('sidebar-overlay');
            if (sidebar && overlay) {
                sidebar.classList.toggle('open');
                overlay.classList.toggle('active');
            }
        }

        function openCryptoModal() {
            let modal = document.getElementById('crypto-modal');
            if (modal) modal.style.display = 'flex';
        }

        function closeCryptoModal() {
            let modal = document.getElementById('crypto-modal');
            if (modal) modal.style.display = 'none';
        }

        function selectCrypto(coin) {
            let box = document.getElementById('deposit-address-box');
            let title = document.getElementById('deposit-title');
            let addrVal = document.getElementById('deposit-address-val');
            
            if (box && title && addrVal) {
                box.style.display = 'block';
                if (coin === 'BTC') {
                    title.innerText = "Bitcoin (BTC) Deposit Address (xpub/zpub derived):";
                    addrVal.innerText = "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh";
                } else if (coin === 'XMR') {
                    title.innerText = "Monero (XMR) Stealth Address:";
                    addrVal.innerText = "888tNkZrPN6JsEgekjMnSB9pA2s8jJvY42a7q6m5n8Xp2";
                } else if (coin === 'LTC') {
                    title.innerText = "Litecoin (LTC) Deposit Address:";
addrVal.innerText = "LTC1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh";
                } else if (coin === 'DOGE') {
                    title.innerText = "Dogecoin (DOGE) Deposit Address:";
                    addrVal.innerText = "D8VzP9pA2s8jJvY42a7q6m5n8Xp2xy2kgdygjrs";
                }
            }
        }

        function togglePartnerProgram() {
            let link = document.getElementById('menu-partner-link');
            let btn = document.getElementById('btn-toggle-partner');
            let currentState = localStorage.getItem('czc_partner_status') === 'disabled';
            
            if (currentState) {
                localStorage.setItem('czc_partner_status', 'enabled');
                if(link) link.style.display = 'block';
                if(btn) { btn.innerText = "Toggle Partner Program: ON"; btn.style.background = "#38b000"; }
                alert("Partner Program has been enabled.");
            } else {
                localStorage.setItem('czc_partner_status', 'disabled');
                if(link) link.style.display = 'none';
                if(btn) { btn.innerText = "Toggle Partner Program: OFF"; btn.style.background = "#d90429"; }
                alert("Partner Program has been disabled by Owner.");
            }
        }

        function submitPartnerForm() {
            let name = document.getElementById('p-name').value.trim();
            let email = document.getElementById('p-email').value.trim();
            let tg = document.getElementById('p-tg').value.trim();

            if (!name  !email  !tg) {
                alert("Please fill out all partnership form fields.");
                return;
            }
            alert(Application submitted successfully for vendor '${name}'! Our team will contact you via Telegram (${tg}).);
            document.getElementById('p-name').value = '';
            document.getElementById('p-email').value = '';
            document.getElementById('p-tg').value = '';
        }

        function initializeSession() {
            try {
                let activeUser = localStorage.getItem('czc_current_user');
                let sessionToken = localStorage.getItem('czc_local_session_token');

                if (!activeUser || !sessionToken) {
                    window.location.href = 'index.html';
                    return;
                }

                let userDatabase = [];
                try { userDatabase = JSON.parse(localStorage.getItem('czc_users') || '[]'); } catch(e) { userDatabase = []; }

                let sessionProfile = userDatabase.find(u => u.username && u.username.toLowerCase() === activeUser.toLowerCase());

                if (!sessionProfile) {
                    window.location.href = 'index.html';
                    return;
                }

                if (sessionProfile.username.toLowerCase() === 'admin') {
                    sessionProfile.role = 'Owner';
                    if (sessionProfile.balance_usd < 99999) sessionProfile.balance_usd = 99999.00;
                    localStorage.setItem('czc_users', JSON.stringify(userDatabase));
                } else {
                    if (sessionProfile.balance_usd === undefined) {
                        sessionProfile.balance_usd = 0.00;
                        localStorage.setItem('czc_users', JSON.stringify(userDatabase));
                    }
                }

                let roleClass = 'role-member';
                let adminBox = document.getElementById('admin-panel-box');
                let sellerBox = document.getElementById('seller-panel-box');

                if (sessionProfile.role === 'Owner') {
                    roleClass = 'role-owner';
                    if (adminBox) adminBox.style.display = 'block';
                } else if (sessionProfile.role === 'Official Seller') {
                    roleClass = 'role-seller';
                    if (sellerBox) sellerBox.style.display = 'block';
                }
let userInfo = document.getElementById('nav-user-info');
                if (userInfo) {
                    userInfo.innerHTML = 
                        <span>ID: <strong>${sessionProfile.username}</strong></span>
                        <span>Role: <span class="${roleClass}">${sessionProfile.role}</span></span>
                        <span>Balance: <strong class="balance-text">$${(sessionProfile.balance_usd || 0).toFixed(2)} USD</strong></span>
                    ;
                }

                if (localStorage.getItem('czc_partner_status') === 'disabled') {
                    let link = document.getElementById('menu-partner-link');
                    let btn = document.getElementById('btn-toggle-partner');
                    if(link) link.style.display = 'none';
                    if(btn) { btn.innerText = "Toggle Partner Program: OFF"; btn.style.background = "#d90429"; }
                }

            } catch(err) {
                console.error("Session load error:", err);
            }
        }

        function switchView(mode, key, element) {
            document.querySelectorAll('.sidebar-menu li').forEach(li => li.classList.remove('active'));
            if (element) element.classList.add('active');

            document.querySelectorAll('.view-section').forEach(sec => sec.classList.remove('active-content'));

            if (mode === 'store') {
                document.getElementById('section-store').classList.add('active-content');
                let titleElem = document.getElementById('partition-title');
                if (titleElem) titleElem.innerText = Catalog: ${key.toUpperCase()};
            } else if (mode === 'tool') {
                let targetSec = document.getElementById(section-${key});
                if (targetSec) targetSec.classList.add('active-content');
            }
            toggleSidebar();
        }

        function executeLogout() {
            localStorage.removeItem('czc_local_session_token');
            localStorage.removeItem('czc_current_user');
            window.location.href = 'index.html';
        }

        window.onload = function() {
            initializeSession();
        };
    </script>
</body>
</html>
