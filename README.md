<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Сообщество Скитальцев</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        /* --- Базовые переменные и сброс --- */
        :root {
            --bg-color: var(--tg-theme-bg-color, #1c1c1e);
            --secondary-bg: var(--tg-theme-secondary-bg-color, #2c2c2e);
            --text-color: var(--tg-theme-text-color, #ffffff);
            --hint-color: var(--tg-theme-hint-color, #8e8e93);
            --button-color: var(--tg-theme-button-color, #007aff);
            --button-text: var(--tg-theme-button-text-color, #ffffff);
            --accent-color: #ff4d6d;
            --success-color: #34c759;
            --error-color: #ff3b30;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            display: flex;
            flex-direction: column;
            height: 100vh;
            overflow: hidden;
        }

        /* --- Контейнер для контента --- */
        .content {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            padding-bottom: 90px;
            scroll-behavior: smooth;
        }

        /* --- Экраны (Страницы) --- */
        .screen {
            display: none;
            animation: fadeIn 0.3s ease-in-out;
        }
        .screen.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- Главный экран --- */
        .hero {
            text-align: center;
            margin-top: 20px;
        }
        .avatar {
            width: 140px;
            height: 140px;
            border-radius: 50%;
            border: 4px solid var(--accent-color);
            object-fit: cover;
            box-shadow: 0 8px 20px rgba(255, 77, 109, 0.3);
            margin-bottom: 15px;
        }
        .hero h1 {
            font-size: 26px;
            color: var(--accent-color);
            margin-bottom: 5px;
        }
        .hero p {
            color: var(--hint-color);
            font-size: 15px;
            margin-bottom: 25px;
        }

        /* --- Карточки --- */
        .card {
            background-color: var(--secondary-bg);
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }
        .card h2 {
            font-size: 18px;
            margin-bottom: 15px;
            color: var(--accent-color);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* --- Списки правил --- */
        .rules-list {
            list-style: none;
        }
        .rules-list li {
            margin-bottom: 15px;
            font-size: 15px;
            line-height: 1.5;
            display: flex;
            gap: 10px;
            align-items: flex-start;
        }
        .rules-list li span.icon {
            font-size: 20px;
            flex-shrink: 0;
        }

        /* --- Карточки Админов --- */
        .admin-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        .admin-card {
            display: flex;
            align-items: center;
            gap: 15px;
            background-color: rgba(255,255,255,0.05);
            padding: 12px;
            border-radius: 12px;
            text-decoration: none;
            color: inherit;
            transition: background 0.2s;
            cursor: pointer;
            border: 1px solid transparent;
        }
        .admin-card:active {
            background-color: rgba(255,255,255,0.1);
            border-color: var(--accent-color);
        }
        .admin-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            object-fit: cover;
            border: 2px solid var(--accent-color);
            flex-shrink: 0;
        }
        .admin-info {
            display: flex;
            flex-direction: column;
        }
        .admin-info h4 {
            font-size: 16px;
            margin-bottom: 3px;
        }
        .admin-info p {
            font-size: 13px;
            color: var(--hint-color);
        }
        .admin-role {
            font-size: 11px;
            background: var(--accent-color);
            color: #fff;
            padding: 2px 8px;
            border-radius: 10px;
            display: inline-block;
            margin-top: 4px;
            font-weight: bold;
        }

        /* --- Промокод --- */
        .promo-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-top: 10px;
        }
        .promo-input {
            width: 100%;
            padding: 15px;
            border-radius: 12px;
            border: 2px solid rgba(255,255,255,0.1);
            background-color: rgba(0,0,0,0.2);
            color: var(--text-color);
            font-size: 16px;
            outline: none;
            transition: border-color 0.3s;
            text-align: center;
            letter-spacing: 1px;
        }
        .promo-input:focus {
            border-color: var(--accent-color);
        }
        .promo-input.error {
            border-color: var(--error-color);
            animation: shake 0.4s;
        }
        .promo-input.success {
            border-color: var(--success-color);
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-8px); }
            75% { transform: translateX(8px); }
        }

        .promo-message {
            text-align: center;
            font-size: 14px;
            margin-top: 10px;
            min-height: 20px;
        }
        .promo-message.error { color: var(--error-color); }
        .promo-message.success { color: var(--success-color); font-weight: bold; }

        .promo-result {
            display: none;
            margin-top: 15px;
            text-align: center;
            animation: fadeIn 0.4s ease-in-out;
        }
        .promo-result.active {
            display: block;
        }

        /* --- Кнопки --- */
        .btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            width: 100%;
            padding: 15px;
            background-color: var(--button-color);
            color: var(--button-text);
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: opacity 0.2s, transform 0.1s;
            text-decoration: none;
            margin-bottom: 10px;
        }
        .btn:active {
            opacity: 0.8;
            transform: scale(0.98);
        }
        .btn-secondary {
            background-color: rgba(255,255,255,0.1);
            color: var(--text-color);
        }
        .btn-success {
            background-color: var(--success-color);
            color: #fff;
        }

        /* --- Нижняя навигация --- */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 75px;
            background-color: var(--secondary-bg);
            display: flex;
            justify-content: space-around;
            align-items: center;
            border-top: 1px solid rgba(255,255,255,0.05);
            padding-bottom: env(safe-area-inset-bottom);
            z-index: 100;
        }
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: var(--hint-color);
            font-size: 10px;
            cursor: pointer;
            transition: color 0.2s;
            width: 20%;
            height: 100%;
            gap: 4px;
        }
        .nav-item.active {
            color: var(--accent-color);
        }
        .nav-item svg {
            width: 22px;
            height: 22px;
            fill: currentColor;
        }

        /* --- Профиль пользователя --- */
        .user-info {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 20px;
        }
        .user-info img {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            object-fit: cover;
        }
        .user-info h3 {
            font-size: 18px;
            margin-bottom: 3px;
        }
        .user-info p {
            color: var(--hint-color);
            font-size: 13px;
        }
    </style>
</head>
<body>

    <!-- ОСНОВНОЙ КОНТЕНТ -->
    <div class="content">
        
        <!-- ЭКРАН 1: ГЛАВНАЯ -->
        <div id="screen-home" class="screen active">
            <div class="hero">
                <img src="https://i.pinimg.com/736x/8a/5e/6f/8a5e6f2f4e6e4b3e2c1a0d9e8f7a6b5c.jpg" alt="Аватар" class="avatar">
                <h1>Сообщество Скитальцев</h1>
                <p>Добро пожаловать, путник! 🏕️</p>
            </div>
            <div class="card">
                <h2>✨ О сообществе</h2>
                <p style="font-size: 14px; line-height: 1.6; color: var(--hint-color);">
                    Мы — уютное место для общения, обмена опытом и поиска единомышленников.
                </p>
            </div>
            <button class="btn" onclick="switchScreen('rules')">📜 Ознакомиться с правилами</button>
            <button class="btn btn-secondary" onclick="openChat()">💬 Перейти в чат</button>
        </div>

        <!-- ЭКРАН 2: ПРАВИЛА -->
        <div id="screen-rules" class="screen">
            <div class="card">
                <h2>📜 Правила сообщества</h2>
                <ul class="rules-list">
                    <li><span class="icon">🕵️‍♂️</span><div><strong>Конфиденциальность:</strong> Убедительная просьба не допытываться у наших админов о личной жизни.</div></li>
                    <li><span class="icon">🤬🚫</span><div><strong>Общение:</strong> Общайтесь дружелюбно, без матов.</div></li>
                    <li><span class="icon">💚</span><div><strong>Доверие:</strong> Админы не желают вам зла, давайте сохранять дружескую атмосферу.</div></li>
                </ul>
            </div>
        </div>

        <!-- ЭКРАН 3: АДМИНЫ -->
        <div id="screen-admins" class="screen">
            <div class="card">
                <h2>🛡️ Администрация</h2>
                <div class="admin-list">
                    <a href="https://t.me/@DesMund0" target="_blank" class="admin-card" onclick="haptic()">
                        <img src="https://i.pravatar.cc/150?img=12" alt="Админ 1" class="admin-avatar">
                        <div class="admin-info">
                            <h4>DesMundo (Основатель)</h4>
                            <p>@DesMund0</p>
                            <span class="admin-role">Владелец</span>
                        </div>
                    </a>
                    <a href="https://t.me/k1tsune9" target="_blank" class="admin-card" onclick="haptic()">
                        <img src="https://i.pravatar.cc/150?img=5" alt="Админ 2" class="admin-avatar">
                        <div class="admin-info">
                            <h4>𝓡𝓲𝓷 · 天狐 🦊</h4>
                            <p>@K1tsune9</p>
                            <span class="admin-role">Гл. Модератор</span>
                        </div>
                    </a>
                </div>
            </div>
        </div>

        <!-- ЭКРАН 4: ПРОМОКОД (НОВЫЙ) -->
        <div id="screen-promo" class="screen">
            <div class="card">
                <h2>🎁 Секретный промокод</h2>
                <p style="font-size: 14px; color: var(--hint-color); margin-bottom: 15px;">
                    Введите секретный код, чтобы получить доступ к закрытому чату.
                </p>
                
                <div class="promo-container">
                    <input type="text" id="promoInput" class="promo-input" placeholder="Введите код..." autocomplete="off">
                    <button class="btn" onclick="checkPromo()">Проверить</button>
                    <div id="promoMessage" class="promo-message"></div>
                </div>

                <div id="promoResult" class="promo-result">
                    <p style="color: var(--success-color); margin-bottom: 15px;">✅ Код верный! Держите ссылку:</p>
                    <a href="https://t.me/+U0b2UZstMRBlYjli" target="_blank" class="btn btn-success" onclick="haptic('heavy')">
                        🔗 Вступить в закрытый чат
                    </a>
                </div>
            </div>
        </div>

        <!-- ЭКРАН 5: ПРОФИЛЬ -->
        <div id="screen-profile" class="screen">
            <div class="card">
                <h2>👤 Ваш профиль</h2>
                <div class="user-info">
                    <img id="tg-avatar" src="https://telegram.org/img/t_logo.png" alt="Аватар">
                    <div>
                        <h3 id="tg-name">Загрузка...</h3>
                        <p id="tg-username">@username</p>
                    </div>
                </div>
            </div>
            <button class="btn btn-secondary" onclick="shareApp()">📤 Поделиться приложением</button>
        </div>

    </div>

    <!-- НИЖНЯЯ НАВИГАЦИЯ (ТЕПЕРЬ 5 КНОПОК) -->
    <div class="bottom-nav">
        <div class="nav-item active" onclick="switchScreen('home', this)">
            <svg viewBox="0 0 24 24"><path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/></svg>
            <span>Главная</span>
        </div>
        <div class="nav-item" onclick="switchScreen('rules', this)">
            <svg viewBox="0 0 24 24"><path d="M14 2H6c-1.1 0-1.99.9-1.99 2L4 20c0 1.1.89 2 1.99 2H18c1.1 0 2-.9 2-2V8l-6-6zm2 16H8v-2h8v2zm0-4H8v-2h8v2zm-3-5V3.5L18.5 9H13z"/></svg>
            <span>Правила</span>
        </div>
        <div class="nav-item" onclick="switchScreen('admins', this)">
            <svg viewBox="0 0 24 24"><path d="M12 1L3 5v6c0 5.55 3.84 10.74 9 12 5.16-1.26 9-6.45 9-12V5l-9-4zm0 10.99h7c-.53 4.12-3.28 7.79-7 8.94V12H5V6.3l7-3.11v8.8z"/></svg>
            <span>Админы</span>
        </div>
        <div class="nav-item" onclick="switchScreen('promo', this)">
            <svg viewBox="0 0 24 24"><path d="M20 6h-2.18c.11-.31.18-.65.18-1 0-1.66-1.34-3-3-3-1.05 0-1.96.54-2.5 1.35l-.5.67-.5-.68C10.96 2.54 10.05 2 9 2 7.34 2 6 3.34 6 5c0 .35.07.69.18 1H4c-1.11 0-1.99.89-1.99 2L2 19c0 1.11.89 2 2 2h16c1.11 0 2-.89 2-2V8c0-1.11-.89-2-2-2zm-5-2c.55 0 1 .45 1 1s-.45 1-1 1-1-.45-1-1 .45-1 1-1zM9 4c.55 0 1 .45 1 1s-.45 1-1 1-1-.45-1-1 .45-1 1-1zm11 15H4v-2h16v2zm0-5H4V8h5.08L7 10.83 8.62 12 11 8.76l1-1.36 1 1.36L15.38 12 17 10.83 14.92 8H20v6z"/></svg>
            <span>Промокод</span>
        </div>
        <div class="nav-item" onclick="switchScreen('profile', this)">
            <svg viewBox="0 0 24 24"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>
            <span>Профиль</span>
        </div>
    </div>

    <script>
        // 1. Инициализация Telegram Web App
        const tg = window.Telegram.WebApp;
        tg.expand();
        tg.ready();
        tg.setHeaderColor('secondary_bg_color');

        // 2. Переключение экранов
        function switchScreen(screenId, navElement = null) {
            document.querySelectorAll('.screen').forEach(el => el.classList.remove('active'));
            document.getElementById('screen-' + screenId).classList.add('active');

            const screenMap = ['home', 'rules', 'admins', 'promo', 'profile'];
            document.querySelectorAll('.nav-item').forEach((item, index) => {
                if (screenMap[index] === screenId) item.classList.add('active');
                else item.classList.remove('active');
            });

            if (tg.HapticFeedback) tg.HapticFeedback.impactOccurred('light');
        }

        // 3. Логика промокода
        function checkPromo() {
            const input = document.getElementById('promoInput');
            const message = document.getElementById('promoMessage');
            const result = document.getElementById('promoResult');
            const code = input.value.trim().toLowerCase(); // Приводим к нижнему регистру для удобства

            // Сброс предыдущих состояний
            input.classList.remove('error', 'success');
            message.className = 'promo-message';
            result.classList.remove('active');

            // ПРОВЕРКА КОДА (измените код здесь, если нужно)
            if (code === 'alpha-485-info') {
                input.classList.add('success');
                message.innerText = 'Код принят!';
                message.classList.add('success');
                result.classList.add('active');
                if (tg.HapticFeedback) tg.HapticFeedback.notificationOccurred('success');
                
                // Скрываем клавиатуру на мобильных
                input.blur();
            } else {
                input.classList.add('error');
                message.innerText = '❌ Неверный код. Попробуйте еще раз.';
                message.classList.add('error');
                if (tg.HapticFeedback) tg.HapticFeedback.notificationOccurred('error');
                
                // Убираем анимацию тряски через 400мс, чтобы можно было повторить
                setTimeout(() => input.classList.remove('error'), 400);
            }
        }

        // 4. Вспомогательные функции
        function openChat() {
            tg.openTelegramLink('https://t.me/your_chat_link_here');
        }

        function shareApp() {
            const shareUrl = `https://t.me/share/url?url=${encodeURIComponent('https://t.me/your_bot_link')}&text=${encodeURIComponent('Присоединяйся к сообществу Скитальцев!')}`;
            tg.openTelegramLink(shareUrl);
        }

        function haptic(type = 'light') {
            if (tg.HapticFeedback) {
                if (type === 'heavy') tg.HapticFeedback.impactOccurred('heavy');
                else tg.HapticFeedback.impactOccurred('medium');
            }
        }

        // 5. Автозагрузка данных пользователя
        const user = tg.initDataUnsafe?.user;
        if (user) {
            document.getElementById('tg-name').innerText = `${user.first_name} ${user.last_name || ''}`;
            document.getElementById('tg-username').innerText = user.username ? `@${user.username}` : 'Нет username';
            if (user.photo_url) document.getElementById('tg-avatar').src = user.photo_url;
        } else {
            document.getElementById('tg-name').innerText = 'Гость';
            document.getElementById('tg-username').innerText = 'Откройте в Telegram';
        }

        // 6. Обработка нажатия Enter в поле ввода
        document.getElementById('promoInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                checkPromo();
            }
        });
    </script>
</body>
</html>
