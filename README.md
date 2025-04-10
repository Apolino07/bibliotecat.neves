<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Biblioteca EREM Tancredo Neves</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-color: #f5f5f5;
      --text-color: #222;
      --primary: #6200ea;
      --secondary: #2c3e50;
      --card-bg: #ffffff;
    }

    [data-theme="dark"] {
      --bg-color: #0d1b2a;
      --text-color: #e0e0e0;
      --card-bg: rgba(255,255,255,0.05);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Poppins', sans-serif;
    }

    body {
      background: var(--bg-color);
      color: var(--text-color);
      font-size: 18px;
      padding: 20px;
      min-height: 100vh;
    }

    .container {
      max-width: 960px;
      margin: 40px auto;
      background: var(--card-bg);
      padding: 30px;
      border-radius: 16px;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
    }

    h1, h2 {
      text-align: center;
      margin-bottom: 20px;
      color: var(--primary);
    }

    input, button {
      display: block;
      width: 100%;
      margin: 10px 0;
      padding: 12px;
      border-radius: 8px;
      border: none;
      font-size: 1rem;
    }

    input {
      background: #eee;
    }

    button {
      background-color: var(--primary);
      color: white;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    button:hover {
      background-color: #5c00e6;
      transform: scale(1.03);
    }

    .tabs {
      display: flex;
      justify-content: center;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }

    .tab {
      background-color: var(--secondary);
      color: white;
      padding: 10px 20px;
      border-radius: 30px;
      margin: 5px;
      cursor: pointer;
      transition: 0.3s;
    }

    .tab.active, .tab:hover {
      background-color: var(--primary);
      transform: scale(1.05);
    }

    .content-section {
      display: none;
    }

    .content-section.active {
      display: block;
    }

    footer {
      text-align: center;
      margin-top: 30px;
      font-size: 0.9rem;
      color: #888;
    }

    .toggle-theme {
      text-align: center;
      margin-top: 20px;
    }

    .toggle-theme button {
      background-color: var(--secondary);
      border-radius: 30px;
      width: auto;
      padding: 10px 20px;
    }

    .card-welcome {
      background: var(--card-bg);
      padding: 20px;
      border-left: 5px solid var(--primary);
      border-radius: 12px;
      margin-bottom: 20px;
    }
  </style>
</head>
<body data-theme="light">

<div class="container" id="loginPage">
  <h1>Bem-vindo à Biblioteca Escolar</h1>
  <input type="email" id="loginEmail" placeholder="Digite seu e-mail" required />
  <input type="password" id="loginSenha" placeholder="Digite sua senha" required />
  <button onclick="login()">Entrar</button>
  <p style="text-align:center;">Não tem uma conta? <a href="#" onclick="showRegister()">Cadastre-se</a></p>
</div>

<div class="container" id="registerPage" style="display:none;">
  <h1>Cadastro</h1>
  <input type="email" id="registerEmail" placeholder="Digite seu e-mail" required />
  <input type="password" id="registerSenha" placeholder="Crie uma senha" required />
  <button onclick="register()">Cadastrar</button>
  <p style="text-align:center;">Já tem uma conta? <a href="#" onclick="showLogin()">Entrar</a></p>
</div>

<div class="container" id="mainApp" style="display:none;">
  <div class="tabs">
    <div class="tab active" onclick="openTab(event, 'cadastro')">Cadastro de Livros</div>
    <div class="tab" onclick="openTab(event, 'lista')">Lista</div>
    <div class="tab" onclick="openTab(event, 'emprestimos')">Empréstimos</div>
    <div class="tab" onclick="openTab(event, 'perfil')">Perfil</div>
    <div class="tab" onclick="openTab(event, 'sobre')">Sobre</div>
  </div>

  <div id="cadastro" class="content-section active">
    <h2>Cadastro de Livros</h2>
    <input type="text" id="titulo" placeholder="Título do Livro" />
    <input type="text" id="autor" placeholder="Autor" />
    <button onclick="cadastrarLivro()">Cadastrar Livro</button>
  </div>

  <div id="lista" class="content-section">
    <h2>Lista de Livros</h2>
    <ul id="listaLivros"></ul>
  </div>

  <div id="emprestimos" class="content-section">
    <h2>Registrar Empréstimo</h2>
    <input type="text" id="tituloEmp" placeholder="Título do Livro" />
    <input type="text" id="emprestadoPara" placeholder="Emprestado para" />
    <input type="date" id="dataEmp" />
    <input type="date" id="dataDev" />
    <button onclick="registrarEmprestimo()">Salvar Empréstimo</button>
    <ul id="listaEmprestimos"></ul>
  </div>

  <div id="perfil" class="content-section">
    <div class="card-welcome">
      <h2>Olá, <span id="nomeUsuario"></span>!</h2>
      <p>Bem-vindo à biblioteca da EREM Presidente Tancredo Neves.</p>
    </div>
    <button onclick="logout()">Sair da Conta</button>
    <div class="toggle-theme">
      <button onclick="toggleTheme()">Alternar Tema</button>
    </div>
  </div>

  <div id="sobre" class="content-section">
    <h2>Sobre o Site</h2>
    <p>Sistema desenvolvido para a biblioteca da EREM Presidente Tancredo Neves.</p>
    <p><strong>Desenvolvido por:</strong> Apollo Aurora, Roberto Bezerra, João Vitor</p>
    <p><strong>Orientador:</strong> Professor Yuri Pascoal</p>
  </div>

  <footer>© 2025 Biblioteca EREM Tancredo Neves</footer>
</div>

<script>
  const usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];

  function showRegister() {
    document.getElementById("loginPage").style.display = "none";
    document.getElementById("registerPage").style.display = "block";
  }

  function showLogin() {
    document.getElementById("registerPage").style.display = "none";
    document.getElementById("loginPage").style.display = "block";
  }

  function login() {
    const email = document.getElementById("loginEmail").value.trim();
    const senha = document.getElementById("loginSenha").value.trim();
    const usuario = usuarios.find(u => u.email === email && u.senha === senha);
    if (usuario) {
      localStorage.setItem("usuarioLogado", JSON.stringify(usuario));
      startApp();
    } else {
      alert("Usuário ou senha inválida.");
    }
  }

  function register() {
    const email = document.getElementById("registerEmail").value.trim();
    const senha = document.getElementById("registerSenha").value.trim();
    if (usuarios.find(u => u.email === email)) {
      alert("Este e-mail já está registrado.");
    } else {
      usuarios.push({ email, senha });
      localStorage.setItem("usuarios", JSON.stringify(usuarios));
      alert("Cadastro realizado com sucesso!");
      showLogin();
    }
  }

  function startApp() {
    document.getElementById("loginPage").style.display = "none";
    document.getElementById("registerPage").style.display = "none";
    document.getElementById("mainApp").style.display = "block";
    const logado = JSON.parse(localStorage.getItem("usuarioLogado"));
    if (logado) document.getElementById("nomeUsuario").innerText = logado.email;
    carregarLivros();
    carregarEmprestimos();
  }

  function logout() {
    localStorage.removeItem("usuarioLogado");
    location.reload();
  }

  function openTab(e, id) {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.content-section').forEach(c => c.classList.remove('active'));
    e.target.classList.add('active');
    document.getElementById(id).classList.add('active');
  }

  function cadastrarLivro() {
    const titulo = document.getElementById("titulo").value.trim();
    const autor = document.getElementById("autor").value.trim();
    if (!titulo || !autor) return alert("Preencha todos os campos!");
    const livros = JSON.parse(localStorage.getItem("livros")) || [];
    livros.push({ titulo, autor });
    localStorage.setItem("livros", JSON.stringify(livros));
    carregarLivros();
    document.getElementById("titulo").value = "";
    document.getElementById("autor").value = "";
  }

  function carregarLivros() {
    const livros = JSON.parse(localStorage.getItem("livros")) || [];
    const lista = document.getElementById("listaLivros");
    lista.innerHTML = "";
    livros.forEach((livro, i) => {
      const li = document.createElement("li");
      li.textContent = `${livro.titulo} - ${livro.autor}`;
      const btn = document.createElement("button");
      btn.textContent = "Remover";
      btn.onclick = () => {
        livros.splice(i, 1);
        localStorage.setItem("livros", JSON.stringify(livros));
        carregarLivros();
      };
      li.appendChild(btn);
      lista.appendChild(li);
    });
  }

  function registrarEmprestimo() {
    const titulo = document.getElementById("tituloEmp").value.trim();
    const para = document.getElementById("emprestadoPara").value.trim();
    const dataEmp = document.getElementById("dataEmp").value;
    const dataDev = document.getElementById("dataDev").value;
    if (!titulo || !para || !dataEmp || !dataDev) return alert("Preencha todos os campos!");
    const emprestimos = JSON.parse(localStorage.getItem("emprestimos")) || [];
    emprestimos.push({ titulo, para, dataEmp, dataDev });
    localStorage.setItem("emprestimos", JSON.stringify(emprestimos));
    carregarEmprestimos();
    document.getElementById("tituloEmp").value = "";
    document.getElementById("emprestadoPara").value = "";
    document.getElementById("dataEmp").value = "";
    document.getElementById("dataDev").value = "";
  }

  function carregarEmprestimos() {
    const emprestimos = JSON.parse(localStorage.getItem("emprestimos")) || [];
    const lista = document.getElementById("listaEmprestimos");
    lista.innerHTML = "";
    emprestimos.forEach((emp, i) => {
      const li = document.createElement("li");
      li.textContent = `${emp.titulo} - Para: ${emp.para} | De: ${emp.dataEmp} até ${emp.dataDev}`;
      const btn = document.createElement("button");
      btn.textContent = "Remover";
      btn.onclick = () => {
        emprestimos.splice(i, 1);
        localStorage.setItem("emprestimos", JSON.stringify(emprestimos));
        carregarEmprestimos();
      };
      li.appendChild(btn);
      lista.appendChild(li);
    });
  }

  function toggleTheme() {
    const body = document.body;
    const current = body.getAttribute("data-theme");
    body.setAttribute("data-theme", current === "light" ? "dark" : "light");
  }

  window.onload = () => {
    const usuario = localStorage.getItem("usuarioLogado");
    if (usuario) startApp();
  };
</script>

</body>
</html>
