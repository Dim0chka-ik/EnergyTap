<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, maximum-scale=1.0, minimum-scale=1.0, shrink-to-fit=yes">
    <title>EnergyTap</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: radial-gradient(circle at 10% 20%, #1a2a1a, #0a140a);
            font-family: 'Segoe UI', 'Orbitron', system-ui, -apple-system, sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1.2rem;
            margin: 0;
            position: relative;
            overflow-x: hidden;
            overflow-y: auto;
        }

        /* звездный фон */
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: radial-gradient(2px 2px at 15px 60px, #c8e6c9, rgba(0,0,0,0)),
                              radial-gradient(2px 2px at 75vw 15vh, #a5d6a7, rgba(0,0,0,0)),
                              radial-gradient(2px 2px at 30vw 80vh, #ffcc80, rgba(0,0,0,0)),
                              radial-gradient(3px 3px at 85vw 70vh, #ffb74d, rgba(0,0,0,0)),
                              radial-gradient(2px 2px at 10vw 45vh, #81c784, rgba(0,0,0,0)),
                              radial-gradient(4px 4px at 92vw 88vh, #ff9800, rgba(0,0,0,0)),
                              radial-gradient(2px 2px at 50vw 10vh, #c8e6c9, rgba(0,0,0,0)),
                              radial-gradient(2px 2px at 68vw 55vh, #4caf50, rgba(0,0,0,0));
            background-size: 200px 200px;
            background-repeat: no-repeat;
            pointer-events: none;
            z-index: 0;
            opacity: 0.7;
        }

        /* пиксельные котики */
        .flying-cat {
            position: fixed;
            font-size: 24px;
            z-index: 1;
            pointer-events: none;
            opacity: 0.8;
            animation: floatCat 15s infinite alternate ease-in-out;
            filter: drop-shadow(0 4px 6px rgba(0,0,0,0.4));
            image-rendering: pixelated;
        }
        .cat1 { top: 10%; left: 5%; animation-duration: 17s; font-size: 28px; }
        .cat2 { top: 25%; left: 90%; animation-duration: 13s; font-size: 22px; animation-delay: -2s; }
        .cat3 { top: 70%; left: 8%; animation-duration: 19s; font-size: 26px; animation-delay: -5s; }
        .cat4 { top: 60%; left: 85%; animation-duration: 14s; font-size: 30px; animation-delay: -7s; }
        .cat5 { top: 40%; left: 45%; animation-duration: 20s; font-size: 24px; animation-delay: -3s; }
        .cat6 { top: 85%; left: 55%; animation-duration: 16s; font-size: 20px; animation-delay: -9s; }

        @keyframes floatCat {
            0% { transform: translate(0, 0) rotate(0deg); }
            25% { transform: translate(25px, -35px) rotate(5deg); }
            50% { transform: translate(-15px, 20px) rotate(-3deg); }
            75% { transform: translate(35px, 10px) rotate(2deg); }
            100% { transform: translate(-20px, -25px) rotate(-4deg); }
        }

        /* облачко профиля */
        .profile-cloud {
            position: fixed;
            top: 1.5rem;
            right: 2rem;
            z-index: 100;
            cursor: pointer;
            filter: drop-shadow(0 8px 16px rgba(0,0,0,0.5));
            transition: transform 0.2s ease;
            display: flex;
            align-items: center;
            gap: 0.6rem;
            background: rgba(255, 152, 0, 0.25);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 4rem;
            padding: 0.4rem 1.2rem 0.4rem 0.9rem;
            border: 2px solid #4caf50;
            box-shadow: 0 0 25px #4caf50, 0 0 15px #ff9800;
            color: white;
        }

        .profile-cloud:hover {
            transform: scale(1.05);
            background: rgba(255, 152, 0, 0.4);
            border-color: #ff9800;
            box-shadow: 0 0 35px #ff9800, 0 0 20px #4caf50;
        }

        .cloud-icon {
            font-size: 2.5rem;
            filter: drop-shadow(0 0 8px #fff);
            display: flex;
            align-items: center;
        }

        .cloud-text {
            font-weight: bold;
            color: #ffffff;
            text-shadow: 0 0 15px #4caf50;
            letter-spacing: 0.5px;
            font-size: 1rem;
            max-width: 140px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        /* модальное окно */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(6px);
            -webkit-backdrop-filter: blur(6px);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 200;
            animation: fadeIn 0.2s ease;
        }

        .modal-overlay.active {
            display: flex;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .auth-modal {
            background: linear-gradient(160deg, #1b2e1b, #0a1f0a);
            border: 2px solid #ff9800;
            border-radius: 2.8rem;
            padding: 2rem 2rem 1.8rem;
            max-width: 380px;
            width: 90%;
            box-shadow: 0 0 55px #ff9800, 0 0 30px #4caf50, 0 20px 40px rgba(0,0,0,0.8);
            color: #e8f5e9;
            position: relative;
            animation: modalPop 0.25s ease;
        }

        @keyframes modalPop {
            from { transform: scale(0.9); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }

        .tab-buttons {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-bottom: 1.8rem;
        }

        .tab-btn {
            background: transparent;
            border: none;
            font-size: 1.25rem;
            font-weight: 600;
            color: #a5d6a7;
            padding: 0.5rem 1.2rem;
            border-radius: 3rem;
            cursor: pointer;
            transition: 0.2s;
            letter-spacing: 0.5px;
            border-bottom: 2px solid transparent;
        }

        .tab-btn.active {
            color: #ffb74d;
            text-shadow: 0 0 10px #ff9800;
            border-bottom: 2px solid #ff9800;
            background: rgba(255, 152, 0, 0.15);
        }

        .form-group {
            margin-bottom: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 0.4rem;
        }

        .form-group label {
            font-size: 0.9rem;
            color: #c8e6c9;
            margin-left: 0.5rem;
        }

        .form-group input {
            background: #0e1a0e;
            border: 1px solid #ff9800;
            border-radius: 2.5rem;
            padding: 0.9rem 1.3rem;
            font-size: 1rem;
            color: white;
            outline: none;
            transition: 0.2s;
            box-shadow: inset 0 0 8px rgba(76, 175, 80, 0.5);
        }

        .form-group input:focus {
            border-color: #4caf50;
            box-shadow: 0 0 18px #4caf50, inset 0 0 8px #4caf50;
        }

        .auth-actions {
            display: flex;
            flex-direction: column;
            gap: 0.8rem;
        }

        .auth-btn {
            background: radial-gradient(circle at 20% 20%, #ff9800, #e65100);
            border: 2px solid #4caf50;
            color: #1e3000;
            font-weight: bold;
            padding: 0.9rem;
            border-radius: 3rem;
            font-size: 1.1rem;
            letter-spacing: 1px;
            cursor: pointer;
            transition: 0.15s;
            box-shadow: 0 0 25px #ff9800, 0 0 15px #4caf50;
            text-transform: uppercase;
        }

        .auth-btn:hover {
            background: radial-gradient(circle at 20% 20%, #ffb74d, #ff6d00);
            box-shadow: 0 0 40px #ff9800, 0 0 20px #4caf50;
        }

        .close-modal {
            position: absolute;
            top: 0.8rem;
            right: 1.2rem;
            background: none;
            border: none;
            font-size: 2rem;
            color: #81c784;
            cursor: pointer;
            transition: 0.2s;
        }

        .close-modal:hover {
            color: #fff;
            transform: rotate(90deg);
        }

        .error-msg {
            color: #ff7a7a;
            font-size: 0.85rem;
            margin-top: 0.3rem;
            display: none;
        }

        /* игровая оболочка */
        .game-wrapper {
            width: 100%;
            max-width: 700px;
            background: rgba(10, 20, 10, 0.7);
            backdrop-filter: blur(18px);
            -webkit-backdrop-filter: blur(18px);
            border: 2px solid #ff9800;
            border-radius: 3.5rem;
            padding: 2rem 1.8rem;
            box-shadow: 0 30px 50px rgba(0, 40, 0, 0.8), 0 0 0 2px #4caf50, inset 0 0 30px rgba(76, 175, 80, 0.4);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2rem;
            position: relative;
            z-index: 2;
            transition: all 0.2s ease;
        }

        h1 {
            font-size: clamp(2.4rem, 8vw, 3.6rem);
            font-weight: 700;
            letter-spacing: 0.3rem;
            background: linear-gradient(180deg, #c8e6c9 0%, #ffb74d 80%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 0 0 20px #4caf50, 0 0 45px #ff9800;
            margin-bottom: -0.5rem;
            filter: drop-shadow(0 4px 4px rgba(0,0,0,0.7));
        }

        .energy-display {
            background: #0e1a0e;
            border-radius: 5rem;
            padding: 0.9rem 2.2rem;
            border: 2px solid #ff9800;
            box-shadow: 0 0 30px #4caf50, inset 0 0 15px #81c784;
            display: flex;
            align-items: center;
            gap: 1rem;
            font-size: 2.4rem;
            font-weight: bold;
            color: #ffcc80;
            text-shadow: 0 0 15px #ff9800;
            letter-spacing: 1px;
        }

        .click-area {
            position: relative;
            margin: 0.7rem 0 0.5rem;
        }

        #click-core {
            background: radial-gradient(circle at 30% 30%, #a5d6a7, #ff9800);
            width: 160px;
            height: 160px;
            border-radius: 50%;
            box-shadow: 0 0 85px #4caf50, 0 20px 35px rgba(0,0,0,0.8), inset 0 -8px 0 rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 5.2rem;
            cursor: pointer;
            transition: transform 0.08s ease, filter 0.1s;
            user-select: none;
            border: 3px solid #ff9800;
            color: #1e3000;
            text-shadow: 0 0 12px white;
        }

        #click-core:active {
            transform: scale(0.89);
            filter: brightness(1.4);
            box-shadow: 0 0 120px #ff9800, 0 15px 30px black;
        }

        .floating-text {
            position: absolute;
            font-weight: bold;
            color: #c8e6c9;
            text-shadow: 0 0 22px #4caf50;
            pointer-events: none;
            animation: floatUp 1.2s ease-out forwards;
            font-size: 2rem;
            z-index: 20;
            left: 50%;
            transform: translateX(-50%);
        }

        @keyframes floatUp {
            0% { opacity: 1; top: 20px; }
            100% { opacity: 0; top: -70px; font-size: 2.8rem; }
        }

        .stats-panel {
            width: 100%;
            background: rgba(8, 18, 8, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 2.5rem;
            padding: 1.3rem 1.5rem;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            gap: 1rem;
            border: 1px solid #ff9800;
            box-shadow: 0 10px 25px rgba(0,0,0,0.7), 0 0 15px #4caf50;
            color: #c8e6c9;
        }

        .stat-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.2rem;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 0.07rem;
            font-weight: 500;
        }

        .stat-value {
            font-size: 1.7rem;
            font-weight: 700;
            color: #fff;
            text-shadow: 0 0 12px #4caf50;
            background: #0e1a0e;
            padding: 0.2rem 1rem;
            border-radius: 2rem;
        }

        .upgrades {
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 1.1rem;
            margin-top: 0.3rem;
        }

        .upgrade-card {
            background: linear-gradient(145deg, #1b2e1b, #090f09);
            border-radius: 2rem;
            padding: 1rem 1.3rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border: 2px solid #4caf50;
            box-shadow: 0 9px 18px rgba(0, 0, 0, 0.8), 0 0 15px #ff9800;
            transition: all 0.2s;
            flex-wrap: wrap;
        }

        .upgrade-info h4 {
            color: #ffb74d;
            font-size: 1.2rem;
            margin-bottom: 0.2rem;
            text-shadow: 0 0 5px #4caf50;
        }

        .upgrade-info p {
            color: #a5d6a7;
            font-size: 0.9rem;
        }

        .buy-btn {
            background: radial-gradient(circle at 20% 20%, #ff9800, #bf360c);
            border: 2px solid #4caf50;
            color: white;
            font-weight: bold;
            padding: 0.7rem 1.5rem;
            border-radius: 3rem;
            font-size: 1rem;
            cursor: pointer;
            box-shadow: 0 0 18px #ff9800, 0 0 10px #4caf50;
            transition: 0.15s;
        }

        .buy-btn:hover:not(:disabled) {
            background: #e68a00;
            box-shadow: 0 0 35px #ff9800, 0 0 20px #4caf50;
            border-color: #ffcc80;
            transform: scale(1.02);
        }

        .buy-btn:disabled {
            opacity: 0.4;
            filter: grayscale(0.6);
            cursor: not-allowed;
            box-shadow: none;
            border-color: #5e5040;
        }

        .reset-btn {
            background: transparent;
            border: 1px solid #81c784;
            color: #c8e6c9;
            padding: 0.5rem 1.4rem;
            border-radius: 2rem;
            font-size: 0.85rem;
            margin-top: 0.6rem;
            cursor: pointer;
            align-self: flex-end;
            box-shadow: 0 0 10px #4caf50;
        }

        .reset-btn:hover {
            background: #1b3b1b;
            color: white;
            border-color: #ff9800;
        }

        @media (max-width: 480px) {
            .game-wrapper {
                padding: 1.4rem;
                border-radius: 2.5rem;
            }
            .profile-cloud {
                top: 1rem;
                right: 0.5rem;
                padding: 0.3rem 0.8rem;
            }
        }
    </style>
</head>
<body>
    <!-- Пиксельные котики (летают на фоне) -->
    <div class="flying-cat cat1">🐈</div>
    <div class="flying-cat cat2">🐱</div>
    <div class="flying-cat cat3">😺</div>
    <div class="flying-cat cat4">😸</div>
    <div class="flying-cat cat5">🐈</div>
    <div class="flying-cat cat6">😻</div>

    <!-- Облачко профиля -->
    <div class="profile-cloud" id="profileCloud">
        <span class="cloud-icon">☁️</span>
        <span class="cloud-text" id="cloudLabel">Войти</span>
    </div>

    <!-- Модальное окно -->
    <div class="modal-overlay" id="modalOverlay">
        <div class="auth-modal">
            <button class="close-modal" id="closeModalBtn">&times;</button>
            <div class="tab-buttons">
                <button class="tab-btn active" data-tab="login">Вход</button>
                <button class="tab-btn" data-tab="register">Регистрация</button>
            </div>

            <!-- Вход -->
            <form id="loginForm" class="auth-form">
                <div class="form-group">
                    <label>Логин</label>
                    <input type="text" id="loginUsername" placeholder="Ваш логин" required>
                </div>
                <div class="form-group">
                    <label>Пароль</label>
                    <input type="password" id="loginPassword" placeholder="Пароль" required>
                </div>
                <div class="error-msg" id="loginError"></div>
                <div class="auth-actions">
                    <button type="submit" class="auth-btn">Войти</button>
                </div>
            </form>

            <!-- Регистрация -->
            <form id="registerForm" class="auth-form" style="display: none;">
                <div class="form-group">
                    <label>Логин</label>
                    <input type="text" id="regUsername" placeholder="Придумайте логин" required>
                </div>
                <div class="form-group">
                    <label>Пароль</label>
                    <input type="password" id="regPassword" placeholder="Придумайте пароль" required>
                </div>
                <div class="form-group">
                    <label>Повторите пароль</label>
                    <input type="password" id="regPasswordConfirm" placeholder="Повторите пароль" required>
                </div>
                <div class="error-msg" id="regError"></div>
                <div class="auth-actions">
                    <button type="submit" class="auth-btn">Зарегистрироваться</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Игровая часть -->
    <div class="game-wrapper">
        <h1>ENERGYTAP</h1>

        <div class="energy-display">
            <span>⚡</span> <span id="coin-counter">0</span>
        </div>

        <div class="click-area" id="click-container">
            <div id="click-core">⚡️</div>
        </div>

        <div class="stats-panel">
            <div class="stat-item"><span>🔷 Всего кликов</span><span class="stat-value" id="stat-total-clicks">0</span></div>
            <div class="stat-item"><span>✨ За клик</span><span class="stat-value" id="stat-per-click">1</span></div>
            <div class="stat-item"><span>⚡ Авто/сек</span><span class="stat-value" id="stat-auto-per-sec">0</span></div>
            <div class="stat-item"><span>🕒 Время игры</span><span class="stat-value" id="stat-playtime">0м</span></div>
        </div>

        <div class="upgrades">
            <div class="upgrade-card">
                <div class="upgrade-info"><h4>⚡ Энерго-буст</h4><p>+1 энергия за клик</p></div>
                <button class="buy-btn" id="btn-multiplier">💎 <span id="cost-multiplier">15</span></button>
            </div>
            <div class="upgrade-card">
                <div class="upgrade-info"><h4>🔋 Автозарядка</h4><p>+0.5 энергии/сек</p></div>
                <button class="buy-btn" id="btn-autoclick">🛸 <span id="cost-autoclick">50</span></button>
            </div>
        </div>

        <button class="reset-btn" id="reset-stats">🔄 Сбросить прогресс</button>
        <div style="color:#a5d6a7; font-size:0.8rem;">статистика сохраняется локально</div>
    </div>

    <script>
        (function() {
            // Состояние игры
            const state = {
                coins: 0,
                totalClicks: 0,
                clickMultiplier: 1,
                autoCoinsPerSec: 0,
                playStartTime: Date.now(),
                multiplierCost: 15,
                autoclickCost: 50,
                multiplierLevel: 0,
                autoclickLevel: 0
            };

            // Пользовательская сессия
            const USER_KEY = 'energytap_user';
            let currentUser = null;

            // DOM элементы игры
            const coinCounterEl = document.getElementById('coin-counter');
            const clickCore = document.getElementById('click-core');
            const clickContainer = document.getElementById('click-container');
            const statTotalClicks = document.getElementById('stat-total-clicks');
            const statPerClick = document.getElementById('stat-per-click');
            const statAutoPerSec = document.getElementById('stat-auto-per-sec');
            const statPlaytime = document.getElementById('stat-playtime');
            const costMultiplierEl = document.getElementById('cost-multiplier');
            const costAutoclickEl = document.getElementById('cost-autoclick');
            const btnMultiplier = document.getElementById('btn-multiplier');
            const btnAutoclick = document.getElementById('btn-autoclick');
            const resetBtn = document.getElementById('reset-stats');

            // DOM авторизации
            const profileCloud = document.getElementById('profileCloud');
            const cloudLabel = document.getElementById('cloudLabel');
            const modalOverlay = document.getElementById('modalOverlay');
            const closeModalBtn = document.getElementById('closeModalBtn');
            const tabBtns = document.querySelectorAll('.tab-btn');
            const loginForm = document.getElementById('loginForm');
            const registerForm = document.getElementById('registerForm');
            const loginError = document.getElementById('loginError');
            const regError = document.getElementById('regError');

            // Сохранение игры
            const SAVE_KEY = 'energy-tap-save';
            function saveGame() {
                localStorage.setItem(SAVE_KEY, JSON.stringify({...state, playStartTime: state.playStartTime}));
            }
            function loadGame() {
                const saved = localStorage.getItem(SAVE_KEY);
                if (!saved) return;
                try {
                    const data = JSON.parse(saved);
                    Object.assign(state, data);
                    if (!state.playStartTime) state.playStartTime = Date.now();
                } catch (e) {}
            }

            // Работа с пользователем
            function loadUserSession() {
                const stored = localStorage.getItem(USER_KEY);
                if (stored) {
                    try { currentUser = JSON.parse(stored); } catch (e) { currentUser = null; }
                }
                updateCloudUI();
            }
            function saveUserSession(user) {
                currentUser = user;
                localStorage.setItem(USER_KEY, JSON.stringify(user));
                updateCloudUI();
            }
            function logoutUser() {
                currentUser = null;
                localStorage.removeItem(USER_KEY);
                updateCloudUI();
            }
            function updateCloudUI() {
                if (currentUser && currentUser.username) {
                    cloudLabel.textContent = currentUser.username;
                } else {
                    cloudLabel.textContent = 'Войти';
                }
            }

            // Модальное окно
            function openModal() {
                modalOverlay.classList.add('active');
                loginError.style.display = 'none';
                regError.style.display = 'none';
                document.getElementById('loginUsername').value = '';
                document.getElementById('loginPassword').value = '';
                document.getElementById('regUsername').value = '';
                document.getElementById('regPassword').value = '';
                document.getElementById('regPasswordConfirm').value = '';
                switchTab('login');
            }
            function closeModal() { modalOverlay.classList.remove('active'); }
            function switchTab(tabName) {
                tabBtns.forEach(btn => {
                    btn.classList.toggle('active', btn.dataset.tab === tabName);
                });
                loginForm.style.display = tabName === 'login' ? 'block' : 'none';
                registerForm.style.display = tabName === 'register' ? 'block' : 'none';
            }

            // Обработчики форм
            loginForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const username = document.getElementById('loginUsername').value.trim();
                const password = document.getElementById('loginPassword').value;
                const usersDB = JSON.parse(localStorage.getItem('energytap_users') || '{}');
                if (!username || !password) {
                    loginError.textContent = 'Заполните все поля';
                    loginError.style.display = 'block';
                    return;
                }
                if (usersDB[username] && usersDB[username] === password) {
                    saveUserSession({ username });
                    closeModal();
                } else {
                    loginError.textContent = 'Неверный логин или пароль';
                    loginError.style.display = 'block';
                }
            });

            registerForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const username = document.getElementById('regUsername').value.trim();
                const password = document.getElementById('regPassword').value;
                const confirm = document.getElementById('regPasswordConfirm').value;
                if (!username || !password || !confirm) {
                    regError.textContent = 'Заполните все поля';
                    regError.style.display = 'block';
                    return;
                }
                if (password !== confirm) {
                    regError.textContent = 'Пароли не совпадают';
                    regError.style.display = 'block';
                    return;
                }
                if (password.length < 3) {
                    regError.textContent = 'Пароль слишком короткий (мин. 3 символа)';
                    regError.style.display = 'block';
                    return;
                }
                const usersDB = JSON.parse(localStorage.getItem('energytap_users') || '{}');
                if (usersDB[username]) {
                    regError.textContent = 'Пользователь уже существует';
                    regError.style.display = 'block';
                    return;
                }
                usersDB[username] = password;
                localStorage.setItem('energytap_users', JSON.stringify(usersDB));
                saveUserSession({ username });
                closeModal();
            });

            tabBtns.forEach(btn => btn.addEventListener('click', () => switchTab(btn.dataset.tab)));
            closeModalBtn.addEventListener('click', closeModal);
            modalOverlay.addEventListener('click', (e) => { if (e.target === modalOverlay) closeModal(); });

            profileCloud.addEventListener('click', () => {
                if (currentUser) {
                    if (confirm(`Вы вошли как ${currentUser.username}. Выйти?`)) logoutUser();
                } else {
                    openModal();
                }
            });

            // Игровая механика
            function updateUI() {
                coinCounterEl.textContent = Math.floor(state.coins);
                statTotalClicks.textContent = state.totalClicks;
                statPerClick.textContent = state.clickMultiplier;
                statAutoPerSec.textContent = state.autoCoinsPerSec.toFixed(1);
                const elapsed = Math.floor((Date.now() - state.playStartTime) / 1000);
                const m = Math.floor(elapsed / 60);
                const s = elapsed % 60;
                statPlaytime.textContent = `${m}м ${s}с`;
                costMultiplierEl.textContent = state.multiplierCost;
                costAutoclickEl.textContent = state.autoclickCost;
                btnMultiplier.disabled = state.coins < state.multiplierCost;
                btnAutoclick.disabled = state.coins < state.autoclickCost;
            }

            function addCoins(amount, sourceEl) {
                if (amount <= 0) return;
                state.coins += amount;
                updateUI();
                saveGame();
                if (sourceEl) spawnFloatingText(`+${amount.toFixed(1)}`, sourceEl);
            }

            function spawnFloatingText(text, relEl) {
                const floatEl = document.createElement('div');
                floatEl.className = 'floating-text';
                floatEl.textContent = text;
                const rect = relEl.getBoundingClientRect();
                const containerRect = clickContainer.getBoundingClientRect();
                floatEl.style.left = (rect.left + rect.width/2 - containerRect.left) + 'px';
                floatEl.style.top = (rect.top - containerRect.top) + 'px';
                clickContainer.appendChild(floatEl);
                floatEl.addEventListener('animationend', () => floatEl.remove());
            }

            function handleClick(e) {
                const earned = state.clickMultiplier;
                state.totalClicks += 1;
                addCoins(earned, clickCore);
                clickCore.style.transform = 'scale(0.9)';
                setTimeout(() => { clickCore.style.transform = ''; }, 80);
            }

            btnMultiplier.addEventListener('click', () => {
                if (state.coins < state.multiplierCost) return;
                state.coins -= state.multiplierCost;
                state.clickMultiplier += 1;
                state.multiplierLevel++;
                state.multiplierCost = Math.floor(state.multiplierCost * 1.6) + 2;
                updateUI();
                saveGame();
            });

            btnAutoclick.addEventListener('click', () => {
                if (state.coins < state.autoclickCost) return;
                state.coins -= state.autoclickCost;
                state.autoCoinsPerSec += 0.5;
                state.autoclickLevel++;
                state.autoclickCost = Math.floor(state.autoclickCost * 1.7) + 5;
                updateUI();
                saveGame();
            });

            resetBtn.addEventListener('click', () => {
                if (confirm('Сбросить прогресс?')) {
                    state.coins = 0;
                    state.totalClicks = 0;
                    state.clickMultiplier = 1;
                    state.autoCoinsPerSec = 0;
                    state.multiplierCost = 15;
                    state.autoclickCost = 50;
                    state.multiplierLevel = 0;
                    state.autoclickLevel = 0;
                    state.playStartTime = Date.now();
                    saveGame();
                    updateUI();
                }
            });

            // Авто-сбор
            let lastAutoTick = Date.now();
            function autoTick() {
                const now = Date.now();
                const delta = (now - lastAutoTick) / 1000;
                if (delta >= 1 && state.autoCoinsPerSec > 0) {
                    const ticks = Math.floor(delta);
                    const earned = state.autoCoinsPerSec * ticks;
                    if (earned > 0) {
                        state.coins += earned;
                        const displayEl = document.querySelector('.energy-display');
                        spawnFloatingText(`+${earned.toFixed(1)}`, displayEl);
                    }
                    lastAutoTick = now - (delta % 1) * 1000;
                    updateUI();
                    saveGame();
                }
                requestAnimationFrame(autoTick);
            }

            setInterval(() => {
                const elapsed = Math.floor((Date.now() - state.playStartTime) / 1000);
                const m = Math.floor(elapsed / 60);
                const s = elapsed % 60;
                statPlaytime.textContent = `${m}м ${s}с`;
            }, 1000);

            function init() {
                loadGame();
                loadUserSession();
                updateUI();
                lastAutoTick = Date.now();
                requestAnimationFrame(autoTick);

                clickCore.addEventListener('click', handleClick);
                clickCore.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    handleClick(e);
                }, { passive: false });
            }

            init();
        })();
    </script>
</body>
</html>
