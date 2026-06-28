<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>2026 同學會選餐廳投票</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif; background: #f5f5f0; color: #1a1a18; min-height: 100vh; }
.page { max-width: 480px; margin: 0 auto; background: #fff; min-height: 100vh; }

.header { padding: 1.5rem 1rem 1rem; text-align: center; border-bottom: 1px solid #e8e6e0; background: #fff; }
.header h1 { font-size: 20px; font-weight: 600; color: #1a1a18; }
.header p { font-size: 13px; color: #888; margin-top: 4px; }
.vote-badge { display: inline-block; background: #e8f0fe; color: #1a56db; font-size: 12px; padding: 3px 12px; border-radius: 20px; margin-top: 8px; font-weight: 500; }

.restaurants { padding: 1rem; display: flex; flex-direction: column; gap: 10px; }
.card { background: #fff; border: 1px solid #e8e6e0; border-radius: 12px; overflow: hidden; cursor: pointer; transition: border-color 0.15s; }
.card:hover { border-color: #b0b0a8; }
.card.selected { border: 2px solid #1a56db; }
.card-header { display: flex; align-items: flex-start; gap: 10px; padding: 12px 12px 8px; }
.card-emoji { font-size: 22px; flex-shrink: 0; width: 40px; height: 40px; display: flex; align-items: center; justify-content: center; background: #f5f5f0; border-radius: 8px; }
.card-title-block { flex: 1; }
.card-name { font-size: 15px; font-weight: 600; color: #1a1a18; }
.card-type { font-size: 12px; color: #888; margin-top: 2px; }
.card-rating { display: flex; align-items: center; gap: 4px; margin-top: 4px; }
.stars { color: #f5a623; font-size: 12px; }
.rating-num { font-size: 12px; color: #888; }
.check { width: 24px; height: 24px; border-radius: 50%; border: 1.5px solid #ccc; display: flex; align-items: center; justify-content: center; flex-shrink: 0; transition: all 0.15s; }
.card.selected .check { background: #1a56db; border-color: #1a56db; }
.check-inner { display: none; }
.card.selected .check-inner { display: block; width: 10px; height: 6px; border-left: 2px solid white; border-bottom: 2px solid white; transform: rotate(-45deg) translate(1px, -1px); }

.toggle-btn { display: flex; align-items: center; justify-content: center; gap: 4px; font-size: 12px; color: #1a56db; padding: 6px 12px; cursor: pointer; background: none; border: none; width: 100%; border-top: 1px solid #f0efe8; }
.toggle-btn svg { transition: transform 0.2s; }
.card.open .toggle-btn svg { transform: rotate(180deg); }

.card-details { display: none; flex-direction: column; gap: 8px; padding: 10px 12px 12px; border-top: 1px solid #f0efe8; background: #fafaf8; }
.card.open .card-details { display: flex; }
.detail-row { display: flex; align-items: flex-start; gap: 8px; font-size: 13px; }
.detail-icon { color: #aaa; flex-shrink: 0; margin-top: 1px; font-size: 15px; }
.detail-text { color: #555; line-height: 1.5; }
.detail-label { color: #aaa; font-size: 11px; margin-right: 4px; display: inline; }

.footer { padding: 1rem; border-top: 1px solid #e8e6e0; position: sticky; bottom: 0; background: #fff; }
.counter { text-align: center; font-size: 13px; color: #888; margin-bottom: 10px; }
.counter span { font-weight: 600; color: #1a1a18; }
.submit-btn { width: 100%; padding: 13px; border-radius: 10px; border: none; background: #1a56db; color: #fff; font-size: 16px; font-weight: 600; cursor: pointer; transition: opacity 0.15s; }
.submit-btn:disabled { opacity: 0.35; cursor: not-allowed; }
.submit-btn:not(:disabled):hover { background: #1446b8; }

.result-view { display: none; padding: 1.5rem 1rem; }
.result-view.show { display: block; }
.voted-msg { text-align: center; padding-bottom: 1.5rem; }
.voted-msg strong { color: #1a1a18; display: block; font-size: 18px; margin-bottom: 4px; font-weight: 600; }
.voted-msg p { font-size: 13px; color: #888; }
.result-item { margin-bottom: 14px; }
.result-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 5px; }
.result-name { font-size: 14px; color: #1a1a18; }
.result-count { font-size: 13px; color: #888; font-weight: 500; }
.result-bar-bg { height: 8px; background: #f0efe8; border-radius: 99px; overflow: hidden; }
.result-bar { height: 100%; background: #1a56db; border-radius: 99px; transition: width 0.6s ease; }
.winner-badge { display: inline-block; background: #e6f4ea; color: #1e7e34; font-size: 11px; padding: 2px 8px; border-radius: 20px; margin-left: 6px; font-weight: 500; }
.result-note { text-align: center; font-size: 12px; color: #aaa; margin-top: 1.5rem; }

.reset-btn { display: block; margin: 1rem auto 0; background: none; border: 1px solid #e8e6e0; border-radius: 8px; padding: 8px 20px; font-size: 13px; color: #888; cursor: pointer; }
.reset-btn:hover { background: #f5f5f0; }
</style>
</head>
<body>
<div class="page">
  <div class="header" id="main-header">
    <span class="vote-badge">一人兩票</span>
    <h1 style="margin-top:8px">2026 同學會選餐廳</h1>
    <p>點卡片展開資訊，最多選 2 間</p>
  </div>

  <div class="restaurants" id="restaurant-list"></div>

  <div class="footer" id="footer">
    <p class="counter">已選 <span id="count">0</span> / 2 間</p>
    <button class="submit-btn" id="submit-btn" disabled>送出投票</button>
  </div>

  <div class="result-view" id="result-view">
    <div class="voted-msg">
      <strong>✅ 投票成功！</strong>
      <p>目前累積票數如下</p>
    </div>
    <div id="result-bars"></div>
    <p class="result-note">每位同學最多投 2 票</p>
    <button class="reset-btn" onclick="resetVote()">重新投票</button>
  </div>
</div>

<script>
const restaurants = [
  {
    id: 'niao',
    name: '鳥以花香',
    type: '中國菜',
    emoji: '🍜',
    rating: 4.4,
    reviews: 1160,
    price: '$$ 中等',
    address: '大安區延吉街233巷3號',
    hours: '11:30–14:30 / 17:30–21:30（每日）',
    parking: '鄰近路邊停車，無專屬停車場',
    menu: '香乾牛肉絲、清炒高麗菜、海鮮豆腐、炸子雞',
    note: '翻修後環境佳，食材新鮮，適合家族聚餐'
  },
  {
    id: 'jinghua',
    name: '長春京華海鮮餐廳',
    type: '海鮮 · 北京烤鴨',
    emoji: '🦆',
    rating: 4.8,
    reviews: 1255,
    price: '$$$ 中高',
    address: '中山區長春路26號',
    hours: '11:30–14:00 / 17:30–21:00（每日）',
    parking: '建議大眾運輸，鄰近捷運',
    menu: '烤鴨三吃、新鮮海鮮、木須鴨、鴨架湯',
    note: '評分超高 4.8，服務親切，可訂私人包廂'
  },
  {
    id: 'taoran',
    name: '陶然亭餐廳',
    type: '中國菜',
    emoji: '🍱',
    rating: 4.2,
    reviews: 4547,
    price: '$$ 中等',
    address: '中山區復興北路86號2F',
    hours: '11:00–14:00 / 17:00–21:00（每日）',
    parking: '鄰近復興北路，路邊停車',
    menu: '北京烤鴨、蔥油餅、臭豆腐、炒水芹、蘿蔔絲酥餅',
    note: '老字號評論數最多，烤鴨桌邊片皮，氣氛傳統'
  },
  {
    id: 'goose',
    name: '鵝肉川-光復店',
    type: '台灣菜',
    emoji: '🦢',
    rating: 4.5,
    reviews: 136,
    price: '$ 平實',
    address: '松山區光復北路7號1樓',
    hours: '11:30–14:00 / 17:00–21:00（週二公休）',
    parking: '附近有路邊停車格',
    menu: '煙燻鵝肉、芋頭米粉湯、豬肝、炸臭豆腐卷、炸蝦',
    note: '懷舊台灣味，環境整潔，注意週二公休'
  },
  {
    id: 'xinjuyuan',
    name: '芯聚苑｜烤鴨·港點·中華',
    type: '中菜館 · 港式點心',
    emoji: '🥟',
    rating: 4.5,
    reviews: 205,
    price: '$$$ 中高',
    address: '信義區松仁路99號B1',
    hours: '11:30–22:00（每日）',
    parking: '宏泰廣場二館停車場，消費折抵2小時',
    menu: '北京烤鴨、港式飲茶、蜜汁叉燒、糖醋排骨',
    note: '停車最方便，信義商圈，環境設計感佳'
  }
];

const STORAGE_KEY = '2026reunion_votes';
let selected = new Set();
let voted = false;
let votes = {};

function loadVotes() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) votes = JSON.parse(saved);
    else restaurants.forEach(r => votes[r.id] = 0);
  } catch(e) {
    restaurants.forEach(r => votes[r.id] = 0);
  }
}

function saveVotes() {
  try { localStorage.setItem(STORAGE_KEY, JSON.stringify(votes)); } catch(e) {}
}

function renderStars(rating) {
  const full = Math.floor(rating);
  const half = (rating % 1) >= 0.3;
  let s = '★'.repeat(full);
  if (half) s += '½';
  s += '☆'.repeat(5 - full - (half ? 1 : 0));
  return s;
}

function renderCards() {
  const list = document.getElementById('restaurant-list');
  list.innerHTML = restaurants.map(r => `
    <div class="card" id="card-${r.id}" onclick="handleCardClick('${r.id}', event)">
      <div class="card-header">
        <div class="card-emoji">${r.emoji}</div>
        <div class="card-title-block">
          <div class="card-name">${r.name}</div>
          <div class="card-type">${r.type}</div>
          <div class="card-rating">
            <span class="stars">${renderStars(r.rating)}</span>
            <span class="rating-num">${r.rating} (${r.reviews.toLocaleString()}則)</span>
          </div>
        </div>
        <div class="check" id="check-${r.id}">
          <div class="check-inner"></div>
        </div>
      </div>
      <button class="toggle-btn" onclick="toggleDetails('${r.id}', event)">
        查看詳情
        <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
          <path d="M3 5l4 4 4-4" stroke="#1a56db" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
      <div class="card-details" id="details-${r.id}">
        <div class="detail-row">
          <span class="detail-icon">📍</span>
          <div class="detail-text"><span class="detail-label">地址</span>${r.address}</div>
        </div>
        <div class="detail-row">
          <span class="detail-icon">🕐</span>
          <div class="detail-text"><span class="detail-label">時間</span>${r.hours}</div>
        </div>
        <div class="detail-row">
          <span class="detail-icon">🚗</span>
          <div class="detail-text"><span class="detail-label">停車</span>${r.parking}</div>
        </div>
        <div class="detail-row">
          <span class="detail-icon">💰</span>
          <div class="detail-text"><span class="detail-label">價位</span>${r.price}</div>
        </div>
        <div class="detail-row">
          <span class="detail-icon">🍽️</span>
          <div class="detail-text"><span class="detail-label">招牌菜</span>${r.menu}</div>
        </div>
        <div class="detail-row">
          <span class="detail-icon">💬</span>
          <div class="detail-text"><span class="detail-label">備註</span>${r.note}</div>
        </div>
      </div>
    </div>
  `).join('');
}

function toggleDetails(id, e) {
  e.stopPropagation();
  document.getElementById('card-' + id).classList.toggle('open');
}

function handleCardClick(id, e) {
  if (voted) return;
  if (e.target.closest('.toggle-btn') || e.target.closest('.card-details')) return;
  const card = document.getElementById('card-' + id);
  if (selected.has(id)) {
    selected.delete(id);
    card.classList.remove('selected');
  } else {
    if (selected.size >= 2) return;
    selected.add(id);
    card.classList.add('selected');
  }
  document.getElementById('count').textContent = selected.size;
  document.getElementById('submit-btn').disabled = selected.size === 0;
}

function showResults() {
  const maxVotes = Math.max(...Object.values(votes), 1);
  document.getElementById('result-bars').innerHTML = restaurants.map(r => {
    const v = votes[r.id] || 0;
    const pct = Math.round((v / maxVotes) * 100);
    const isWinner = v > 0 && v === maxVotes;
    return `
      <div class="result-item">
        <div class="result-header">
          <span class="result-name">${r.emoji} ${r.name}${isWinner ? '<span class="winner-badge">領先</span>' : ''}</span>
          <span class="result-count">${v} 票</span>
        </div>
        <div class="result-bar-bg">
          <div class="result-bar" style="width:${pct}%"></div>
        </div>
      </div>`;
  }).join('');
}

function resetVote() {
  voted = false;
  selected.clear();
  document.getElementById('result-view').classList.remove('show');
  document.getElementById('main-header').style.display = '';
  document.getElementById('restaurant-list').style.display = '';
  document.getElementById('footer').style.display = '';
  document.getElementById('count').textContent = '0';
  document.getElementById('submit-btn').disabled = true;
  renderCards();
}

document.getElementById('submit-btn').addEventListener('click', function() {
  if (selected.size === 0 || voted) return;
  selected.forEach(id => { votes[id] = (votes[id] || 0) + 1; });
  saveVotes();
  voted = true;
  document.getElementById('main-header').style.display = 'none';
  document.getElementById('restaurant-list').style.display = 'none';
  document.getElementById('footer').style.display = 'none';
  document.getElementById('result-view').classList.add('show');
  showResults();
});

loadVotes();
renderCards();
</script>
</body>
</html>
