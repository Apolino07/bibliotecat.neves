<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Jornal Escolar Futurista - EREM Tancredo Neves</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary: #0A2F5C;
      --background: #f7f9fc;
      --text: #1a1a1a;
      --accent: #e1e9f0;
      --shadow: rgba(0, 0, 0, 0.1);
      --category-educacao: #4CAF50;
      --category-eventos: #FF9800;
      --category-esportes: #2196F3;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      background: var(--background);
      font-family: 'Poppins', sans-serif;
      color: var(--text);
      transition: background 0.3s, color 0.3s;
    }

    header {
      background: var(--primary);
      color: white;
      padding: 1rem 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 1rem;
      box-shadow: 0 4px 12px var(--shadow);
    }

    .logo-redonda {
      height: 70px;
      width: 70px;
      object-fit: cover;
      border-radius: 50%;
      border: 2px solid white;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    }

    nav {
      background: var(--accent);
      padding: 10px 2rem;
      display: flex;
      justify-content: flex-end;
      gap: 15px;
    }

    .nav-actions img {
      height: 32px;
      cursor: pointer;
      transition: transform 0.3s;
    }

    .nav-actions img:hover {
      transform: scale(1.1);
    }

    main {
      max-width: 1000px;
      margin: 2rem auto;
      padding: 0 1rem;
    }

    .slider {
      overflow: hidden;
      border-radius: 16px;
      box-shadow: 0 4px 20px var(--shadow);
      margin-bottom: 2rem;
      background: white;
    }

    article {
      background: var(--accent);
      padding: 1.5rem;
      border-radius: 16px;
      margin-bottom: 2rem;
      box-shadow: 0 4px 12px var(--shadow);
      position: relative;
    }

    .admin, .comment-section {
      background: white;
      padding: 1rem;
      border-radius: 16px;
      box-shadow: 0 4px 12px var(--shadow);
      margin-bottom: 2rem;
    }

    .admin {
      max-width: 600px;
      margin: 2rem auto;
    }

    input, textarea, select, button {
      width: 100%;
      padding: 0.8rem;
      margin-top: 0.5rem;
      font-size: 1rem;
      border-radius: 12px;
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
      background: #08335f;
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

    .categoria-tag {
      display: inline-block;
      padding: 4px 10px;
      border-radius: 12px;
      font-size: 0.75rem;
      color: white;
      margin-bottom: 8px;
    }

    .educacao { background: var(--category-educacao); }
    .eventos  { background: var(--category-eventos); }
    .esportes { background: var(--category-esportes); }
  </style>
</head>
<body>

<header>
  <img src="brasao.png" alt="Brasão da Escola" class="logo-redonda">
  <h1>Jornal Escolar Futurista - EREM Tancredo Neves</h1>
</header>

<nav class="nav-actions">
  <a href="#loginPanel" title="Login do Administrador">
    <img src="user-gear.png" alt="Login">
  </a>
  <a href="https://www.instagram.com/erempresidentet.neves" target="_blank">
    <img src="instagram-icon.png" alt="Instagram">
  </a>
</nav>

<main>
  <div class="slider" id="sliderDestaques"></div>
  <div id="noticias"></div>
</main>

<div class="admin" id="adminPanel" style="display:none;">
  <h2>Nova Notícia</h2>
  <input type="text" id="titulo" placeholder="Título da notícia">
  <textarea id="conteudo" rows="5" placeholder="Conteúdo da notícia"></textarea>
  <select id="categoria">
    <option value="">Escolha uma categoria</option>
    <option value="educacao">Educação</option>
    <option value="eventos">Eventos</option>
    <option value="esportes">Esportes</option>
  </select>
  <input type="file" id="imagem">
  <button onclick="adicionarNoticia()">Publicar</button>
</div>

<div class="admin" id="loginPanel">
  <h2>Login do Administrador</h2>
  <input type="password" id="senha" placeholder="Digite a senha">
  <button onclick="login()">Entrar</button>
</div>

<footer>
  © 2025 Jornal Escolar - EREM Presidente Tancredo Neves | Desenvolvido por Apolino07
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
    const categoria = document.getElementById("categoria").value;
    const imagemInput = document.getElementById("imagem");
    const imagem = imagemInput.files[0] ? URL.createObjectURL(imagemInput.files[0]) : "";

    if (!titulo || !conteudo || !categoria) return alert("Preencha todos os campos!");

    const noticia = {
      titulo,
      conteudo,
      imagem,
      categoria,
      comentarios: []
    };

    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];
    noticias.unshift(noticia);
    localStorage.setItem("noticias", JSON.stringify(noticias));
    renderizarNoticias();
    document.getElementById("titulo").value = "";
    document.getElementById("conteudo").value = "";
    document.getElementById("categoria").value = "";
    imagemInput.value = "";
  }

  function adicionarComentario(index, inputId) {
    const input = document.getElementById(inputId);
    const comentario = escapeHTML(input.value.trim());
    if (!comentario) return;
    const nomeUsuario = prompt("Digite seu nome de usuário:") || "Anônimo";
    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];
    noticias[index].comentarios.push(`${nomeUsuario}: ${comentario}`);
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
    const slider = document.getElementById("sliderDestaques");
    const noticias = JSON.parse(localStorage.getItem("noticias")) || [];

    container.innerHTML = noticias.map((n, i) => `
      <article>
        ${isAdmin ? `<button class="delete-btn" onclick="removerNoticia(${i})">Excluir Notícia</button>` : ''}
        <span class="categoria-tag ${n.categoria}">${n.categoria.charAt(0).toUpperCase() + n.categoria.slice(1)}</span>
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

    slider.innerHTML = noticias.slice(0, 3).map(n => `
      <div style="padding:1rem;background:#fff;">
        <strong style="color:#0A2F5C;">${escapeHTML(n.titulo)}</strong>
        <p>${escapeHTML(n.conteudo.slice(0, 100))}...</p>
      </div>
    `).join('');
  }

  window.onload = () => {
    renderizarNoticias();
  };
</script>

</body>
</html>
