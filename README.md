<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Камень, Ножницы, Бумага - Онлайн</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            color: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            margin-top: 20px;
        }
        
        h1 {
            text-align: center;
            margin-bottom: 20px;
            font-size: 2.5rem;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        .player {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            width: 48%;
            text-align: center;
            margin-bottom: 10px;
        }
        
        .player h2 {
            font-size: 1.5rem;
            margin-bottom: 10px;
        }
        
        .player-choice {
            font-size: 3rem;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .choices {
            display: flex;
            justify-content: space-around;
            margin: 30px 0;
            flex-wrap: wrap;
        }
        
        .choice {
            background-color: rgba(255, 255, 255, 0.1);
            border: none;
            border-radius: 50%;
            width: 100px;
            height: 100px;
            font-size: 3rem;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 10px;
        }
        
        .choice:hover {
            transform: scale(1.1);
            background-color: rgba(255, 255, 255, 0.2);
        }
        
        .choice:active {
            transform: scale(0.95);
        }
        
        .choice.selected {
            background-color: rgba(76, 175, 80, 0.3);
            box-shadow: 0 0 15px rgba(76, 175, 80, 0.5);
        }
        
        .choice:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .game-id-section {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin: 20px 0;
            text-align: center;
        }
        
        .game-id {
            font-size: 1.8rem;
            font-weight: bold;
            letter-spacing: 3px;
            margin: 10px 0;
            background-color: rgba(0, 0, 0, 0.3);
            padding: 10px;
            border-radius: 5px;
        }
        
        .buttons {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }
        
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 50px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
        }
        
        button:hover {
            background-color: #45a049;
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        button:disabled {
            background-color: #666;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .result {
            text-align: center;
            font-size: 2rem;
            margin: 20px 0;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .win {
            color: #4CAF50;
            text-shadow: 0 0 10px rgba(76, 175, 80, 0.5);
        }
        
        .lose {
            color: #f44336;
            text-shadow: 0 0 10px rgba(244, 67, 54, 0.5);
        }
        
        .draw {
            color: #FFC107;
            text-shadow: 0 0 10px rgba(255, 193, 7, 0.5);
        }
        
        .instructions {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            line-height: 1.6;
        }
        
        .instructions h3 {
            margin-bottom: 10px;
            text-align: center;
        }
        
        .instructions ol {
            padding-left: 20px;
        }
        
        .instructions li {
            margin-bottom: 8px;
        }
        
        .connection-status {
            text-align: center;
            margin: 10px 0;
            font-size: 1.1rem;
        }
        
        .connected {
            color: #4CAF50;
        }
        
        .waiting {
            color: #FFC107;
        }
        
        .disconnected {
            color: #f44336;
        }
        
        @media (max-width: 600px) {
            .player {
                width: 100%;
            }
            
            .choice {
                width: 80px;
                height: 80px;
                font-size: 2.5rem;
            }
            
            h1 {
                font-size: 2rem;
            }
        }
    </style>
    <!-- Подключаем Firebase -->
    <script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-database-compat.js"></script>
</head>
<body>
    <h1>✊ Камень, Ножницы, Бумага ✂️</h1>
    
    <div class="container">
        <div class="connection-status" id="connection-status">
            <span class="waiting">⏳ Ожидаем подключения...</span>
        </div>
        
        <div class="game-info">
            <div class="player">
                <h2>Игрок 1 (Вы)</h2>
                <div class="player-choice" id="player1-choice">❓</div>
            </div>
            <div class="player">
                <h2>Игрок 2</h2>
                <div class="player-choice" id="player2-choice">❓</div>
            </div>
        </div>
        
        <div class="game-id-section">
            <h3>ID игры для подключения:</h3>
            <div class="game-id" id="game-id">Загрузка...</div>
            <p>Отправьте этот ID вашему другу для подключения к игре</p>
        </div>
        
        <div class="choices">
            <button class="choice" data-choice="rock" id="rock-btn">✊</button>
            <button class="choice" data-choice="paper" id="paper-btn">✋</button>
            <button class="choice" data-choice="scissors" id="scissors-btn">✌️</button>
        </div>
        
        <div class="result" id="result">Выберите ваш ход</div>
        
        <div class="buttons">
            <button id="new-game">Новый раунд</button>
            <button id="copy-link">Копировать ссылку</button>
            <button id="leave-game">Покинуть игру</button>
        </div>
        
        <div class="instructions">
            <h3>Как играть:</h3>
            <ol>
                <li>Отправьте ID игры или ссылку другу</li>
                <li>Дождитесь подключения оппонента</li>
                <li>Сделайте свой выбор (камень, ножницы или бумага)</li>
                <li>После выбора обоих игроков будет показан результат</li>
                <li>Нажмите "Новый раунд" для следующей игры</li>
            </ol>
            <p style="text-align: center; margin-top: 10px;"><strong>Помните:</strong> Камень бьёт ножницы, ножницы бьют бумагу, бумага бьёт камень!</p>
        </div>
    </div>

    <script>
        // Конфигурация Firebase (замените на свою конфигурацию)
        const firebaseConfig = {
            apiKey: "AIzaSyEXAMPLE1234567890ABCDEFGHIJKLMNOP",
            authDomain: "rock-paper-scissors-online.firebaseapp.com",
            databaseURL: "https://rock-paper-scissors-online-default-rtdb.firebaseio.com",
            projectId: "rock-paper-scissors-online",
            storageBucket: "rock-paper-scissors-online.appspot.com",
            messagingSenderId: "1234567890",
            appId: "1:1234567890:web:abcdef1234567890"
        };

        // Инициализация Firebase
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        // Элементы DOM
        const player1ChoiceEl = document.getElementById('player1-choice');
        const player2ChoiceEl = document.getElementById('player2-choice');
        const resultEl = document.getElementById('result');
        const gameIdEl = document.getElementById('game-id');
        const newGameBtn = document.getElementById('new-game');
        const copyLinkBtn = document.getElementById('copy-link');
        const leaveGameBtn = document.getElementById('leave-game');
        const choiceBtns = document.querySelectorAll('.choice');
        const connectionStatusEl = document.getElementById('connection-status');
        
        // Состояние игры
        let gameState = {
            gameId: null,
            playerId: null,
            playerNumber: null,
            player1Choice: null,
            player2Choice: null,
            gameStarted: false,
            connected: false
        };
        
        // Генерация ID игры
        function generateGameId() {
            return 'RPS-' + Math.random().toString(36).substring(2, 6).toUpperCase();
        }
        
        // Генерация ID игрока
        function generatePlayerId() {
            return 'player-' + Math.random().toString(36).substring(2, 9);
        }
        
        // Инициализация игры
        function initGame() {
            // Проверка параметров URL для подключения к существующей игре
            const urlParams = new URLSearchParams(window.location.search);
            const gameIdFromUrl = urlParams.get('game');
            
            if (gameIdFromUrl) {
                // Подключение к существующей игре
                joinGame(gameIdFromUrl);
            } else {
                // Создание новой игры
                createNewGame();
            }
            
            updateGameDisplay();
        }
        
        // Создание новой игры
        function createNewGame() {
            gameState.gameId = generateGameId();
            gameState.playerId = generatePlayerId();
            gameState.playerNumber = 1;
            gameState.gameStarted = true;
            
            gameIdEl.textContent = gameState.gameId;
            
            // Создание игры в Firebase
            const gameRef = database.ref('games/' + gameState.gameId);
            gameRef.set({
                player1: {
                    id: gameState.playerId,
                    choice: null
                },
                player2: null,
                status: 'waiting'
            });
            
            // Слушаем изменения в игре
            gameRef.on('value', (snapshot) => {
                const gameData = snapshot.val();
                if (gameData) {
                    updateGameFromFirebase(gameData);
                }
            });
            
            connectionStatusEl.innerHTML = '<span class="waiting">⏳ Ожидаем подключения игрока 2...</span>';
        }
        
        // Подключение к существующей игре
        function joinGame(gameId) {
            gameState.gameId = gameId;
            gameState.playerId = generatePlayerId();
            gameState.playerNumber = 2;
            
            gameIdEl.textContent = gameState.gameId;
            
            // Подключение к игре в Firebase
            const gameRef = database.ref('games/' + gameState.gameId);
            gameRef.on('value', (snapshot) => {
                const gameData = snapshot.val();
                if (gameData) {
                    if (gameData.status === 'waiting') {
                        // Подключаемся как игрок 2
                        gameRef.update({
                            player2: {
                                id: gameState.playerId,
                                choice: null
                            },
                            status: 'playing'
                        });
                        gameState.gameStarted = true;
                        connectionStatusEl.innerHTML = '<span class="connected">✅ Подключено к игре</span>';
                    }
                    updateGameFromFirebase(gameData);
                } else {
                    connectionStatusEl.innerHTML = '<span class="disconnected">❌ Игра не найдена</span>';
                }
            });
        }
        
        // Обновление состояния игры из Firebase
        function updateGameFromFirebase(gameData) {
            if (gameData.player1) {
                gameState.player1Choice = gameData.player1.choice;
            }
            
            if (gameData.player2) {
                gameState.player2Choice = gameData.player2.choice;
                connectionStatusEl.innerHTML = '<span class="connected">✅ Оба игрока подключены</span>';
            } else {
                connectionStatusEl.innerHTML = '<span class="waiting">⏳ Ожидаем подключения игрока 2...</span>';
            }
            
            updateGameDisplay();
        }
        
        // Отправка выбора в Firebase
        function sendChoice(choice) {
            const gameRef = database.ref('games/' + gameState.gameId);
            const playerField = gameState.playerNumber === 1 ? 'player1' : 'player2';
            
            gameRef.child(playerField).update({
                choice: choice
            });
        }
        
        // Обновление отображения игры
        function updateGameDisplay() {
            // Отображение выбора игроков
            player1ChoiceEl.textContent = getChoiceSymbol(gameState.player1Choice);
            player2ChoiceEl.textContent = getChoiceSymbol(gameState.player2Choice);
            
            // Сброс выделения кнопок
            choiceBtns.forEach(btn => {
                btn.classList.remove('selected');
                if (btn.dataset.choice === getCurrentPlayerChoice()) {
                    btn.classList.add('selected');
                }
                
                // Блокируем кнопки после выбора
                btn.disabled = getCurrentPlayerChoice() !== null;
            });
            
            // Определение результата
            if (gameState.player1Choice && gameState.player2Choice) {
                const result = determineWinner(gameState.player1Choice, gameState.player2Choice);
                displayResult(result);
                newGameBtn.disabled = false;
            } else if (getCurrentPlayerChoice() && !getOpponentChoice()) {
                resultEl.textContent = "Ожидаем ход противника...";
                resultEl.className = "result";
                newGameBtn.disabled = true;
            } else {
                resultEl.textContent = "Выберите ваш ход";
                resultEl.className = "result";
                newGameBtn.disabled = true;
            }
        }
        
        // Получение выбора текущего игрока
        function getCurrentPlayerChoice() {
            return gameState.playerNumber === 1 ? gameState.player1Choice : gameState.player2Choice;
        }
        
        // Получение выбора противника
        function getOpponentChoice() {
            return gameState.playerNumber === 1 ? gameState.player2Choice : gameState.player1Choice;
        }
        
        // Получение символа для выбора
        function getChoiceSymbol(choice) {
            switch(choice) {
                case 'rock': return '✊';
                case 'paper': return '✋';
                case 'scissors': return '✌️';
                default: return '❓';
            }
        }
        
        // Определение победителя
        function determineWinner(choice1, choice2) {
            if (choice1 === choice2) return 'draw';
            
            if (
                (choice1 === 'rock' && choice2 === 'scissors') ||
                (choice1 === 'paper' && choice2 === 'rock') ||
                (choice1 === 'scissors' && choice2 === 'paper')
            ) {
                return gameState.playerNumber === 1 ? 'win' : 'lose';
            }
            
            return gameState.playerNumber === 1 ? 'lose' : 'win';
        }
        
        // Отображение результата
        function displayResult(result) {
            switch(result) {
                case 'win':
                    resultEl.textContent = "Вы выиграли! 🎉";
                    resultEl.className = "result win";
                    break;
                case 'lose':
                    resultEl.textContent = "Вы проиграли 😔";
                    resultEl.className = "result lose";
                    break;
                case 'draw':
                    resultEl.textContent = "Ничья! 🤝";
                    resultEl.className = "result draw";
                    break;
            }
        }
        
        // Обработчики событий
        choiceBtns.forEach(btn => {
            btn.addEventListener('click', () => {
                if (!gameState.gameStarted) {
                    alert("Игра еще не началась!");
                    return;
                }
                
                const choice = btn.dataset.choice;
                sendChoice(choice);
            });
        });
        
        newGameBtn.addEventListener('click', () => {
            // Сбрасываем выборы в Firebase
            const gameRef = database.ref('games/' + gameState.gameId);
            gameRef.update({
                player1: {
                    ...gameRef.player1,
                    choice: null
                },
                player2: {
                    ...gameRef.player2,
                    choice: null
                }
            });
        });
        
        copyLinkBtn.addEventListener('click', () => {
            const gameLink = `${window.location.origin}${window.location.pathname}?game=${gameState.gameId}`;
            
            // Копирование в буфер обмена
            navigator.clipboard.writeText(gameLink)
                .then(() => {
                    alert("Ссылка скопирована в буфер обмена!");
                })
                .catch(err => {
                    console.error('Ошибка копирования: ', err);
                    // Альтернативный способ для старых браузеров
                    const textArea = document.createElement("textarea");
                    textArea.value = gameLink;
                    document.body.appendChild(textArea);
                    textArea.select();
                    document.execCommand("copy");
                    document.body.removeChild(textArea);
                    alert("Ссылка скопирована в буфер обмена!");
                });
        });
        
        leaveGameBtn.addEventListener('click', () => {
            if (confirm("Вы уверены, что хотите покинуть игру?")) {
                // Очищаем данные игры если это создатель
                if (gameState.playerNumber === 1) {
                    const gameRef = database.ref('games/' + gameState.gameId);
                    gameRef.remove();
                }
                window.location.href = window.location.pathname;
            }
        });
        
        // Запуск игры при загрузке страницы
        window.addEventListener('load', initGame);
    </script>
</body>
</html>
