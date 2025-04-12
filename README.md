
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Jornal Escolar - EREM Tancredo Neves</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary: #0a2f5c;
      --background: #ffffff;
      --text: #222;
      --accent: #f0f4f8;
      --shadow: rgba(0, 0, 0, 0.1);
    }

    body {
      margin: 0;
      background: var(--background);
      font-family: 'Poppins', sans-serif;
      color: var(--text);
    }

    header {
      background: var(--primary);
      color: white;
      padding: 1rem 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 1rem;
    }

    header img {
      height: 60px;
    }

    nav {
      background: var(--accent);
      padding: 10px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .nav-actions {
      display: flex;
      align-items: center;
      gap: 15px;
    }

    .nav-actions a img {
      height: 28px;
      cursor: pointer;
    }

    main {
      max-width: 900px;
      margin: 2rem auto;
      padding: 0 1rem;
    }

    article {
      background: var(--accent);
      padding: 1.5rem;
      border-radius: 12px;
      margin-bottom: 2rem;
      box-shadow: 0 4px 12px var(--shadow);
      position: relative;
    }

    .admin, .comment-section {
      background: var(--accent);
      padding: 1rem;
      border-radius: 12px;
      box-shadow: 0 4px 12px var(--shadow);
      margin-bottom: 2rem;
    }

    .admin {
      max-width: 600px;
      margin: 2rem auto;
    }

    input, textarea, button {
      width: 100%;
      padding: 0.8rem;
      margin-top: 0.5rem;
      font-size: 1rem;
      border-radius: 8px;
      border: 1px solid #ccc;
    }

    button {
      background: var(--primary);
      color: white;
      font-weight: bold;
      border: none;
      cursor: pointer;
      transition: background 0.3s;
    }

    button:hover {
      background: #084074;
    }

    footer {
      text-align: center;
      padding: 1rem;
      font-size: 0.9rem;
      color: #555;
      background: var(--accent);
      margin-top: 4rem;
    }

    .delete-btn {
      position: absolute;
      top: 15px;
      right: 15px;
      background: red;
      padding: 6px 10px;
      border-radius: 8px;
      color: white;
      font-size: 0.8rem;
      display: none;
    }

    article:hover .delete-btn {
      display: block;
    }
  </style>
</head>
<body>

<header>
  <img src="brasao.png" alt="Brasão da Escola">
  <h1>Jornal Escolar - EREM Tancredo Neves</h1>
</header>

<nav>
  <div class="nav-actions">
    <a href="#loginPanel" title="Login do Administrador">
      <img src="gear-icon.png" alt="Login">
    </a>
    <a href="https://www.instagram.com/erempresidentet.neves?igsh=bWh6N2w0bnpsYXll" target="_blank">
      <img src="insta.png" alt="Instagram">
    </a>
  </div>
</nav>

<main id="noticias"></main>

<div class="admin" id="adminPanel" style="display:none;">
  <h2>Nova Notícia</h2>
  <input type="text" id="titulo" placeholder="Título da notícia">
  <textarea id="conteudo" rows="5" placeholder="Conteúdo da notícia"></textarea>
  <input type="file" id="imagem">
  <button onclick="adicionarNoticia()">Publicar</button>
</div>

<div class="admin" id="loginPanel">
  <h2>Login do Administrador</h2>
  <input type="password" id="senha" placeholder="Digite a senha">
  <button onclick="login()">Entrar</button>
</div>

<footer>
  © 2025 Jornal Escolar - EREM Presidente Tancredo Neves
</footer>

<script>
  const SENHA_ADMIN = "eremtneves1204";
  let isAdmin = false;

  function escapeHTML(str) {
    return str.replace(/</g, "&lt;").replace(/>/g, "&gt;");
  }

  function login() {
    const senha = document.getElementById("senha").value;
    if (senha === SENHA_ADMIN) {
      isAdmin = true;
      document.getElementById("loginPanel").style.display = "none";
      document.getElementById("adminPanel").style.display = "block";
      renderizarNoticias();
    } else {
      alert("Senha incorreta.");
    }
  }

  function adicionarNoticia() {
    const titulo = document.getElementById("titulo").value.trim();
    const conteudo = document.getElementById("conteudo").value.trim();
    const imagemInput = document.getElementById("imagem");
    const imagem = imagemInput.files[0] ? URL.createObjectURL(imagemInput.files[0]) : "";

    if (!titulo || !conteudo) return alert("Preencha todos os campos!");

    const noticia = { titulo, conteudo, imagem, comentarios: [] };
    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];
    noticias.unshift(noticia);
    localStorage.setItem("noticias", JSON.stringify(noticias));
    renderizarNoticias();
    document.getElementById("titulo").value = "";
    document.getElementById("conteudo").value = "";
    imagemInput.value = "";
  }

  function adicionarComentario(index, inputId) {
    const comentario = escapeHTML(document.getElementById(inputId).value.trim());
    if (!comentario) return;
    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];
    noticias[index].comentarios.push(comentario);
    localStorage.setItem("noticias", JSON.stringify(noticias));
    renderizarNoticias();
  }

  function removerComentario(noticiaIndex, comentarioIndex) {
    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];
    noticias[noticiaIndex].comentarios.splice(comentarioIndex, 1);
    localStorage.setItem("noticias", JSON.stringify(noticias));
    renderizarNoticias();
  }

  function removerNoticia(index) {
    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];
    noticias.splice(index, 1);
    localStorage.setItem("noticias", JSON.stringify(noticias));
    renderizarNoticias();
  }

  function renderizarNoticias() {
    const container = document.getElementById("noticias");
    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];
    container.innerHTML = noticias.map((n, i) => `
      <article>
        ${isAdmin ? `<button class="delete-btn" onclick="removerNoticia(${i})">Excluir Notícia</button>` : ''}
        <h2>${escapeHTML(n.titulo)}</h2>
        <p>${escapeHTML(n.conteudo)}</p>
        ${n.imagem ? `<img src="${n.imagem}" style="max-width:100%;border-radius:8px;margin-top:10px;">` : ''}
        <div class="comment-section">
          <h4>Comentários</h4>
          <ul>
            ${n.comentarios.map((c, j) => `
              <li>
                ${c}
                ${isAdmin ? `<button onclick="removerComentario(${i}, ${j})" style="margin-left:10px;padding:4px 8px;font-size:0.8rem;background:red;color:white;">Excluir</button>` : ''}
              </li>`).join('')}
          </ul>
          <input type="text" id="comentario-${i}" placeholder="Deixe um comentário...">
          <button onclick="adicionarComentario(${i}, 'comentario-${i}')">Comentar</button>
        </div>
      </article>
    `).join('');
  }

  window.onload = () => {
    renderizarNoticias();
  };
</script>

</body>
</html>

