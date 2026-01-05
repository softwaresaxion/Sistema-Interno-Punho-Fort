<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sistema Interno Corporativo</title>

<!-- ICONES -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<!-- CHART -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root{
  --verde:#2ecc71;
  --azul:#3498db;
  --amarelo:#f1c40f;
  --vermelho:#e74c3c;
  --laranja:#e67e22;
  --fundo:#f4f6f9;
  --texto:#2c3e50;
}

body{
  margin:0;
  font-family:Segoe UI, Arial, sans-serif;
  background:var(--fundo);
  color:var(--texto);
}

header{
  background:#fff;
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:15px 30px;
  box-shadow:0 2px 6px rgba(0,0,0,0.1);
}

header h1{
  font-size:20px;
  margin:0;
}

main{
  padding:30px;
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
  gap:20px;
}

.card{
  background:#fff;
  border-radius:12px;
  padding:20px;
  box-shadow:0 4px 12px rgba(0,0,0,0.08);
}

.card h3{
  margin-top:0;
}

.icon-btn{
  width:70px;
  height:70px;
  border-radius:50%;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:26px;
  cursor:pointer;
  background:transparent;
  border:2px solid currentColor;
  transition:0.3s;
}

.icon-btn:hover{
  transform:scale(1.1);
}

.menu{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(120px,1fr));
  gap:30px;
  justify-items:center;
}

.menu-item{
  text-align:center;
}

.voltar{
  margin-bottom:20px;
}

.hidden{display:none;}

/* CORES */
.verde{color:var(--verde);}
.azul{color:var(--azul);}
.amarelo{color:var(--amarelo);}
.vermelho{color:var(--vermelho);}
.laranja{color:var(--laranja);}

  @media print {
  body * {
    visibility: hidden;
  }

  #printArea, #printArea * {
    visibility: visible;
  }

  #printArea {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    padding: 30px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 30px;
  }

  th, td {
    border: 1px solid #333;
    padding: 8px;
    text-align: left;
  }

  th {
    background: #f0f0f0;
  }

  h2 {
    margin-top: 40px;
  }
}

  .ficha-card{
  position: relative;
  max-width: 800px;
  margin: auto;
  padding: 30px;
}

.btn-fechar{
  position: absolute;
  top: 15px;
  right: 15px;
  background: transparent;
  border: none;
  font-size: 22px;
  cursor: pointer;
  color: var(--vermelho);
  transition: 0.3s;
}

.btn-fechar:hover{
  transform: scale(1.2);
}
.top-actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

  
</style>
</head>
<body>

<header>
  <h1>Sistema Interno Corporativo</h1>
  <div id="top-actions"></div>
</header>

<main>

<!-- DASHBOARD -->
<section id="home">
  <div class="menu">
    <div class="menu-item">
      <div class="icon-btn verde" onclick="show('dashboard')"><i class="fa-solid fa-chart-line"></i></div>
      <p>Dashboard</p>
    </div>
    <div class="menu-item">
      <div class="icon-btn azul" onclick="show('funcionarios')"><i class="fa-solid fa-users"></i></div>
      <p>Funcionários</p>
    </div>
    <div class="menu-item">
      <div class="icon-btn amarelo" onclick="show('financeiro')"><i class="fa-solid fa-coins"></i></div>
      <p>Financeiro</p>
    </div>
    <div class="menu-item">
      <div class="icon-btn laranja" onclick="show('planner')"><i class="fa-solid fa-calendar-days"></i></div>
      <p>Planner</p>
    </div>
    <div class="menu-item">
      <div class="icon-btn vermelho" onclick="show('documentos')"><i class="fa-solid fa-folder-open"></i></div>
      <p>Documentos</p>
    </div>
  </div>
</section>

<!-- DASHBOARD -->
<section id="dashboard" class="hidden">

  <div class="top-actions">
    <button class="icon-btn azul voltar" onclick="voltar()">
      <i class="fa-solid fa-house"></i>
    </button>

    <button class="icon-btn azul" onclick="recarregarDashboard()">
      <i class="fa-solid fa-rotate"></i>
    </button>
  </div>

  <div class="grid">
 
  <div class="grid">
<div class="card">
  <h3>Funcionários Ativos</h3>
  <h1 id="totalFuncionarios">0</h1>
</div>
    <div class="card"><h3>Faturamento Mensal</h3><h1>R$ 1.250.000</h1></div>
    <div class="card"><h3>Contratos Ativos</h3><h1>46</h1></div>
  </div>
  <div class="card" style="margin-top:30px">
    <canvas id="grafico"></canvas>
  </div>
</section>

<!-- FUNCIONARIOS -->
<section id="funcionarios" class="hidden">
  <button class="icon-btn verde voltar" onclick="voltar()"><i class="fa-solid fa-house"></i></button>

  <!-- BARRA DE PESQUISA -->
<div class="card" style="display:flex; gap:15px; align-items:center; margin-bottom:20px;">
  <input id="search" type="text" placeholder="Pesquisar funcionário..." style="flex:1; padding:12px; border-radius:8px; border:1px solid #ccc" onkeyup="filtrar()">

  <div class="icon-btn verde" onclick="imprimirFuncionarios()" title="Imprimir relação">
    <i class="fa-solid fa-print"></i>
  </div>

  <div class="icon-btn azul" onclick="abrirForm()" title="Novo funcionário">
    <i class="fa-solid fa-user-plus"></i>
  </div>
</div>

  <!-- GALERIA -->
  <div id="galeria" class="grid"></div>
</section>

<!-- FINANCEIRO -->
<section id="financeiro" class="hidden">
  <button class="icon-btn amarelo voltar" onclick="voltar()"><i class="fa-solid fa-house"></i></button>
  <div class="card">
    <h3>Relatórios Financeiros Personalizados</h3>
    <p>Fluxo de caixa, contratos, centros de custo.</p>
  </div>
</section>

<!-- PLANNER -->
<section id="planner" class="hidden">
  <button class="icon-btn laranja voltar" onclick="voltar()"><i class="fa-solid fa-house"></i></button>
  <div class="card">
    <h3>Planner Corporativo</h3>
    <p>Escalas, atividades, prazos e projetos.</p>
  </div>
</section>

<!-- DOCUMENTOS -->
<section id="documentos" class="hidden">
  <button class="icon-btn vermelho voltar" onclick="voltar()"><i class="fa-solid fa-house"></i></button>
  <div class="card">
    <h3>Documentação Digital</h3>
    <p>Contratos, políticas internas, arquivos e uploads.</p>
  </div>
</section>

<!-- FICHA DO FUNCIONÁRIO -->
<section id="ficha" class="hidden">
  <button class="icon-btn azul voltar" onclick="voltar()">
    <i class="fa-solid fa-house"></i>
  </button>

  <div class="card ficha-card">
    
    <!-- BOTÃO FECHAR -->
    <button class="btn-fechar" onclick="fecharFicha()">
      <i class="fa-solid fa-xmark"></i>
    </button>

    <div style="display:grid;grid-template-columns:200px 1fr;gap:30px">
      <img id="fichaFoto" style="width:200px;height:200px;border-radius:12px;object-fit:cover">
      <div id="fichaDados">
    </div>

    <div id="fichaDados"></div>
  </div>
</section>

</main>

  <script>
function fecharFicha(){
  show('funcionarios');
}
</script>

<script>
function atualizarTotalFuncionarios() {
  const funcionarios = JSON.parse(localStorage.getItem('funcionarios')) || [];
  document.getElementById('totalFuncionarios').innerText = funcionarios.length;
}
</script>

  <script>
function imprimirFuncionarios(){
  if(funcionarios.length === 0){
    alert('Nenhum funcionário cadastrado.');
    return;
  }

  // Agrupa por departamento
  const departamentos = {};
  funcionarios.forEach(f => {
    const dep = f.departamento || 'Sem Departamento';
    if(!departamentos[dep]) departamentos[dep] = [];
    departamentos[dep].push(f);
  });

  let html = `<h1>Relação de Funcionários</h1>`;

  Object.keys(departamentos).sort().forEach(dep => {
    html += `
      <h2>Departamento: ${dep}</h2>
      <table>
        <thead>
          <tr>
            <th>Nome</th>
            <th>Data de Admissão</th>
            <th>CPF</th>
            <th>Telefone</th>
          </tr>
        </thead>
        <tbody>
    `;

    departamentos[dep].forEach(f => {
      html += `
        <tr>
          <td>${f.nome}</td>
          <td>${f.admissao || '-'}</td>
          <td>${f.cpf || '-'}</td>
          <td>${f.telefone || '-'}</td>
        </tr>
      `;
    });

    html += `
        </tbody>
      </table>
    `;
  });

  const area = document.getElementById('printArea');
  area.innerHTML = html;
  area.classList.remove('hidden');

  window.print();

  area.classList.add('hidden');
}
    
</script>

<script>
function recarregarDashboard() {
  localStorage.setItem('secaoAtiva', 'dashboard');
  location.reload();
}
</script>
<script>
document.addEventListener('DOMContentLoaded', () => {
  const secao = localStorage.getItem('secaoAtiva') || 'home';
  show(secao);
});
</script>

<script>
function show(id){
  document.querySelectorAll('section').forEach(s=>s.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
function voltar(){show('home')}

// ===== FUNCIONÁRIOS =====
let funcionarios = [];

function abrirForm(){
  document.getElementById('modal').style.display = 'flex';
}

function fecharForm(){
  document.getElementById('modal').style.display = 'none';
}

function adicionarFuncionario(){
  const funcionario = {
    nome: document.getElementById('nome').value,
    cpf: document.getElementById('cpf').value,
    email: document.getElementById('email').value,
    telefone: document.getElementById('telefone').value,
    cargo: document.getElementById('cargo').value,
    departamento: document.getElementById('departamento').value,
    unidade: document.getElementById('unidade').value,
    admissao: document.getElementById('admissao').value,
    foto: document.getElementById('foto').value || 'https://via.placeholder.com/120'
  };

  if(!funcionario.nome || !funcionario.departamento){
    alert('Preencha ao menos nome e departamento');
    return;
  }

  funcionarios.push(funcionario);

  salvarFuncionarios();   // 💾 SALVA NO LOCALSTORAGE
  renderGaleria();

  limparFormularioFuncionario();
  fecharForm();
    atualizarTotalFuncionarios();
}


function renderGaleria(){
  const galeria = document.getElementById('galeria');
  galeria.innerHTML = '';
  funcionarios.forEach((f, index) => {
    galeria.innerHTML += `
      <div class="card" style="display:flex; align-items:center; gap:15px; cursor:pointer" onclick="abrirFicha(${index})">
        <img src="${f.foto}" style="width:60px;height:60px;border-radius:50%">
        <div>
          <strong>${f.nome}</strong><br>
          <small>${f.departamento}</small>
        </div>
      </div>`;
  });
}

function filtrar(){
  const termo = document.getElementById('search').value.toLowerCase();
  document.querySelectorAll('#galeria .card').forEach(card=>{
    card.style.display = card.innerText.toLowerCase().includes(termo) ? 'flex':'none';
  });
}

// ===== FICHA DO FUNCIONÁRIO =====
function abrirFicha(index){
  const f = funcionarios[index];

  document.getElementById('fichaFoto').src = f.foto;

  document.getElementById('fichaDados').innerHTML = `
    <h2>${f.nome}</h2>
    <p><strong>CPF:</strong> ${f.cpf || '-'}</p>
    <p><strong>E-mail:</strong> ${f.email || '-'}</p>
    <p><strong>Telefone:</strong> ${f.telefone || '-'}</p>
    <p><strong>Cargo:</strong> ${f.cargo || '-'}</p>
    <p><strong>Departamento:</strong> ${f.departamento || '-'}</p>
    <p><strong>Unidade:</strong> ${f.unidade || '-'}</p>
    <p><strong>Data de admissão:</strong> ${f.admissao || '-'}</p>
  `;

  show('ficha');
}

// ===== DASHBOARD =====
const ctx=document.getElementById('grafico');
if(ctx){
new Chart(ctx,{type:'bar',data:{labels:['Jan','Fev','Mar','Abr','Mai','Jun'],datasets:[{data:[120,150,180,170,200,230]}]}});
}

function salvarFuncionarios(){
  localStorage.setItem('funcionarios', JSON.stringify(funcionarios));
}

function carregarFuncionarios(){
  const dados = localStorage.getItem('funcionarios');
  if(dados){
    funcionarios = JSON.parse(dados);
    renderGaleria();
  }
}

  carregarFuncionarios();
document.addEventListener('DOMContentLoaded', atualizarTotalFuncionarios);

</script>

<!-- MODAL FORM -->
<div id="modal" style="display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.6);align-items:center;justify-content:center;">
  <div class="card" style="width:320px">
    <h3>Novo Funcionário</h3>
    <input id="nome" placeholder="Nome completo" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="cpf" placeholder="CPF" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="email" placeholder="E-mail corporativo" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="telefone" placeholder="Telefone" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="cargo" placeholder="Cargo" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="departamento" placeholder="Departamento" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="unidade" placeholder="Unidade / Local de trabalho" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="admissao" type="date" placeholder="Data de admissão" style="width:100%;padding:10px;margin-bottom:10px">
    <input id="foto" placeholder="URL da foto do funcionário" style="width:100%;padding:10px;margin-bottom:10px">
    <div style="display:flex;gap:10px;justify-content:flex-end">
      <button onclick="fecharForm()">Cancelar</button>
      <button onclick="adicionarFuncionario()">Salvar</button>
    </div>
  </div>
</div>
<div id="printArea" class="hidden"></div>

</body>
</html>
