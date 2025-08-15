[[<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Секретный информатор</title>
<style>
  :root{
    --bg:#070707; --panel:#0f0f12; --muted:#8a8a8a; --txt:#e8e8ea;
    --accent:#ff6b6b; --gold:#ffd54a; --violet:#c86bff; --ok:#21d07a; --warn:#ffb74d;
    --card:#13131a; --br:14px; --shadow:0 12px 40px rgba(0,0,0,.55);
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0; font-family:Inter,system-ui,Arial,sans-serif; color:var(--txt);
    background:
      radial-gradient(1200px 600px at 120% -10%, rgba(100,0,255,.08), transparent 60%),
      radial-gradient(800px 400px at -10% 110%, rgba(255,200,0,.06), transparent 60%),
      linear-gradient(#060608,#09090c 30%, #0a0a0d);
  }
  .center-screen{
    min-height:100dvh; display:none; align-items:center; justify-content:center; flex-direction:column; gap:22px; padding:28px;
  }
  .active{display:flex}
  /* header badge */
  .user-badge{
    position:fixed; top:10px; right:12px; display:none;
    padding:6px 10px; border-radius:999px; background:rgba(255,255,255,.06);
    border:1px solid rgba(255,255,255,.08); color:var(--gold); font-size:13px; letter-spacing:.2px;
    backdrop-filter: blur(6px);
  }
  /* common UI */
  .card{
    width:min(820px,94vw); background:var(--card); border:1px solid rgba(255,255,255,.06);
    border-radius:var(--br); box-shadow:var(--shadow); padding:22px;
  }
  h1,h2{margin:.2em 0 .4em 0}
  p.muted{color:var(--muted); margin:.2em 0 1em}
  .btn{
    border:none; cursor:pointer; padding:16px 28px; border-radius:12px; font-weight:600; letter-spacing:.2px;
    background:#1b1b22; color:var(--txt); transition:transform .12s ease, box-shadow .2s ease, background .2s;
    box-shadow:0 10px 24px rgba(0,0,0,.4), inset 0 1px 0 rgba(255,255,255,.05);
  }
  .btn:hover{transform:translateY(-1px)}
  .btn:active{transform:translateY(0)}
  .btn-accent{background:linear-gradient(135deg, #ff4d6d, #d43bff); box-shadow:0 15px 40px rgba(212,59,255,.35)}
  .btn-gold{background:linear-gradient(135deg, #ffe082, #ffc107); color:#2b2100; box-shadow:0 15px 40px rgba(255,208,0,.35)}
  .btn-ghost{background:#14141a}
  .stack{display:flex; gap:12px; flex-wrap:wrap}
  .input{
    width:100%; padding:14px 14px; border-radius:10px; background:#121219; border:1px solid rgba(255,255,255,.08); color:var(--txt);
  }
  .row{display:flex; align-items:center; justify-content:space-between; gap:12px}
  .pill{
    font-size:12px; padding:6px 10px; border-radius:999px; border:1px solid rgba(255,255,255,.08); background:#141421; color:#c8c8d0;
  }
  .pill.req-none{color:#a8ffac; border-color:rgba(33,208,122,.4)}
  .pill.req-1{color:#ffd54a; border-color:rgba(255,213,74,.45)}
  .pill.req-2{color:#d7a4ff; border-color:rgba(200,107,255,.45)}
  .list{display:grid; gap:14px; margin-top:10px}
  .tile{
    display:flex; align-items:center; justify-content:space-between; gap:12px;
    padding:16px; border-radius:12px; border:1px solid rgba(255,255,255,.06); background:#111119;
    transition:transform .12s ease, box-shadow .2s ease;
  }
  .tile:hover{transform:translateY(-1px); box-shadow:0 10px 28px rgba(0,0,0,.35)}
  .tile .title{font-weight:700}
  .hint{font-size:13px; color:var(--muted)}
  .divider{height:1px; background:rgba(255,255,255,.06); margin:14px 0}
  .notice{background:#12121b; border:1px dashed rgba(255,255,255,.18); padding:12px; border-radius:12px; color:#cbd5ff}

  /* colored “value by price” styles */
  .tier-free{box-shadow: 0 0 0 1px rgba(150,150,150,.25) inset}
  .tier-1{background:linear-gradient(145deg, rgba(255,224,130,.12), rgba(255,193,7,.06))}
  .tier-2{background:linear-gradient(145deg, rgba(200,107,255,.16), rgba(100,50,180,.08))}

  /* modal-ish purchase panel */
  .sheet{width:min(820px,94vw); background:var(--panel); border:1px solid rgba(255,255,255,.08); border-radius:16px; box-shadow:var(--shadow); padding:18px}
  .grid2{display:grid; grid-template-columns:1fr 1fr; gap:12px}
  @media (max-width:720px){ .grid2{grid-template-columns:1fr} }
</style>
</head>
<body>

<!-- floating username -->
<div id="userBadge" class="user-badge"></div>

<!-- START -->
<section id="start" class="center-screen active">
  <div class="card" style="text-align:center; padding:36px 26px;">
    <h1 style="letter-spacing:.6px;">🔒 Секретный информатор</h1>
    <p class="muted">Тёмный доступ к инсайдам: релизы подарков, NFT-каналы, боты и премиум-инструменты.</p>
    <button class="btn btn-accent" onclick="go('lang')">НАЧАТЬ</button>
  </div>
</section>

<!-- LANGUAGE -->
<section id="lang" class="center-screen">
  <div class="card">
    <h2>Выберите язык</h2>
    <div class="stack">
      <button class="btn btn-ghost" onclick="setLang('ru')">Русский</button>
      <button class="btn btn-ghost" onclick="setLang('en')">English</button>
    </div>
  </div>
</section>

<!-- AUTH -->
<section id="auth" class="center-screen">
  <div class="card">
    <h2>Вход или регистрация</h2>
    <div class="stack" style="margin-bottom:8px">
      <button class="btn btn-ghost" onclick="switchAuth('login')">Войти</button>
      <button class="btn btn-ghost" onclick="switchAuth('register')">Зарегистрироваться</button>
      <button class="btn btn-ghost" onclick="switchAuth('key')">Войти по ключу</button>
    </div>
    <div id="authLogin" style="display:none">
      <input id="loginUser" class="input" placeholder="Username"/>
      <input id="loginPass" class="input" type="password" placeholder="Пароль"/>
      <div class="stack">
        <button class="btn btn-accent" onclick="login()">Войти</button>
      </div>
    </div>
    <div id="authRegister" style="display:none">
      <input id="regUser" class="input" placeholder="Username"/>
      <input id="regPass" class="input" type="password" placeholder="Пароль"/>
      <div class="stack">
        <button class="btn btn-accent" onclick="register()">Зарегистрироваться</button>
      </div>
    </div>
    <div id="authKey" style="display:none">
      <input id="keyInput" class="input" placeholder="Вставьте секретный ключ"/>
      <div class="stack">
        <button class="btn btn-accent" onclick="loginByKey()">Войти по ключу</button>
      </div>
      <p class="hint">Ключ можно скопировать из «Профиля» на другом устройстве.</p>
    </div>
  </div>
</section>

<!-- PROFILE (shows key & subscription) -->
<section id="profile" class="center-screen">
  <div class="card">
    <h2>Профиль</h2>
    <div class="row">
      <div>
        <div class="title" id="profileName">@user</div>
        <div class="hint">Подписка: <span id="profileSub">Нет</span></div>
      </div>
      <button class="btn" onclick="go('info')">К функциям →</button>
    </div>
    <div class="divider"></div>
    <div class="notice">
      <b>Секретный ключ (для входа с другого устройства):</b>
      <div class="row" style="gap:8px; margin-top:8px">
        <input id="profileKey" class="input" readonly/>
        <button class="btn" onclick="copyKey()">Скопировать</button>
      </div>
      <div class="hint">Храните ключ в надёжном месте. Кто владеет ключом — владеет доступом.</div>
    </div>
    <div class="divider"></div>
    <div class="stack">
      <button class="btn" onclick="logout()">Выйти</button>
      <button class="btn" onclick="clearDevice()">Очистить устройство</button>
    </div>
  </div>
</section>

<!-- INFO SELECTOR -->
<section id="info" class="center-screen">
  <div class="card">
    <h2>Доступные функции</h2>
    <p class="hint">Наведите курсор, чтобы увидеть краткое описание. Нужная подписка указана справа.</p>
    <div class="list">
      <div class="tile tier-free" onclick="openInfo(1)" title="Слухи и сводки по датам, сигналы от инсайдеров, ориентиры по времени.">
        <div class="title">📅 Дата выхода Telegram-подарков</div>
        <div class="pill req-none">Доступ: Без подписки</div>
      </div>
      <div class="tile tier-1" onclick="openInfo(2)" title="Список отобранных NFT-каналов, приватные фиды, боты, которые звонят при релизе.">
        <div class="title">📰 NFT-каналы + звонящие боты</div>
        <div class="pill req-1">Нужна подписка: Первый класс</div>
      </div>
      <div class="tile tier-2" onclick="openInfo(3)" title="Автоскупка через ботов, быстрые связки, анти-лаг скрипты и рекомендации по инфраструктуре.">
        <div class="title">🤖 Автоскупка + скрипты</div>
        <div class="pill req-2">Нужна подписка: Второй класс</div>
      </div>
      <div class="tile tier-2" onclick="openInfo(4)" title="Эксклюзивные премиум-функции: мгновенные алерты, приоритетные каналы, бета-фичи.">
        <div class="title">👑 Премиум-функции</div>
        <div class="pill req-2">Нужна подписка: Второй класс</div>
      </div>
    </div>
    <div class="divider"></div>
    <div class="row">
      <div class="hint">Нет нужной подписки? Оформите вечный доступ раз и навсегда.</div>
      <button class="btn btn-gold" onclick="go('pay')">Купить подписку</button>
    </div>
  </div>
</section>

<!-- PURCHASE -->
<section id="pay" class="center-screen">
  <div class="sheet">
    <h2>Вечные подписки</h2>
    <div class="grid2">
      <div class="card" style="background:linear-gradient(145deg, rgba(255,224,130,.12), rgba(255,193,7,.06));">
        <h3>Первый класс</h3>
        <p class="hint">Открывает «NFT-каналы + звонящие боты» (уровень 2).</p>
        <div class="divider"></div>
        <div class="row">
          <div class="title">Цена:</div>
          <div class="pill">2000 ⭐ / 3000 ₽ / $35</div>
        </div>
        <div class="divider"></div>
        <div class="stack">
          <button class="btn" onclick="choosePlan(1,'stars')">Оплатить ⭐</button>
          <button class="btn" onclick="choosePlan(1,'rub')">Оплатить ₽</button>
          <button class="btn" onclick="choosePlan(1,'usd')">Оплатить $</button>
        </div>
      </div>
      <div class="card" style="background:linear-gradient(145deg, rgba(200,107,255,.16), rgba(100,50,180,.08));">
        <h3>Второй класс</h3>
        <p class="hint">Открывает «Автоскупка + скрипты» и «Премиум-функции» (уровни 3–4).</p>
        <div class="divider"></div>
        <div class="row">
          <div class="title">Цена:</div>
          <div class="pill">10000 ⭐ / 16000 ₽ / $165</div>
        </div>
        <div class="divider"></div>
        <div class="stack">
          <button class="btn" onclick="choosePlan(2,'stars')">Оплатить ⭐</button>
          <button class="btn" onclick="choosePlan(2,'rub')">Оплатить ₽</button>
          <button class="btn" onclick="choosePlan(2,'usd')">Оплатить $</button>
        </div>
      </div>
    </div>
    <div class="divider"></div>
    <div id="payConfirm" style="display:none">
      <div class="row">
        <div class="hint">Выбрано:</div>
        <div class="pill" id="paySummary"></div>
      </div>
      <div class="stack" style="margin-top:8px">
        <button class="btn btn-accent" onclick="doPay()">Оплатить</button>
        <button class="btn" onclick="cancelPay()">Отмена</button>
      </div>
    </div>
    <div class="row" style="margin-top:10px">
      <button class="btn" onclick="go('info')">← Назад к функциям</button>
    </div>
  </div>
</section>

<!-- CONTENT -->
<section id="content" class="center-screen">
  <div class="card">
    <div class="row">
      <h2 id="contentTitle">Информация</h2>
      <button class="btn" onclick="go('info')">← К функциям</button>
    </div>
    <div class="divider"></div>
    <div id="contentBody" style="text-align:left; line-height:1.6"></div>
  </div>
</section>

<script>
  // --- Simple "cloudless" account storage with portable KEY ---
  // Demo approach: we encode username + password + subscription into a base64 key.
  // On another device, user can "Login by Key" to restore the account locally.

  const screens = ['start','lang','auth','profile','info','pay','content'];
  const $ = sel => document.querySelector(sel);

  let state = {
    lang:'ru',
    user:null, // {username, pass, level}
    pendingPay:null // {planLevel, currency}
  };

  // Navigation
  function go(id){
    screens.forEach(s=>$('#'+s).classList.remove('active'));
    $('#'+id).classList.add('active');
  }

  function setLang(l){ state.lang=l; go('auth'); switchAuth('login'); }

  function switchAuth(which){
    $('#authLogin').style.display = which==='login' ? 'block':'none';
    $('#authRegister').style.display = which==='register' ? 'block':'none';
    $('#authKey').style.display = which==='key' ? 'block':'none';
  }

  // Utils
  const b64 = {
    enc: s => btoa(unescape(encodeURIComponent(s))),
    dec: s => decodeURIComponent(escape(atob(s)))
  };

  function buildKey(u){
    // VERY simple key (demo), includes subscription level
    const payload = JSON.stringify({u:u.username, p:u.pass, l:u.level||0});
    return b64.enc(payload);
  }

  function parseKey(k){
    try{
      const o = JSON.parse(b64.dec(k));
      if(typeof o.u==='string' && typeof o.p==='string' && typeof o.l!=='undefined'){
        return {username:o.u, pass:o.p, level:o.l|0};
      }
    }catch(e){}
    return null;
  }

  function saveLocal(u){
    localStorage.setItem('si_user', JSON.stringify(u));
  }
  function loadLocal(){
    const raw = localStorage.getItem('si_user');
    if(!raw) return null;
    try{ return JSON.parse(raw); }catch(e){ return null }
  }
  function setBadge(u){
    if(u){ $('#userBadge').style.display='inline-flex'; $('#userBadge').textContent='@'+u.username; }
    else { $('#userBadge').style.display='none'; }
  }
  function refreshProfile(){
    const u = state.user;
    $('#profileName').textContent='@'+u.username;
    $('#profileSub').textContent = subName(u.level);
    $('#profileKey').value = buildKey(u);
  }
  function subName(lvl){
    if(lvl>=2) return 'Второй класс (вечная)';
    if(lvl>=1) return 'Первый класс (вечная)';
    return 'Нет';
  }

  // Auth
  function register(){
    const username = $('#regUser').value.trim();
    const pass = $('#regPass').value.trim();
    if(!username || !pass){ alert('Введите username и пароль'); return; }
    state.user = {username, pass, level:0};
    saveLocal(state.user);
    setBadge(state.user);
    refreshProfile();
    go('profile');
  }
  function login(){
    const username = $('#loginUser').value.trim();
    const pass = $('#loginPass').value.trim();
    const saved = loadLocal();
    if(saved && saved.username===username && saved.pass===pass){
      state.user = saved;
      setBadge(state.user);
      refreshProfile();
      go('profile');
    } else {
      alert('Неверный логин/пароль или на этом устройстве нет сохранённого аккаунта.\nЕсли у вас есть секретный ключ — войдите по ключу.');
    }
  }
  function loginByKey(){
    const key = $('#keyInput').value.trim();
    const obj = parseKey(key);
    if(!obj){ alert('Неверный ключ'); return; }
    state.user = {username:obj.username, pass:obj.pass, level:obj.level|0};
    saveLocal(state.user);
    setBadge(state.user);
    refreshProfile();
    go('profile');
  }
  function logout(){
    state.user = null;
    setBadge(null);
    alert('Вы вышли из аккаунта на этом устройстве.');
    go('auth'); switchAuth('login');
  }
  function clearDevice(){
    localStorage.removeItem('si_user');
    alert('Данные на этом устройстве очищены.');
  }
  function copyKey(){
    $('#profileKey').select();
    document.execCommand('copy');
    const b = $('#profileKey');
    b.blur();
    alert('Ключ скопирован. Используйте «Войти по ключу» на другом устройстве.');
  }

  // Info access logic
  function openInfo(level){
    if(!state.user){ alert('Сначала войдите.'); go('auth'); return; }
    const have = state.user.level||0;
    if(level===1){ showContent(level); return; }
    if(have===0){
      alert('Подписка отсутствует. Оформите вечный доступ.');
      go('pay'); return;
    }
    if(level===2 && have>=1){ showContent(level); return; }
    if((level===3 || level===4) && have>=2){ showContent(level); return; }
    // not enough
    alert('Недостаточный уровень подписки. Требуется: ' + (level===2?'Первый класс':'Второй класс'));
    go('pay');
  }

  function showContent(level){
    const titleEl = $('#contentTitle');
    const bodyEl = $('#contentBody');

    if(level===1){
      titleEl.textContent = '📅 Дата выхода Telegram-подарков';
      bodyEl.innerHTML = `
        <div class="notice">Кратко: ориентировочное окно запуска следующей волны подарков — <b>20 августа 2025</b> ± 48 часов.</div>
        <h3>Что это значит</h3>
        <ul>
          <li>Пик активности маркетплейса — ~30 минут после старта.</li>
          <li>Резервируйте баланс заранее (звёзды/рубли/карта).</li>
          <li>Проверьте связки: устройство → сеть → прокси (если требуется).</li>
        </ul>
        <h3>Сигналы к старту</h3>
        <ul>
          <li>Изменения в API каталога подарков (поведение эндпоинтов).</li>
          <li>Метаданные в новых превью иконок.</li>
          <li>Аномальные всплески в логах мониторинга.</li>
        </ul>`;
    }
    if(level===2){
      titleEl.textContent = '📰 NFT-каналы + звонящие боты';
      bodyEl.innerHTML = `
        <div class="notice">Отбор по шуму/сигналу и латентности публикаций.</div>
        <h3>Каналы наблюдения</h3>
        <ul>
          <li><b>@nft_news</b> — оперативные анонсы, низкий спам.</li>
          <li><b>@crypto_trends</b> — макро-сводки и редкие коллекции.</li>
          <li><b>@rare_drop_watch</b> — фильтрованные дроп-снапшоты.</li>
        </ul>
        <h3>Звонящие боты</h3>
        <ul>
          <li><b>@gift_alert_bot</b> — триггер по ключевым событиям → звонок.</li>
          <li><b>@drop_call_me_bot</b> — резервный канал оповещений.</li>
        </ul>
        <h3>Настройка</h3>
        <ul>
          <li>Разделите алерты: основной номер / резервный номер.</li>
          <li>Задайте <i>quiet hours</i>, чтобы не терять звонок ночью.</li>
          <li>Склейте с календарём релизов (ical) для напоминаний.</li>
        </ul>`;
    }
    if(level===3){
      titleEl.textContent = '🤖 Автоскупка + скрипты';
      bodyEl.innerHTML = `
        <div class="notice">Сценарии на скорость: минимизация кликов, пред-подписи, обход лагов UI.</div>
        <h3>Боты автоскупки</h3>
        <ul>
          <li><b>@auto_gift_bot</b> — очередь заявок, приоритет «горячие» позиции.</li>
          <li><b>@fast_sweep_bot</b> — агрессивный режим, осторожно с лимитами.</li>
        </ul>
        <h3>Скрипты</h3>
        <ul>
          <li><code>gift-buy.js</code> — скрипт браузера (эмуляция кликов, prefetch, retry-логика).</li>
          <li><code>telegram-gift-sniper.py</code> — работа через API (тайм-слоты, backoff).</li>
        </ul>
        <h3>Практика</h3>
        <ul>
          <li>Грейдируйте прокси: выделенные под «горячие» покупки.</li>
          <li>Разнесите платежи по источникам (кошелёк/карта/звёзды).</li>
          <li>Ставьте лимиты расходов и логирование действий.</li>
        </ul>`;
    }
    if(level===4){
      titleEl.textContent = '👑 Премиум-функции (Второй класс)';
      bodyEl.innerHTML = `
        <div class="notice">Доступно только со «Вторым классом». Набор расширяется.</div>
        <h3>Что доступно</h3>
        <ul>
          <li><b>Мгновенные алерты</b> (push + звонок + e-mail) с приоритетом.</li>
          <li><b>Приоритетные каналы</b> с приватными анонсами.</li>
          <li><b>Бета-фичи</b>: предварительные превью карточек, автозаполнение.</li>
        </ul>
        <h3>Рекомендации</h3>
        <ul>
          <li>Создайте двойной контур оповещений (основной/резерв).</li>
          <li>Храните бэкапы ключей и токенов в менеджере паролей.</li>
        </ul>`;
    }
    go('content');
  }

  // Purchase flow
  function choosePlan(level, currency){
    state.pendingPay = {planLevel:level, currency};
    const text = (level===2?'Второй класс':'Первый класс') + ' • ' + (currency==='stars'?'⭐ Звёзды':currency==='rub'?'₽ Рубли':'$ Доллары');
    $('#paySummary').textContent = text;
    $('#payConfirm').style.display='block';
  }
  function cancelPay(){ state.pendingPay=null; $('#payConfirm').style.display='none'; }
  function doPay(){
    if(!state.user){ alert('Сначала войдите или зарегистрируйтесь.'); go('auth'); return; }
    if(!state.pendingPay){ alert('Выберите подписку и валюту.'); return; }
    const lvl = state.pendingPay.planLevel; // 1 or 2
    // Activate subscription
    state.user.level = Math.max(state.user.level||0, lvl);
    saveLocal(state.user);
    refreshProfile();
    $('#payConfirm').style.display='none';
    alert('Оплата успешна. Активирована подписка: ' + subName(state.user.level));
    go('info');
  }

  // Auto boot: if account exists locally, go straight to profile
  (function boot(){
    const saved = loadLocal();
    if(saved){ state.user=saved; setBadge(saved); refreshProfile(); }
  })();
</script>
</body>
</html>
]
