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
  #mainContent { display:flex; gap:20px; margin-top:30px; flex-wrap:wrap; }
  #userProducts { flex:0 0 300px; max-height:600px; overflow-y:auto; border:1px solid #ddd; padding:10px; border-radius:6px; background:#f5f5f5; }
  #feedContainer { flex:1; min-width:300px; }

  #feed { margin-top:20px; }
  .product { border:1px solid #ddd; padding:10px; border-radius:6px; margin-bottom:30px; display:flex; gap:10px; background:#fafafa; position:relative; }
  .product img { width:100px; height:100px; object-fit:cover; border-radius:6px; }
  .product-info { flex:1; }
  .product-info p { margin:4px 0; }

  .product-actions { position:absolute; top:10px; right:10px; display:flex; flex-direction:column; gap:5px; }

  .filters { display:flex; gap:10px; margin-bottom:20px; flex-wrap:wrap;}
  .filters input, .filters select { flex:1; padding:8px; border-radius:6px; border:1px solid #ccc; }

  #loading { text-align:center; padding:10px; font-weight:bold; color:#555; display:none; }

  /* Popup */
  #popupOverlay { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.6); justify-content:center; align-items:center; z-index:1000; }
  #popupContent { background:#fff; padding:20px; border-radius:12px; width:90%; max-width:600px; max-height:80%; overflow-y:auto; position:relative; }

  /* Chat modal */
  .chat-overlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,0.5); z-index:1100; align-items:center; justify-content:center; }
  .chat-window { width:90%; max-width:900px; height:80%; background:#fff; border-radius:12px; display:flex; overflow:hidden; box-shadow:0 12px 40px rgba(0,0,0,0.25); flex-wrap:wrap; }
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
    <div id="userProducts">
      <h3>Seus anúncios</h3>
    </div>
    <div id="feedContainer">
      <div id="feed"></div>
      <div id="loading">Carregando...</div>
    </div>
  </div>
</div>

<!-- POPUP (produto) -->
<div id="popupOverlay">
  <div id="popupContent">
    <span id="popupClose" onclick="closePopup()">×</span>
    <div id="popupImageContainer"><img id="popupImage" src="" style="width:100%;border-radius:8px;"></div>
    <div id="popupInfo"></div>
  </div>
</div>

<!-- CHAT OVERLAY -->
<div id="chatOverlay" class="chat-overlay">
  <div class="chat-window" role="dialog" aria-modal="true">
    <div class="chat-sidebar">
      <div class="me-summary">
        <div style="font-weight:700;">Você: <span id="meEmailShow"></span></div>
        <div class="small">Status: <span id="meStatusText">offline</span></div>
      </div>
      <div class="chat-list" id="chatList"></div>
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

      <div class="chat-body" id="chatBody"></div>

      <div style="padding:0 16px 8px 16px;">
        <div id="typingIndicator" class="typing-indicator"></div>
      </div>

      <div class="chat-footer">
        <input id="chatInput" class="chat-input" placeholder="Escreva uma mensagem..." oninput="notifyTyping()" onkeydown="handleChatKey(event)">
        <button class="chat-send" onclick="sendChatMessage()">Enviar</button>
      </div>
    </div>
  </div>
</div>

<script>
// TODO: Cole aqui todo o seu JS do HTML original sem alterações, como você já tinha
</script>
</body>
</html>
