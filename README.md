<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pixel Quiz - Мультиплеер 10x10</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        /* Экран авторизации */
        .login-screen {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 80vh;
        }

        .login-form {
            background: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            text-align: center;
            width: 100%;
            max-width: 400px;
        }

        .login-form h1 {
            color: #333;
            margin-bottom: 10px;
        }

        .login-input {
            width: 100%;
            padding: 15px;
            margin-bottom: 20px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
        }

        .login-btn {
            width: 100%;
            padding: 15px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
        }

        .login-btn:hover {
            background: linear-gradient(135deg, #5a6fd8 0%, #6a4190 100%);
        }

        .admin-hint {
            margin-top: 15px;
            font-size: 12px;
            color: #666;
        }

        /* Игровой экран */
        .game-screen {
            display: none;
        }

        .header {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .user-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .welcome {
            font-size: 20px;
            color: #333;
        }

        .user-role {
            background: #28a745;
            color: white;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 12px;
            margin-left: 10px;
        }

        .user-role.admin {
            background: #dc3545;
        }

        .score {
            background: #667eea;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: bold;
        }

        .logout-btn {
            background: #ff6b6b;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 5px;
            cursor: pointer;
        }

        .logout-btn:hover {
            background: #ff5252;
        }

        .game-content {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 20px;
        }

        .game-board {
            background: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }

        canvas {
            border: 2px solid #ddd;
            border-radius: 8px;
            cursor: pointer;
        }

        .sidebar {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .players-list, .admin-panel {
            background: white;
            padding: 20px;
            border-radius: 10px;
        }

        .admin-panel {
            display: none;
            border: 2px solid #dc3545;
        }

        h3 {
            color: #333;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #f0f0f0;
        }

        .player-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px solid #f0f0f0;
        }

        .player-name {
            font-weight: bold;
        }

        .player-score {
            background: #667eea;
            color: white;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 12px;
        }

        /* Админ панель */
        .admin-controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 15px;
        }

        .control-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .control-group label {
            font-size: 12px;
            color: #666;
            font-weight: bold;
        }

        .control-group input {
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 12px;
        }

        .admin-buttons {
            display: flex;
            gap: 10px;
        }

        .admin-btn {
            flex: 1;
            padding: 8px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
        }

        .save-btn {
            background: #28a745;
            color: white;
        }

        .save-btn:hover {
            background: #218838;
        }

        .reset-btn {
            background: #ffc107;
            color: #000;
        }

        .reset-btn:hover {
            background: #e0a800;
        }

        /* Модальное окно */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            width: 90%;
            max-width: 400px;
        }

        .question-text {
            font-size: 18px;
            margin-bottom: 20px;
            color: #555;
        }

        .answer-input {
            width: 100%;
            padding: 12px;
            margin-bottom: 20px;
            border: 2px solid #ddd;
            border-radius: 8px;
        }

        .modal-buttons {
            display: flex;
            gap: 10px;
        }

        .submit-btn {
            flex: 1;
            padding: 12px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
        }

        .submit-btn:hover {
            background: #5a6fd8;
        }

        .cancel-btn {
            flex: 1;
            padding: 12px;
            background: #ddd;
            color: #333;
            border: none;
            border-radius: 8px;
            cursor: pointer;
        }

        .cancel-btn:hover {
            background: #ccc;
        }

        /* Уведомления */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 8px;
            color: white;
            font-weight: bold;
            z-index: 1001;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .notification.show {
            opacity: 1;
        }

        .success { background: #28a745; }
        .error { background: #dc3545; }
        .info { background: #17a2b8; }
    </style>
</head>
<body>
    <div class="container">
        <!-- Экран авторизации -->
        <div class="login-screen" id="loginScreen">
            <div class="login-form">
                <h1>🎮 Pixel Quiz 10x10</h1>
                <p>Мультиплеерная викторина</p>
                <input type="text" class="login-input" id="usernameInput" placeholder="Ваш логин" maxlength="15">
                <button class="login-btn" onclick="login()">Присоединиться к игре</button>
                <div style="margin-top: 20px; font-size: 14px; color: #666;">
                    <p>📢 Откройте эту страницу на нескольких устройствах для мультиплеера</p>
                </div>
            </div>
        </div>

        <!-- Игровой экран -->
        <div class="game-screen" id="gameScreen">
            <div class="header">
                <div class="user-info">
                    <div>
                        <span class="welcome">Игрок: <span id="userName">-</span></span>
                        <span class="user-role" id="userRole">игрок</span>
                    </div>
                    <div class="score">Очки: <span id="userScore">0</span></div>
                </div>
                <button class="logout-btn" onclick="logout()">Выйти</button>
            </div>

            <div class="game-content">
                <div class="game-board">
                    <h3>🎯 Игровое поле 10x10</h3>
                    <canvas id="gameCanvas" width="600" height="600"></canvas>
                    <div style="margin-top: 15px; color: #666;">
                        Кликайте на серые пиксели и отвечайте на вопросы!
                    </div>
                </div>
                
                <div class="sidebar">
                    <div class="players-list">
                        <h3>👥 Игроки онлайн (<span id="playersCount">0</span>)</h3>
                        <div id="playersList"></div>
                    </div>

                    <!-- Админ панель -->
                    <div class="admin-panel" id="adminPanel">
                        <h3>⚙️ Панель администратора</h3>
                        <div class="admin-controls">
                            <div class="control-group">
                                <label>Координата X (0-9):</label>
                                <input type="number" id="adminX" min="0" max="9" value="0">
                            </div>
                            <div class="control-group">
                                <label>Координата Y (0-9):</label>
                                <input type="number" id="adminY" min="0" max="9" value="0">
                            </div>
                            <div class="control-group">
                                <label>Цвет пикселя:</label>
                                <input type="color" id="adminColor" value="#ff6b6b">
                            </div>
                            <div class="control-group">
                                <label>Вопрос:</label>
                                <input type="text" id="adminQuestion" placeholder="Введите вопрос">
                            </div>
                            <div class="control-group">
                                <label>Правильный ответ:</label>
                                <input type="text" id="adminAnswer" placeholder="Правильный ответ">
                            </div>
                            <div class="control-group">
                                <label>Сбросить пиксель:</label>
                                <input type="checkbox" id="adminReset">
                            </div>
                        </div>
                        <div class="admin-buttons">
                            <button class="admin-btn save-btn" onclick="updatePixel()">Сохранить изменения</button>
                            <button class="admin-btn reset-btn" onclick="resetAllPixels()">Сбросить все пиксели</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Модальное окно вопроса -->
        <div class="modal" id="questionModal">
            <div class="modal-content">
                <h2>❓ Вопрос</h2>
                <div class="question-text" id="questionText">Загрузка вопроса...</div>
                <input type="text" class="answer-input" id="answerInput" placeholder="Ваш ответ...">
                <div class="modal-buttons">
                    <button class="cancel-btn" onclick="closeQuestion()">Отмена</button>
                    <button class="submit-btn" onclick="submitAnswer()">Ответить</button>
                </div>
            </div>
        </div>

        <!-- Уведомление -->
        <div class="notification" id="notification"></div>
    </div>

    <script>
        // ========== КОНФИГУРАЦИЯ ИГРЫ ==========
        const CONFIG = {
            fieldSize: 10, // 10x10 пикселей
            pixelSize: 60, // размер пикселя на canvas
            adminLogin: "admin123"
        };

        // ========== ГЛОБАЛЬНЫЕ ПЕРЕМЕННЫЕ ==========
        let currentUser = null;
        let isAdmin = false;
        let gameState = {
            pixels: [],
            players: {},
            selectedPixel: null
        };

        // ========== ВОПРОСЫ ДЛЯ ВИКТОРИНЫ ==========
        const QUESTIONS = [
            { q: "Столица Франции?", a: "Париж" },
            { q: "2 + 2 = ?", a: "4" },
            { q: "Сколько цветов у радуги?", a: "7" },
            { q: "Самая большая планета?", a: "Юпитер" },
            { q: "Столица Японии?", a: "Токио" },
            { q: "Сколько дней в високосном году?", a: "366" },
            { q: "Формула воды?", a: "H2O" },
            { q: "Сколько континентов?", a: "7" },
            { q: "Самое глубокое озеро?", a: "Байкал" },
            { q: "Сколько часов в сутках?", a: "24" },
            { q: "Первый элемент таблицы Менделеева?", a: "Водород" },
            { q: "Сколько сторон у квадрата?", a: "4" },
            { q: "Столица Италии?", a: "Рим" },
            { q: "Корень из 16?", a: "4" },
            { q: "Сколько лет в веке?", a: "100" },
            { q: "Самая длинная река?", a: "Нил" },
            { q: "Сколько планет в Солнечной системе?", a: "8" },
            { q: "Химический символ золота?", a: "Au" },
            { q: "Сколько минут в часе?", a: "60" },
            { q: "Столица Германии?", a: "Берлин" },
            { q: "Сколько дней в неделе?", a: "7" },
            { q: "Самый большой океан?", a: "Тихий" },
            { q: "Год полета Гагарина?", a: "1961" },
            { q: "Сколько букв в русском алфавите?", a: "33" },
            { q: "Столица России?", a: "Москва" },
            { q: "Сколько месяцев в году?", a: "12" },
            { q: "Химический символ кислорода?", a: "O" },
            { q: "Сколько дней в апреле?", a: "30" },
            { q: "Самый большой материк?", a: "Евразия" },
            { q: "Сколько лет в тысячелетии?", a: "1000" },
            { q: "Столица США?", a: "Вашингтон" },
            { q: "Сколько сантиметров в метре?", a: "100" },
            { q: "Химический символ железа?", a: "Fe" },
            { q: "Сколько дней в ноябре?", a: "30" },
            { q: "Самая высокая гора?", a: "Эверест" },
            { q: "Сколько секунд в минуте?", a: "60" },
            { q: "Столица Китая?", a: "Пекин" },
            { q: "Сколько грамм в килограмме?", a: "1000" },
            { q: "Химический символ углерода?", a: "C" },
            { q: "Сколько дней в феврале високосного года?", a: "29" },
            { q: "Самая большая пустыня?", a: "Сахара" },
            { q: "Сколько миллилитров в литре?", a: "1000" },
            { q: "Столица Канады?", a: "Оттава" },
            { q: "Сколько сторон у треугольника?", a: "3" },
            { q: "Химический символ азота?", a: "N" },
            { q: "Сколько дней в марте?", a: "31" },
            { q: "Самый большой остров?", a: "Гренландия" },
            { q: "Сколько минут в двух часах?", a: "120" },
            { q: "Столица Австралии?", a: "Канберра" },
            { q: "Сколько градусов в прямом угле?", a: "90" },
            { q: "Химический символ серебра?", a: "Ag" },
            { q: "Сколько дней в июле?", a: "31" },
            { q: "Самое соленое море?", a: "Мертвое" },
            { q: "Сколько сантиметров в дециметре?", a: "10" },
            { q: "Столица Бразилии?", a: "Бразилиа" },
            { q: "Сколько сторон у шестиугольника?", a: "6" },
            { q: "Химический символ меди?", a: "Cu" },
            { q: "Сколько дней в сентябре?", a: "30" },
            { q: "Самый большой лес?", a: "Тайга" },
            { q: "Сколько миллиметров в сантиметре?", a: "10" },
            { q: "Столица Индии?", a: "Дели" },
            { q: "Сколько градусов в окружности?", a: "360" },
            { q: "Химический символ ртути?", a: "Hg" },
            { q: "Сколько дней в октябре?", a: "31" },
            { q: "Самое холодное место на Земле?", a: "Антарктида" },
            { q: "Сколько килограммов в тонне?", a: "1000" },
            { q: "Столица Египта?", a: "Каир" },
            { q: "Сколько сторон у восьмиугольника?", a: "8" },
            { q: "Химический символ свинца?", a: "Pb" },
            { q: "Сколько дней в декабре?", a: "31" },
            { q: "Самый большой вулкан?", a: "Мауна-Лоа" },
            { q: "Сколько метров в километре?", a: "1000" },
            { q: "Столица Аргентины?", a: "Буэнос-Айрес" },
            { q: "Сколько градусов в развернутом угле?", a: "180" },
            { q: "Химический символ калия?", a: "K" },
            { q: "Сколько дней в январе?", a: "31" },
            { q: "Самый большой водопад?", a: "Анхель" },
            { q: "Столица Мексики?", a: "Мехико" },
            { q: "Сколько сторон у пятиугольника?", a: "5" },
            { q: "Химический символ натрия?", a: "Na" },
            { q: "Сколько дней в августе?", a: "31" },
            { q: "Самое глубокое море?", a: "Филиппинское" },
            { q: "Сколько миллиграммов в грамме?", a: "1000" },
            { q: "Столица Южной Кореи?", a: "Сеул" },
            { q: "Сколько градусов в острои угле?", a: "меньше 90" },
            { q: "Химический символ кальция?", a: "Ca" },
            { q: "Сколько дней в июне?", a: "30" },
            { q: "Самый большой полуостров?", a: "Аравийский" },
            { q: "Столица Турции?", a: "Анкара" },
            { q: "Сколько сторон у семиугольника?", a: "7" },
            { q: "Химический символ фосфора?", a: "P" },
            { q: "Сколько дней в мае?", a: "31" }
        ];

        // ========== ФУНКЦИИ УВЕДОМЛЕНИЙ ==========
        function showNotification(message, type = 'info') {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type} show`;
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // ========== СИСТЕМА СОХРАНЕНИЯ ==========
        function saveGameState() {
            localStorage.setItem('pixelQuizGameState', JSON.stringify(gameState));
        }

        function loadGameState() {
            const saved = localStorage.getItem('pixelQuizGameState');
            if (saved) {
                try {
                    const parsed = JSON.parse(saved);
                    gameState = parsed;
                    
                    // Очищаем старых игроков (тех, кто не онлайн)
                    const now = new Date();
                    Object.keys(gameState.players).forEach(username => {
                        const player = gameState.players[username];
                        // Удаляем игроков, которые не админы и не активны
                        if (player.username !== CONFIG.adminLogin) {
                            delete gameState.players[username];
                        }
                    });
                    
                    console.log('Состояние игры загружено, старые игроки очищены');
                } catch (e) {
                    console.log('Ошибка загрузки состояния, создаем новую игру');
                    initializeGame();
                }
            } else {
                console.log('Сохраненного состояния нет, создаем новую игру');
                initializeGame();
            }
        }

        function setupMultiplayer() {
            // Загружаем состояние при старте
            loadGameState();
            
            // Слушаем изменения в localStorage
            window.addEventListener('storage', function(e) {
                if (e.key === 'pixelQuizGameState') {
                    console.log('Обнаружено изменение состояния игры');
                    loadGameState();
                    if (currentUser) {
                        updateGameInterface();
                    }
                }
            });
            
            // Периодически сохраняем состояние
            setInterval(saveGameState, 1000);
        }

        // ========== АВТОРИЗАЦИЯ ==========
        function login() {
            const username = document.getElementById('usernameInput').value.trim();
            if (!username) {
                showNotification('Введите логин', 'error');
                return;
            }

            if (username.length < 2) {
                showNotification('Логин должен быть от 2 символов', 'error');
                return;
            }

            currentUser = username;
            isAdmin = (username === CONFIG.adminLogin);
            
            // Инициализируем игру при первом входе
            if (Object.keys(gameState.players).length === 0) {
                initializeGame();
            }

            // Регистрируем игрока (только если не админ)
            if (!gameState.players[username] && !isAdmin) {
                gameState.players[username] = {
                    id: username,
                    name: username,
                    score: 0,
                    isAdmin: false,
                    joinedAt: new Date()
                };
                
                showNotification(`Добро пожаловать, ${username}!`, 'success');
            } else if (isAdmin) {
                showNotification(`Администратор ${username} вошел в систему`, 'success');
            }

            // Сохраняем состояние
            saveGameState();

            // Показываем игровой экран
            document.getElementById('loginScreen').style.display = 'none';
            document.getElementById('gameScreen').style.display = 'block';
            document.getElementById('userName').textContent = username;

            // Показываем/скрываем админ панель
            if (isAdmin) {
                document.getElementById('adminPanel').style.display = 'block';
                document.getElementById('userRole').textContent = 'админ';
                document.getElementById('userRole').classList.add('admin');
            } else {
                document.getElementById('userRole').textContent = 'игрок';
                document.getElementById('userRole').classList.remove('admin');
            }

            // Обновляем интерфейс
            updateGameInterface();
        }

        function logout() {
            if (currentUser && gameState.players[currentUser]) {
                // Удаляем игрока из списка (кроме админа)
                if (!isAdmin) {
                    delete gameState.players[currentUser];
                }
                saveGameState();
            }
            
            currentUser = null;
            isAdmin = false;
            document.getElementById('gameScreen').style.display = 'none';
            document.getElementById('loginScreen').style.display = 'flex';
            document.getElementById('usernameInput').value = '';
            document.getElementById('adminPanel').style.display = 'none';
            document.getElementById('userRole').classList.remove('admin');
        }

        // ========== ИНИЦИАЛИЗАЦИЯ ИГРЫ ==========
        function initializeGame() {
            console.log('Инициализация новой игры');
            
            // Создаем пиксели 10x10
            gameState.pixels = [];
            for (let x = 0; x < CONFIG.fieldSize; x++) {
                for (let y = 0; y < CONFIG.fieldSize; y++) {
                    const questionIndex = (x * CONFIG.fieldSize + y) % QUESTIONS.length;
                    gameState.pixels.push({
                        id: `${x}_${y}`,
                        x, y,
                        defaultColor: getRandomColor(),
                        currentColor: "#CCCCCC",
                        question: QUESTIONS[questionIndex].q,
                        correctAnswer: QUESTIONS[questionIndex].a,
                        isAnswered: false,
                        answeredBy: null
                    });
                }
            }
            
            gameState.players = {};
            gameState.selectedPixel = null;
            
            saveGameState();
        }

        function getRandomColor() {
            const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4', '#FFEAA7', '#DDA0DD', '#98D8C8', '#F7DC6F', '#A29BFE', '#FD79A8'];
            return colors[Math.floor(Math.random() * colors.length)];
        }

        // ========== ОТРИСОВКА ИГРОВОГО ПОЛЯ ==========
        function drawGameBoard() {
            const canvas = document.getElementById('gameCanvas');
            const ctx = canvas.getContext('2d');
            const pixelSize = CONFIG.pixelSize;

            ctx.clearRect(0, 0, canvas.width, canvas.height);

            gameState.pixels.forEach(pixel => {
                // Рисуем пиксель
                ctx.fillStyle = pixel.currentColor;
                ctx.fillRect(pixel.x * pixelSize, pixel.y * pixelSize, pixelSize, pixelSize);
                
                // Рамка
                ctx.strokeStyle = pixel.isAnswered ? '#333' : '#999';
                ctx.lineWidth = pixel.isAnswered ? 2 : 1;
                ctx.strokeRect(pixel.x * pixelSize, pixel.y * pixelSize, pixelSize, pixelSize);

                // Если пиксель отвечен, показываем кто ответил
                if (pixel.isAnswered && pixel.answeredBy) {
                    ctx.fillStyle = '#000';
                    ctx.font = '10px Arial';
                    ctx.textAlign = 'center';
                    ctx.fillText(
                        pixel.answeredBy.substring(0, 3), 
                        pixel.x * pixelSize + pixelSize/2, 
                        pixel.y * pixelSize + pixelSize/2 + 3
                    );
                }

                // Для админа показываем координаты
                if (isAdmin) {
                    ctx.fillStyle = '#666';
                    ctx.font = '8px Arial';
                    ctx.textAlign = 'left';
                    ctx.fillText(
                        `${pixel.x},${pixel.y}`,
                        pixel.x * pixelSize + 2,
                        pixel.y * pixelSize + 10
                    );
                }
            });
        }

        // ========== ОБРАБОТКА КЛИКОВ ==========
        document.getElementById('gameCanvas').addEventListener('click', function(e) {
            if (!currentUser) return;

            const canvas = this;
            const rect = canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            const pixelSize = CONFIG.pixelSize;
            
            const pixelX = Math.floor(x / pixelSize);
            const pixelY = Math.floor(y / pixelSize);
            
            if (pixelX < CONFIG.fieldSize && pixelY < CONFIG.fieldSize) {
                const pixel = gameState.pixels.find(p => p.x === pixelX && p.y === pixelY);
                if (pixel) {
                    if (isAdmin) {
                        // Режим админа - заполняем форму
                        document.getElementById('adminX').value = pixelX;
                        document.getElementById('adminY').value = pixelY;
                        document.getElementById('adminColor').value = pixel.defaultColor;
                        document.getElementById('adminQuestion').value = pixel.question;
                        document.getElementById('adminAnswer').value = pixel.correctAnswer;
                        document.getElementById('adminReset').checked = false;
                    } else if (!pixel.isAnswered) {
                        // Режим игрока - показываем вопрос
                        gameState.selectedPixel = pixel;
                        document.getElementById('questionText').textContent = pixel.question;
                        document.getElementById('answerInput').value = '';
                        document.getElementById('questionModal').style.display = 'flex';
                        document.getElementById('answerInput').focus();
                    }
                }
            }
        });

        // ========== РАБОТА С ВОПРОСАМИ ==========
        function closeQuestion() {
            document.getElementById('questionModal').style.display = 'none';
            gameState.selectedPixel = null;
        }

        function submitAnswer() {
            const answer = document.getElementById('answerInput').value.trim();
            const pixel = gameState.selectedPixel;

            if (!answer) {
                showNotification('Введите ответ', 'error');
                return;
            }

            if (pixel && answer.toLowerCase() === pixel.correctAnswer.toLowerCase()) {
                // Правильный ответ
                pixel.currentColor = pixel.defaultColor;
                pixel.isAnswered = true;
                pixel.answeredBy = currentUser;
                
                // Обновляем счет игрока
                gameState.players[currentUser].score++;
                
                // Сохраняем состояние
                saveGameState();
                
                // Обновляем интерфейс
                updateGameInterface();
                
                showNotification('✅ Правильно! +1 очко', 'success');
            } else {
                showNotification('❌ Неправильно, попробуйте другой пиксель', 'error');
            }

            closeQuestion();
        }

        // ========== АДМИН ФУНКЦИИ ==========
        function updatePixel() {
            if (!isAdmin) return;

            const x = parseInt(document.getElementById('adminX').value);
            const y = parseInt(document.getElementById('adminY').value);
            const color = document.getElementById('adminColor').value;
            const question = document.getElementById('adminQuestion').value;
            const answer = document.getElementById('adminAnswer').value;
            const reset = document.getElementById('adminReset').checked;

            const pixel = gameState.pixels.find(p => p.x === x && p.y === y);
            if (pixel) {
                pixel.defaultColor = color;
                pixel.question = question;
                pixel.correctAnswer = answer;
                
                if (reset) {
                    pixel.isAnswered = false;
                    pixel.currentColor = "#CCCCCC";
                    pixel.answeredBy = null;
                }

                drawGameBoard();
                saveGameState();
                showNotification('✅ Пиксель обновлен', 'success');
            }
        }

        function resetAllPixels() {
            if (!isAdmin) return;

            if (!confirm('Вы уверены, что хотите сбросить ВСЕ пиксели?')) return;

            gameState.pixels.forEach(pixel => {
                pixel.isAnswered = false;
                pixel.currentColor = "#CCCCCC";
                pixel.answeredBy = null;
            });

            // Сбрасываем очки всех игроков
            Object.values(gameState.players).forEach(player => {
                player.score = 0;
            });

            drawGameBoard();
            updateGameInterface();
            saveGameState();
            showNotification('✅ Все пиксели сброшены', 'success');
        }

        // ========== ОБНОВЛЕНИЕ ИНТЕРФЕЙСА ==========
        function updateGameInterface() {
            drawGameBoard();
            updatePlayersList();
            
            // Обновляем счет текущего пользователя
            if (currentUser && gameState.players[currentUser]) {
                document.getElementById('userScore').textContent = gameState.players[currentUser].score;
            }
        }

        function updatePlayersList() {
            const playersList = document.getElementById('playersList');
            const playersCount = document.getElementById('playersCount');
            
            // Фильтруем игроков - убираем админа из списка
            const players = Object.values(gameState.players)
                .filter(player => !player.isAdmin)
                .sort((a, b) => b.score - a.score);

            playersCount.textContent = players.length;
            playersList.innerHTML = players.map(player => `
                <div class="player-item">
                    <span class="player-name">${player.name}</span>
                    <span class="player-score">${player.score}</span>
                </div>
            `).join('');
        }

        // ========== ИНИЦИАЛИЗАЦИЯ ==========
        document.getElementById('usernameInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') login();
        });

        document.getElementById('answerInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') submitAnswer();
        });

        // Запускаем при загрузке
        window.onload = function() {
            document.getElementById('usernameInput').focus();
            setupMultiplayer();
        };
    </script>
</body>
</html>
