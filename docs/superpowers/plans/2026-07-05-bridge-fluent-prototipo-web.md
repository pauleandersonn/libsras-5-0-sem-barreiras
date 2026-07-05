# Bridge Fluent / Libras 5.0 — Plano de Implementação do Protótipo

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Construir um protótipo web single-file funcional do app **Bridge Fluent** (Libras 5.0) com 4 features navegáveis, demonstrando tradução voz→texto em tempo real, dicionário de sinais, aulas acessíveis com quiz e home com atalhos.

**Architecture:** SPA mobile-first num único arquivo `index.html` (HTML5 + CSS3 + JS vanilla embutidos). Roteamento hash-based (`#/`, `#/tradutor`, `#/dicionario`, `#/aulas`). Estado global em objeto JS + persistência via `localStorage`. Web Speech API nativa do navegador para a única feature de IA real (tradutor). Sem build, sem CDN, sem backend — abre direto no navegador.

**Tech Stack:** HTML5, CSS3 (variáveis + media queries), JavaScript ES2020+ (vanilla, sem framework), Web Speech API (`SpeechRecognition`), `localStorage`.

**Localização do projeto:** `C:\Users\paule\Documents\PROGRAMAÇÃO\Libras 5.0 Sem Barreiras Projeto App\`

## Global Constraints

- **Single-file:** Todo código (HTML, CSS, JS) vive em `index.html` na raiz do projeto.
- **Sem dependências externas:** Nenhum CDN, nenhum npm install, nenhum `<script src="https://...">`. Tudo inline.
- **Sem backend:** Não há servidor, não há fetch para APIs externas (exceto Web Speech API que é nativa).
- **Mobile-first:** Layout base em 375px; breakpoints em 768px (tablet) e 1024px (desktop).
- **Tema:** Dark por padrão (`#0F2A47` background, `#F59E0B` accent), toggle pra light salva em localStorage.
- **Acessibilidade mínima:** Contraste AA (4.5:1), HTML semântico, `aria-label` em ícones, foco visível.
- **Idiomas:** Interface em PT-BR. Conteúdo das aulas em PT-BR.
- **Roteamento:** Hash-based (`#/rota`), browser back/forward deve funcionar.
- **Encoding:** UTF-8 sem BOM.
- **Convenção de nomes de arquivos:** kebab-case pra nomes compostos (ex: `index.html`). Variáveis e funções JS em camelCase.
- **Convenção de commits:** Conventional Commits em português (`feat:`, `fix:`, `style:`, `docs:`, `chore:`).

---

## File Structure

```
Libras 5.0 Sem Barreiras Projeto App/
├── index.html                          ← arquivo único do protótipo (criado na Task 1)
├── docs/
│   └── superpowers/
│       ├── specs/
│       │   └── 2026-07-05-bridge-fluent-prototipo-web-design.md  (existente)
│       └── plans/
│           └── 2026-07-05-bridge-fluent-prototipo-web.md  (este arquivo)
└── README.md                            ← instruções de uso (criado na Task 12)
```

**Estrutura interna do `index.html` (seções delimitadas por comentários):**

```
<!DOCTYPE html>
<html>
<head>
  <meta>, <title>
  <style>
    /* === CSS VARIABLES (tema) === */
    /* === RESET + BASE === */
    /* === LAYOUT (header, main, bottom nav) === */
    /* === COMPONENTES (botões, cards, inputs, toasts) === */
    /* === VIEW: HOME === */
    /* === VIEW: TRADUTOR === */
    /* === VIEW: DICIONARIO === */
    /* === VIEW: AULAS === */
    /* === RESPONSIVO === */
  </style>
</head>
<body>
  <header id="app-header">...</header>
  <main id="app-main"></main>
  <nav id="bottom-nav">...</nav>
  <div id="toast-container"></div>
  <script>
    /* === DADOS MOCK (SINAIS, AULAS) === */
    /* === ESTADO + STORAGE === */
    /* === UTILITARIOS (toast, formatadores) === */
    /* === ROTEAMENTO === */
    /* === VIEW: HOME === */
    /* === VIEW: TRADUTOR (Web Speech API) === */
    /* === VIEW: DICIONARIO === */
    /* === VIEW: AULAS === */
    /* === INIT === */
  </script>
</body>
</html>
```

---

## Tasks

### Task 1: Scaffold inicial do `index.html`

**Files:**
- Create: `index.html` (raiz do projeto)

**Objetivo:** Criar a estrutura HTML mínima com head, body, header placeholder, main vazio, bottom nav com 4 links de hash. CSS reset + variáveis de tema dark. Sem JS ainda.

**Interfaces:**
- Produz: arquivo `index.html` que abre no navegador mostrando header + bottom nav, com main vazio entre eles.

- [ ] **Step 1: Criar o arquivo `index.html` com scaffold completo**

```html
<!DOCTYPE html>
<html lang="pt-BR" data-theme="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Bridge Fluent — Tradução e aprendizado inclusivo com IA">
  <title>Bridge Fluent</title>
  <style>
    /* === CSS VARIABLES === */
    :root[data-theme="dark"] {
      --bg: #0F2A47;
      --surface: #1E3A5F;
      --surface-elevated: #2A4A75;
      --accent: #F59E0B;
      --accent-hover: #FBBF24;
      --text: #F8FAFC;
      --text-muted: #94A3B8;
      --border: #334155;
      --success: #22C55E;
      --error: #EF4444;
      --shadow: 0 4px 12px rgba(0,0,0,0.3);
      --radius: 14px;
    }
    :root[data-theme="light"] {
      --bg: #F8FAFC;
      --surface: #FFFFFF;
      --surface-elevated: #F1F5F9;
      --accent: #F59E0B;
      --accent-hover: #D97706;
      --text: #0F2A47;
      --text-muted: #64748B;
      --border: #E2E8F0;
      --success: #16A34A;
      --error: #DC2626;
      --shadow: 0 4px 12px rgba(0,0,0,0.08);
      --radius: 14px;
    }
    /* === RESET === */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.5;
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }
    button { font-family: inherit; cursor: pointer; border: none; background: none; color: inherit; }
    a { color: inherit; text-decoration: none; }
    ul { list-style: none; }
    /* === HEADER === */
    #app-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 16px 20px;
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      position: sticky; top: 0; z-index: 10;
    }
    .logo { font-size: 18px; font-weight: 700; }
    .logo span { color: var(--accent); }
    .theme-toggle {
      width: 40px; height: 40px;
      border-radius: 50%;
      background: var(--surface-elevated);
      font-size: 18px;
    }
    /* === MAIN === */
    #app-main {
      flex: 1;
      padding: 20px;
      padding-bottom: 100px;
      max-width: 800px;
      width: 100%;
      margin: 0 auto;
    }
    /* === BOTTOM NAV === */
    #bottom-nav {
      position: fixed; bottom: 0; left: 0; right: 0;
      background: var(--surface);
      border-top: 1px solid var(--border);
      display: flex;
      justify-content: space-around;
      padding: 8px 0 max(8px, env(safe-area-inset-bottom));
      z-index: 10;
    }
    #bottom-nav a {
      display: flex; flex-direction: column; align-items: center; gap: 4px;
      padding: 8px 12px;
      font-size: 11px;
      color: var(--text-muted);
      border-radius: var(--radius);
      transition: color 0.2s;
    }
    #bottom-nav a:hover, #bottom-nav a.active { color: var(--accent); }
    #bottom-nav .nav-icon { font-size: 22px; }
  </style>
</head>
<body>
  <header id="app-header">
    <div class="logo">Bridge <span>Fluent</span></div>
    <button class="theme-toggle" aria-label="Alternar tema">🌙</button>
  </header>
  <main id="app-main">
    <!-- Views renderizadas aqui -->
  </main>
  <nav id="bottom-nav">
    <a href="#/" class="active" aria-label="Home">
      <span class="nav-icon">🏠</span>
      <span>Home</span>
    </a>
    <a href="#/tradutor" aria-label="Tradutor">
      <span class="nav-icon">🎤</span>
      <span>Tradutor</span>
    </a>
    <a href="#/dicionario" aria-label="Dicionário">
      <span class="nav-icon">📖</span>
      <span>Dicionário</span>
    </a>
    <a href="#/aulas" aria-label="Aulas">
      <span class="nav-icon">🎓</span>
      <span>Aulas</span>
    </a>
  </nav>
</body>
</html>
```

- [ ] **Step 2: Testar abrindo no navegador**

1. Abra o Explorador de Arquivos em `C:\Users\paule\Documents\PROGRAMAÇÃO\Libras 5.0 Sem Barreiras Projeto App\`
2. Duplo-clique em `index.html`
3. **Esperado:** Abre no navegador mostrando "Bridge Fluent" no header, área central vazia, e bottom nav com 4 ícones (Home, Tradutor, Dicionário, Aulas). Tema escuro (fundo azul-marinho).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: scaffold inicial do index.html com tema dark e bottom nav"
```

---

### Task 2: Estado global, storage e roteador

**Files:**
- Modify: `index.html` (adicionar `<script>` antes de `</body>`)

**Objetivo:** Implementar o sistema de roteamento hash-based, o objeto de estado global, e helpers de localStorage. Ao final, clicar nos itens da bottom nav deve alternar a classe `active` corretamente, e o hash deve aparecer na URL.

**Interfaces:**
- Produz: `function navigate(hash)` — muda o hash programaticamente.
- Produz: `function router()` — lê `location.hash` e chama o renderizador correspondente (a ser definido nas próximas tasks).
- Produz: `function loadState()` / `function saveState()` — sincroniza com localStorage.
- Produz: `const state` — objeto global com `user`, `sessions`, `lessonProgress`.

- [ ] **Step 1: Adicionar o bloco `<script>` antes de `</body>`**

Adicione este conteúdo imediatamente antes da tag `</body>`:

```html
  <script>
    /* === ESTADO + STORAGE === */
    const STORAGE_KEY = 'bf_state_v1';

    const defaultState = {
      user: { nome: 'Visitante', perfil: 'aluno', tema: 'dark' },
      sessions: [],
      lessonProgress: {}
    };

    let state = loadState();

    function loadState() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return structuredClone(defaultState);
        return { ...structuredClone(defaultState), ...JSON.parse(raw) };
      } catch (e) {
        console.warn('localStorage indisponível:', e);
        return structuredClone(defaultState);
      }
    }

    function saveState() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      } catch (e) {
        console.warn('Não foi possível salvar:', e);
      }
    }

    /* === TEMA === */
    function applyTheme(tema) {
      document.documentElement.setAttribute('data-theme', tema);
      const btn = document.querySelector('.theme-toggle');
      if (btn) btn.textContent = tema === 'dark' ? '🌙' : '☀️';
      state.user.tema = tema;
      saveState();
    }

    function initTheme() {
      applyTheme(state.user.tema);
      document.querySelector('.theme-toggle').addEventListener('click', () => {
        applyTheme(state.user.tema === 'dark' ? 'light' : 'dark');
      });
    }

    /* === ROTEAMENTO === */
    const routes = {
      '#/': () => renderHome(),
      '#/tradutor': () => renderTradutor(),
      '#/dicionario': () => renderDicionario(),
      '#/aulas': () => renderAulas()
    };

    function router() {
      const hash = location.hash || '#/';
      const main = document.getElementById('app-main');
      const renderer = routes[hash];
      if (!renderer) {
        main.innerHTML = '<p>Rota não encontrada.</p>';
        return;
      }
      renderer();
      // Atualiza bottom nav
      document.querySelectorAll('#bottom-nav a').forEach(a => {
        a.classList.toggle('active', a.getAttribute('href') === hash);
      });
      window.scrollTo(0, 0);
    }

    function navigate(hash) {
      if (location.hash === hash) {
        router(); // re-renderiza se já está na rota
      } else {
        location.hash = hash;
      }
    }

    /* === INIT === */
    window.addEventListener('hashchange', router);
    window.addEventListener('DOMContentLoaded', () => {
      initTheme();
      router();
    });
  </script>
```

- [ ] **Step 2: Adicionar stubs vazios pras views (pra evitar erro de router)**

No mesmo bloco `<script>`, **antes** de `/* === INIT === */`, adicione:

```js
    /* === VIEW STUBS (preenchidos nas próximas tasks) === */
    function renderHome() { document.getElementById('app-main').innerHTML = '<h1>Home</h1><p>Em construção.</p>'; }
    function renderTradutor() { document.getElementById('app-main').innerHTML = '<h1>Tradutor</h1><p>Em construção.</p>'; }
    function renderDicionario() { document.getElementById('app-main').innerHTML = '<h1>Dicionário</h1><p>Em construção.</p>'; }
    function renderAulas() { document.getElementById('app-main').innerHTML = '<h1>Aulas</h1><p>Em construção.</p>'; }
```

- [ ] **Step 3: Testar navegação**

1. Recarregue `index.html` no navegador (F5).
2. **Esperado:** aparece "Home" + "Em construção." no centro.
3. Clique em **Tradutor** na bottom nav.
4. **Esperado:** URL muda pra `#/tradutor`, conteúdo muda pra "Tradutor" + "Em construção." O ícone "Tradutor" na bottom nav fica destacado (cor âmbar).
5. Clique no botão de voltar do navegador.
6. **Esperado:** volta pra Home, ícone Home fica destacado.
7. Abra DevTools (F12) → Application → Local Storage → `file://`. **Esperado:** chave `bf_state_v1` existe.

- [ ] **Step 4: Testar toggle de tema**

1. Clique no botão 🌙 no header.
2. **Esperado:** ícone vira ☀️, fundo vira claro (azul-marinho → cinza-claro).
3. Recarregue a página (F5).
4. **Esperado:** tema continua claro (persistido).

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: estado global, storage, roteamento hash e toggle de tema"
```

---



### Task 3: Sistema de toasts e utilitários

**Files:**
- Modify: `index.html` (adicionar CSS para toasts + funções JS utilitárias)

**Objetivo:** Criar um sistema simples de notificações toast que aparece no topo da tela por 3 segundos, mais helpers de formatação de data/duração.

**Interfaces:**
- Produz: `function showToast(mensagem, tipo)` onde `tipo` é `'success' | 'error' | 'info'`.
- Produz: `function formatDate(isoString)` — retorna "há X min" / "há X horas" / data formatada.
- Produz: `function formatDuration(segundos)` — retorna "Xs" / "Xmin Ys".

- [ ] **Step 1: Adicionar CSS para o container de toasts**

Adicione no `<style>` (no final da seção /* === COMPONENTES === */, antes do /* === RESPONSIVO === */ — vamos adicionar essa seção depois). Por ora, adicione logo antes da tag `</style>`:

```css
    /* === TOASTS === */
    #toast-container {
      position: fixed;
      top: 80px;
      left: 50%;
      transform: translateX(-50%);
      z-index: 100;
      display: flex;
      flex-direction: column;
      gap: 8px;
      pointer-events: none;
    }
    .toast {
      background: var(--surface-elevated);
      color: var(--text);
      padding: 12px 20px;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      border-left: 4px solid var(--accent);
      font-size: 14px;
      max-width: 90vw;
      animation: toast-in 0.3s ease-out, toast-out 0.3s ease-in 2.7s forwards;
    }
    .toast.success { border-left-color: var(--success); }
    .toast.error { border-left-color: var(--error); }
    @keyframes toast-in {
      from { opacity: 0; transform: translateY(-20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes toast-out {
      to { opacity: 0; transform: translateY(-20px); }
    }
```

- [ ] **Step 2: Adicionar o `<div id="toast-container">` no body**

Adicione imediatamente antes da tag `<nav id="bottom-nav">`:

```html
  <div id="toast-container" aria-live="polite" aria-atomic="true"></div>
```

- [ ] **Step 3: Adicionar funções de toast e formatadores no JS**

No bloco `<script>`, **após** o bloco `/* === VIEW STUBS === */` e **antes** de `/* === INIT === */`, adicione:

```js
    /* === UTILITARIOS === */
    function showToast(mensagem, tipo = 'info') {
      const container = document.getElementById('toast-container');
      const toast = document.createElement('div');
      toast.className = `toast ${tipo}`;
      toast.textContent = mensagem;
      toast.setAttribute('role', 'status');
      container.appendChild(toast);
      setTimeout(() => toast.remove(), 3000);
    }

    function formatDate(isoString) {
      const date = new Date(isoString);
      const agora = new Date();
      const diffMs = agora - date;
      const diffMin = Math.floor(diffMs / 60000);
      const diffHoras = Math.floor(diffMs / 3600000);
      const diffDias = Math.floor(diffMs / 86400000);
      if (diffMin < 1) return 'agora';
      if (diffMin < 60) return `há ${diffMin} min`;
      if (diffHoras < 24) return `há ${diffHoras} h`;
      if (diffDias < 7) return `há ${diffDias} d`;
      return date.toLocaleDateString('pt-BR', { day: '2-digit', month: '2-digit' });
    }

    function formatDuration(segundos) {
      const min = Math.floor(segundos / 60);
      const seg = Math.floor(segundos % 60);
      if (min === 0) return `${seg}s`;
      return `${min}min ${seg}s`;
    }

    function uid() {
      return Date.now().toString(36) + Math.random().toString(36).slice(2, 7);
    }
```

- [ ] **Step 4: Testar toast**

1. Abra DevTools Console (F12).
2. Cole: `showToast('Teste de toast!', 'success')`
3. Pressione Enter.
4. **Esperado:** Aparece um toast verde com "Teste de toast!" no topo da tela, some após 3 segundos.

- [ ] **Step 5: Testar formatadores**

No console:

```js
formatDate(new Date().toISOString())     // esperado: "agora"
formatDuration(125)                      // esperado: "2min 5s"
formatDuration(45)                       // esperado: "45s"
uid()                                    // esperado: string aleatória
```

**Esperado:** Todos retornam o valor esperado.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: sistema de toasts e utilitarios de formatacao"
```

---

### Task 4: Dados mock — Sinais do dicionário (20 itens)

**Files:**
- Modify: `index.html` (adicionar array `SINAIS` no JS)

**Objetivo:** Criar o array de dados com 20 sinais cobrindo 6 categorias (Saudações, Família, Números, Cores, Sentimentos, Cotidiano). Cada sinal tem: id, termo, descrição, categoria, exemplo de uso, ícone (emoji).

**Interfaces:**
- Produz: `const SINAIS = [...]` — array de 20 objetos.
- Produz: `const CATEGORIAS_SINAIS = [...]` — array de strings com as 6 categorias pra filtro.

- [ ] **Step 1: Adicionar o array `SINAIS` no JS**

No bloco `<script>`, **após** o bloco `/* === ESTADO + STORAGE === */` e **antes** de `/* === TEMA === */`, adicione:

```js
    /* === DADOS MOCK: SINAIS === */
    const CATEGORIAS_SINAIS = ['Saudações', 'Família', 'Números', 'Cores', 'Sentimentos', 'Cotidiano'];

    const SINAIS = [
      // Saudações (3)
      { id: 's1', termo: 'Olá', descricao: 'Saudação informal. Mão aberta próxima ao rosto, movimento de onda.', categoria: 'Saudações', exemplo: 'Use ao encontrar amigos casualmente.', icone: '👋' },
      { id: 's2', termo: 'Bom dia', descricao: 'Saudação para o período matutino. Sinal de "bom" seguido de "dia".', categoria: 'Saudações', exemplo: 'Cumprimente colegas no início da aula.', icone: '🌅' },
      { id: 's3', termo: 'Tchau', descricao: 'Despedida informal. Mão aberta acenando com os dedos.', categoria: 'Saudações', exemplo: 'Use ao se despedir de forma amigável.', icone: '👋' },
      // Família (3)
      { id: 's4', termo: 'Mãe', descricao: 'Polegar da mão dominante encosta no queixo.', categoria: 'Família', exemplo: 'Referir-se à figura materna.', icone: '👩' },
      { id: 's5', termo: 'Pai', descricao: 'Polegar da mão dominante encosta na testa.', categoria: 'Família', exemplo: 'Referir-se à figura paterna.', icone: '👨' },
      { id: 's6', termo: 'Irmão/Irmã', descricao: 'Indicador e polegar formam "L" encostando no peito.', categoria: 'Família', exemplo: 'Apresentar um irmão para alguém.', icone: '🧑' },
      // Números (3)
      { id: 's7', termo: 'Um', descricao: 'Indicador levantado, palma pra frente.', categoria: 'Números', exemplo: 'Contar ou indicar quantidade um.', icone: '1️⃣' },
      { id: 's8', termo: 'Dois', descricao: 'Indicador e médio levantados em "V".', categoria: 'Números', exemplo: 'Indicar a quantidade dois.', icone: '2️⃣' },
      { id: 's9', termo: 'Três', descricao: 'Indicador, médio e anelar levantados.', categoria: 'Números', exemplo: 'Mostrar a quantidade três.', icone: '3️⃣' },
      // Cores (3)
      { id: 's10', termo: 'Azul', descricao: 'Polegar indicador e médio juntos, balançando lateralmente.', categoria: 'Cores', exemplo: 'Descrever objetos azuis como o céu.', icone: '🔵' },
      { id: 's11', termo: 'Amarelo', descricao: 'Mão em "Y" (polegar e mindinho), balança lateral.', categoria: 'Cores', exemplo: 'Apontar para o sol ou bananas.', icone: '🟡' },
      { id: 's12', termo: 'Verde', descricao: 'Indicador e médio batem de leve no queixo.', categoria: 'Cores', exemplo: 'Referir-se a folhas ou grama.', icone: '🟢' },
      // Sentimentos (3)
      { id: 's13', termo: 'Feliz', descricao: 'Mãos abertas sobem pelo peito em direção ao rosto.', categoria: 'Sentimentos', exemplo: 'Expressar alegria ou satisfação.', icone: '😊' },
      { id: 's14', termo: 'Triste', descricao: 'Indicadores sobem pelo rosto simulando lágrimas.', categoria: 'Sentimentos', exemplo: 'Expressar tristeza ou decepção.', icone: '😢' },
      { id: 's15', termo: 'Cansado', descricao: 'Mãos abertas sobem pelo peito e "caem" em direção ao chão.', categoria: 'Sentimentos', exemplo: 'Dizer que precisa de descanso.', icone: '😴' },
      // Cotidiano (5)
      { id: 's16', termo: 'Obrigado', descricao: 'Mão aberta encosta no queixo e sai em direção à pessoa.', categoria: 'Cotidiano', exemplo: 'Agradecer por algo recebido.', icone: '🙏' },
      { id: 's17', termo: 'Comer', descricao: 'Mão fechada em "O" vai em direção à boca repetidamente.', categoria: 'Cotidiano', exemplo: 'Indicar que vai se alimentar.', icone: '🍽️' },
      { id: 's18', termo: 'Beber', descricao: 'Mão em "C" (curva) vai em direção à boca.', categoria: 'Cotidiano', exemplo: 'Pedir água ou se hidratar.', icone: '🥤' },
      { id: 's19', termo: 'Estudar', descricao: 'Mão em "L" encosta na palma da outra mão e sobe.', categoria: 'Cotidiano', exemplo: 'Dizer que vai estudar ou aprender.', icone: '📚' },
      { id: 's20', termo: 'Entender', descricao: 'Indicador sobe perto da têmpora e "abre" como chaveamento.', categoria: 'Cotidiano', exemplo: 'Confirmar que compreendeu algo.', icone: '💡' }
    ];
```

- [ ] **Step 2: Validar no console**

No DevTools Console:

```js
console.log(SINAIS.length)        // esperado: 20
console.log(new Set(SINAIS.map(s => s.categoria)).size)  // esperado: 6
console.log(SINAIS.filter(s => s.categoria === 'Cotidiano').length)  // esperado: 5
```

**Esperado:** 20, 6, 5.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: dados mock de 20 sinais em 6 categorias"
```

---



### Task 5: Dados mock — Aulas (6 itens com tópicos e quiz)

**Files:**
- Modify: `index.html` (adicionar array `AULAS` no JS)

**Objetivo:** Criar o array com 6 aulas. Cada aula tem: id, título, nível (Iniciante/Intermediário/Avançado), descrição, tópicos (array de {título, conteúdo}), e quiz (array de {pergunta, opções[4], correta}). Cobrir: Alfabeto manual, Cumprimentos, Verbos essenciais, Estrutura frasal, Sinais do cotidiano, Cultura surda.

**Interfaces:**
- Produz: `const AULAS = [...]` — array de 6 objetos.

- [ ] **Step 1: Adicionar o array `AULAS` no JS**

No bloco `<script>`, **logo após** o array `SINAIS` da Task 4, adicione:

```js
    /* === DADOS MOCK: AULAS === */
    const AULAS = [
      {
        id: 'a1',
        titulo: 'Alfabeto Manual',
        nivel: 'Iniciante',
        descricao: 'Conheça as 26 letras do alfabeto em Libras, base para soletrar nomes e palavras.',
        topicos: [
          { titulo: 'O que é o alfabeto manual?', conteudo: 'O alfabeto manual (datilologia) é o conjunto de configurações de mão que representam cada letra do português. É essencial para soletrar nomes próprios, palavras sem sinal conhecido ou termos técnicos.' },
          { titulo: 'Como praticar', conteudo: 'Comece com seu próprio nome. Soletre na frente do espelho. A repetição diária de 10 minutos já traz fluência em poucas semanas. Use apps de videochamada com amigos surdos para praticar em contexto real.' },
          { titulo: 'Dicas importantes', conteudo: 'Mantenha a mão na altura do ombro, com a palma virada para frente. Velocidade moderada é mais importante que velocidade alta. Erre sem medo — a comunidade surda valoriza a tentativa.' }
        ],
        quiz: [
          { pergunta: 'Para que serve o alfabeto manual?', opcoes: ['Substituir todos os sinais', 'Soletrar nomes e palavras sem sinal', 'Apenas escrever', 'Decorar letras'], correta: 1 },
          { pergunta: 'Onde deve ficar a mão ao soletrar?', opcoes: ['No colo', 'Abaixo da cintura', 'Na altura do ombro', 'Atrás da cabeça'], correta: 2 },
          { pergunta: 'Qual a melhor forma de praticar?', opcoes: ['Apenas ler sobre', 'Prática diária com repetição', 'Uma vez por mês', 'Só em provas'], correta: 1 }
        ]
      },
      {
        id: 'a2',
        titulo: 'Cumprimentos Básicos',
        nivel: 'Iniciante',
        descricao: 'Aprenda a cumprimentar em Libras: Olá, Bom dia, Tudo bem, Prazer e Tchau.',
        topicos: [
          { titulo: 'Cumprimentos formais vs informais', conteudo: 'Em Libras, existem cumprimentos formais (usados com desconhecidos e pessoas mais velhas) e informais (entre amigos e familiares). A diferença está na intensidade do movimento e no uso de expressões faciais.' },
          { titulo: 'Os 5 essenciais', conteudo: 'Olá 👋 (informal), Bom dia 🌅 (período matutino), Boa tarde ☀️ (período vespertino), Boa noite 🌙 (período noturno), Tchau 👋 (despedida).' },
          { titulo: 'Expressões faciais', conteudo: 'As expressões faciais em Libras são gramaticais, não emocionais. Um cumprimento sem sorriso pode parecer frio ou rude, mesmo sendo correto na configuração de mão.' }
        ],
        quiz: [
          { pergunta: 'Qual cumprimento usar de manhã?', opcoes: ['Boa noite', 'Olá', 'Bom dia', 'Tchau'], correta: 2 },
          { pergunta: 'Por que as expressões faciais importam?', conteudo: null, opcoes: ['Não importam', 'São gramaticais, não só emocionais', 'Só para teatro', 'Apenas formalidade'], correta: 1 },
          { pergunta: 'Tchau é usado para:', opcoes: ['Iniciar conversa', 'Despedir-se', 'Apresentar-se', 'Agradecer'], correta: 1 }
        ].filter(q => q.conteudo === null ? false : true) // remove placeholder se houver
      },
      {
        id: 'a3',
        titulo: 'Verbos Essenciais',
        nivel: 'Iniciante',
        descricao: 'Os verbos mais usados no dia a dia: ir, vir, querer, poder, saber, entender.',
        topicos: [
          { titulo: 'Por que verbos primeiro?', conteudo: 'Com poucos verbos e muitos substantivos, você já consegue formar frases simples. Priorizar verbos essenciais acelera a comunicação real.' },
          { titulo: 'Lista prioritária', conteudo: 'IR (movimento pra longe), VIR (movimento pra perto), QUERER (mãos em garra puxando pra si), PODER (mão aberta batendo na outra), SABER (pontas dos dedos na testa), ENTENDER (girar chave mental).' },
          { titulo: 'Conjugação em Libras', conteudo: 'Diferente do português, Libras não conjuga verbos por tempo verbal (presente, passado, futuro). A noção temporal é dada pelo contexto da frase e por advérbios de tempo.' }
        ],
        quiz: [
          { pergunta: 'Libras conjuga verbos em tempos verbais?', opcoes: ['Sim, como português', 'Não, usa contexto e advérbios', 'Só no presente', 'Só no passado'], correta: 1 },
          { pergunta: 'O verbo IR é sinalizado com movimento para:', opcoes: ['Cima', 'Baixo', 'Longe', 'Perto'], correta: 2 },
          { pergunta: 'Por que priorizar verbos essenciais?', opcoes: ['São mais fáceis', 'Permitem frases úteis com poucos sinais', 'Não há alternativa', 'É regra da escola'], correta: 1 }
        ]
      },
      {
        id: 'a4',
        titulo: 'Estrutura Frasal',
        nivel: 'Intermediário',
        descricao: 'Como montar frases em Libras: ordem Sujeito-Objeto-Verbo e topicalização.',
        topicos: [
          { titulo: 'SOV: a ordem natural', conteudo: 'A estrutura mais comum em Libras é Sujeito-Objeto-Verbo. Exemplo: "EU MAÇÃ COMER" em vez de "Eu como maçã". Isso não é erro — é a gramática natural da língua.' },
          { titulo: 'Topicalização', conteudo: 'Você pode colocar um tópico no início da frase para dar ênfase. "CARRO, MEU QUEBRAR" significa "o carro, o meu quebrou". O tópico é marcado com pausa e expressão facial.' },
          { titulo: 'Perguntas sim/não', conteudo: 'Para perguntas de sim/não, levante as sobrancelhas no final da afirmação e incline levemente o corpo pra frente. Para perguntas com "quê/quem/quando", franze a testa.' }
        ],
        quiz: [
          { pergunta: 'Qual a ordem frasal mais comum em Libras?', opcoes: ['SVO (como português)', 'SOV (Sujeito-Objeto-Verbo)', 'VSO', 'OSV'], correta: 1 },
          { pergunta: 'Perguntas de sim/não exigem:', opcoes: ['Tom de voz agudo', 'Sobrancelhas levantadas', 'Punho cerrado', 'Aceno de cabeça'], correta: 1 },
          { pergunta: 'Topicalização serve para:', opcoes: ['Acabar a frase', 'Dar ênfase a um tema', 'Substituir verbo', 'Apenas em entrevistas'], correta: 1 }
        ]
      },
      {
        id: 'a5',
        titulo: 'Sinais do Cotidiano',
        nivel: 'Intermediário',
        descricao: 'Sinais práticos para o dia a dia: comer, beber, estudar, trabalhar, dormir.',
        topicos: [
          { titulo: 'Sinais icônicos', conteudo: 'Muitos sinais cotidianos são icônicos (imitam a ação). COMER lembra levar comida à boca. BEBER lembra segurar um copo. Isso facilita muito a memorização inicial.' },
          { titulo: 'Sinais arbitrários', conteudo: 'Outros sinais não têm ligação visual com o significado. ESTUDAR, por exemplo, não imita "estudar". Aí a memorização exige repetição e uso em contexto.' },
          { titulo: 'Criando rotina de prática', conteudo: 'Associe sinais a ações do seu dia. Ao acordar, sinalize BOM DIA. Ao comer, sinalize COMER. Isso fixa o vocabulário muito mais rápido que estudar isoladamente.' }
        ],
        quiz: [
          { pergunta: 'Sinais icônicos facilitam a memorização porque:', opcoes: ['São mais bonitos', 'Imitam a ação real', 'São mais curtos', 'Não têm regra'], correta: 1 },
          { pergunta: 'Qual é uma boa estratégia de prática?', opcoes: ['Estudar 5h só no fim de semana', 'Associar sinais a ações do dia', 'Memorizar lista e nunca usar', 'Assistir sem praticar'], correta: 1 },
          { pergunta: 'BEBER é sinal:', opcoes: ['Arbitrário', 'Icônico (imita copo)', 'Numérico', 'Sem categoria'], correta: 1 }
        ]
      },
      {
        id: 'a6',
        titulo: 'Cultura Surda',
        nivel: 'Avançado',
        descricao: 'Compreenda a comunidade surda, sua história, identidade e etiqueta.',
        topicos: [
          { titulo: 'Surdo x Deficiente Auditivo', conteudo: '"Surdo" (S maiúsculo conceitual) refere-se à identidade cultural e linguística. "deficiente auditivo" é o termo clínico. A preferência varia, mas a comunidade geralmente prefere "surdo" ao falar de si e da cultura.' },
          { titulo: 'História da Libras', conteudo: 'A Libras foi oficialmente reconhecida no Brasil pela Lei 10.436/2002. Tem origem na Língua de Sinais Francesa (LSF), trazida por Thomas Hull e Eduard Huet em 1856, mas se desenvolveu independentemente no contexto brasileiro.' },
          { titulo: 'Etiqueta essencial', conteudo: 'Em conversas em grupo, mantenha-se visível. Não fale enquanto outra pessoa está sinalizando. Ao chegar, cumprimente todas as pessoas. Atenção visual é a base da comunicação — não vire as costas.' }
        ],
        quiz: [
          { pergunta: 'Qual lei reconheceu oficialmente a Libras?', opcoes: ['Lei 9.394/96', 'Lei 10.436/2002', 'Lei 8.069/90', 'Lei 13.146/15'], correta: 1 },
          { pergunta: 'A preferência da comunidade é:', opcoes: ['Deficiente auditivo sempre', 'Surdo (identidade cultural)', 'Ambos são iguais', 'Apenas surdo profundo'], correta: 1 },
          { pergunta: 'Em conversa em grupo, deve-se:', opcoes: ['Falar por cima', 'Manter visibilidade e atenção visual', 'Sussurrar', 'Ficar de costas'], correta: 1 }
        ]
      }
    ];
```

- [ ] **Step 2: Validar no console**

```js
console.log(AULAS.length)                                          // esperado: 6
console.log(AULAS.every(a => a.quiz.length === 3))                 // esperado: true
console.log(AULAS.every(a => a.quiz.every(q => q.opcoes.length === 4 && q.correta >= 0 && q.correta <= 3)))  // esperado: true
console.log(new Set(AULAS.map(a => a.nivel)).size)                 // esperado: 3 (Iniciante, Intermediário, Avançado)
```

**Esperado:** 6, true, true, 3.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: dados mock de 6 aulas com topicos e quiz"
```

---

### Task 6: View — Home com saudação e cards de atalho

**Files:**
- Modify: `index.html` (substituir stub `renderHome` por implementação real + adicionar CSS)

**Objetivo:** Renderizar a home com saudação dinâmica baseada em horário, 4 cards grandes de atalho (Traduzir, Dicionário, Aulas, Sobre), e barra de busca global que filtra e navega para a tela correspondente.

**Interfaces:**
- Substitui: `function renderHome()` — implementa renderização completa.
- Consome: `state.user.nome` (saudação).

- [ ] **Step 1: Adicionar CSS específico da home**

Logo antes do bloco `/* === TOASTS === */` que adicionamos na Task 3, adicione:

```css
    /* === VIEW: HOME === */
    .home-hero {
      text-align: center;
      padding: 20px 0 30px;
    }
    .home-hero h1 {
      font-size: 26px;
      font-weight: 700;
      margin-bottom: 8px;
    }
    .home-hero p {
      color: var(--text-muted);
      font-size: 15px;
    }
    .search-bar {
      display: flex;
      align-items: center;
      gap: 8px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 12px 16px;
      margin-bottom: 24px;
    }
    .search-bar input {
      flex: 1;
      background: none;
      border: none;
      outline: none;
      color: var(--text);
      font-size: 15px;
      font-family: inherit;
    }
    .search-bar input::placeholder { color: var(--text-muted); }
    .quick-actions {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 16px;
    }
    .action-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 24px 16px;
      text-align: center;
      transition: transform 0.2s, border-color 0.2s;
      cursor: pointer;
    }
    .action-card:hover {
      transform: translateY(-2px);
      border-color: var(--accent);
    }
    .action-card .card-icon {
      font-size: 40px;
      margin-bottom: 12px;
      display: block;
    }
    .action-card .card-title {
      font-weight: 600;
      margin-bottom: 4px;
      font-size: 15px;
    }
    .action-card .card-desc {
      font-size: 12px;
      color: var(--text-muted);
    }
```

- [ ] **Step 2: Substituir o stub `renderHome` pela implementação real**

No JS, substitua:

```js
    function renderHome() { document.getElementById('app-main').innerHTML = '<h1>Home</h1><p>Em construção.</p>'; }
```

Por:

```js
    function renderHome() {
      const hora = new Date().getHours();
      let saudacao = 'Olá';
      if (hora < 12) saudacao = 'Bom dia';
      else if (hora < 18) saudacao = 'Boa tarde';
      else saudacao = 'Boa noite';

      const main = document.getElementById('app-main');
      main.innerHTML = `
        <section class="home-hero">
          <h1>${saudacao}, ${escapeHtml(state.user.nome)}!</h1>
          <p>O que você quer aprender hoje?</p>
        </section>
        <div class="search-bar">
          <span aria-hidden="true">🔍</span>
          <input type="search" id="home-search" placeholder="Buscar sinais, aulas ou sessões..." aria-label="Busca global">
        </div>
        <div class="quick-actions">
          <button class="action-card" data-route="#/tradutor">
            <span class="card-icon" aria-hidden="true">🎤</span>
            <div class="card-title">Traduzir Agora</div>
            <div class="card-desc">Voz para texto em tempo real</div>
          </button>
          <button class="action-card" data-route="#/dicionario">
            <span class="card-icon" aria-hidden="true">📖</span>
            <div class="card-title">Dicionário</div>
            <div class="card-desc">20 sinais essenciais</div>
          </button>
          <button class="action-card" data-route="#/aulas">
            <span class="card-icon" aria-hidden="true">🎓</span>
            <div class="card-title">Aulas Acessíveis</div>
            <div class="card-desc">Aprenda no seu ritmo</div>
          </button>
          <button class="action-card" data-route="#/aulas">
            <span class="card-icon" aria-hidden="true">🌟</span>
            <div class="card-title">Cultura Surda</div>
            <div class="card-desc">Conheça a comunidade</div>
          </button>
        </div>
      `;

      // Listeners
      main.querySelectorAll('.action-card').forEach(card => {
        card.addEventListener('click', () => navigate(card.dataset.route));
      });

      const search = document.getElementById('home-search');
      search.addEventListener('keypress', e => {
        if (e.key === 'Enter' && search.value.trim()) {
          performGlobalSearch(search.value.trim());
        }
      });
    }

    function performGlobalSearch(query) {
      const q = query.toLowerCase();
      const sinaisMatch = SINAIS.filter(s =>
        s.termo.toLowerCase().includes(q) ||
        s.descricao.toLowerCase().includes(q) ||
        s.categoria.toLowerCase().includes(q)
      );
      const aulasMatch = AULAS.filter(a =>
        a.titulo.toLowerCase().includes(q) ||
        a.descricao.toLowerCase().includes(q)
      );
      if (sinaisMatch.length > 0) {
        navigate('#/dicionario');
        setTimeout(() => {
          const input = document.getElementById('dict-search');
          if (input) {
            input.value = query;
            input.dispatchEvent(new Event('input'));
          }
        }, 100);
      } else if (aulasMatch.length > 0) {
        navigate('#/aulas');
      } else {
        showToast(`Nada encontrado para "${query}"`, 'error');
      }
    }
```

- [ ] **Step 3: Adicionar helper `escapeHtml` no bloco de utilitários**

Localize a função `uid()` no JS (Task 3). Adicione **antes** dela:

```js
    function escapeHtml(str) {
      const div = document.createElement('div');
      div.textContent = str;
      return div.innerHTML;
    }
```

- [ ] **Step 4: Testar a home**

1. Recarregue `index.html` (F5).
2. **Esperado:** Saudação "Bom dia", "Boa tarde" ou "Boa noite" conforme o horário, + nome do usuário.
3. **Esperado:** 4 cards: Traduzir Agora, Dicionário, Aulas Acessíveis, Cultura Surda.
4. Clique no card **Traduzir Agora**.
5. **Esperado:** navega pra `#/tradutor` (vai aparecer "Em construção" — esperado, view não foi implementada ainda).
6. Volte pra Home (clique no ícone Home).
7. Na busca, digite `obrigado` e pressione Enter.
8. **Esperado:** navega pra `#/dicionario` e pré-popula a busca (a view do dicionário ainda não foi implementada, mas o comportamento de navegação deve funcionar).
9. Na busca, digite `xyz123` e pressione Enter.
10. **Esperado:** aparece toast vermelho "Nada encontrado para 'xyz123'".

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: view home com saudacao, cards de atalho e busca global"
```

---

### Task 7: View — Tradutor com Web Speech API

**Files:**
- Modify: `index.html` (substituir stub `renderTradutor` + adicionar CSS)

**Objetivo:** Renderizar a tela do tradutor com botão grande de microfone, área de transcrição em tempo real, e botões Limpar/Salvar. Usar Web Speech API quando disponível, com fallback de "modo demo" se a API não existir.

**Interfaces:**
- Substitui: `function renderTradutor()` — implementa tradução real.
- Produz: `function startRecognition()` e `function stopRecognition()` — controle da captura.
- Consome: Web Speech API (`SpeechRecognition` ou `webkitSpeechRecognition`).
- Persiste em `state.sessions` ao clicar Salvar.

- [ ] **Step 1: Adicionar CSS do tradutor**

Antes do bloco `/* === VIEW: HOME === */`, adicione:

```css
    /* === VIEW: TRADUTOR === */
    .tradutor-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding-top: 20px;
      text-align: center;
    }
    .tradutor-container h1 {
      font-size: 22px;
      margin-bottom: 8px;
    }
    .tradutor-container .subtitle {
      color: var(--text-muted);
      margin-bottom: 32px;
      font-size: 14px;
    }
    .mic-button {
      width: 120px;
      height: 120px;
      border-radius: 50%;
      background: var(--surface);
      border: 3px solid var(--border);
      font-size: 50px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.3s;
      position: relative;
    }
    .mic-button:hover { transform: scale(1.05); border-color: var(--accent); }
    .mic-button.recording {
      background: var(--error);
      border-color: var(--error);
      animation: pulse 1.5s infinite;
    }
    @keyframes pulse {
      0%, 100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.5); }
      50% { box-shadow: 0 0 0 20px rgba(239, 68, 68, 0); }
    }
    .mic-status {
      margin-top: 16px;
      font-size: 14px;
      color: var(--text-muted);
      min-height: 20px;
    }
    .mic-status.listening { color: var(--error); }
    .transcript-box {
      width: 100%;
      min-height: 120px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 16px;
      margin-top: 24px;
      font-size: 16px;
      line-height: 1.6;
      text-align: left;
      overflow-wrap: break-word;
    }
    .transcript-box.empty { color: var(--text-muted); font-style: italic; }
    .transcript-box .interim { opacity: 0.6; }
    .tradutor-actions {
      display: flex;
      gap: 12px;
      margin-top: 16px;
      width: 100%;
    }
    .btn {
      flex: 1;
      padding: 12px 16px;
      border-radius: var(--radius);
      font-weight: 600;
      font-size: 14px;
      transition: opacity 0.2s;
    }
    .btn:disabled { opacity: 0.4; cursor: not-allowed; }
    .btn-primary { background: var(--accent); color: #1A1A1A; }
    .btn-primary:hover:not(:disabled) { background: var(--accent-hover); }
    .btn-secondary { background: var(--surface-elevated); color: var(--text); }
    .btn-secondary:hover:not(:disabled) { opacity: 0.85; }
```

- [ ] **Step 2: Substituir o stub `renderTradutor` pela implementação**

Substitua:

```js
    function renderTradutor() { document.getElementById('app-main').innerHTML = '<h1>Tradutor</h1><p>Em construção.</p>'; }
```

Por:

```js
    let recognition = null;
    let recognitionStartTime = null;

    function renderTradutor() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      const suportado = !!SpeechRecognition;

      const main = document.getElementById('app-main');
      main.innerHTML = `
        <section class="tradutor-container">
          <h1>Tradutor Voz → Texto</h1>
          <p class="subtitle">${suportado ? 'Clique no microfone e comece a falar' : '⚠️ Seu navegador não suporta reconhecimento de voz — modo demo ativo'}</p>
          <button class="mic-button" id="mic-btn" aria-label="Iniciar reconhecimento de voz">
            🎤
          </button>
          <div class="mic-status" id="mic-status">Pronto para ouvir</div>
          <div class="transcript-box empty" id="transcript-box" aria-live="polite">
            Sua fala aparecerá aqui...
          </div>
          <div class="tradutor-actions">
            <button class="btn btn-secondary" id="btn-clear" disabled>Limpar</button>
            <button class="btn btn-primary" id="btn-save" disabled>Salvar Sessão</button>
          </div>
        </section>
      `;

      const micBtn = document.getElementById('mic-btn');
      const statusEl = document.getElementById('mic-status');
      const transcriptEl = document.getElementById('transcript-box');
      const clearBtn = document.getElementById('btn-clear');
      const saveBtn = document.getElementById('btn-save');

      let finalText = '';

      function setTranscript(text, isFinal) {
        if (text) {
          transcriptEl.classList.remove('empty');
          transcriptEl.innerHTML = `<span class="${isFinal ? '' : 'interim'}">${escapeHtml(text)}</span>`;
          clearBtn.disabled = !text.trim();
          saveBtn.disabled = !finalText.trim();
        } else {
          transcriptEl.classList.add('empty');
          transcriptEl.textContent = 'Sua fala aparecerá aqui...';
          clearBtn.disabled = true;
          saveBtn.disabled = true;
        }
      }

      function startDemo() {
        // Modo demo: simula fala com texto pré-definido após 1s
        micBtn.classList.add('recording');
        statusEl.classList.add('listening');
        statusEl.textContent = 'Ouvindo (demo)...';
        const demoTexts = [
          'Olá, tudo bem?',
          'Meu nome é Visitante e estou aprendendo Libras.',
          'Este é o modo demonstração porque o navegador não suporta reconhecimento de voz.',
          'Em produção, este texto viria da sua fala real.'
        ];
        let i = 0;
        const interval = setInterval(() => {
          if (i >= demoTexts.length) {
            stopDemo(interval);
            return;
          }
          setTranscript(demoTexts[i], false);
          finalText = demoTexts.slice(0, i + 1).join(' ');
          setTranscript(finalText, true);
          i++;
        }, 1500);
        recognition = { stop: () => stopDemo(interval) };
      }

      function stopDemo(interval) {
        clearInterval(interval);
        micBtn.classList.remove('recording');
        statusEl.classList.remove('listening');
        statusEl.textContent = 'Parado';
        recognition = null;
      }

      function startReal() {
        recognition = new SpeechRecognition();
        recognition.lang = 'pt-BR';
        recognition.continuous = true;
        recognition.interimResults = true;
        recognitionStartTime = Date.now();

        recognition.onresult = (event) => {
          let interim = '';
          for (let i = event.resultIndex; i < event.results.length; i++) {
            const t = event.results[i][0].transcript;
            if (event.results[i].isFinal) finalText += t + ' ';
            else interim += t;
          }
          setTranscript((finalText + interim).trim(), !interim);
        };

        recognition.onerror = (event) => {
          console.error('Erro no reconhecimento:', event.error);
          if (event.error === 'not-allowed' || event.error === 'service-not-allowed') {
            showToast('Permissão de microfone negada. Habilite nas configurações do navegador.', 'error');
          }
          stopRecognition();
        };

        recognition.onend = () => {
          if (micBtn.classList.contains('recording')) {
            // Auto-restart se ainda está em modo gravação
            try { recognition.start(); } catch (e) { stopRecognition(); }
          } else {
            stopRecognition();
          }
        };

        try {
          recognition.start();
          micBtn.classList.add('recording');
          statusEl.classList.add('listening');
          statusEl.textContent = 'Ouvindo...';
        } catch (e) {
          showToast('Erro ao iniciar microfone', 'error');
        }
      }

      function stopRecognition() {
        if (recognition) {
          try { recognition.stop(); } catch (e) {}
        }
        micBtn.classList.remove('recording');
        statusEl.classList.remove('listening');
        statusEl.textContent = 'Parado';
        recognition = null;
      }

      micBtn.addEventListener('click', () => {
        if (micBtn.classList.contains('recording')) {
          stopRecognition();
        } else {
          finalText = '';
          setTranscript('', true);
          if (suportado) startReal();
          else startDemo();
        }
      });

      clearBtn.addEventListener('click', () => {
        finalText = '';
        setTranscript('', true);
        if (micBtn.classList.contains('recording')) stopRecognition();
      });

      saveBtn.addEventListener('click', () => {
        if (!finalText.trim()) return;
        const duracao = Math.floor((Date.now() - (recognitionStartTime || Date.now())) / 1000);
        state.sessions.unshift({
          id: uid(),
          texto: finalText.trim(),
          dataISO: new Date().toISOString(),
          duracaoSeg: duracao
        });
        saveState();
        showToast('Sessão salva com sucesso!', 'success');
      });
    }
```

- [ ] **Step 3: Testar tradutor (modo demo, sem microfone real)**

1. Abra a home e clique em **Traduzir Agora**.
2. **Esperado:** tela do tradutor aparece com botão 🎤 e mensagem "⚠️ Seu navegador não suporta reconhecimento de voz" (pode variar conforme o navegador).
3. Clique no botão 🎤.
4. **Esperado:** botão fica vermelho com pulse animation, status vira "Ouvindo (demo)...", texto começa a aparecer na transcript-box, trocando a cada 1.5s.
5. Após 4 trocas (6 segundos), status volta pra "Parado", botão volta ao normal.
6. Clique em **Limpar** → texto some, botões Limpar/Salvar ficam desabilitados.
7. Clique no botão 🎤 de novo → texto volta a aparecer. Após parar, clique **Salvar Sessão** → toast verde "Sessão salva com sucesso!".

- [ ] **Step 4: Verificar persistência da sessão salva**

1. Abra DevTools → Application → Local Storage → `file://` → `bf_state_v1`.
2. **Esperado:** dentro do JSON, array `sessions` tem pelo menos 1 item com `texto`, `dataISO`, `duracaoSeg`.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: view tradutor com Web Speech API e fallback modo demo"
```

---



### Task 8: View — Dicionário com busca e filtro por categoria

**Files:**
- Modify: `index.html` (substituir stub `renderDicionario` + adicionar CSS)

**Objetivo:** Renderizar grid de cards de sinais com barra de busca e chips horizontais de filtro por categoria. Estado de busca vazia deve mostrar mensagem amigável.

**Interfaces:**
- Substitui: `function renderDicionario()` — busca + filtro + grid.
- Consome: `SINAIS`, `CATEGORIAS_SINAIS` (das Tasks 4).
- ID exposto: `dict-search` (input), `dict-categories` (chips), `dict-grid` (container). Usado pela busca global da Task 6.

- [ ] **Step 1: Adicionar CSS do dicionário**

Antes do bloco `/* === VIEW: TRADUTOR === */`, adicione:

```css
    /* === VIEW: DICIONARIO === */
    .dict-toolbar { margin-bottom: 20px; }
    .dict-search-bar {
      display: flex;
      align-items: center;
      gap: 8px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 12px 16px;
      margin-bottom: 16px;
    }
    .dict-search-bar input {
      flex: 1;
      background: none;
      border: none;
      outline: none;
      color: var(--text);
      font-size: 15px;
      font-family: inherit;
    }
    .dict-search-bar input::placeholder { color: var(--text-muted); }
    .category-chips {
      display: flex;
      gap: 8px;
      overflow-x: auto;
      padding-bottom: 8px;
      scrollbar-width: thin;
    }
    .category-chips::-webkit-scrollbar { height: 4px; }
    .category-chips::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
    .chip {
      padding: 6px 14px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      font-size: 13px;
      white-space: nowrap;
      color: var(--text-muted);
      transition: all 0.2s;
    }
    .chip.active, .chip:hover {
      background: var(--accent);
      color: #1A1A1A;
      border-color: var(--accent);
    }
    .signs-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
    }
    .sign-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 16px;
      cursor: pointer;
      transition: transform 0.2s, border-color 0.2s;
    }
    .sign-card:hover {
      transform: translateY(-2px);
      border-color: var(--accent);
    }
    .sign-icon {
      font-size: 36px;
      display: block;
      margin-bottom: 8px;
    }
    .sign-term {
      font-weight: 700;
      font-size: 16px;
      margin-bottom: 4px;
    }
    .sign-desc {
      font-size: 12px;
      color: var(--text-muted);
      line-height: 1.4;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }
    .sign-cat-badge {
      display: inline-block;
      margin-top: 8px;
      padding: 2px 8px;
      background: var(--surface-elevated);
      border-radius: 10px;
      font-size: 10px;
      color: var(--text-muted);
    }
    .empty-state {
      text-align: center;
      padding: 60px 20px;
      color: var(--text-muted);
    }
    .empty-state .empty-icon { font-size: 48px; margin-bottom: 16px; display: block; }
    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.6);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 50;
      padding: 20px;
    }
    .modal {
      background: var(--surface);
      border-radius: var(--radius);
      padding: 24px;
      max-width: 400px;
      width: 100%;
      max-height: 80vh;
      overflow-y: auto;
    }
    .modal h2 {
      font-size: 22px;
      margin-bottom: 8px;
    }
    .modal .modal-icon {
      font-size: 64px;
      text-align: center;
      margin-bottom: 16px;
    }
    .modal p { margin-bottom: 12px; line-height: 1.6; }
    .modal .modal-section-title {
      font-weight: 600;
      margin-top: 16px;
      margin-bottom: 4px;
      color: var(--accent);
    }
    .modal-close {
      margin-top: 20px;
      width: 100%;
      padding: 12px;
      background: var(--accent);
      color: #1A1A1A;
      border-radius: var(--radius);
      font-weight: 600;
    }
```

- [ ] **Step 2: Substituir o stub `renderDicionario` pela implementação**

Substitua:

```js
    function renderDicionario() { document.getElementById('app-main').innerHTML = '<h1>Dicionário</h1><p>Em construção.</p>'; }
```

Por:

```js
    let dictState = { busca: '', categoria: 'Todas' };

    function renderDicionario() {
      const main = document.getElementById('app-main');
      main.innerHTML = `
        <h1 style="margin-bottom: 16px;">Dicionário</h1>
        <div class="dict-toolbar">
          <div class="dict-search-bar">
            <span aria-hidden="true">🔍</span>
            <input type="search" id="dict-search" placeholder="Buscar sinais..." value="${escapeHtml(dictState.busca)}" aria-label="Buscar sinais">
          </div>
          <div class="category-chips" id="dict-categories" role="tablist">
            <button class="chip ${dictState.categoria === 'Todas' ? 'active' : ''}" data-cat="Todas" role="tab">Todas</button>
            ${CATEGORIAS_SINAIS.map(c => `<button class="chip ${dictState.categoria === c ? 'active' : ''}" data-cat="${escapeHtml(c)}" role="tab">${escapeHtml(c)}</button>`).join('')}
          </div>
        </div>
        <div id="dict-grid"></div>
      `;

      const searchInput = document.getElementById('dict-search');
      searchInput.addEventListener('input', e => {
        dictState.busca = e.target.value;
        renderSignsGrid();
      });

      document.querySelectorAll('#dict-categories .chip').forEach(chip => {
        chip.addEventListener('click', () => {
          dictState.categoria = chip.dataset.cat;
          document.querySelectorAll('#dict-categories .chip').forEach(c => c.classList.toggle('active', c.dataset.cat === dictState.categoria));
          renderSignsGrid();
        });
      });

      renderSignsGrid();
    }

    function renderSignsGrid() {
      const grid = document.getElementById('dict-grid');
      if (!grid) return;
      const q = dictState.busca.toLowerCase();
      const sinaisFiltrados = SINAIS.filter(s => {
        const matchBusca = !q || s.termo.toLowerCase().includes(q) || s.descricao.toLowerCase().includes(q) || s.categoria.toLowerCase().includes(q);
        const matchCat = dictState.categoria === 'Todas' || s.categoria === dictState.categoria;
        return matchBusca && matchCat;
      });

      if (sinaisFiltrados.length === 0) {
        grid.innerHTML = `
          <div class="empty-state">
            <span class="empty-icon" aria-hidden="true">🔍</span>
            <p>Nenhum sinal encontrado</p>
            <p style="font-size: 13px; margin-top: 8px;">Tente outra palavra ou remova os filtros.</p>
          </div>
        `;
        return;
      }

      grid.innerHTML = `<div class="signs-grid">${sinaisFiltrados.map(s => `
        <button class="sign-card" data-id="${s.id}" style="text-align: left; width: 100%;">
          <span class="sign-icon" aria-hidden="true">${s.icone}</span>
          <div class="sign-term">${escapeHtml(s.termo)}</div>
          <div class="sign-desc">${escapeHtml(s.descricao)}</div>
          <span class="sign-cat-badge">${escapeHtml(s.categoria)}</span>
        </button>
      `).join('')}</div>`;

      grid.querySelectorAll('.sign-card').forEach(card => {
        card.addEventListener('click', () => openSignModal(card.dataset.id));
      });
    }

    function openSignModal(sinalId) {
      const sinal = SINAIS.find(s => s.id === sinalId);
      if (!sinal) return;
      const backdrop = document.createElement('div');
      backdrop.className = 'modal-backdrop';
      backdrop.innerHTML = `
        <div class="modal" role="dialog" aria-labelledby="sign-modal-title">
          <div class="modal-icon" aria-hidden="true">${sinal.icone}</div>
          <h2 id="sign-modal-title">${escapeHtml(sinal.termo)}</h2>
          <p>${escapeHtml(sinal.descricao)}</p>
          <div class="modal-section-title">Exemplo de uso</div>
          <p>${escapeHtml(sinal.exemplo)}</p>
          <div class="modal-section-title">Categoria</div>
          <p>${escapeHtml(sinal.categoria)}</p>
          <button class="modal-close">Fechar</button>
        </div>
      `;
      document.body.appendChild(backdrop);
      const close = () => backdrop.remove();
      backdrop.querySelector('.modal-close').addEventListener('click', close);
      backdrop.addEventListener('click', e => { if (e.target === backdrop) close(); });
    }
```

- [ ] **Step 3: Testar dicionário**

1. Navegue pra **Dicionário** (clique no ícone 📖).
2. **Esperado:** aparece barra de busca + chips horizontais ("Todas", "Saudações", "Família", "Números", "Cores", "Sentimentos", "Cotidiano") + grid com 20 sinais.
3. Digite `obrigado` na busca.
4. **Esperado:** grid filtra pra apenas o card "Obrigado" (id s16).
5. Limpe a busca.
6. Clique no chip **Família**.
7. **Esperado:** grid mostra apenas 3 sinais (Mãe, Pai, Irmão/Irmã). Chip "Família" fica destacado em âmbar.
8. Clique no card **Mãe**.
9. **Esperado:** modal abre com ícone 👩, título "Mãe", descrição completa, exemplo de uso, categoria. Botão "Fechar" ou clicar fora fecha.
10. Digite `xyz` na busca + chip "Todas".
11. **Esperado:** empty state com ícone 🔍 e mensagem "Nenhum sinal encontrado".

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: view dicionario com busca, filtro de categoria e modal de detalhe"
```

---

### Task 9: View — Aulas com lista, detalhe e quiz

**Files:**
- Modify: `index.html` (substituir stub `renderAulas` + adicionar CSS)

**Objetivo:** Renderizar lista de aulas com badge de nível, tela de detalhe com tópicos (checkbox de progresso) e mini-quiz. Salvar progresso em `state.lessonProgress`.

**Interfaces:**
- Substitui: `function renderAulas()` — renderiza lista OU detalhe baseado em URL.
- Função adicional: `function renderAula(id)` — detalhe da aula individual.
- Consome: `AULAS` (Task 5).
- Persiste em `state.lessonProgress[aulaId]`.

- [ ] **Step 1: Adicionar CSS das aulas**

Antes do bloco `/* === VIEW: DICIONARIO === */`, adicione:

```css
    /* === VIEW: AULAS === */
    .lesson-list { display: flex; flex-direction: column; gap: 12px; }
    .lesson-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 16px;
      cursor: pointer;
      display: flex;
      gap: 12px;
      align-items: center;
      transition: border-color 0.2s, transform 0.2s;
    }
    .lesson-card:hover { border-color: var(--accent); transform: translateX(2px); }
    .lesson-card.completed { border-left: 4px solid var(--success); }
    .lesson-icon {
      font-size: 32px;
      flex-shrink: 0;
      width: 56px;
      height: 56px;
      display: flex;
      align-items: center;
      justify-content: center;
      background: var(--surface-elevated);
      border-radius: var(--radius);
    }
    .lesson-info { flex: 1; min-width: 0; }
    .lesson-nivel {
      display: inline-block;
      font-size: 11px;
      padding: 2px 8px;
      border-radius: 10px;
      margin-bottom: 4px;
      font-weight: 600;
    }
    .lesson-nivel.Iniciante { background: #16A34A33; color: var(--success); }
    .lesson-nivel.Intermediário { background: #F59E0B33; color: var(--accent); }
    .lesson-nivel.Avançado { background: #DC262633; color: var(--error); }
    .lesson-title { font-weight: 600; font-size: 15px; margin-bottom: 4px; }
    .lesson-desc { font-size: 12px; color: var(--text-muted); line-height: 1.4; }
    .lesson-progress-mini {
      font-size: 11px;
      color: var(--accent);
      margin-top: 4px;
    }
    .back-button {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 8px 14px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      font-size: 14px;
      margin-bottom: 16px;
    }
    .back-button:hover { border-color: var(--accent); }
    .lesson-detail h1 { font-size: 22px; margin-bottom: 8px; }
    .lesson-detail .lesson-desc-full {
      color: var(--text-muted);
      font-size: 14px;
      margin-bottom: 24px;
    }
    .topics-list { display: flex; flex-direction: column; gap: 12px; margin-bottom: 24px; }
    .topic-item {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 16px;
    }
    .topic-header {
      display: flex;
      align-items: flex-start;
      gap: 12px;
      cursor: pointer;
      user-select: none;
    }
    .topic-checkbox {
      width: 22px;
      height: 22px;
      border: 2px solid var(--border);
      border-radius: 6px;
      flex-shrink: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-top: 2px;
    }
    .topic-item.done .topic-checkbox {
      background: var(--success);
      border-color: var(--success);
      color: white;
    }
    .topic-title { font-weight: 600; flex: 1; }
    .topic-content {
      color: var(--text-muted);
      font-size: 14px;
      line-height: 1.6;
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.3s, padding-top 0.3s;
    }
    .topic-item.done .topic-content { padding-top: 12px; max-height: 500px; }
    .quiz-container {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 20px;
    }
    .quiz-container h2 {
      font-size: 18px;
      margin-bottom: 16px;
      text-align: center;
    }
    .quiz-question { margin-bottom: 16px; }
    .quiz-question-text {
      font-weight: 600;
      margin-bottom: 12px;
      font-size: 15px;
    }
    .quiz-option {
      display: block;
      width: 100%;
      padding: 12px 16px;
      margin-bottom: 8px;
      background: var(--surface-elevated);
      border: 2px solid transparent;
      border-radius: var(--radius);
      text-align: left;
      font-size: 14px;
      transition: all 0.2s;
    }
    .quiz-option:hover:not(.locked) { border-color: var(--accent); }
    .quiz-option.correct { background: var(--success); color: white; border-color: var(--success); }
    .quiz-option.wrong { background: var(--error); color: white; border-color: var(--error); }
    .quiz-option.locked { cursor: not-allowed; }
    .quiz-result {
      text-align: center;
      padding: 16px;
      margin-top: 16px;
      border-radius: var(--radius);
      font-weight: 600;
    }
    .quiz-result.pass { background: #16A34A33; color: var(--success); }
    .quiz-result.fail { background: #DC262633; color: var(--error); }
```

- [ ] **Step 2: Substituir o stub `renderAulas` pela implementação**

Substitua:

```js
    function renderAulas() { document.getElementById('app-main').innerHTML = '<h1>Aulas</h1><p>Em construção.</p>'; }
```

Por:

```js
    function renderAulas(aulaId) {
      if (aulaId) {
        renderAula(aulaId);
      } else {
        renderAulaList();
      }
    }

    function renderAulaList() {
      const main = document.getElementById('app-main');
      main.innerHTML = `
        <h1 style="margin-bottom: 20px;">Aulas Acessíveis</h1>
        <div class="lesson-list" id="lesson-list"></div>
      `;
      const list = document.getElementById('lesson-list');
      list.innerHTML = AULAS.map(a => {
        const prog = state.lessonProgress[a.id];
        const feito = prog && prog.quizNota !== undefined && prog.quizNota >= 2;
        const topicosFeitos = prog ? prog.topicosFeitos.length : 0;
        const totalTopicos = a.topicos.length;
        return `
          <button class="lesson-card ${feito ? 'completed' : ''}" data-id="${a.id}" style="text-align: left; width: 100%;">
            <div class="lesson-icon" aria-hidden="true">${a.nivel === 'Iniciante' ? '🌱' : a.nivel === 'Intermediário' ? '🌿' : '🌳'}</div>
            <div class="lesson-info">
              <span class="lesson-nivel ${a.nivel}">${a.nivel}</span>
              <div class="lesson-title">${escapeHtml(a.titulo)}</div>
              <div class="lesson-desc">${escapeHtml(a.descricao)}</div>
              ${prog ? `<div class="lesson-progress-mini">${topicosFeitos}/${totalTopicos} tópicos${prog.quizNota !== undefined ? ` · Quiz: ${prog.quizNota}/3` : ''}</div>` : ''}
            </div>
          </button>
        `;
      }).join('');

      list.querySelectorAll('.lesson-card').forEach(card => {
        card.addEventListener('click', () => navigate(`#/aulas/${card.dataset.id}`));
      });
    }

    function renderAula(aulaId) {
      const aula = AULAS.find(a => a.id === aulaId);
      if (!aula) {
        document.getElementById('app-main').innerHTML = '<p>Aula não encontrada.</p><button class="back-button" onclick="navigate(\\'#/aulas\\')">← Voltar</button>';
        return;
      }

      if (!state.lessonProgress[aula.id]) {
        state.lessonProgress[aula.id] = { topicosFeitos: [], quizNota: null };
        saveState();
      }
      const prog = state.lessonProgress[aula.id];

      const main = document.getElementById('app-main');
      main.innerHTML = `
        <button class="back-button" onclick="navigate('#/aulas')">← Voltar</button>
        <section class="lesson-detail">
          <h1>${escapeHtml(aula.titulo)}</h1>
          <p class="lesson-desc-full">${escapeHtml(aula.descricao)}</p>
          <div class="topics-list" id="topics-list"></div>
          <div class="quiz-container" id="quiz-container"></div>
        </section>
      `;

      // Renderiza tópicos
      const topicsList = document.getElementById('topics-list');
      topicsList.innerHTML = aula.topicos.map((t, i) => `
        <div class="topic-item ${prog.topicosFeitos.includes(i) ? 'done' : ''}" data-idx="${i}">
          <div class="topic-header">
            <div class="topic-checkbox" role="checkbox" aria-checked="${prog.topicosFeitos.includes(i)}">${prog.topicosFeitos.includes(i) ? '✓' : ''}</div>
            <div class="topic-title">${escapeHtml(t.titulo)}</div>
          </div>
          <div class="topic-content">${escapeHtml(t.conteudo)}</div>
        </div>
      `).join('');

      topicsList.querySelectorAll('.topic-item').forEach(item => {
        item.querySelector('.topic-header').addEventListener('click', () => {
          const idx = parseInt(item.dataset.idx);
          const feitoIdx = prog.topicosFeitos.indexOf(idx);
          if (feitoIdx >= 0) {
            prog.topicosFeitos.splice(feitoIdx, 1);
            item.classList.remove('done');
          } else {
            prog.topicosFeitos.push(idx);
            item.classList.add('done');
          }
          saveState();
          const cb = item.querySelector('.topic-checkbox');
          cb.innerHTML = prog.topicosFeitos.includes(idx) ? '✓' : '';
          cb.setAttribute('aria-checked', prog.topicosFeitos.includes(idx));
        });
      });

      // Renderiza quiz
      renderQuiz(aula, prog);
    }

    function renderQuiz(aula, prog) {
      const container = document.getElementById('quiz-container');
      const respostas = prog.quizRespostas || {};
      const finalizado = prog.quizNota !== null && prog.quizNota !== undefined;

      let notaCalculada = 0;
      aula.quiz.forEach((q, i) => {
        if (respostas[i] === q.correta) notaCalculada++;
      });

      container.innerHTML = `
        <h2>📝 Quiz — ${aula.titulo}</h2>
        ${aula.quiz.map((q, qi) => `
          <div class="quiz-question">
            <div class="quiz-question-text">${qi + 1}. ${escapeHtml(q.pergunta)}</div>
            ${q.opcoes.map((op, oi) => {
              const selected = respostas[qi];
              let cls = '';
              if (finalizado) {
                cls = 'locked';
                if (oi === q.correta) cls += ' correct';
                else if (oi === selected && oi !== q.correta) cls += ' wrong';
              }
              return `<button class="quiz-option ${cls}" data-qi="${qi}" data-oi="${oi}" ${finalizado ? 'disabled' : ''}>${escapeHtml(op)}</button>`;
            }).join('')}
          </div>
        `).join('')}
        ${finalizado ? `
          <div class="quiz-result ${notaCalculada >= 2 ? 'pass' : 'fail'}">
            ${notaCalculada >= 2 ? '🎉 Parabéns!' : '📚 Continue estudando'} — Você acertou ${notaCalculada} de ${aula.quiz.length}.
            <br><button class="btn btn-secondary" id="retry-quiz" style="margin-top: 12px;">Tentar novamente</button>
          </div>
        ` : ''}
      `;

      if (!finalizado) {
        container.querySelectorAll('.quiz-option:not(.locked)').forEach(btn => {
          btn.addEventListener('click', () => {
            const qi = parseInt(btn.dataset.qi);
            const oi = parseInt(btn.dataset.oi);
            respostas[qi] = oi;
            prog.quizRespostas = respostas;
            // Marca visualmente
            container.querySelectorAll(`.quiz-option[data-qi="${qi}"]`).forEach(o => {
              o.classList.add('locked');
              if (parseInt(o.dataset.oi) === oi) {
                o.classList.add(oi === aula.quiz[qi].correta ? 'correct' : 'wrong');
              }
            });
            // Se todas respondidas, finaliza
            if (Object.keys(respostas).length === aula.quiz.length) {
              let nota = 0;
              aula.quiz.forEach((q, i) => { if (respostas[i] === q.correta) nota++; });
              prog.quizNota = nota;
              saveState();
              setTimeout(() => renderQuiz(aula, prog), 600);
            } else {
              saveState();
            }
          });
        });
      } else {
        const retryBtn = document.getElementById('retry-quiz');
        if (retryBtn) {
          retryBtn.addEventListener('click', () => {
            prog.quizNota = null;
            prog.quizRespostas = {};
            saveState();
            renderQuiz(aula, prog);
          });
        }
      }
    }
```

- [ ] **Step 3: Atualizar o roteador pra suportar `#/aulas/:id`**

No bloco `routes` do JS, **substitua**:

```js
      '#/aulas': () => renderAulas()
```

Por:

```js
      '#/aulas': () => renderAulas(),
      '#/aulas/a1': () => renderAulas('a1'),
      '#/aulas/a2': () => renderAulas('a2'),
      '#/aulas/a3': () => renderAulas('a3'),
      '#/aulas/a4': () => renderAulas('a4'),
      '#/aulas/a5': () => renderAulas('a5'),
      '#/aulas/a6': () => renderAulas('a6')
```

- [ ] **Step 4: Testar aulas — lista**

1. Navegue pra **Aulas** (clique no ícone 🎓).
2. **Esperado:** 6 cards de aula aparecem com badges de nível coloridos (verde/Iniciante, âmbar/Intermediário, vermelho/Avançado), ícone da planta (🌱 🌿 🌳), título, descrição.

- [ ] **Step 5: Testar aulas — detalhe e progresso**

1. Clique no card **Alfabeto Manual** (primeira aula).
2. **Esperado:** carrega detalhe da aula com 3 tópicos colapsados e o quiz embaixo (3 perguntas, 4 opções cada).
3. Clique no checkbox de "O que é o alfabeto manual?". 
4. **Esperado:** checkbox marca ✓ (verde), conteúdo do tópico expande embaixo.
5. Marque os 3 tópicos. **Esperado:** todos marcados.
6. Recarregue a página (F5).
7. **Esperado:** ao voltar em `#/aulas/a1`, os 3 tópicos ainda estão marcados.

- [ ] **Step 6: Testar quiz**

1. Ainda na aula, responda o quiz: clique na opção **correta** da pergunta 1, opção **errada** da pergunta 2, opção **correta** da pergunta 3.
2. **Esperado:** cada opção clicada fica com cor (verde se correta, vermelho se errada) e trava. Após a 3ª resposta, aparece o resultado: "Você acertou X de 3" + botão "Tentar novamente".

Para validar as respostas corretas, consulte na Task 5: `a1.quiz[0].correta = 1`, `a1.quiz[1].correta = 1`, `a1.quiz[2].correta = 1`. As outras opções são `0`, `2`, `3`.

3. Clique **Tentar novamente**. **Esperado:** respostas resetam, botões voltam ao estado original.
4. Clique no botão **← Voltar**. **Esperado:** volta pra lista de aulas.
5. A aula concluída deve ter borda verde à esquerda.

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: view aulas com lista, detalhe, topicos com progresso e quiz"
```

---

### Task 10: CSS Responsivo + Acessibilidade (media queries e focus styles)

**Files:**
- Modify: `index.html` (adicionar media queries no final do `<style>`)

**Objetivo:** Garantir layout responsivo (mobile, tablet, desktop) e foco visível pra acessibilidade de teclado.

- [ ] **Step 1: Adicionar media queries**

Adicione ao final do `<style>`, **antes** de `</style>`:

```css
    /* === RESPONSIVO === */
    @media (min-width: 768px) {
      .quick-actions {
        grid-template-columns: repeat(4, 1fr);
      }
      .signs-grid {
        grid-template-columns: repeat(3, 1fr);
      }
      #bottom-nav {
        position: sticky;
        bottom: 0;
        max-width: 800px;
        margin: 0 auto;
        left: 0;
        right: 0;
      }
      #app-main {
        padding: 30px;
      }
    }

    @media (min-width: 1024px) {
      .signs-grid {
        grid-template-columns: repeat(4, 1fr);
      }
      .quick-actions {
        max-width: 600px;
        margin: 0 auto;
      }
    }

    /* === FOCO ACESSÍVEL === */
    button:focus-visible,
    a:focus-visible,
    input:focus-visible {
      outline: 3px solid var(--accent);
      outline-offset: 2px;
    }

    /* === REDUCED MOTION === */
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
      }
    }
```

- [ ] **Step 2: Testar responsividade**

1. Abra DevTools (F12) → modo dispositivo (Ctrl+Shift+M no Chrome).
2. **375px (mobile):** Bottom nav fixa embaixo. Cards de quick actions em 2 colunas. Grid de sinais em 2 colunas. Bottom nav com 4 ícones.
3. **768px (tablet):** Quick actions em 4 colunas. Grid de sinais em 3 colunas. Bottom nav vira sticky (mesma posição mas centralizada).
4. **1280px (desktop):** Grid de sinais em 4 colunas. Quick actions centralizadas com max-width.

- [ ] **Step 3: Testar foco de teclado**

1. Pressione Tab repetidamente desde o topo da página.
2. **Esperado:** cada elemento focável (botões, links, inputs) recebe outline âmbar visível.
3. Navegue até a busca do dicionário, digite algo. **Esperado:** funciona normalmente.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "style: media queries responsivas e estilos de foco acessivel"
```

---



### Task 11: README com instruções de uso

**Files:**
- Create: `README.md` (raiz do projeto)

**Objetivo:** Documentar como abrir e usar o protótipo, funcionalidades e limitações conhecidas.

- [ ] **Step 1: Criar o arquivo `README.md`**

```markdown
# Bridge Fluent — Protótipo Web

Protótipo funcional single-file do app **Bridge Fluent** (Libras 5.0: Educação com IA), uma plataforma de educação inclusiva com tradução de voz para texto, dicionário de sinais e aulas acessíveis.

## Como abrir

1. Abra o Explorador de Arquivos na pasta do projeto.
2. Duplo-clique em `index.html`.
3. O navegador (Chrome ou Edge recomendados) vai abrir o app.

**Sem build, sem npm, sem servidor.** Tudo funciona direto.

## Funcionalidades

- **🏠 Home:** Saudação dinâmica, 4 cards de atalho e busca global.
- **🎤 Tradutor:** Captura voz em tempo real (Web Speech API) e mostra transcrição. Permite salvar a sessão.
- **📖 Dicionário:** 20 sinais essenciais em 6 categorias (Saudações, Família, Números, Cores, Sentimentos, Cotidiano). Busca + filtro por categoria + modal de detalhe.
- **🎓 Aulas:** 6 aulas de Libras (Iniciante, Intermediário, Avançado) com tópicos interativos e mini-quiz com nota.

## Requisitos

- **Navegador:** Chrome ou Edge (Firefox e Safari não suportam bem a Web Speech API — o app detecta e ativa modo demo automaticamente).
- **Internet:** Não obrigatória após o primeiro carregamento.
- **Microfone:** Opcional. Se bloqueado, o app mostra modo demo.

## Estrutura

- `index.html` — arquivo único com HTML, CSS e JS embutidos.
- `docs/superpowers/specs/` — especificação de design.
- `docs/superpowers/plans/` — plano de implementação.

## Dados e privacidade

- Todos os dados ficam no seu navegador via `localStorage` (chave `bf_state_v1`).
- Nada é enviado pra servidor externo (exceto a fala, que é processada pela Web Speech API do navegador).
- Pra limpar seus dados: abra DevTools → Application → Local Storage → Delete `bf_state_v1`.

## Limitações conhecidas

- **Reconhecimento de voz:** apenas em navegadores Chromium. Outros entram em modo demo.
- **Sem avatar 3D:** a versão atual mostra só o texto reconhecido, não um avatar animado em Libras.
- **Sem reconhecimento de sinais por câmera:** a feature foi explicitamente deixada pra versões futuras (precisa modelo ML).
- **Sem backend:** tudo é local. Sessões e progresso ficam só no seu navegador.
- **Catálogo pequeno:** 20 sinais e 6 aulas são suficientes pra demonstração, não pra uso educacional sério.

## Próximos passos (ideias)

- Reconhecimento de sinais via webcam (TensorFlow.js + MoveNet).
- Integração com VLibras para avatar 3D.
- Mais aulas e sinais (crowdsourcing com a comunidade surda).
- Backend com autenticação e sincronização entre dispositivos.
- PWA com Service Worker para offline real.

## Créditos

Projeto baseado no documento descritivo "Libras 5.0: Educação com IA" (PDF na raiz).
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: README com instrucoes de uso, funcionalidades e limitacoes"
```

---

### Task 12: Auto-revisão final e verificação manual completa

**Files:**
- Read: `index.html` (revisão de placeholder e consistência)

**Objetivo:** Verificar que o arquivo final não tem placeholders, todas as features funcionam end-to-end, e os critérios de pronto da spec estão atendidos.

- [ ] **Step 1: Buscar placeholders no código**

```bash
grep -n -E "TODO|FIXME|XXX|implementar depois|TBD" index.html
```

**Esperado:** Nenhum resultado.

- [ ] **Step 2: Verificar contagem de linhas**

```bash
wc -l index.html
```

**Esperado:** entre 700 e 1100 linhas (estimativa da spec era 700-900).

- [ ] **Step 3: Verificar que todas as views estão implementadas**

```bash
grep -n "function render" index.html
```

**Esperado:** 4 funções: `renderHome`, `renderTradutor`, `renderDicionario`, `renderAulas` (e auxiliares `renderAulaList`, `renderAula`, `renderQuiz`, `renderSignsGrid`).

- [ ] **Step 4: Verificar contagem de dados mock**

```bash
grep -c "id: 's" index.html      # esperado: 20
grep -c "id: 'a[0-9]" index.html  # esperado: 6
```

- [ ] **Step 5: Executar o fluxo de teste manual completo da spec**

Siga exatamente os 10 passos do fluxo documentado na spec (`docs/superpowers/specs/2026-07-05-bridge-fluent-prototipo-web-design.md` seção "Fluxo de teste manual"). Marque cada passo mentalmente como OK ou com problema.

- [ ] **Step 6: Checklist de DoD (Definition of Done)**

Abra o arquivo `index.html` no Chrome. Confirme cada item:

- [ ] Abre sem erros no console (F12 → Console limpo)
- [ ] Home mostra saudação + 4 cards de atalho
- [ ] Bottom nav navega entre todas as 4 telas via hash routing
- [ ] Tradutor: clica mic → aparece texto (real ou demo) → botão Salvar fica ativo → salva sessão → toast aparece
- [ ] Dicionário: 20 sinais visíveis, busca funciona pra "família", "obrigado", "estudar", "azul", "feliz"
- [ ] Filtro de categoria funciona (testar pelo menos 2 categorias)
- [ ] Aulas: 6 cards visíveis, clique abre detalhe, marcar tópico persiste após reload
- [ ] Quiz: respostas certas/erradas marcam visualmente, nota final aparece
- [ ] Recarregar a página preserva sessões salvas, progresso de tópicos e notas de quiz
- [ ] Botão Voltar do navegador funciona entre as telas
- [ ] Tab pelo teclado mostra foco âmbar visível em todos os botões
- [ ] Modo escuro/claro: clique no botão de tema alterna, persiste após reload
- [ ] Responsivo: redimensionar a janela de mobile pra desktop reorganiza layout
- [ ] localStorage tem chave `bf_state_v1` com JSON válido (DevTools → Application → Local Storage)

- [ ] **Step 7: Se algum item falhou, criar uma task de fix no plano**

Se algum item do DoD falhou, **NÃO PROSSIGA**. Volte, identifique o problema, corrija o código, e rode a verificação de novo.

- [ ] **Step 8: Commit de qualquer ajuste (se houve)**

```bash
git add index.html
git commit -m "fix: ajustes finais baseados na auto-revisao manual"
```

(Só rode se houve mudança.)

---

### Task 13: Commit final e tag de release

**Files:** (nenhum — apenas git)

**Objetivo:** Marcar o release do protótipo com tag semântica.

- [ ] **Step 1: Verificar que está tudo commitado**

```bash
git status
```

**Esperado:** "nothing to commit, working tree clean".

- [ ] **Step 2: Criar tag de release**

```bash
git tag -a v0.1.0-prototipo -m "Prototipo funcional Bridge Fluent: 4 features, single-file"
```

- [ ] **Step 3: Verificar a tag**

```bash
git tag -l
git log --oneline -5
```

**Esperado:** tag `v0.1.0-prototipo` aparece. Log mostra os commits das 12 tasks anteriores.

- [ ] **Step 4: Anunciar a entrega ao usuário**

Resuma o que foi entregue:
- Quantas linhas tem o `index.html`
- Quais features funcionam
- Onde está o arquivo
- Como abrir e testar

---

## Self-Review (executada pelo autor do plano)

**1. Spec coverage:**
- ✅ Home com saudação e 4 cards → Task 6
- ✅ Tradutor com Web Speech API → Task 7
- ✅ Dicionário 20 sinais em 6 categorias → Tasks 4 (dados) + 8 (view)
- ✅ Aulas 6 com tópicos e quiz → Tasks 5 (dados) + 9 (view)
- ✅ Hash routing → Task 2
- ✅ localStorage → Task 2
- ✅ Tema dark/light → Task 2 (toggle) + 10 (CSS variáveis)
- ✅ Responsivo → Task 10
- ✅ Acessibilidade (foco, ARIA, contraste) → Tasks 1, 6, 7, 8, 9, 10
- ✅ Tratamento de erros (Web Speech API ausente, permissão negada) → Task 7
- ✅ Sistema de toasts → Task 3
- ✅ README → Task 11

**2. Placeholder scan:**
- Nenhum "TODO"/"TBD"/"implementar depois" no plano.

**3. Type consistency:**
- `state.user.nome` consistente em todas as tasks.
- `state.sessions` array de objetos com `id`, `texto`, `dataISO`, `duracaoSeg` — consistente.
- `state.lessonProgress[aulaId]` com `topicosFeitos` (array de índices), `quizNota` (number|null), `quizRespostas` (objeto) — consistente.
- Função `uid()` definida na Task 3, usada em Tasks 7 e 9.
- Função `escapeHtml()` definida na Task 6, usada em Tasks 6, 8, 9.
- Função `showToast()` definida na Task 3, usada em Tasks 6, 7, 8.
- Função `formatDate()`, `formatDuration()` definidas na Task 3, prontas pra uso futuro.

**4. Verificações:**
- Nenhum conflito de nomes de funções.
- Todas as funções referenciadas estão definidas.
- Nomes de IDs no DOM únicos e referenciados consistentemente.

---

## Execution Handoff

**Plano completo salvo em:**
`C:\Users\paule\Documents\PROGRAMAÇÃO\Libras 5.0 Sem Barreiras Projeto App\docs/superpowers/plans/2026-07-05-bridge-fluent-prototipo-web.md`

**13 tarefas no total:**
1. Scaffold HTML
2. Estado + storage + roteador
3. Sistema de toasts + utilitários
4. Dados mock: 20 sinais
5. Dados mock: 6 aulas
6. View Home
7. View Tradutor (com Web Speech API)
8. View Dicionário
9. View Aulas (com quiz)
10. CSS Responsivo + Acessibilidade
11. README
12. Auto-revisão final + verificação manual
13. Commit final + tag

**Duas opções de execução:**

**1. Subagent-Driven (recomendado)** — Eu disparo um subagent fresh por tarefa, reviso entre tarefas, iteração rápida. Bom pra manter qualidade alta.

**2. Inline Execution** — Eu executo as tarefas nesta sessão com checkpoints pra você revisar. Bom pra você acompanhar de perto cada commit.

Qual abordagem?
