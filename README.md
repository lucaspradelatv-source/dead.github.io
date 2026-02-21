<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>...</title>

<style>
body { margin:0; font-family:sans-serif; background:white; }

/* TELA INICIAL */
#selectScreen {
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
  gap:40px;
}

.char {
  width:150px;
  height:200px;
  box-shadow:0 0 20px rgba(0,0,0,0.2);
  display:flex;
  justify-content:center;
  align-items:center;
  color:black;
  opacity:0;
  cursor:pointer;
  transition:0.3s;
}

.char:hover {
  opacity:1;
  background:white;
}

/* CELULAR */
#phone {
  display:none;
  height:100vh;
  background:#f0f0f0;
  flex-direction:column;
}

.tabs {
  display:flex;
  background:white;
  border-bottom:1px solid #ccc;
}

.tab {
  flex:1;
  text-align:center;
  padding:10px;
  cursor:pointer;
}

.active { background:#e0e0e0; }

.screen {
  flex:1;
  display:none;
  overflow-y:auto;
}

.activeScreen { display:block; }

/* CHAT */
.chat { padding:10px; }

.msg {
  padding:8px 12px;
  margin:5px;
  border-radius:10px;
  max-width:70%;
}

.me {
  background:#dcf8c6;
  margin-left:auto;
}

.other {
  background:white;
  border:1px solid #ccc;
}

.inputBar {
  display:flex;
  padding:10px;
  background:white;
}

input {
  flex:1;
  padding:10px;
}

button {
  padding:10px;
}

/* RADIO */
.radio {
  text-align:center;
  padding:40px;
}

.glitch {
  color:red;
  font-weight:bold;
  animation:glitch 0.2s infinite;
}

@keyframes glitch {
  0% { opacity:1; }
  50% { opacity:0.2; }
  100% { opacity:1; }
}
</style>
</head>

<body>

<!-- TELA DE SELEÇÃO -->
<div id="selectScreen">
  <div class="char" onclick="selectChar('Thiago')">Thiago</div>
  <div class="char" onclick="selectChar('Murilo')">Murilo</div>
  <div class="char" onclick="selectChar('Bernardo')">Bernardo</div>
</div>

<!-- CELULAR -->
<div id="phone">
  <div class="tabs">
    <div class="tab active" onclick="openTab(event,'chat')">WhatsApp</div>
    <div class="tab" onclick="openTab(event,'radio')">Rádio</div>
  </div>

  <!-- CHAT -->
  <div id="chat" class="screen activeScreen">
    <div class="chat" id="chatBox"></div>
    <div class="inputBar">
      <input id="msgInput" placeholder="Mensagem">
      <button onclick="sendMsg()">Enviar</button>
    </div>
  </div>

  <!-- RADIO -->
  <div id="radio" class="screen">
    <div class="radio">
      <h2>Hits Heaven Moon</h2>
      <button onclick="playRadio()">Ouvir Rádio</button>
      <p id="radioStatus"></p>
    </div>
  </div>
</div>

<script>
let currentChar = "";

/* RELAÇÕES */
const relations = {
  Thiago: ["Murilo","Bernardo"],
  Murilo: ["Thiago","Bernardo"],
  Bernardo: ["Thiago","Murilo"]
};

function selectChar(name){
  currentChar = name;
  document.getElementById("selectScreen").style.display="none";
  document.getElementById("phone").style.display="flex";
}

/* TABS */
function openTab(event,tab){
  document.querySelectorAll(".tab").forEach(t=>t.classList.remove("active"));
  event.target.classList.add("active");

  document.querySelectorAll(".screen").forEach(s=>s.classList.remove("activeScreen"));
  document.getElementById(tab).classList.add("activeScreen");
}

/* CHAT */
function sendMsg(){
  let input = document.getElementById("msgInput");
  if(!input.value) return;

  addMsg(input.value,"me");
  input.value="";
}

function addMsg(text,type){
  let div = document.createElement("div");
  div.className="msg "+type;
  div.textContent=text;

  /* segurar para apagar */
  let timer;
  div.addEventListener("mousedown",()=>{
    timer=setTimeout(()=>div.remove(),500);
  });
  div.addEventListener("mouseup",()=>clearTimeout(timer));
  div.addEventListener("mouseleave",()=>clearTimeout(timer));

  document.getElementById("chatBox").appendChild(div);
}

/* RADIO */
function playRadio(){
  let status = document.getElementById("radioStatus");
  status.innerHTML="SINTONIZANDO...";
  setTimeout(()=>{
    status.innerHTML='<span class="glitch">SEM SINAL ▓▒░</span>';
  },1500);
}
</script>

</body>
</html>
