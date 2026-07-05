# Bridge Fluent / Libras 5.0 — Protótipo Web (Design)

**Data:** 2026-07-05
**Status:** Aprovado (brainstorm concluído)
**Tipo:** Protótipo funcional single-file

## Visão

App web mobile-first que demonstra o conceito do **Bridge Fluent** (Libras 5.0: Educação com IA) com **4 features 100% funcionais** num único arquivo HTML aberto no navegador. Foco em amplitude da visão + profundidade em features-chave.

## Decisões de escopo

- **Objetivo:** Protótipo web demonstrável (não proposta, não MVP de produção).
- **Stack:** HTML5 + CSS3 + JS vanilla, single-file, sem build, sem CDN.
- **Arquitetura:** SPA com hash routing (4 rotas).
- **IA real:** Apenas o tradutor de voz→texto usa Web Speech API do navegador. Resto é mock com dados realistas.
- **Persistência:** localStorage pra dados do usuário (nome, perfil, sessões salvas, progresso de aulas).

## Features incluídas

1. **Home** — saudação dinâmica, 4 cards de atalho, busca global.
2. **Tradutor Voz→Texto** — botão de microfone com Web Speech API, transcrição em tempo real, salvamento de sessão.
3. **Dicionário de Sinais** — 20 sinais mock, busca + filtro por categoria, cards com descrição e ícone.
4. **Aulas Acessíveis** — 6 aulas mock, conteúdo por tópicos, mini-quiz de 3 perguntas, progresso salvo.

## Features explicitamente fora (futuro)

- Reconhecimento de sinais via câmera (precisa modelo ML).
- Avatar 3D animado para Libras (precisa VLibras ou similar).
- Chat multimodal.
- Tradução multilíngue.
- Modo offline real (Service Worker).
- Backend, banco de dados, autenticação real.
- Admin, relatórios, métricas.
- Gamificação, banco colaborativo de sinais.

## Arquitetura técnica

### Arquivo único

`C:\Users\paule\Documents\PROGRAMAÇÃO\Libras 5.0 Sem Barreiras Projeto App\index.html`

Estimativa: ~700-900 linhas (HTML + CSS embutido + JS embutido).

### Estrutura interna

```
<head>        → meta, title, <style> com CSS variables
<body>
  <header>    → top bar fixa
  <main>      → container que renderiza a view ativa
  <nav>       → bottom nav (mobile-first)
  <script>    → JS no fim do body
</body>
```

### Roteamento

Hash-based, sem dependências:

```js
const routes = {
  '#/': renderHome,
  '#/tradutor': renderTradutor,
  '#/dicionario': renderDicionario,
  '#/aulas': renderAulas
}
window.addEventListener('hashchange', router)
router() // on load
```

### Estado global

Objeto simples + persistência:

```js
const state = {
  user: { nome, perfil, tema },
  sessions: [],
  lessonProgress: {}
}
```

## Layout e visual

### Paleta (aprovada)

- **Primária (azul profundo):** `#0F2A47` (background), `#1E3A5F` (surface)
- **Accent (âmbar quente):** `#F59E0B` (CTAs, destaques)
- **Texto principal:** `#F8FAFC` (claro no dark)
- **Borda:** `#334155`
- **Sucesso:** `#22C55E`
- **Erro:** `#EF4444`

### Tema

- **Dark por padrão** (conforme spec do PDF)
- Toggle no header → salva em localStorage
- CSS variables permitem trocar tema sem re-render

### Responsividade

- Mobile-first (375px base)
- Breakpoint em 768px (tablet) e 1024px (desktop)
- Bottom nav mobile (4 ícones); no desktop vira top nav horizontal

### Acessibilidade

- Contraste mínimo AA (4.5:1)
- HTML semântico (`<nav>`, `<main>`, `<button>`, `<label>`)
- `aria-label` em ícones
- Foco visível (outline custom)
- Suporte a `prefers-reduced-motion`

## Dados mock

### Sinais (20 itens)

Categorias: Saudações (3), Família (3), Números (3), Cores (3), Sentimentos (3), Cotidiano (5).

```js
{ id, termo, descricao, categoria, exemplo, icone }
```

### Aulas (6 itens)

Níveis: 3 Iniciante, 2 Intermediário, 1 Avançado.

```js
{
  id, titulo, nivel, descricao,
  topicos: [{ titulo, conteudo }],
  quiz: [{ pergunta, opcoes: [4 strings], correta: 0-3 }]
}
```

Tópicos cobertos: Alfabeto manual, Cumprimentos básicos, Verbos essenciais, Estrutura frasal, Sinais do cotidiano, Cultura surda.

### localStorage

- `bf_user` → `{ nome: string, perfil: 'aluno'|'professor', tema: 'dark'|'light' }`
- `bf_sessions` → `[{ id, texto, dataISO, duracaoSeg }]`
- `bf_lesson_progress` → `{ [aulaId]: { topicosFeitos: [indices], quizNota: number } }`

## Tratamento de erros

| Cenário | Comportamento |
|---|---|
| Web Speech API indisponível (Firefox/Safari sem flag) | Botão muda pra "Modo Demo" e mostra texto simulado digitando aos poucos |
| Permissão de microfone negada | Toast explicativo + instrução pra permitir no browser |
| localStorage desabilitado (modo privado) | App funciona; banner avisa que progresso não persistirá |
| Busca sem resultados | Empty state amigável: "Nenhum sinal encontrado pra 'xyz'" |
| Sessão de tradução vazia (usuário clicou Salvar sem falar nada) | Botão Salvar fica desabilitado até ter texto |

## Fluxo de teste manual

1. Abre `index.html` no Chrome/Edge
2. Vê Home com saudação "Olá!"
3. Clica **Traduzir Agora** → vai pra `#/tradutor`
4. Clica botão de microfone → permite acesso
5. Fala "Olá, tudo bem?" → texto aparece embaixo
6. Clica **Salvar Sessão** → toast "Sessão salva"
7. Volta pra Home (botão ← ou bottom nav)
8. Clica **Dicionário** → busca "família" → vê 3 cards
9. Clica **Aulas** → escolhe "Cumprimentos básicos" → marca tópicos feitos → responde quiz (3 perguntas) → vê nota final
10. Volta pra Home → recarrega a página → progresso e sessões persistiram

## Critérios de pronto (DoD)

- Arquivo abre em Chrome/Edge sem erros no console
- Todas as 4 telas renderizam corretamente
- Bottom nav navega entre telas via hash routing
- Tradutor realmente captura voz e mostra texto (testado com microfone real)
- Dicionário retorna resultados pra 5+ buscas diferentes
- Quiz das aulas marca resposta certa/errada e calcula nota
- Tema escuro/claro alterna sem quebrar layout
- localStorage persiste sessões e progresso entre reloads
- Botão Voltar do navegador funciona
- Acessibilidade: navegação por teclado completa, foco visível
- Responsivo: testado em 375px (mobile), 768px (tablet), 1280px (desktop)
- Funciona abrindo o arquivo direto (file://) sem servidor

## Próximo passo

Salvar esta spec, commitar no git local, e usar a skill `superpowers:writing-plans` pra gerar o plano de implementação detalhado.
