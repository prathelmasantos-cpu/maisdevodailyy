# Instruções para agentes de código (Copilot / agentes AI)

Este projeto é um app web estático em single-file. Use estas instruções para ser produtivo rapidamente e preservar as convenções existentes.

**Visão Rápida**
- **Arquivo principal:** `devodaily-app.html` — contém HTML, CSS e JavaScript (Vanilla) na mesma página.
- **Stack:** HTML5, CSS3, JavaScript (sem bundler). Não há backend no repositório.

**Arquitetura & Fluxos Importantes**
- Telas DOM identificadas por sufixo `Screen`: `loginScreen`, `registerScreen`, `forgotScreen`, `homeScreen`. Use a função `showScreen(screen)` para alternar telas.
- A constante `API_URL` (no topo do `<script>`) aponta atualmente para `http://localhost:3000/api`. Todas as chamadas `fetch` usam essa constante.
- Autenticação espera respostas com formato `{ success, data: { user, token }, message }` (veja os handlers de `login` e `register`).
- Dados de sessão são persistidos em `localStorage` sob as chaves `token` e `user`.

**Padrões e Convenções do Código**
- Mantém HTML/CSS/JS juntos no arquivo principal para simplicidade — pequenas mudanças seguem esse padrão.
- IDs do DOM são referenciados diretamente via `document.getElementById(...)`. Ao renomear um ID, atualize todas as ocorrências JS.
- Objetos de usuário esperados: `currentUser.name` e `currentUser.subscription.plan` (valores esperados: `free`, `premium_individual`, `premium_family`).
- Função `loadDevotional()` usa um array local como fallback; em produção, o conteúdo virá do backend.
- Alertas usam `showAlert(screenId, message, type)` e classes CSS `.alert`, `.alert-success`, `.alert-error`.

**Debug e Fluxos Locais (exemplos práticos)**
- Servir localmente sem backend (preview estático):
  ```bash
  python3 -m http.server 8000
  # ou
  npx http-server -c-1 .
  ```
  Acesse `http://localhost:8000/devodaily-app.html`.

- Simular usuário logado no DevTools (Console):
  ```js
  localStorage.setItem('token','fake-token');
  localStorage.setItem('user', JSON.stringify({ name: 'Teste', subscription: { plan: 'free' } }));
  window.location.reload();
  ```

- Testar chamadas API → observe `Network` e `Console`; erros comuns: CORS ou `API_URL` apontando para backend inexistente.

**O que alterar com cuidado / revisão de PR**
- Mudanças na forma do `user` (`name`, `subscription.plan`) exigem atualização em `loadHome()`.
- Renomear IDs/elementos DOM — execute busca por `getElementById(` e atualize todos os pontos.
- Mover CSS inline para arquivo separado é aceitável, mas acrescente um pequeno PR que atualize o `<head>` e valide estilos em telas mobile (`@media (max-width: 480px)`).

**Integrações e Expectativas de Backend**
- Endpoints usados (front-end):
  - `POST ${API_URL}/auth/login`
  - `POST ${API_URL}/auth/register`
  - `POST ${API_URL}/auth/forgot-password`
- Formato da resposta esperado: `{ success: boolean, data: { user, token }, message: string }`.

**Limitações detectadas**
- Não existem testes automatizados nem CI configurado.
- Não há `manifest.json` ou `service worker` apesar do README mencionar PWA.

**Checklist rápido para agentes que fizerem mudanças**
- Atualizar `README.md` se adicionar rotas/funcionalidades que mudem a UX.
- Documentar novos endpoints e atualizar a `API_URL` usada pelo front-end.
- Verificar `localStorage` keys (`token`, `user`) e `planNames` mapeamento.

Se algo estiver faltando ou você quiser exemplos adicionais (snippets de testes manuais, política de commits, ou checklist estendido para PRs), peça que eu expanda este arquivo.

***
Arquivo gerado automaticamente por agente. Peça ajustes se quiser detalhes extras.
