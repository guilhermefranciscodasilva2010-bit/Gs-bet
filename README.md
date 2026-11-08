
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GS BET</title>
<style>
* { box-sizing: border-box; }
body {
  margin: 0;
  background: #10151d;
  color: white;
  font-family: Arial, sans-serif;
}
header {
  background: #080c12;
  padding: 22px;
  text-align: center;
}
header h1 { color: #00df83; margin: 0; font-size: 34px; }
nav {
  background: #19212c;
  padding: 15px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 18px;
}
nav a { color: white; text-decoration: none; }
main { max-width: 850px; margin: auto; padding: 18px; }
section, .match {
  background: #202a37;
  padding: 18px;
  margin: 16px 0;
  border-radius: 12px;
}
.banner { background: #073d30; }
.banner h2 { color: #00df83; }
button {
  background: #00df83;
  color: #07130e;
  border: 0;
  padding: 12px 18px;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
}
input {
  display: block;
  width: 100%;
  padding: 12px;
  margin: 8px 0 15px;
  border: 1px solid #526174;
  border-radius: 7px;
  background: #111a25;
  color: white;
  font-size: 16px;
}
label { color: #dce5ef; }
.notice { color: #ffd98a; font-size: 13px; line-height: 1.5; }
#painel { display: none; }
footer { text-align: center; color: #aaa; padding: 25px; }
</style>
</head>
<body>

<header>
  <h1>GS BET</h1>
  <p>Seu espaço de esportes</p>
</header>

<nav>
  <a href="#inicio">Início</a>
  <a href="#jogos">Jogos</a>
  <a href="#cadastro">Cadastro</a>
  <a href="#login">Entrar</a>
  <a href="#painel" onclick="mostrarPainel()">Painel</a>
</nav>

<main>
  <section class="banner" id="inicio">
    <h2>Bem-vindo à GS BET!</h2>
    <p>Acompanhe partidas esportivas demonstrativas.</p>
  </section>

  <h2 id="jogos">⚽ Partidas</h2>

  <div class="match">
    <h3>Flamengo x Palmeiras</h3>
    <p>Placar demonstrativo: 0 x 0</p>
    <button onclick="alert('Partida de demonstração!')">
      Ver partida
    </button>
  </div>

  <div class="match">
    <h3>Barcelona x Real Madrid</h3>
    <p>Placar demonstrativo: 0 x 0</p>
    <button onclick="alert('Partida de demonstração!')">
      Ver partida
    </button>
  </div>

  <div class="match">
    <h3>Manchester City x Liverpool</h3>
    <p>Placar demonstrativo: 0 x 0</p>
    <button onclick="alert('Partida de demonstração!')">
      Ver partida
    </button>
  </div>

  <section id="cadastro">
    <h2>👤 Criar cadastro</h2>
    <form id="formCadastro">
      <label for="nome">Nome</label>
      <input id="nome" required maxlength="60" placeholder="Seu nome">

      <label for="email">E-mail</label>
      <input id="email" type="email" required placeholder="seuemail@exemplo.com">

      <label for="senha">Senha de teste</label>
      <input id="senha" type="password" minlength="6" required
        placeholder="Mínimo de 6 caracteres">

      <button type="submit">Cadastrar</button>
    </form>
    <p class="notice">
      Demonstração: a senha não é salva. Nome e e-mail ficam somente
      no armazenamento deste navegador.
    </p>
    <p id="mensagemCadastro" aria-live="polite"></p>
  </section>

  <section id="login">
    <h2>🔐 Entrar</h2>
    <form id="formLogin">
      <label for="emailLogin">E-mail cadastrado</label>
      <input id="emailLogin" type="email" required placeholder="seuemail@exemplo.com">
      <button type="submit">Entrar</button>
    </form>
    <p id="mensagemLogin" aria-live="polite"></p>
  </section>

  <section id="painel">
    <h2>📋 Painel administrativo demonstrativo</h2>
    <p class="notice">
      Este painel mostra apenas os cadastros salvos neste navegador.
      Não é privado e não recebe os cadastros de outros visitantes.
    </p>
    <p id="totalUsuarios">Cadastros: 0</p>
    <ul id="listaUsuarios"></ul>
    <button onclick="apagarCadastros()">Apagar cadastros locais</button>
  </section>

  <section>
    <h2>Sobre a GS BET</h2>
    <p>Projeto demonstrativo para acompanhar esportes.</p>
    <p class="notice">
      Não aceita apostas, depósitos, saques ou pagamentos.
      Não use senhas reais ou dados pessoais sensíveis.
    </p>
  </section>
</main>

<footer>
  GS BET © 2026 — Versão demonstrativa
</footer>

<script>
const CHAVE = "gsbet_cadastros_demo";

function obterUsuarios() {
  try {
    return JSON.parse(localStorage.getItem(CHAVE) || "[]");
  } catch {
    return [];
  }
}

function atualizarPainel() {
  const usuarios = obterUsuarios();
  document.getElementById("totalUsuarios").textContent =
    "Cadastros neste navegador: " + usuarios.length;

  const lista = document.getElementById("listaUsuarios");
  lista.replaceChildren();

  usuarios.forEach(usuario => {
    const item = document.createElement("li");
    item.textContent = usuario.nome + " — " + usuario.email;
    lista.appendChild(item);
  });
}

document.getElementById("formCadastro").addEventListener("submit", function(event) {
  event.preventDefault();

  const nome = document.getElementById("nome").value.trim();
  const email = document.getElementById("email").value.trim().toLowerCase();
  const senha = document.getElementById("senha").value;
  const mensagem = document.getElementById("mensagemCadastro");

  if (senha.length < 6) {
    mensagem.textContent = "A senha de teste precisa ter 6 caracteres.";
    return;
  }

  const usuarios = obterUsuarios();

  if (usuarios.some(usuario => usuario.email === email)) {
    mensagem.textContent = "Este e-mail já está cadastrado neste navegador.";
    return;
  }

  usuarios.push({
    nome: nome,
    email: email
  });

  localStorage.setItem(CHAVE, JSON.stringify(usuarios));

  mensagem.textContent = "Cadastro demonstrativo realizado!";
  this.reset();
  atualizarPainel();
});

document.getElementById("formLogin").addEventListener("submit", function(event) {
  event.preventDefault();

  const email = document.getElementById("emailLogin").value.trim().toLowerCase();
  const usuario = obterUsuarios().find(u => u.email === email);

  document.getElementById("mensagemLogin").textContent = usuario
    ? "Olá, " + usuario.nome + "! Você entrou na demonstração."
    : "Cadastro não encontrado neste navegador.";
});

function mostrarPainel() {
  const painel = document.getElementById("painel");
  painel.style.display = "block";
  atualizarPainel();
}

function apagarCadastros() {
  if (confirm("Deseja apagar os cadastros deste navegador?")) {
    localStorage.removeItem(CHAVE);
    atualizarPainel();
  }
}

atualizarPainel();
</script>

</body>
</html>
