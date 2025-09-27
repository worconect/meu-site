[index.html](https://github.com/user-attachments/files/22576202/index.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Worconect - Mini OLX</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap" rel="stylesheet">
<style>
  * { box-sizing: border-box; margin:0; padding:0; }
  body, html { height:100%; font-family:'Montserrat', sans-serif; background:#1877f2; }

  .container { background:#fff; padding:50px 40px; border-radius:12px; box-shadow:0 8px 24px rgba(0,0,0,0.2); width:100%; max-width:500px; text-align:center; margin:50px auto;}
  h1 { color:#1877f2; font-size:40px; margin-bottom:30px; }
  label { display:block; text-align:left; margin-top:10px; font-weight:600; }
  input, select, textarea { width:100%; padding:12px; margin-top:5px; margin-bottom:15px; border:1px solid #ddd; border-radius:8px; font-size:16px; }
  
  .btn-primary { width:100%; padding:14px; margin-top:10px; border:none; border-radius:8px; background-color:#1877f2; color:#fff; font-size:16px; font-weight:700; cursor:pointer; font-family:'Montserrat', sans-serif; transition:background 0.3s ease; display:inline-block; }
  .btn-primary:hover { background-color:#155ab6; }

  .btn-secondary { width:100%; padding:14px; margin-top:10px; border:none; border-radius:8px; background-color:#4CAF50; color:#fff; font-size:16px; font-weight:700; cursor:pointer; font-family:'Montserrat', sans-serif; transition:background 0.3s ease; display:inline-block; }
  .btn-secondary:hover { background-color:#388E3C; }

  .btn-view { width:100%; padding:14px; margin-top:5px; border:none; border-radius:8px; background-color:#FF9800; color:#fff; font-size:16px; font-weight:700; cursor:pointer; font-family:'Montserrat', sans-serif; transition:background 0.3s ease; display:inline-block; }
  .btn-view:hover { background-color:#F57C00; }

  .error-message { color:red; margin-top:12px; font-size:14px; display:none; }
  .success-message { color:green; margin-top:12px; font-size:14px; display:none; }

  #homePage { display:none; padding:40px 20px; max-width:1200px; margin:20px auto; background:#fff; border-radius:8px; box-shadow:0 4px 12px rgba(0,0,0,0.1); }

  /* Layout com produtos do usuário à esquerda e feed à direita */
  #mainContent { display:flex; gap:20px; margin-top:30px; }
  #userProducts { flex:0 0 300px; max-height:600px; overflow-y:auto; border:1px solid #ddd; padding:10px; border-radius:6px; background:#f5f5f5; }
  #feedContainer { flex:1; }

  #feed { margin-top:20px; }
  .product { border:1px solid #ddd; padding:10px; border-radius:6px; margin-bottom:30px; display:flex; gap:10px; background:#fafafa; position:relative; }
  .product img { width:100px; height:100px; object-fit:cover; border-radius:6px; }
  .product-info { flex:1; }
  .product-info p { margin:4px 0; }

  .product-actions { position:absolute; top:10px; right:10px; display:flex; flex-direction:column; gap:5px; }

  .filters { display:flex; gap:10px; margin-bottom:20px; flex-wrap:wrap;}
  .filters input, .filters select { flex:1; padding:8px; border-radius:6px; border:1px solid #ccc; }

  #loading { text-align:center; padding:10px; font-weight:bold; color:#555; display:none; }

  /* Popup (existing) stays as is */
  #popupOverlay { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.6); justify-content:center; align-items:center; z-index:1000; }
  #popupContent { background:#fff; padding:20px; border-radius:12px; width:90%; max-width:600px; max-height:80%; overflow-y:auto; position:relative; }

  /* Chat modal styles */
  .chat-overlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,0.5); z-index:1100; align-items:center; justify-content:center; }
  .chat-window { width:90%; max-width:900px; height:80%; background:#fff; border-radius:12px; display:flex; overflow:hidden; box-shadow:0 12px 40px rgba(0,0,0,0.25); }
  .chat-sidebar { width:320px; border-right:1px solid #eee; display:flex; flex-direction:column; background:#fafafa; }
  .chat-sidebar .me-summary { padding:16px; border-bottom:1px solid #eee; }
  .chat-list { flex:1; overflow-y:auto; padding:8px; }
  .chat-list .chat-list-item { padding:10px; border-radius:8px; display:flex; gap:10px; align-items:center; cursor:pointer; }
  .chat-list .chat-list-item:hover { background:#f0f0f0; }
  .chat-list .chat-list-item.active { background:#e9f5ff; }

  .chat-main { flex:1; display:flex; flex-direction:column; }
  .chat-header { padding:12px 16px; border-bottom:1px solid #eee; display:flex; align-items:center; justify-content:space-between; gap:12px; }
  .chat-header .user-info { display:flex; gap:12px; align-items:center; }
  .chat-body { flex:1; padding:20px; overflow-y:auto; background:linear-gradient(#fff,#f7fbff); display:flex; flex-direction:column; gap:10px; }
  .bubble { max-width:70%; padding:10px 14px; border-radius:16px; line-height:1.3; }
  .bubble.me { background:#1877f2; color:#fff; align-self:flex-end; border-bottom-right-radius:6px; }
  .bubble.other { background:#f1f1f1; color:#111; align-self:flex-start; border-bottom-left-radius:6px; }
  .chat-footer { padding:12px; border-top:1px solid #eee; display:flex; gap:8px; align-items:center; }
  .chat-input { flex:1; padding:10px 12px; border-radius:999px; border:1px solid #ccc; font-size:15px; }
  .chat-send { padding:10px 14px; border-radius:999px; border:none; background:#FF9800; color:#fff; font-weight:700; cursor:pointer; }
  .status-dot { width:10px; height:10px; border-radius:50%; display:inline-block; margin-right:6px; }
  .status-online { background:#4CAF50; }
  .status-offline { background:#bbb; }

  .typing-indicator { font-size:13px; color:#666; margin-top:6px; }
  .small { font-size:13px; color:#666; }

  /* Responsive */
  @media (max-width:700px){
    .chat-window { flex-direction:column; height:92%; width:96%; }
    .chat-sidebar { width:100%; height:180px; border-right:none; border-bottom:1px solid #eee; }
  }
</style>
</head>
<body>

<!-- LOGIN -->
<div class="container" id="loginForm">
  <h1>Worconect</h1>
  <input type="text" id="loginEmail" placeholder="Email">
  <input type="password" id="loginPassword" placeholder="Senha">
  <button class="btn-primary" onclick="login()">Entrar</button>
  <button class="btn-secondary" onclick="showCadastro()">Cadastre-se</button>
  <div class="error-message" id="loginError">Email ou senha incorretos!</div>
</div>

<!-- PÁGINA INICIAL -->
<div id="homePage">
  <h1>Worconect</h1>

  <h2>Cadastrar Serviço</h2>
  <input type="text" id="title" placeholder="Título">
  <textarea id="description" placeholder="Descrição"></textarea>
  <input type="text" id="city" placeholder="Cidade">
  <input type="text" id="bairro" placeholder="Bairro">
  <input type="text" id="state" placeholder="Estado">
  <select id="category">
    <option value="">Profisão</option>
    <option value="Advogado">Advogado(a)</option>
    <option value="Eletricista">Eletricista</option>
    <option value="Encanador">Encanador(a)</option>
    <option value="Médico">Médico(a)</option>
    <option value="Professor">Professor(a)</option>
  </select>
  <input type="file" id="image">
  <button class="btn-primary" onclick="addProduct()">Cadastrar</button>
  <div class="success-message" id="successMsg">Produto cadastrado com sucesso!</div>

  <h2>Filtrar Produtos</h2>
  <div class="filters">
    <input type="text" id="search" placeholder="Buscar por palavra-chave" oninput="resetFeed()">
    <select id="filterCategory" onchange="resetFeed()">
      <option value="">Todas as categorias</option>
      <option value="eletronicos">Eletrônicos</option>
      <option value="moveis">Móveis</option>
      <option value="vestuario">Vestuário</option>
      <option value="outros">Outros</option>
    </select>
    <select id="sortFeed" onchange="resetFeed()">
      <option value="recentes">Mais recentes</option>
    </select>
  </div>

  <div id="mainContent">
    <!-- Produtos do usuário -->
    <div id="userProducts">
      <h3>Seus anúncios</h3>
    </div>
    <!-- Feed de outros usuários -->
    <div id="feedContainer">
      <div id="feed"></div>
      <div id="loading">Carregando...</div>
    </div>
  </div>
</div>

<!-- POPUP (produto) -->
<div id="popupOverlay" style="display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.6);justify-content:center;align-items:center;z-index:1000;">
  <div id="popupContent" style="background:#fff;padding:20px;border-radius:12px;width:90%;max-width:600px;max-height:80%;overflow-y:auto;position:relative;">
    <span id="popupClose" style="position:absolute;top:10px;right:10px;cursor:pointer;font-size:20px;font-weight:bold;" onclick="closePopup()">×</span>
    <div id="popupImageContainer" style="margin-bottom:10px;"><img id="popupImage" src="" style="width:100%;border-radius:8px;"></div>
    <div id="popupInfo"></div>
  </div>
</div>

<!-- CHAT OVERLAY -->
<div id="chatOverlay" class="chat-overlay" style="display:none;align-items:center;justify-content:center;">
  <div class="chat-window" role="dialog" aria-modal="true">
    <div class="chat-sidebar">
      <div class="me-summary">
        <div style="font-weight:700;">Você: <span id="meEmailShow"></span></div>
        <div class="small">Status: <span id="meStatusText">offline</span></div>
      </div>
      <div class="chat-list" id="chatList">
        <!-- Lista de conversas recentes gerada dinamicamente -->
      </div>
    </div>

    <div class="chat-main">
      <div class="chat-header" id="chatHeader">
        <div class="user-info">
          <div style="display:flex;flex-direction:column;">
            <div style="font-weight:700;" id="chatWithName">Usuário</div>
            <div class="small" id="chatWithStatus"><span class="status-dot status-offline"></span>offline</div>
          </div>
        </div>
        <div>
          <button class="btn-secondary" onclick="closeChat()">Fechar</button>
        </div>
      </div>

      <div class="chat-body" id="chatBody">
        <!-- messages -->
      </div>

      <div style="padding:0 16px 8px 16px;">
        <div id="typingIndicator" class="typing-indicator" style="display:none;"></div>
      </div>

      <div class="chat-footer">
        <input id="chatInput" class="chat-input" placeholder="Escreva uma mensagem..." oninput="notifyTyping()" onkeydown="handleChatKey(event)">
        <button class="chat-send" onclick="sendChatMessage()">Enviar</button>
      </div>
    </div>
  </div>
</div>

<script>
/* -------------------------
   Dados e estado local
   ------------------------- */
let usuarioAtual = null;
let products = JSON.parse(localStorage.getItem('products')) || [];
let feedIndex = 0;
const feedBatch = 5;

/* Chat data stored in localStorage under key 'wc_messages' as array of messages:
   { id, from, to, text, productId, timestamp, read:boolean }
   Presence stored under 'wc_presence' : { email: timestampMillis }
   Typing stored under 'wc_typing' : { "<from>_<to>": timestampMillis }
*/

function ensureChatStorage(){
  if(!localStorage.getItem('wc_messages')) localStorage.setItem('wc_messages', JSON.stringify([]));
  if(!localStorage.getItem('wc_presence')) localStorage.setItem('wc_presence', JSON.stringify({}));
  if(!localStorage.getItem('wc_typing')) localStorage.setItem('wc_typing', JSON.stringify({}));
}
ensureChatStorage();

/* -------------------------
   Login / Presença
   ------------------------- */
let presenceInterval = null;
function login() {
  const email = document.getElementById('loginEmail').value;
  const password = document.getElementById('loginPassword').value;
  if(!email || !password){
    document.getElementById('loginError').style.display='block';
    return;
  }
  document.getElementById('loginError').style.display='none';
  usuarioAtual = {email};
  document.getElementById('loginForm').style.display='none';
  document.getElementById('homePage').style.display='block';
  document.getElementById('meEmailShow').innerText = email;
  updatePresence(); // set now
  if(presenceInterval) clearInterval(presenceInterval);
  presenceInterval = setInterval(updatePresence, 5000); // update presence every 5s
  window.addEventListener('beforeunload', () => { setPresenceOffline(); });
  resetFeed();
  loadUserProducts();
  refreshChatList();
}

/* called periodically to mark user as online */
function updatePresence(){
  if(!usuarioAtual) return;
  const p = JSON.parse(localStorage.getItem('wc_presence')||'{}');
  p[usuarioAtual.email] = Date.now();
  localStorage.setItem('wc_presence', JSON.stringify(p));
  // also update UI label
  const now = Date.now();
  document.getElementById('meStatusText').innerText = 'online';
}

/* set offline timestamp (older) */
function setPresenceOffline(){
  if(!usuarioAtual) return;
  const p = JSON.parse(localStorage.getItem('wc_presence')||'{}');
  p[usuarioAtual.email] = Date.now() - 1000*60*60; // mark older
  localStorage.setItem('wc_presence', JSON.stringify(p));
}

/* returns true if email is online (last active within threshold ms) */
function isOnline(email){
  const p = JSON.parse(localStorage.getItem('wc_presence')||'{}');
  if(!p[email]) return false;
  return (Date.now() - p[email]) < 15000; // 15 seconds considered online for demo
}

/* -------------------------
   Cadastro produto (sem alteração)
   ------------------------- */
function showCadastro(){
  const email = prompt("Digite seu email:");
  const senha = prompt("Digite sua senha:");
  if(email && senha){
    alert("Cadastro realizado com sucesso! Faça login.");
  }
}

function addProduct() {
  const title = document.getElementById('title').value;
  const description = document.getElementById('description').value;
  const city = document.getElementById('city').value;
  const bairro = document.getElementById('bairro').value;
  const state = document.getElementById('state').value;
  const category = document.getElementById('category').value;
  const imageFile = document.getElementById('image').files[0];

  if(!title || !description || !category || !city || !bairro || !state) {
    alert('Preencha todos os campos!');
    return;
  }

  const timestamp = new Date();
  const reader = new FileReader();
  reader.onload = function() {
    const imageData = reader.result;
    const product = { id: Date.now(), user: usuarioAtual.email, title, description, city, bairro, state, category, image:imageData, timestamp: timestamp.toLocaleString() };
    products.unshift(product);
    localStorage.setItem('products', JSON.stringify(products));
    resetFeed();
    loadUserProducts();
    clearForm();
    showSuccessMsg();
  }
  if(imageFile){
    reader.readAsDataURL(imageFile);
  } else {
    const product = { id: Date.now(), user: usuarioAtual.email, title, description, city, bairro, state, category, image:'', timestamp: timestamp.toLocaleString() };
    products.unshift(product);
    localStorage.setItem('products', JSON.stringify(products));
    resetFeed();
    loadUserProducts();
    clearForm();
    showSuccessMsg();
  }
}

function showSuccessMsg() {
  const msg = document.getElementById('successMsg');
  msg.style.display = 'block';
  setTimeout(()=>{ msg.style.display='none'; }, 3000);
}

function clearForm() {
  document.getElementById('title').value='';
  document.getElementById('description').value='';
  document.getElementById('category').value='';
  document.getElementById('image').value='';
  document.getElementById('city').value='';
  document.getElementById('bairro').value='';
  document.getElementById('state').value='';
}

/* -------------------------
   Feed / Produtos (sem alteração funcional)
   ------------------------- */

function getFilteredProducts() {
  const searchText = document.getElementById('search').value.toLowerCase();
  const filterCat = document.getElementById('filterCategory').value;
  const sortOption = document.getElementById('sortFeed').value;

  let filtered = products.filter(p =>
    (p.title.toLowerCase().includes(searchText) || p.description.toLowerCase().includes(searchText)) &&
    (filterCat === '' || p.category === filterCat)
  );

  if(sortOption === 'recentes') filtered.sort((a,b)=>b.id - a.id);

  return filtered;
}

function loadFeed() {
  const feed = document.getElementById('feed');
  const filtered = getFilteredProducts();
  const loading = document.getElementById('loading');
  if(feedIndex >= filtered.length){
    loading.style.display = 'none';
    return;
  }
  loading.style.display = 'block';
  setTimeout(() => {
    const batch = filtered.slice(feedIndex, feedIndex + feedBatch);
    batch.forEach(p => {
      const div = document.createElement('div');
      div.className = 'product';
      div.innerHTML = `
        <img src="${p.image || 'https://via.placeholder.com/100'}" alt="produto">
        <div class="product-info">
          <p><b>${p.title}</b></p>
          <p>${p.description}</p>
          <p>Local: ${p.city}, ${p.bairro}, ${p.state}</p>
          <p>Categoria: ${p.category}</p>
          <p>Data: ${p.timestamp}</p>
        </div>
        <div class="product-actions">
          ${p.user === usuarioAtual.email ? `
            <button class="btn-primary" onclick="editProduct(${p.id})">Editar</button>
            <button class="btn-secondary" onclick="deleteProduct(${p.id})">Excluir</button>` 
            : `<button class="btn-view" onclick="viewProduct(${p.id})">Enviar Mensagem</button>`}
        </div>
      `;
      feed.appendChild(div);
    });
    feedIndex += feedBatch;
    loading.style.display='none';
  }, 200);
}

function loadUserProducts() {
  const userDiv = document.getElementById('userProducts');
  userDiv.innerHTML = '<h3>Seus anúncios</h3>';
  const userProducts = products.filter(p => p.user === usuarioAtual.email);
  userProducts.forEach(p => {
    const div = document.createElement('div');
    div.className = 'product';
    div.innerHTML = `
      <img src="${p.image || 'https://via.placeholder.com/100'}" alt="produto">
      <div class="product-info">
        <p><b>${p.title}</b></p>
        <p>${p.description}</p>
        <p>Local: ${p.city}, ${p.bairro}, ${p.state}</p>
        <p>Categoria: ${p.category}</p>
        <p>Data: ${p.timestamp}</p>
      </div>
      <div class="product-actions">
        <button class="btn-primary" onclick="editProduct(${p.id})">Editar</button>
        <button class="btn-secondary" onclick="deleteProduct(${p.id})">Excluir</button>
      </div>
    `;
    userDiv.appendChild(div);
  });
}

/* Funções auxiliares de produtos */
function deleteProduct(id){ if(confirm('Deseja realmente excluir este produto?')){ products = products.filter(p=>p.id!==id); localStorage.setItem('products', JSON.stringify(products)); resetFeed(); loadUserProducts(); } }
function editProduct(id){ /* função de edição */ }

/* -------------------------
   VIEW PRODUCT -> abrir chat
   ------------------------- */
function viewProduct(id){
  const product = products.find(p => p.id === id);
  if(!product) return;
  // open chat with product.user
  openChatWithUser(product.user, product.id, product.title);
}

/* -------------------------
   Chat: open/close/refresh
   ------------------------- */
let currentChatWith = null; // email
let currentProductContext = null; // productId
let chatPollInterval = null;
let typingTimeout = null;

function openChatWithUser(email, productId=null, productTitle=''){
  if(!usuarioAtual) { alert('Faça login para enviar mensagens'); return; }
  ensureChatStorage();
  currentChatWith = email;
  currentProductContext = productId;
  document.getElementById('chatOverlay').style.display = 'flex';
  document.getElementById('chatWithName').innerText = productTitle ? `${productTitle} — ${email}` : email;
  updateChatHeaderStatus();
  refreshChatBody();
  refreshChatList();
  // mark messages as read for this conversation
  markMessagesRead(usuarioAtual.email, currentChatWith, currentProductContext);
  // poll for new messages and presence updates
  if(chatPollInterval) clearInterval(chatPollInterval);
  chatPollInterval = setInterval(()=> {
    refreshChatBody();
    updateChatHeaderStatus();
    refreshChatList();
    markMessagesRead(usuarioAtual.email, currentChatWith, currentProductContext);
    checkTypingIndicator();
  }, 1000);
  // focus input after opening
  setTimeout(()=>{ document.getElementById('chatInput').focus(); }, 250);
}

/* close chat */
function closeChat(){
  document.getElementById('chatOverlay').style.display = 'none';
  currentChatWith = null;
  currentProductContext = null;
  if(chatPollInterval) clearInterval(chatPollInterval);
  // clear typing state for this user-to-other
  clearTypingState();
}

/* update header status (online/offline) */
function updateChatHeaderStatus(){
  if(!currentChatWith) return;
  const statusEl = document.getElementById('chatWithStatus');
  const dot = statusEl.querySelector('.status-dot');
  const online = isOnline(currentChatWith);
  statusEl.innerHTML = `<span class="status-dot ${online ? 'status-online' : 'status-offline'}"></span>${online ? 'online' : 'offline'}`;
}

/* prepare and display messages in chat body */
function refreshChatBody(){
  if(!currentChatWith) return;
  const all = JSON.parse(localStorage.getItem('wc_messages')||'[]');
  // filter messages between usuarioAtual and currentChatWith, optionally by product context
  const conv = all.filter(m => {
    const sameContext = (currentProductContext == null) || (m.productId === currentProductContext);
    return sameContext && ((m.from === usuarioAtual.email && m.to === currentChatWith) || (m.from === currentChatWith && m.to === usuarioAtual.email));
  }).sort((a,b)=>a.timestamp - b.timestamp);
  const chatBody = document.getElementById('chatBody');
  chatBody.innerHTML = '';
  conv.forEach(m => {
    const div = document.createElement('div');
    div.className = 'bubble ' + (m.from === usuarioAtual.email ? 'me' : 'other');
    const timeText = new Date(m.timestamp).toLocaleTimeString();
    div.innerHTML = `<div style="font-size:14px;">${escapeHtml(m.text)}</div><div style="font-size:11px;margin-top:6px;opacity:0.8;text-align:${m.from===usuarioAtual.email?'right':'left'}">${timeText}${m.from===usuarioAtual.email ? (m.read ? ' ✓✓' : ' ✓') : ''}</div>`;
    chatBody.appendChild(div);
  });
  // scroll to bottom
  chatBody.scrollTop = chatBody.scrollHeight;
}

/* send a chat message */
function sendChatMessage(){
  const input = document.getElementById('chatInput');
  const text = input.value.trim();
  if(!text || !currentChatWith) return;
  const messages = JSON.parse(localStorage.getItem('wc_messages')||'[]');
  const msg = { id: Date.now() + Math.floor(Math.random()*1000), from: usuarioAtual.email, to: currentChatWith, text, productId: currentProductContext, timestamp: Date.now(), read: false };
  messages.push(msg);
  localStorage.setItem('wc_messages', JSON.stringify(messages));
  input.value = '';
  // notify typing cleared
  clearTypingState();
  refreshChatBody();
  refreshChatList();
  // update storage so other tabs get event
  localStorage.setItem('wc_lastupdate', Date.now());
}

/* mark messages as read when opening conversation */
function markMessagesRead(me, other, productId=null){
  const messages = JSON.parse(localStorage.getItem('wc_messages')||'[]');
  let changed = false;
  messages.forEach(m=>{
    if(m.from === other && m.to === me && (productId==null || m.productId===productId) && !m.read){
      m.read = true;
      changed = true;
    }
  });
  if(changed) {
    localStorage.setItem('wc_messages', JSON.stringify(messages));
    localStorage.setItem('wc_lastupdate', Date.now());
  }
}

/* Chat list (conversations) */
function getConversations(){
  const messages = JSON.parse(localStorage.getItem('wc_messages')||'[]');
  const convoMap = {};
  messages.forEach(m => {
    const keyA = `${m.from}|${m.to}|${m.productId||''}`;
    // create normalized pairs: we'll aggregate by partner + productId
    const partner = (m.from === usuarioAtual.email) ? m.to : m.from;
    const id = partner + '||' + (m.productId||'');
    if(!convoMap[id]) convoMap[id] = { partner, productId: m.productId, lastTimestamp: m.timestamp, lastText: m.text, unread:0 };
    if(m.timestamp > convoMap[id].lastTimestamp) convoMap[id].lastTimestamp = m.timestamp, convoMap[id].lastText = m.text;
    if(m.to === usuarioAtual.email && !m.read) convoMap[id].unread++;
  });
  // also add conversations from products (if no messages yet) to allow starting chat
  products.forEach(p=>{
    if(p.user !== usuarioAtual.email){
      const id = p.user + '||' + (p.id||'');
      if(!convoMap[id]) convoMap[id] = { partner: p.user, productId: p.id, lastTimestamp: 0, lastText: `Anúncio: ${p.title}`, unread:0 };
    }
  });
  // convert to array and sort by lastTimestamp desc
  const arr = Object.values(convoMap).sort((a,b)=>b.lastTimestamp - a.lastTimestamp);
  return arr;
}

function refreshChatList(){
  const listEl = document.getElementById('chatList');
  listEl.innerHTML = '';
  if(!usuarioAtual) return;
  const convos = getConversations();
  convos.forEach(c => {
    const el = document.createElement('div');
    el.className = 'chat-list-item' + ((currentChatWith === c.partner && currentProductContext == c.productId) ? ' active' : '');
    el.innerHTML = `
      <div style="flex:1;">
        <div style="display:flex;justify-content:space-between;align-items:center;">
          <div style="font-weight:700;">${c.partner}</div>
          <div style="font-size:12px;color:#666;">${c.lastTimestamp? new Date(c.lastTimestamp).toLocaleTimeString() : ''}</div>
        </div>
        <div style="font-size:13px;color:#666;margin-top:6px;">${c.lastText}</div>
      </div>
    `;
    el.onclick = () => { openChatWithUser(c.partner, c.productId, c.productId ? (`Anúncio #${c.productId}`) : c.partner); };
    listEl.appendChild(el);
  });
}

/* Typing indicator handling */
function notifyTyping(){
  if(!usuarioAtual || !currentChatWith) return;
  const key = `${usuarioAtual.email}_${currentChatWith}_${currentProductContext||''}`;
  const t = JSON.parse(localStorage.getItem('wc_typing')||'{}');
  t[key] = Date.now();
  localStorage.setItem('wc_typing', JSON.stringify(t));
  localStorage.setItem('wc_lasttyping', Date.now());
  // remove after a short time locally
  if(typingTimeout) clearTimeout(typingTimeout);
  typingTimeout = setTimeout(()=>{ clearTypingState(); }, 4000);
}

function clearTypingState(){
  if(!usuarioAtual) return;
  const t = JSON.parse(localStorage.getItem('wc_typing')||'{}');
  // remove entries from this user
  Object.keys(t).forEach(k => {
    if(k.startsWith(usuarioAtual.email + '_')) delete t[k];
  });
  localStorage.setItem('wc_typing', JSON.stringify(t));
}

function checkTypingIndicator(){
  if(!currentChatWith) return;
  const t = JSON.parse(localStorage.getItem('wc_typing')||'{}');
  const key = `${currentChatWith}_${usuarioAtual.email}_${currentProductContext||''}`; // partner->me
  let show = false;
  if(t[key] && (Date.now() - t[key] < 4000)) show = true;
  const el = document.getElementById('typingIndicator');
  if(show) { el.style.display = 'block'; el.innerText = `${currentChatWith} está digitando...`; }
  else { el.style.display = 'none'; el.innerText = ''; }
}

/* helper to escape HTML */
function escapeHtml(text){
  const map = { '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;' };
  return String(text).replace(/[&<>"']/g, m => map[m]);
}

/* handle Enter to send */
function handleChatKey(e){
  if(e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault();
    sendChatMessage();
  }
}

/* storage event to sync across tabs */
window.addEventListener('storage', (ev) => {
  if(ev.key === 'wc_messages' || ev.key === 'wc_lastupdate' || ev.key === 'wc_presence' || ev.key === 'wc_typing'){
    refreshChatList();
    refreshChatBody();
    updateChatHeaderStatus();
    checkTypingIndicator();
  }
});

/* helper to periodically cleanup old typing/presence entries */
setInterval(()=> {
  // cleanup old typing entries
  const t = JSON.parse(localStorage.getItem('wc_typing')||'{}');
  let changed = false;
  Object.keys(t).forEach(k => { if(Date.now() - t[k] > 10000){ delete t[k]; changed = true; } });
  if(changed) localStorage.setItem('wc_typing', JSON.stringify(t));
  // cleanup presence that are too old is okay (we keep record but online check handles it)
}, 15000);

/* -------------------------
   Chat UI utilities
   ------------------------- */

/* When opening chat show status etc. */
function updateChatUIState(){
  document.getElementById('meEmailShow').innerText = usuarioAtual ? usuarioAtual.email : '';
  if(usuarioAtual) updatePresence();
}

/* mark messages read when focus on chat input */
document.getElementById('chatInput').addEventListener('focus', ()=> {
  if(currentChatWith) markMessagesRead(usuarioAtual.email, currentChatWith, currentProductContext);
});

/* -------------------------
   Misc: popup product view
   ------------------------- */
function closePopup(){ document.getElementById('popupOverlay').style.display='none'; }

function resetFeed() { feedIndex = 0; document.getElementById('feed').innerHTML=''; loadFeed(); }

window.addEventListener('scroll', () => { if(window.innerHeight + window.scrollY >= document.body.offsetHeight - 50){ loadFeed(); } });

/* init: load feed if user already logged (from previous session) */
(function init(){
  // restore products var
  products = JSON.parse(localStorage.getItem('products')) || [];
  // If a user email was left in localStorage previously we won't auto-login for security.
})();

/* small utility: open chat via "Enviar Mensagem" button already wired to viewProduct() */

/* Expose a couple functions for debugging if needed */
window.openChatWithUser = openChatWithUser;
window.closeChat = closeChat;

/* Extra: when user clicks outside chat overlay close? We'll require explicit close button for safety. */
/* Styling/behavior designed to not alter other page layout or flows. */

</script>
</body>
</html>
