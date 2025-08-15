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
    .info-btn:nth-child(1) { background: linear-gradient(45deg, #555, #777); box-shadow: 0 0 15px rgba(200, 200, 200, 0.5);}
    .info-btn:nth-child(2) { background: linear-gradient(45deg, #ffd700, #ffcc00); color: black; box-shadow: 0 0 20px rgba(255, 215, 0, 0.8);}
    .info-btn:nth-child(3) { background: linear-gradient(45deg, #ff00ff, #cc00cc); box-shadow: 0 0 25px rgba(255, 0, 255, 0.9);}
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
    .info-block {
        background: rgba(255,255,255,0.05);
        padding: 20px;
        border-radius: 10px;
        margin: 15px;
        max-width: 600px;
        text-align: left;
        line-height: 1.5;
    }
</style>
</head>
<body>

<div id="username-display"></div>

<!-- Старт -->
<div id="start-screen" class="screen active">
    <button style="background:linear-gradient(45deg,#ff5f5f,#ff8f8f);color:white;box-shadow:0 0 20px rgba(255,80,80,0.8);" onclick="goToLanguage()">НАЧАТЬ</button>
</div>

<!-- Выбор языка -->
<div id="language-screen" class="screen">
    <h2>Выберите язык</h2>
    <button onclick="setLanguage('Русский')">Русский</button>
    <button onclick="setLanguage('English')">English</button>
</div>

<!-- Регистрация -->
<div id="register-screen" class="screen">
    <h2>Регистрация</h2>
    <input type="text" id="reg-username" placeholder="Username"><br>
    <input type="password" id="reg-password" placeholder="Пароль"><br>
    <button onclick="registerUser()">Зарегистрироваться</button>
    <p>Уже есть аккаунт? <a href="#" style="color:gold;" onclick="showScreen('login-screen')">Войти</a></p>
</div>

<!-- Вход -->
<div id="login-screen" class="screen">
    <h2>Вход</h2>
    <input type="text" id="login-username" placeholder="Username"><br>
    <input type="password" id="login-password" placeholder="Пароль"><br>
    <button onclick="loginUser()">Войти</button>
</div>

<!-- Выбор информации -->
<div id="info-screen" class="screen">
    <h2>Выберите информацию</h2>
    <button class="info-btn" onclick="getInfo(1)">📅 Дата выхода подарков</button>
    <button class="info-btn" onclick="getInfo(2)">📰 NFT каналы + боты</button>
    <button class="info-btn" onclick="getInfo(3)">👑 Автоскупка и скрипты</button>
</div>

<!-- Покупка подписки -->
<div id="buy-screen" class="screen">
    <h2>Нет подписки</h2>
    <p>Выберите срок подписки:</p>
    <div class="price-option" onclick="selectPlan('week')">Неделя — 500 ⭐</div>
    <div class="price-option" onclick="selectPlan('week-rub')">Неделя — 500 ₽</div>
    <div class="price-option" onclick="selectPlan('week-usd')">Неделя — $<span id="usd-price"></span></div>
</div>

<!-- Оплата -->
<div id="payment-screen" class="screen">
    <h2>Вы выбрали: <span id="plan-name"></span></h2>
    <p>Выберите способ оплаты:</p>
    <button onclick="pay('Кошелёк')">💰 Кошелёк</button>
    <button onclick="pay('Карта')">💳 Карта</button>
    <button onclick="pay('Звёзды')">⭐ Звёздами</button>
</div>

<!-- Успешная оплата -->
<div id="success-screen" class="screen">
    <h2>✅ Оплата прошла успешно</h2>
    <p>Подписка активирована.</p>
    <button onclick="goToInfo()">Продолжить</button>
</div>

<!-- Экран информации -->
<div id="data-screen" class="screen">
    <h2>Информация</h2>
    <div id="info-content" class="info-block"></div>
    <button onclick="goToInfo()">Назад</button>
</div>

<script>
let language = '';
let username = '';
let password = '';
let subscription = 0;
let selectedPlan = '';
let usdRate = 90;

function goToLanguage() { showScreen('language-screen'); }
function setLanguage(lang) {
    language = lang;
    if (localStorage.getItem('username')) {
        showScreen('login-screen');
    } else {
        showScreen('register-screen');
    }
}
function registerUser() {
    username = document.getElementById('reg-username').value.trim();
    password = document.getElementById('reg-password').value.trim();
    if (!username || !password) { alert('Введите username и пароль'); return; }
    localStorage.setItem('username', username);
    localStorage.setItem('password', password);
    document.getElementById('username-display').textContent = '@' + username;
    document.getElementById('username-display').style.display = 'block';
    showScreen('info-screen');
}
function loginUser() {
    let u = document.getElementById('login-username').value.trim();
    let p = document.getElementById('login-password').value.trim();
    if (u === localStorage.getItem('username') && p === localStorage.getItem('password')) {
        username = u;
        password = p;
        document.getElementById('username-display').textContent = '@' + username;
        document.getElementById('username-display').style.display = 'block';
        showScreen('info-screen');
    } else {
        alert('Неверный логин или пароль');
    }
}
function getInfo(level) {
    if (subscription < level) {
        showScreen('buy-screen');
        document.getElementById('usd-price').textContent = (500 / usdRate).toFixed(2);
    } else {
        let content = '';
        if (level === 1) {
            content = "<b>Дата выхода новых Telegram подарков:</b><br>Ожидается 20 августа 2025 года. Наши источники сообщают, что релиз приурочен к годовщине запуска Telegram Stars.";
        } else if (level === 2) {
            content = "<b>Лучшие NFT-каналы:</b><br>@nft_news — свежие релизы.<br>@crypto_trends — тренды и инвестиции.<br><br><b>Боты-оповещатели:</b><br>@gift_alert_bot — звонит при выходе подарков.";
        } else if (level === 3) {
            content = "<b>Боты для автоскупки:</b><br>@auto_gift_bot — молниеносная скупка редких подарков.<br><br><b>Скрипты:</b><br>gift-buy.js — браузерная автоматизация.<br>telegram-gift-sniper.py — покупка через API.";
        }
        document.getElementById('info-content').innerHTML = content;
        showScreen('data-screen');
    }
}
function selectPlan(plan) {
    selectedPlan = plan;
    let name = '';
    if (plan === 'week') name = 'Неделя — 500 ⭐';
    if (plan === 'week-rub') name = 'Неделя — 500 ₽';
    if (plan === 'week-usd') name = 'Неделя — $' + (500 / usdRate).toFixed(2);
    document.getElementById('plan-name').textContent = name;
    showScreen('payment-screen');
}
function pay(method) {
    console.log("Оплата через:", method, "План:", selectedPlan);
    subscription = 3;
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
