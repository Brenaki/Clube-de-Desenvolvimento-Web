# AGENTS.md

Repositório de materiais de aula do Clube de Desenvolvimento Web. Sem códigos de aplicação, sem build/test/lint — apenas slides.

## Estrutura

- Uma pasta por semana: `000N - Semana/`. Note que `0002 - Semana ` tem um **espaço no final do nome** (histórico); não "corrija" isso sem pedir.
- Cada semana contém:
  - `README.md` — fonte da apresentação em **Marp** (front-matter `marp: true` com CSS embutido). Edite aqui.
  - `README.html` — export HTML gerado pelo Marp CLI. Atualize-o ao alterar o `.md` (Semana 1 e 2 usam nomes históricos diferentes: `clube-dev-web.html`, `semana2-clube-web.html`).
  - `assets/` — SVGs de diagramas (quando existem).

## Comandos

- Exportar slides: `npx @marp-team/marp-cli README.md -o README.html` (rode dentro da pasta da semana; sem `package.json` no repo, o `npx` baixa o CLI sob demanda).

## Convenções

- Idioma: conteúdo em português; mantenha termos técnicos originais quando já usados (ex.: `localStorage`, `JWT`).
- Mensagens de commit seguem o padrão: `add: Semana N do clube DW` (ou `rename:` quando for renomeação).
- Título/cabeçalho de cada semana segue `# NNNN - Semana N: <tema>` — verifique o padrão nos READMEs existentes antes de criar uma semana nova.