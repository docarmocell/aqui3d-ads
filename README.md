# aqui5d-ads

<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>AQUI3D ADS</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f2f4f8;color:#17233c}
header{background:#101d36;color:white;padding:24px 18px}
header h1{margin:0;color:#ff9b36;font-size:27px}
header p{margin:7px 0 0}
nav{display:flex;gap:8px;flex-wrap:wrap;padding:14px;background:white}
button{border:0;border-radius:8px;padding:12px 15px;background:#ff9b36;color:#101d36;font-weight:bold;cursor:pointer}
main{padding:18px;max-width:950px;margin:auto}
h2{color:#101d36}
.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(145px,1fr));gap:12px}
.card,.panel{background:white;padding:18px;border-radius:12px;margin-bottom:15px;box-shadow:0 2px 9px #0000000d}
.number{font-size:26px;font-weight:bold;color:#f28b25}
input,select,textarea{width:100%;padding:12px;margin:7px 0 14px;border:1px solid #ccd2dc;border-radius:7px;font-size:15px}
label{font-weight:bold}
section{display:none}
section.active{display:block}
small{color:#667085}
footer{text-align:center;padding:25px;color:#667085}
</style>
</head>
<body>
<header>
<h1>AQUI3D ADS</h1>
<p>Publicidade digital em todos os lugares.</p>
</header>

<nav>
<button onclick="abrir('inicio')">Painel</button>
<button onclick="abrir('campanhas')">Campanhas</button>
<button onclick="abrir('espacos')">Espaços parceiros</button>
<button onclick="abrir('sobre')">Sobre</button>
</nav>

<main>
<section id="inicio" class="active">
<h2>Painel principal</h2>
<div class="cards">
<div class="card">Campanhas cadastradas<div class="number" id="totalCampanhas">0</div></div>
<div class="card">Espaços parceiros<div class="number" id="totalEspacos">0</div></div>
<div class="card">Plataforma<strong class="number">Ativa</strong></div>
</div>
<div class="panel">
<h3>Bem-vindo ao AQUI3D ADS!</h3>
<p>Uma plataforma para conectar anunciantes e parceiros de publicidade.</p>
<p><small>Esta é uma versão inicial demonstrativa.</small></p>
</div>
</section>

<section id="campanhas">
<h2>Cadastrar campanha</h2>
<div class="panel">
<form id="formCampanha">
<label>Nome da campanha</label>
<input id="nomeCampanha" required placeholder="Ex.: Promoção de inauguração">
<label>Nome do anunciante</label>
<input id="anunciante" required placeholder="Nome da empresa">
<label>Descrição do anúncio</label>
<textarea id="descricao" required placeholder="Descreva sua publicidade"></textarea>
<label>Orçamento (R$)</label>
<input id="orcamento" type="number" min="1" step="0.01" required placeholder="100,00">
<button type="submit">Cadastrar campanha</button>
</form>
</div>
<h3>Campanhas cadastradas</h3>
<div id="listaCampanhas"></div>
</section>

<section id="espacos">
<h2>Cadastro de espaço parceiro</h2>
<div class="panel">
<form id="formEspaco">
<label>Nome do parceiro</label>
<input id="nomeParceiro" required placeholder="Nome da empresa ou parceiro">
<label>Tipo de espaço</label>
<select id="tipoEspaco">
<option>TV ou painel digital</option>
<option>Site ou aplicativo</option>
<option>Comércio ou estabelecimento</option>
<option>Outro</option>
</select>
<label>Cidade</label>
<input id="cidade" required placeholder="Sua cidade">
<button type="submit">Cadastrar espaço</button>
</form>
</div>
<h3>Espaços cadastrados</h3>
<div id="listaEspacos"></div>
</section>

<section id="sobre">
<h2>Sobre o AQUI3D ADS</h2>
<div class="panel">
<p>O AQUI3D ADS tem como objetivo aproximar empresas que desejam anunciar de parceiros que possuem espaços publicitários.</p>
<p><strong>Visão:</strong> conectar publicidade, tecnologia e oportunidades.</p>
<p><small>Cadastro demonstrativo: os dados ficam salvos somente neste navegador. Ainda não há contas reais, pagamentos ou distribuição automática de anúncios.</small></p>
</div>
</section>
</main>

<footer>AQUI3D ADS © 2026</footer>

<script>
let campanhas=JSON.parse(localStorage.getItem('campanhasAQUI3D')||'[]');
let espacos=JSON.parse(localStorage.getItem('espacosAQUI3D')||'[]');

function abrir(id){
document.querySelectorAll('main section').forEach(s=>s.classList.remove('active'));
document.getElementById(id).classList.add('active');
atualizar();
}
function atualizar(){
document.getElementById('totalCampanhas').textContent=campanhas.length;
document.getElementById('totalEspacos').textContent=espacos.length;
document.getElementById('listaCampanhas').innerHTML=campanhas.map(c=>`<div class="card"><strong>${esc(c.nome)}</strong><p>${esc(c.anunciante)}</p><p>${esc(c.descricao)}</p><b>R$ ${Number(c.orcamento).toFixed(2)}</b></div>`).join('')||'<p>Nenhuma campanha cadastrada.</p>';
document.getElementById('listaEspacos').innerHTML=espacos.map(e=>`<div class="card"><strong>${esc(e.nome)}</strong><p>${esc(e.tipo)} — ${esc(e.cidade)}</p></div>`).join('')||'<p>Nenhum espaço cadastrado.</p>';
}
function esc(v){return String(v).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));}

document.getElementById('formCampanha').addEventListener('submit',e=>{
e.preventDefault();
campanhas.push({
nome:nomeCampanha.value,
anunciante:anunciante.value,
descricao:descricao.value,
orcamento:orcamento.value
});
localStorage.setItem('campanhasAQUI3D',JSON.stringify(campanhas));
e.target.reset();atualizar();alert('Campanha cadastrada neste navegador!');
});
document.getElementById('formEspaco').addEventListener('submit',e=>{
e.preventDefault();
espacos.push({nome:nomeParceiro.value,tipo:tipoEspaco.value,cidade:cidade.value});
localStorage.setItem('espacosAQUI3D',JSON.stringify(espacos));
e.target.reset();atualizar();alert('Espaço cadastrado neste navegador!');
});
atualizar();
</script>
</body>
</html>
