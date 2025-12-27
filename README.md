<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>A.CSfit - Dieta & Treino</title>
<style>
/* ===== RESET E FONTE ===== */
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:Arial,sans-serif;color:#fff;overflow-x:hidden;background:#111;}
body::before{
  content:"";
  position:fixed;top:0;left:0;width:100%;height:100%;
  background:linear-gradient(270deg,#ff512f,#dd2476,#1fa2ff,#12d8fa);
  background-size:800% 800%;
  animation:grad 20s ease infinite;
  z-index:-1;
}
@keyframes grad{
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}
header{
  padding:20px;text-align:center;font-size:28px;font-weight:bold;
  background:rgba(0,0,0,0.6);border-bottom:2px solid #1db954;
  text-shadow:0 0 10px #1db954;
  position:sticky;top:0;z-index:100;
}
section{
  background:rgba(0,0,0,0.8);margin:20px auto;padding:20px;
  border-radius:12px;max-width:1000px;box-shadow:0 0 20px rgba(0,0,0,0.7);
  animation:fadeIn 1s ease forwards;opacity:0;
}
@keyframes fadeIn{to{opacity:1;}}
input, select, button{
  width:100%;padding:10px;margin:5px 0;border-radius:6px;border:none;
  font-size:16px;
}
button{background:#1db954;color:#fff;cursor:pointer;transition:0.3s;}
button:hover{background:#12c44d;transform:scale(1.05);}
.card, .exercise{
  background:#222;padding:10px;border-radius:8px;margin-top:10px;
  box-shadow:0 0 10px rgba(0,0,0,0.6);transition:0.3s;
}
.card:hover,.exercise:hover{transform:translateY(-3px);box-shadow:0 0 20px #1db954;}
.exercise{cursor:pointer;}
.collapsible{
  background:#333;color:white;cursor:pointer;padding:10px;border:none;
  text-align:left;font-size:16px;border-radius:6px;margin-top:5px;
}
.content{padding:0 18px;display:none;overflow:hidden;background-color:#222;border-radius:6px;margin-top:5px;}
.progressContainer{
  background:#333;border-radius:6px;overflow:hidden;margin:5px 0;
}
.progressBar{
  height:20px;background:#1db954;width:0%;transition:width 1s;
  border-radius:6px;
}
</style>
</head>
<body>

<header>🏋️‍♂️ A.CSfit</header>
<div style="text-align:center;margin-top:10px;font-size:18px;">Tempo no site: <span id="timer">0h 0m</span></div>

<!-- HORÁRIOS MINIMIZADOS -->
<button class="collapsible">⏰ Horários das Refeições</button>
<div class="content">
  <input type="time" id="cafeHora" placeholder="Café da manhã">
  <input type="time" id="almocoHora" placeholder="Almoço">
  <input type="time" id="lancheHora" placeholder="Lanche da tarde">
  <input type="time" id="jantarHora" placeholder="Jantar">
  <input type="time" id="ceiaHora" placeholder="Ceia">
  <button onclick="salvarHorarios()">Salvar Horários</button>
</div>

<!-- DIETA -->
<section>
<h2>🥗 Montar Dieta</h2>
<div style="display:flex;gap:10px;flex-wrap:wrap;">
<input id="foodName" placeholder="Pesquisar alimento">
<select id="meal">
  <option value="cafe">Café da manhã</option>
  <option value="almoco">Almoço</option>
  <option value="lanche">Lanche</option>
  <option value="jantar">Jantar</option>
  <option value="ceia">Ceia</option>
</select>
<select id="measure">
  <option value="g">Gramas</option>
  <option value="cupSmall">Copo pequeno (200ml)</option>
  <option value="cupLarge">Copo grande (300ml)</option>
  <option value="unit">Unidade</option>
</select>
<input type="number" id="foodQty" placeholder="Quantidade">
<button onclick="addFood()">Adicionar alimento</button>
</div>
<div id="diet"></div>
<h3>Total diário:</h3>
<div class="progressContainer"><div class="progressBar" id="calBar"></div></div>
<div class="progressContainer"><div class="progressBar" id="protBar"></div></div>
<div class="progressContainer"><div class="progressBar" id="carbBar"></div></div>
<div class="progressContainer"><div class="progressBar" id="gordBar"></div></div>
</section>

<!-- TREINO POR GRUPO -->
<section>
<h2>🏋️ Treinos por Grupo Muscular</h2>
<select id="grupo" onchange="mostrarTreino()">
  <option value="">Selecione grupo</option>
  <option value="peito">Peito</option>
  <option value="costas">Costas</option>
  <option value="biceps">Bíceps</option>
  <option value="triceps">Tríceps</option>
  <option value="ombro">Ombro</option>
  <option value="trapezio">Trapézio</option>
  <option value="perna">Perna</option>
</select>
<select id="intensidade" onchange="mostrarTreino()">
  <option value="iniciante">Iniciante</option>
  <option value="intermediario">Intermediário</option>
  <option value="avancado">Avançado</option>
</select>
<div id="listaTreino"></div>
</section>

<!-- FICHA -->
<section>
<h2>📝 Minha Ficha de Treino</h2>
<select id="grupoFicha" onchange="carregarExercicios()">
  <option value="">Selecione Grupo</option>
  <option value="peito">Peito</option>
  <option value="costas">Costas</option>
  <option value="biceps">Bíceps</option>
  <option value="triceps">Tríceps</option>
  <option value="ombro">Ombro</option>
  <option value="trapezio">Trapézio</option>
  <option value="perna">Perna</option>
</select>
<select id="exercicioFicha"></select>
<select id="intensidadeFicha">
  <option value="iniciante">Iniciante</option>
  <option value="intermediario">Intermediário</option>
  <option value="avancado">Avançado</option>
</select>
<input type="number" id="carga" placeholder="Carga (kg)">
<button onclick="addFicha()">Adicionar exercício</button>
<div id="minhaFicha"></div>
</section>

<!-- SOM ALERTA -->
<audio id="alerta"><source src="https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg" type="audio/ogg"></audio>

<script>
// ===== TIMER =====
let segundosNoSite=0;
setInterval(()=>{
  segundosNoSite++;
  let h=Math.floor(segundosNoSite/3600);
  let m=Math.floor((segundosNoSite%3600)/60);
  document.getElementById("timer").innerText=`${h}h ${m}m`;
},60000);

// ===== COLLAPSIBLE =====
const coll=document.getElementsByClassName("collapsible");
for(let i=0;i<coll.length;i++){
  coll[i].addEventListener("click",function(){
    const content=this.nextElementSibling;
    content.style.display=(content.style.display==="block")?"none":"block";
  });
}

// ===== BANCO DE ALIMENTOS =====
const bancoAlimentos={
"carne bovina":{cal:250,prot:26,carb:0,gord:15},"carne suina":{cal:242,prot:25,carb:0,gord:14},
"frango peito":{cal:165,prot:31,carb:0,gord:3.6},"frango coxa":{cal:209,prot:26,carb:0,gord:10.9},
"peru":{cal:135,prot:30,carb:0,gord:1},"peixe":{cal:206,prot:22,carb:0,gord:12},
"ovos":{cal:155,prot:13,carb:1.1,gord:11},
"feijão":{cal:127,prot:9,carb:23,gord:0.5},"lentilha":{cal:116,prot:9,gord:0.4,carb:20},
"banana":{cal:89,prot:1.1,carb:23,gord:0.3},"maçã":{cal:52,prot:0.3,carb:14,gord:0.2},
"arroz branco":{cal:130,prot:2.7,carb:28,gord:0.3},"arroz integral":{cal:123,prot:2.6,carb:25.6,gord:1},
"pão francês":{cal:275,prot:9,gord:3.2,carb:57}
};

const refeicoes={cafe:[],almoco:[],lanche:[],jantar:[],ceia:[]};
function converter(n,m,q){if(m==="g")return q;if(m==="cupSmall")return q*200;if(m==="cupLarge")return q*300;if(m==="unit")return 100*q;}
function addFood(){
  const n=foodName.value.toLowerCase();
  const a=bancoAlimentos[n];
  if(!a){alert("Alimento não encontrado");return;}
  const g=converter(n,measure.value,Number(foodQty.value));
  const f=g/100;
  refeicoes[meal.value].push({cal:a.cal*f,prot:a.prot*f,carb:a.carb*f,gord:a.gord*f});
  renderDiet();
}
function renderDiet(){
  let t={cal:0,prot:0,carb:0,gord:0};
  diet.innerHTML="";
  for(let r in refeicoes){
    let s={cal:0,prot:0,carb:0,gord:0};
    refeicoes[r].forEach(i=>{for(let k in s){s[k]+=i[k];t[k]+=i[k];}});
    diet.innerHTML+=`<div class='card'>${r.toUpperCase()}: 🔥${s.cal.toFixed(1)} kcal, 💪${s.prot.toFixed(1)}g, 🍞${s.carb.toFixed(1)}g, 🥑${s.gord.toFixed(1)}g</div>`;
  }
  document.getElementById("calBar").style.width=Math.min(t.cal/2000*100,100)+"%";
  document.getElementById("protBar").style.width=Math.min(t.prot/150*100,100)+"%";
  document.getElementById("carbBar").style.width=Math.min(t.carb/300*100,100)+"%";
  document.getElementById("gordBar").style.width=Math.min(t.gord/70*100,100)+"%";
}

// ===== TREINOS =====
const treinos={peito:{iniciante:["Supino Reto","Flexão de braço"],intermediario:["Supino com Halteres","Peck Deck"],avancado:["Supino Barra Pesada","Crossover"]}};
function mostrarTreino(){
  const g=grupo.value;const i=intensidade.value;listaTreino.innerHTML="";
  if(!treinos[g]||!treinos[g][i])return;
  treinos[g][i].forEach(e=>{listaTreino.innerHTML+=`<div class='exercise'>• ${e}</div>`;});
}

// ===== FICHA =====
function carregarExercicios(){exercicioFicha.innerHTML="";const g=grupoFicha.value;if(!treinos[g])return;treinos[g].iniciante.forEach((e, idx)=>exercicioFicha.innerHTML+=`<option value='${idx}'>${e}</option>`);}
function addFicha(){const g=grupoFicha.value;const idx=exercicioFicha.value;const e=treinos[g].iniciante[idx];minhaFicha.innerHTML+=`<div class='exercise'>• ${e} - ${intensidadeFicha.value} - ${carga.value}kg</div>`;}

// ===== HORÁRIOS E ALERTA =====
function salvarHorarios(){
  localStorage.setItem("horarios",JSON.stringify({
    cafe:cafeHora.value,
    almoco:almocoHora.value,
    lanche:lancheHora.value,
    jantar:jantarHora.value,
    ceia:ceiaHora.value
  }));
  alert("Horários salvos!");
}

setInterval(()=>{
  const h=new Date().toTimeString().slice(0,5);
  const s=JSON.parse(localStorage.getItem("horarios"))||{};
  for(let k in s){
    if(s[k]===h){
      document.getElementById("alerta").play();
      alert("Hora da refeição: "+k.toUpperCase());
    }
  }
},60000);
</script>

</body>
</html>
 # A.c
