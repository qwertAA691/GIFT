<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Секретный информатор</title>
<style>
    body {
        margin: 0;
        font-family: Arial, sans-serif;
        background: #0a0a0a;
        color: white;
        text-align: center;
    }
    .screen {
        display: none;
        min-height: 100vh;
        padding-top: 50px;
    }
    .active {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    button {
        padding: 15px 40px;
        margin: 10px;
        font-size: 18px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        transition: 0.3s;
    }
    button:hover { transform: scale(1.05); }
    /* Цвета кнопок информации */
    .info-btn:nth-child(1) { background: linear-gradient(45deg, #555, #777); box-shadow: 0 0 15px rgba(200, 200, 200, 0.5);}
    .info-btn:nth-child(2) { background: linear-gradient(45deg, #ffd700, #ffcc00); color: black; box-shadow: 0 0 20px rgba(255, 215, 0, 0.8);}
    .info-btn:nth-child(3) { background: linear-gradient(45deg, #ff00ff, #cc00cc); box-shadow: 0 0 25px rgba(255, 0, 255, 0.9);}
    /* Username в углу */
    #username-display {
        position: fixed;
        top: 10px;
        right: 15px;
        background: rgba(255,255,255,0.1);
        padding: 5px 10px;
        border-radius: 6px;
        font-size: 14px;
        color: gold;
        display: none;
    }
    input {
        padding: 10px;
        font-size: 16px;
        border-radius: 6px;
        border: none;
        margin: 5px;
    }
    .price-option {
        background: linear-gradient(45deg, #333, #555);
        color: white;
        padding: 15px;
        border-radius: 8px;
        cursor: pointer;
        width: 260px;
        margin: 8px 0;
        transition: 0.3s;
    }
    .price-option:hover {
        transform: scale(1.05);
        background: linear-gradient(45deg, #444, #666);
    }
</style>
</head>
<body>

<div id="username-display"></div>

<!-- Экран 1: старт -->
<div id="start-screen" class="screen active">
    <button class="start-btn" style="background:linear-gradient(45deg,#ff5f5f,#ff8f8f);color:white;box-shadow:0 0 20px rgba(255,80,80,0.8);" onclick="goToLanguage()">НАЧАТЬ</button>
</div>

<!-- Экран 2: выбор языка -->
<div id="language-screen" class="screen">
    <h2>Выберите язык</h2>
    <button onclick="setLanguage('Русский')">Русский</button>
    <button onclick="setLanguage('English')">English</button>
</div>

<!-- Экран 3: регистрация -->
<div id="register-screen" class="screen">
    <h2>Регистрация</h2>
    <input type="text" id="reg-username" placeholder="Username"><br>
    <input type="password" id="reg-password" placeholder="Пароль"><br>
    <button onclick="registerUser()">Зарегистрироваться</button>
</div>

<!-- Экран 4: выбор информации -->
<div id="info-screen" class="screen">
    <h2>Выберите информацию</h2>
    <button class="info-btn" onclick="getInfo(1)">📅 Дата выхода подарков</button>
    <button class="info-btn" onclick="getInfo(2)">📰 NFT каналы + боты</button>
    <button class="info-btn" onclick="getInfo(3)">👑 Автоскупка и скрипты</button>
</div>

<!-- Экран 5: покупка подписки -->
<div id="buy-screen" class="screen">
    <h2>Нет подписки</h2>
    <p>Выберите срок подписки:</p>
    <div class="price-option" onclick="selectPlan('week')">Неделя — 500 ⭐</div>
    <div class="price-option" onclick="selectPlan('week-rub')">Неделя — 500 ₽</div>
    <div class="price-option" onclick="selectPlan('week-usd')">Неделя — $<span id="usd-price"></span></div>
</div>

<!-- Экран 6: оплата -->
<div id="payment-screen" class="screen">
    <h2>Вы выбрали: <span id="plan-name"></span></h2>
    <p>Выберите способ оплаты:</p>
    <button onclick="pay('Кошелёк')">💰 Кошелёк</button>
    <button onclick="pay('Карта')">💳 Карта</button>
    <button onclick="pay('Звёзды')">⭐ Звёздами</button>
</div>

<!-- Экран 7: успешная оплата -->
<div id="success-screen" class="screen">
    <h2>✅ Оплата прошла успешно</h2>
    <p>Подписка активирована.</p>
    <button onclick="goToInfo()">Продолжить</button>
</div>

<script>
let language = '';
let username = '';
let password = '';
let subscription = 0;
let selectedPlan = '';

function goToLanguage() { showScreen('language-screen'); }
function setLanguage(lang) {
    language = lang;
    showScreen('register-screen');
}
function registerUser() {
    username = document.getElementById('reg-username').value.trim();
    password = document.getElementById('reg-password').value.trim();
    if (!username || !password) {
        alert('Введите username и пароль');
        return;
    }
    document.getElementById('username-display').textContent = '@' + username;
    document.getElementById('username-display').style.display = 'block';
    showScreen('info-screen');
}
function getInfo(level) {
    if (subscription < level) {
        showScreen('buy-screen');
        let usdPrice = (500 / 90).toFixed(2);
        document.getElementById('usd-price').textContent = usdPrice;
    } else {
        alert("Секретная информация уровня " + level);
    }
}
function selectPlan(plan) {
    selectedPlan = plan;
    let name = '';
    if (plan === 'week') name = 'Неделя — 500 ⭐';
    if (plan === 'week-rub') name = 'Неделя — 500 ₽';
    if (plan === 'week-usd') name = 'Неделя — $' + (500 / 90).toFixed(2);
    document.getElementById('plan-name').textContent = name;
    showScreen('payment-screen');
}
function pay(method) {
    console.log("Оплата через:", method, "План:", selectedPlan);
    subscription = 3; // Активируем максимальную подписку
    showScreen('success-screen');
}
function goToInfo() { showScreen('info-screen'); }
function showScreen(id) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(id).classList.add('active');
}
</script>

</body>
</html>
