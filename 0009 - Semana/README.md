---
marp: true
theme: default
paginate: true
size: 16:9
style: |
  section {
    background-color: #ffffff;
    color: #1a1a2e;
    font-family: 'Segoe UI', 'Inter', sans-serif;
    font-size: 23px;
    line-height: 1.45;
    padding: 40px 52px;
  }

  h1 {
    font-size: 38px;
    font-weight: 800;
    color: #1a1a2e;
    letter-spacing: -0.02em;
    margin-bottom: 0.35em;
  }

  h2 {
    font-size: 28px;
    font-weight: 700;
    color: #1a1a2e;
    margin-bottom: 0.3em;
  }

  h3 {
    font-size: 22px;
    font-weight: 700;
    color: #be123c;
    text-transform: uppercase;
    letter-spacing: 0.04em;
    margin: 0.8em 0 0.3em;
  }

  p {
    margin: 0.35em 0;
  }

  strong {
    color: #1a1a2e;
    font-weight: 700;
  }

  em {
    color: #6b7280;
  }

  code {
    background: #f3f4f6;
    color: #9d174d;
    padding: 1px 6px;
    border-radius: 3px;
    font-size: 0.88em;
    font-family: 'Consolas', 'Courier New', monospace;
    font-weight: 600;
  }

  pre {
    background: #f9fafb;
    border: 1px solid #e5e7eb;
    border-left: 4px solid #be123c;
    padding: 10px 16px;
    border-radius: 0 6px 6px 0;
    font-size: 17px;
    line-height: 1.3;
    margin: 0.5em 0;
  }

  pre code {
    background: transparent;
    color: #111827;
    padding: 0;
    font-size: 1em;
    font-weight: 400;
  }

  blockquote {
    background: #fff1f2;
    border-left: 4px solid #be123c;
    color: #4b5563;
    padding: 8px 16px;
    border-radius: 0 6px 6px 0;
    margin: 0.7em 0;
    font-style: normal;
    font-size: 0.9em;
  }

  ul, ol {
    padding-left: 1.3em;
    margin: 0.25em 0;
  }

  li {
    margin-bottom: 0.35em;
  }

  table {
    font-size: 18px;
    width: 100%;
    border-collapse: collapse;
    margin-top: 0.5em;
  }

  th {
    background: #1a1a2e;
    color: #ffffff;
    padding: 6px 10px;
    text-align: left;
    font-weight: 600;
    font-size: 0.8em;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  td {
    background: #ffffff;
    padding: 6px 10px;
    border-bottom: 1px solid #e5e7eb;
    color: #374151;
    vertical-align: top;
  }

  tr:nth-child(even) td {
    background: #f9fafb;
  }

  .nota {
    font-size: 0.85em;
    color: #4b5563;
    background: #fff1f2;
    border-left: 4px solid #be123c;
    padding: 8px 16px;
    border-radius: 0 6px 6px 0;
    margin-top: 0.8em;
  }

  section.cover {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    background: #ffffff;
    border-top: 6px solid #be123c;
  }

  section.cover h1 {
    font-size: 52px;
    border: none;
    padding: 0;
    margin-bottom: 0.15em;
    line-height: 1.15;
  }

  section.cover h2 {
    color: #be123c;
    font-size: 0.95rem;
    letter-spacing: 0.14em;
    font-weight: 700;
    text-transform: uppercase;
    margin-bottom: 1em;
  }

  section.cover p {
    color: #6b7280;
    font-size: 0.95rem;
  }
---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 9

# CRUD com Bun e Docker

Construindo o backend do seu blog pessoal

---

## A aula muda hoje

Até aqui: **8 semanas de fundamentos** — HTTP, segurança, banco de dados, API, auth, modelagem, algoritmos.

A partir de hoje: **construir**. Cada encontro entrega uma peça do seu blog pessoal.

Hoje você sai daqui com:

- uma **API própria** respondendo HTTP
- rodando **dentro de um container Docker**

<div class="nota">
Fundamento sem prática esquece. Prática sem fundamento copia.
Agora nós temos os dois — e a aula vira oficina.
</div>

---

## O destino: o seu blog pessoal

| Semana | Entrega |
|---|---|
| **9 — hoje** | CRUD da API em Bun, rodando no Docker |
| 10 | O array vira banco: PostgreSQL + Prisma |
| 11 | Frontend do blog |
| 12 | Auth e segurança |
| 13 | Produção: deploy e DevOps |

No caminho: **ORM, banco relacional, requisições, segurança, DevOps**.

<div class="nota">
Cada participante sai do final do ano com um blog próprio,
feito por você, rodando em produção.
</div>

---

## Agenda

| Bloco | Assunto |
|---|---|
| 1 | O que é Bun e por que ele existe |
| 2 | Primeiro servidor com `Bun.serve` |
| 3 | Testes, TDD e Refactoring |
| 4 | CRUD: as quatro operações de um blog |
| 5 | A API dentro de um container Docker |

Entre os blocos: **prática na sua máquina**, sempre.

---

<!-- _class: cover -->

## Bloco 1

# O que é Bun e por que ele

---

## O que falta para o blog existir

Na Semana 5 vocês **consumiram** uma API: o navegador pedia, alguém respondia.

Quem escreve o lado que **responde**?

```
navegador  --requisicao-->  ??????  --resposta-->  navegador
                           um programa ouvindo uma porta
```

Esse programa é o **backend** do blog. Pra escrevê-lo, precisamos de:

1. uma **linguagem** — JavaScript/TypeScript, que vocês já conhecem
2. um **runtime** — o programa que executa essa linguagem no servidor

<div class="nota">
JavaScript sozinho não roda em servidor nenhum.
Ele precisa de um runtime — e é aí que o Bun entra.
</div>

---

## O que é um runtime?

JavaScript é só uma **especificação**. Quem executa o código é uma **engine**:

| Engine | Quem usa |
|---|---|
| **V8** (Google) | Chrome, Node.js |
| **JavaScriptCore** (Apple) | Safari, **Bun** |

A engine só executa o programa. O **runtime** é a engine **+ as APIs** que o código usa para falar com o mundo externo: arquivos, rede, sistema.

<div class="nota">
Analogia: a engine é o motor. O runtime é o carro inteiro —
motor, mais tudo o que faz o motor ser útil.
</div>

---

## Mesma linguagem, mundos diferentes

| Onde roda | O que o JavaScript enxerga |
|---|---|
| Navegador | `window`, `fetch`, DOM, `localStorage` (Semana 3) |
| Node.js | `process`, `node:fs`, `node:http` |
| **Bun** | as APIs Web **e** as do Node |

O runtime define **qual mundo** o seu código enxerga.

---

## Bun em uma frase

> Bun é um **kit completo** para JavaScript e TypeScript:
> um único binário com **runtime, gerenciador de pacotes,
> test runner e bundler**.

Ele nasceu como **substituto direto do Node.js** — mais rápido e mais moderno — movido pela engine **JavaScriptCore**, a mesma do Safari.

---

## O que o Bun substitui

| Ferramenta que você já ouviu falar | No Bun |
|---|---|
| `node` | `bun run` |
| `npm` | `bun install` |
| `jest` | `bun test` |
| `tsc` / `tsx` (rodar TypeScript) | **não precisa — é nativo** |

Quatro ferramentas, um executável só.

<div class="nota">
O Bun foi escrito em Zig e C++, sobre a engine JavaScriptCore.
O Node roda sobre a V8. Engines diferentes, propostas iguais.
</div>

---

## Por que Bun — para nós, agora

1. **TypeScript sem configuração** — `.ts` roda direto. Zero build, zero `tsconfig`
2. **Servidor HTTP nativo** — `Bun.serve` faz o trabalho do Express **sem instalar nada**
3. **Rápido** — o processo Bun inicia ~4x mais rápido que o Node (`5.2ms` vs `25.1ms` no hello world da documentação)
4. **Nivelamento** — todo mundo começa igual: um binário, zero dependências, um arquivo

<div class="nota">
Ponto 4 é o mais importante hoje: sem dependência instalada,
não existe "na minha máquina não roda".
</div>

---

## Instalação

```bash
# Linux / macOS / WSL
curl -fsSL https://bun.sh/install | bash

# Windows (PowerShell)
powershell -c "irm bun.sh/install.ps1 | iex"
```

Verificar:

```bash
bun --version
```

<div class="nota">
Se o install falhar na sua máquina, levante a mão.
O caminho alternativo é pular direto para o Docker —
a imagem oficial do Bun já vem com ele dentro.
</div>

---

<!-- _class: cover -->

## Atividade 1

# Hello world

---

## Prática — 5 minutos

Crie a pasta do projeto e um arquivo TypeScript:

```bash
mkdir blog-api && cd blog-api
```

```ts
// index.ts
const nome: string = "Bun";
console.log(`Hello via ${nome}!`);
```

Rode:

```bash
bun run index.ts
```

<div class="nota">
Todo mundo rode agora. Se apareceu "Hello via Bun!",
o TypeScript já está rodando na sua máquina.
</div>

---

## O que NÃO precisou existir

Repare no que foi necessário para TypeScript rodar:

- nenhum `npm init`, nenhum `package.json`
- nenhum `tsc`, nenhum `tsconfig`
- nenhuma dependência instalada

O transpiler do Bun converte `.ts` para JavaScript **na hora**, antes de executar.

<div class="nota">
Conversem com quem está do lado: quanto disso vocês
achariam necessário antes de ver funcionar?
</div>

---

<!-- _class: cover -->

## Bloco 2

# Primeiro servidor com Bun.serve

---

## O objetivo do bloco

Um programa que fica **ouvindo uma porta** e responde requisições HTTP.

```
curl http://localhost:3000/   ->   "Blog no ar!"
```

Em Node, isso pede `node:http` ou o Express.
Em Bun, é **uma chamada só**: `Bun.serve`.

---

## O código mínimo

```bash
bun init        # escolha o template: Blank
```

```ts
// index.ts
const server = Bun.serve({
  port: 3000,
  routes: {
    "/": () => new Response("Blog no ar!"),
  },
});

console.log(`Ouvindo em ${server.url}`);
```

Uma rota. Uma função. Uma `Response`.

---

## Rodar e testar

```bash
bun run index.ts
```

```
Ouvindo em http://localhost:3000/
```

Em outro terminal:

```bash
curl http://localhost:3000/
```

```
Blog no ar!
```

<div class="nota">
Se apareceu "Blog no ar!", sua primeira API já está no ar.
</div>

---

## Response é API Web — nada novo

`Bun.serve` não usa `req`/`res` como o Express. Usa **`Request` e `Response`** — os mesmos objetos do `fetch` do navegador.

```ts
new Response("texto")                        // 200 implícito
new Response("não achei", { status: 404 })  // status explícito
Response.json({ ok: true })                  // JSON com 200
Response.json({ criado: true }, { status: 201 })
```

| No Express você escreveria | No Bun |
|---|---|
| `res.send("ok")` | `new Response("ok")` |
| `res.status(404).json({...})` | `Response.json({...}, { status: 404 })` |
| body parser no meio | `await req.json()` — nativo |

<div class="nota">
É o fetch que vocês conhecem, espelhado para o lado do servidor.
</div>

---

## Adicionar uma rota de JSON

```ts
routes: {
  "/": () => new Response("Blog no ar!"),
  "/api/status": () => Response.json({
    ok: true,
    runtime: "bun",
    hora: new Date().toISOString(),
  }),
},
```

```bash
curl http://localhost:3000/api/status
```

```json
{"ok":true,"runtime":"bun","hora":"2026-09-08T19:30:00.000Z"}
```

---

## --watch e --hot: o servidor se atualiza sozinho

```bash
bun --watch index.ts   # reinicia o PROCESSO a cada alteração
bun --hot   index.ts   # troca o CÓDIGO sem derrubar o servidor
```

| | `--watch` | `--hot` |
|---|---|---|
| Como recarrega | reinicia tudo | soft reload, mesmo processo |
| Estado em memória | perde | **mantém** |
| Uso típico | testes | **servidor em desenvolvimento** |

<div class="nota">
Hoje, rodem com <code>bun --hot index.ts</code>:
salvaram o arquivo, a resposta muda — a porta 3000 nunca cai.
</div>

---

<!-- _class: cover -->

## Atividade 2

# Suas próprias rotas

---

## Prática — 10 minutos

Adicione ao objeto `routes`:

1. `/api/sobre` — JSON com seu nome e uma frase sua
2. `/api/hora` — a hora atual com `new Date().toISOString()`

Com o `--hot` rodando, **salve o arquivo** e teste sem reiniciar nada:

```bash
curl http://localhost:3000/api/sobre
```

Enquanto isso, discutam:

- o que acontece com uma rota que **não existe**?
- qual status code seria o correto aí?

<div class="nota">
O 404 vira código daqui a pouco — no CRUD.
</div>

---

<!-- _class: cover -->

## Bloco 3

# Testes, TDD e Refactoring

---

## Um bug para começar a conversa

Imagine que a Semana 10 chega, o array vira Postgres, e **metade das rotas para de funcionar**.

Sem testes, você descobre isso **abrindo o curl e testando tudo na mão**, uma rota por uma, depois de cada mudança.

Com testes:

```
bun test
```

```
 5 pass
 2 fail       <- as duas que quebraram, em 50ms
```

<div class="nota">
Teste automatizado é um revisor de código que roda
toda a sua API em segundos, quantas vezes quiser, de graça.
</div>

---

## O Bun já traz o test runner

Lembra do "kit completo, um binário só"? Isso inclui os testes:

- test runner **nativo** — nada de instalar o Jest
- compatível com a API do Jest (`test`, `expect`)
- TypeScript direto, sem configuração

```bash
bun test                # roda todos os testes
bun test --watch        # reexecuta a cada save
```

O runner procura automaticamente arquivos chamados **`*.test.ts`**.

---

## O primeiro teste, ao vivo

```ts
// soma.test.ts
import { expect, test } from "bun:test";

test("2 + 2", () => {
  expect(2 + 2).toBe(4);
});
```

```bash
bun test
```

```
✓ soma.test.ts:
✓ 2 + 2 [0.03ms]

 1 pass
 0 fail
```

<div class="nota">
A saída verde é o som que vocês vão ouvir o resto do semestre.
</div>

---

## test e expect — a anatomia

```ts
test("cria um post com id", () => {
  //      ^ nome: o que o teste garante
  expect(criarPost("titulo")).toBe(42);
  // ^ expect(valor).afirmacao(valorEsperado)
});
```

| Peça | Papel |
|---|---|
| `test(nome, fn)` | um caso: "isso deveria acontecer" |
| `expect(x)` | pega um valor para afirmar algo sobre ele |
| `.toBe(y)` | afirmação: igual a `y` |
| `.toEqual(y)` | igualdade **profunda** — para objetos e arrays |

<div class="nota">
Nomes importam: o nome do teste é a descrição do que quebrou
quando ele falha. "deveria rejeitar post sem título" > "teste 3".
</div>

---

## O que TDD significa

**TDD = Test-Driven Development.** Desenvolvimento guiado por testes.

A ordem tradicional:

```
1. escrever o código
2. escrever o teste
3. rodar e torcer
```

A ordem do TDD — **invertida**:

```
1. escrever o teste        (para um código que não existe)
2. rodar -> FALHA          (vermelho — e falhar é o objetivo)
3. escrever o código
4. rodar -> PASSA          (verde)
5. refatorar, rodando de novo
```

<div class="nota">
Parece ao contrário. É ao contrário — de propósito.
O teste é a primeira especificação do que o código deve fazer.
</div>

---

## Vermelho -> Verde -> Refatora

O ciclo do TDD tem nome: **Red, Green, Refactor**.

```
   1. RED       escreva o TESTE e o veja FALHAR
                (o código ainda não existe)

   2. GREEN     escreva o código MÍNIMO para passar

   3. REFACTOR  melhore o design — o teste protege

        ^______ volta ao 1 (próximo teste)
```

<div class="nota">
Um ciclo por vez, passos pequenos.
Nunca dois ciclos sem rodar os testes.
</div>

---

## Por que codar assim

1. **o teste falha primeiro** — você vê o teste falhar e depois passar; se ele falhar por outro motivo depois, você percebe
2. **código nascido testável** — código escrito para ser testado é mais simples, mais isolado, menos engessado
3. **a especificação virou executável** — o que o código deve fazer está escrito no teste, não num comentário
4. **coragem pra mudar** — o teste avisa na hora se a mudança quebrou algo

<div class="nota">
Ponto 4 é o que nos permite chegar na Semana 13:
trocar banco, refazer rota, mexer em tudo — sem medo.
</div>

---

## Refactoring — a palavra de Martin Fowler

> "**Refatorar**: mudar a estrutura interna do software
> **sem mudar o comportamento externo**,
> para deixá-lo mais fácil de entender e mais barato de modificar."

Refactoring **não é** reescrever, **não é** otimizar, **não é** corrigir bug.

É: o código funciona, mas está feio/confuso/duplicado — você melhora a forma, **e o comportamento continua idêntico**.

<div class="nota">
Como você prova que o comportamento não mudou?
Os testes continuam verdes. É isso que eles garantem.
</div>

---

## Como o teste protege o refactoring

Sem teste, refatorar é fé:

```
mexi num arquivo ... o resto ainda funciona?  acho que sim?
```

Com teste:

```
refatorou -> bun test -> 12 pass  -> garantido, nada quebrou
```

<div class="nota">
No TDD, refatorar é seguro porque os testes são a rede de segurança:
se a estrutura interna mudar e um comportamento quebrar,
o teste falha na hora — e diz qual quebrou.
</div>

---

## TDD na prática — o que vamos fazer

Vamos aplicar o ciclo em uma função **pura**, antes das rotas:

1. a lógica dos posts (`criar`, `buscar`, `atualizar`, `remover`) vive em funções separadas, testáveis
2. as rotas do CRUD ficam finas: chamam a lógica e devolvem a `Response`
3. **a lógica nasce por TDD**; as rotas vocês testam com `curl` (por hoje)

```
index.ts          ->  rotas (HTTP)          -> teste com curl
posts.ts          ->  lógica (regras)       -> teste com bun test
```

<div class="nota">
Essa separação é o primeiro design de verdade do blog.
E é ela que permite trocar o array pelo Prisma na
Semana 10 sem tocar nas rotas.
</div>

---

<!-- _class: cover -->

## Atividade 3

# O ciclo RED -> GREEN, ao vivo

---

## Prática — 10 minutos, em dupla

**Passo 1 — RED.** Crie `posts.ts` (vazio por enquanto) e escreva o teste primeiro:

```ts
// posts.test.ts
import { expect, test } from "bun:test";
import { criarPost } from "./posts";

test("criarPost devolve um post com id e criadoEm", () => {
  const post = criarPost("Meu primeiro post", "Olá, mundo!");

  expect(post.titulo).toBe("Meu primeiro post");
  expect(post.id).toBeTruthy();       // existe e não é vazio
  expect(post.criadoEm).toBeTruthy();
});
```

Rode: `bun test` — **precisa falhar.** Esse é o RED.

<div class="nota">
Se o teste passa antes do código existir,
o teste está testando nada. RED é obrigatório.
</div>

---

## Passo 2 — GREEN

Agora, o **mínimo** para o teste passar:

```ts
// posts.ts
type Post = {
  id: string;
  titulo: string;
  conteudo: string;
  criadoEm: string;
};

export function criarPost(titulo: string, conteudo: string): Post {
  return {
    id: crypto.randomUUID(),
    titulo, conteudo,
    criadoEm: new Date().toISOString(),
  };
}
```

---

## Passo 2 — GREEN (continuação)

```bash
bun test
```

```
✓ criarPost devolve um post com id e criadoEm

 1 pass
```

<div class="nota">
MÍNIMO mesmo. Não antecipe o que o próximo teste
ainda não pediu — a regra é deixar o teste guiar.
</div>

---

## Passo 3 — REFACTOR

O teste está verde — agora é a hora segura de melhorar.

Exemplo: o `type Post` vive no `posts.ts`, mas o CRUD inteiro vai usar.
Mova-o para `types.ts` e atualize o `import`:

```ts
// types.ts
export type Post = {
  id: string;
  titulo: string;
  conteudo: string;
  criadoEm: string;
};
```

```bash
bun test      # ainda verde? o refactor é seguro.
```

<div class="nota">
Estrutura mudou, comportamento não — e o teste
acaba de provar isso em 50ms. Esse é o refactoring de Fowler:
protegido por teste.
</div>

---

<!-- _class: cover -->

## Bloco 4

# CRUD — as quatro operações de um blog

---

## O que é um CRUD

Toda aplicação com dados se resume a quatro operações:

| Operação | Significa | No blog |
|---|---|---|
| **C**reate | criar | publicar um post |
| **R**ead | ler | listar / abrir posts |
| **U**pdate | editar | corrigir um post |
| **D**elete | apagar | remover um post |

Semana 5: **o recurso é o substantivo, o método HTTP é o verbo.**

Hoje isso vira código.

---

## As rotas REST do blog

| Método | Rota | Ação |
|---|---|---|
| `POST` | `/api/posts` | criar um post |
| `GET` | `/api/posts` | listar todos |
| `GET` | `/api/posts/:id` | abrir um post |
| `PUT` | `/api/posts/:id` | editar um post |
| `DELETE` | `/api/posts/:id` | apagar um post |

Mesmo recurso (`posts`), cinco operações — diferenciadas **pelo método e pelo `:id`**.

---

## Antes de codar: modelar (Semana 7)

Um post do blog, antes de existir código, é:

```ts
type Post = {
  id: string;
  titulo: string;
  conteudo: string;
  criadoEm: string;
};
```

É isso que a Semana 7 ensinou: **desenhar o que construir, antes de construir**.

---

## O "banco de dados" de hoje

```ts
const posts: Post[] = [];
```

Um array **em memória**. É o nosso banco provisório.

```
hoje:            array em memória     (reinicia, morre)
próxima semana:  PostgreSQL + Prisma  (reinicia, sobrevive)
```

<div class="nota">
O design das rotas NÃO muda quando o banco chegar.
Só a fonte de dados muda. É exatamente por isso
que separamos lógica (posts.ts) de rotas (index.ts) —
e que os testes vão provar isso na hora.
</div>

---

## O CRUD agora nasce por TDD

Cada operação da lógica, **um ciclo RED -> GREEN -> REFACTOR**:

| Ciclo | Teste que nasce primeiro (RED) | Código mínimo (GREEN) |
|---|---|---|
| 1 | `listarPosts` devolve o que existe | `posts` vazio -> `[]` |
| 2 | `buscarPostPorId` acha pelo id | `posts.find(...)` |
| 3 | `buscarPostPorId` com id inventado -> `undefined` | idem (sem if!) |
| 4 | `atualizarPost` muda o título e mantém o resto | `??` preserva campos |
| 5 | `removerPost` tira do array | `posts.splice(...)` |

<div class="nota">
Ciclo 3 é o espírito do TDD: escreva o teste do
caso "não encontrado" ANTES de existir o if que o trata.
</div>

---

## A lógica completa nasce dos testes

Depois dos 5 ciclos, `posts.ts` fica assim — cada linha existe porque um teste pediu:

```ts
import type { Post } from "./types";

const posts: Post[] = [];

export function criarPost(titulo: string, conteudo: string): Post {
  const novo = { id: crypto.randomUUID(), titulo, conteudo,
                 criadoEm: new Date().toISOString() };
  posts.push(novo);
  return novo;
}
```

---

## A lógica completa (continuação)

```ts
export function listarPosts(): Post[] {
  return posts;
}

export function buscarPostPorId(id: string): Post | undefined {
  return posts.find((p) => p.id === id);
}
```

`atualizarPost` e `removerPost`: completar nos ciclos 4 e 5.

```bash
bun test   # 5 pass — a lógica está pronta e provada
```

---

## Sintaxe nova: handlers por método

No `routes`, uma mesma URL aceita um objeto **por método HTTP**:

```ts
routes: {
  "/api/posts": {
    GET:  () => Response.json(posts),
    POST: async (req) => { /* criar */ },
  },
},
```

```
GET  /api/posts   ->  roda o GET
POST /api/posts   ->  roda o POST
```

REST literal: a URL decide **o recurso**, o método decide **a operação**.

---

## CREATE — o POST por dentro

Com a lógica já testada, a rota fica **fina**: lê o corpo, chama a função, devolve a resposta.

```ts
POST: async (req) => {
  const body = await req.json();
  const novo = criarPost(body.titulo, body.conteudo);

  return Response.json(novo, { status: 201 });
},
```

| Passo | O que faz |
|---|---|
| `await req.json()` | lê o corpo da requisição — nativo, sem parser |
| `criarPost(...)` | a lógica — **já testada pelo `bun test`** |
| `status: 201` | **Created** — não é 200, é mais específico |

<div class="nota">
Compare com a versão anterior: a rota não sabe mais
como um post é criado — só chama quem sabe.
Rotas finas, lógica testada.
</div>

---

## Testar o CREATE

```bash
curl -X POST http://localhost:3000/api/posts \
  -H "Content-Type: application/json" \
  -d '{"titulo": "Meu primeiro post", "conteudo": "Olá, mundo!"}'
```

```json
{
  "id": "3f8c1d2a-9b04-4e33-a1c7-2d5b7e9f0a11",
  "titulo": "Meu primeiro post",
  "conteudo": "Olá, mundo!",
  "criadoEm": "2026-09-08T19:42:10.000Z"
}
```

O servidor devolveu o post **completo** — com o `id` que ele gerou.

---

## READ — listar todos

```ts
GET: () => Response.json(listarPosts()),
```

A rota inteira é **uma linha** — porque a lógica já existe e já está testada.

```bash
curl http://localhost:3000/api/posts
```

```json
[
  {
    "id": "3f8c1d2a-...",
    "titulo": "Meu primeiro post",
    "criadoEm": "2026-09-08T19:42:10.000Z"
  }
]
```

---

## Rotas dinâmicas — o `:id`

E se quisermos **um** post específico? Não dá para escrever uma rota por post.

```ts
"/api/posts/:id": (req) => {
  const post = buscarPostPorId(req.params.id);
  // ...
},
```

- `:id` é um **coringa**: casa com qualquer valor naquela posição
- `req.params.id` entrega o valor **já extraído**
- a busca é a função `buscarPostPorId` — já testada nos ciclos 2 e 3

```
/api/posts/3f8c...   ->   req.params.id === "3f8c..."
```

---

## READ de um — com o 404 no lugar dele

```ts
"/api/posts/:id": (req) => {
  const post = buscarPostPorId(req.params.id);

  if (!post) {
    return Response.json(
      { erro: "post não encontrado" },
      { status: 404 },
    );
  }

  return Response.json(post);
},
```

Semana 5, agora em código: **404 = o recurso existe, mas não com esse id**.

<div class="nota">
O comportamento "id inventado -> undefined" foi definido
no TESTE do ciclo 3 — antes de existir este if. A rota
só traduz undefined para o status code certo.
</div>

---

## Testar: o mesmo endpoint, dois finais

```bash
curl http://localhost:3000/api/posts/3f8c1d2a-9b04...
```

```json
{"id": "3f8c1d2a-...", "titulo": "Meu primeiro post", ...}
```

```bash
curl http://localhost:3000/api/posts/id-inventado
```

```json
{"erro": "post não encontrado"}
```

<div class="nota">
Faça os dois na sua máquina.
O segundo também está certo — ele conta a história de "não existe".
</div>

---

## UPDATE — a rota fica fina

Regra da rota fina: a rota **traduz HTTP**, a lógica **decide**.

```ts
PUT: async (req) => {
  const post = buscarPostPorId(req.params.id);
  if (!post) {
    return Response.json({ erro: "não encontrado" }, { status: 404 });
  }

  const body = await req.json();
  const atualizado = atualizarPost(post, body.titulo, body.conteudo);

  return Response.json(atualizado);
},
```

---

## UPDATE — a lógica (nascida no ciclo 4)

Em `posts.ts`:

```ts
export function atualizarPost(
  post: Post, titulo?: string, conteudo?: string
): Post {
  post.titulo = titulo ?? post.titulo;
  post.conteudo = conteudo ?? post.conteudo;
  return post;
}
```

<div class="nota">
O <code>??</code> — "se veio, usa; senão, mantém" —
fica na lógica, onde o teste do ciclo 4 protege ele.
</div>

---

## Testar o UPDATE

```bash
curl -X PUT http://localhost:3000/api/posts/3f8c1d2a-9b04... \
  -H "Content-Type: application/json" \
  -d '{"titulo": "Título editado"}'
```

```json
{
  "id": "3f8c1d2a-...",
  "titulo": "Título editado",
  "conteudo": "Olá, mundo!",
  "criadoEm": "2026-09-08T19:42:10.000Z"
}
```

O `conteudo` veio junto — porque o `??` manteve o que não foi enviado.

---

## DELETE — a rota

```ts
  DELETE: (req) => {
    const ok = removerPost(req.params.id);

    if (!ok) {
      return Response.json({ erro: "não encontrado" }, { status: 404 });
    }

    return new Response(null, { status: 204 });
  },
},
```

**204 No Content** — "deu certo, e não há corpo para devolver".

---

## DELETE — a lógica (nascida no ciclo 5)

Em `posts.ts`:

```ts
export function removerPost(id: string): boolean {
  const index = posts.findIndex((p) => p.id === id);
  if (index === -1) return false;
  posts.splice(index, 1);
  return true;
}
```

<div class="nota">
Status code também é linguagem:
<b>201</b> criei, <b>200</b> aqui está, <b>204</b> fiz e não devolvo nada,
<b>404</b> não existe. Quem consome sua API entende sem ler nada.
</div>

---

<!-- _class: cover -->

## Atividade 4

# O CRUD inteiro, na mão

---

## Prática — 15 minutos

Complete os ciclos 4 e 5 (atualizar e remover) em dupla, **test-first**:
escreva o teste, veja falhar, escreva o mínimo para passar.

```ts
// ciclos 4 e 5 — posts.test.ts
test("atualizarPost muda o titulo e mantém o resto", () => { ... });

test("removerPost devolve true e remove do array", () => { ... });
```

Só depois de `bun test` verde, teste as rotas com `curl`.

---

## Prática — os comandos do CRUD

```bash
# CREATE
curl -X POST http://localhost:3000/api/posts \
  -H "Content-Type: application/json" \
  -d '{"titulo": "Meu primeiro post", "conteudo": "Olá!"}'

# READ (lista e um)
curl http://localhost:3000/api/posts
curl http://localhost:3000/api/posts/<id-do-post>
```

```bash
# UPDATE
curl -X PUT http://localhost:3000/api/posts/<id> \
  -H "Content-Type: application/json" \
  -d '{"titulo": "Título editado"}'

# DELETE
curl -X DELETE http://localhost:3000/api/posts/<id>
```

---

## Checklist de status codes

```
POST   -> 201
GET lista -> 200
GET um  -> 200 ou 404
PUT    -> 200 ou 404
DELETE -> 204 ou 404
```

Depois, **teste os erros de propósito**:

- POST sem `titulo` — sua API deixa criar? Deveria?
- GET com id inventado — responde 404 bonito?

<div class="nota">
API boa não é só a que funciona com entrada certa.
É a que se comporta bem com entrada errada.
E amanhã, quando alguém refatorar algo, é a que
o <code>bun test</code> continua verde.
</div>

---

<!-- _class: cover -->

## Bloco 5

# A API dentro de um container

---

## Por que container — de novo, e agora a sério

Semana 3: o problema do "na minha máquina funciona".

Hoje ele ganha um papel novo — o blog de vocês vai para **produção** na Semana 13:

```
sua máquina:     Bun instalado via curl      -> funciona
colega:          sem Bun, só Docker          -> funciona igual
servidor:        não tem Bun, tem Docker    -> funciona igual
```

<div class="nota">
Quem sobe em container não precisa de
"instalação no servidor" — nunca.
</div>

---

## A imagem oficial do Bun

O Bun mantém uma imagem Docker pública, no Docker Hub:

```bash
docker pull oven/bun:1
```

- `oven/bun` — imagem oficial, da equipe do Bun
- `1` — a versão major do Bun (mesma lógica do `node:lts` da Semana 3)

Ela já vem com **o binário `bun` inteiro** dentro: runtime, transpiler, tudo.

<div class="nota">
Analogia da oficina continua valendo: a imagem base é a
massa de pizza pronta. Nós só colocamos o recheio —
que hoje é um único arquivo.
</div>

---

## O Dockerfile inteiro

```dockerfile
FROM oven/bun:1

WORKDIR /usr/src/app

COPY index.ts posts.ts types.ts ./

USER bun
EXPOSE 3000/tcp
ENTRYPOINT ["bun", "run", "index.ts"]
```

Nove linhas — e os testes **não entram na imagem** (`.dockerignore` cuida disso, adiante). Vamos ler uma por uma.

---

## FROM e WORKDIR

```dockerfile
FROM oven/bun:1

WORKDIR /usr/src/app
```

**`FROM`** — a base: um sistema com o Bun pronto.
Você não começa do zero, começa de algo que já funciona.

**`WORKDIR`** — o diretório de trabalho dentro da imagem.
Cria se não existir; tudo o que vier depois roda a partir dele.
Equivale a um `cd` que fica valendo daqui em diante.

---

## COPY — o momento em que o código entra

```dockerfile
COPY index.ts posts.ts types.ts ./
```

Copia do **seu computador** para dentro da **imagem**.

- origem: seus arquivos, relativo à pasta onde roda o `docker build` (o **contexto de build**)
- destino: `./` — que aqui é `/usr/src/app`, por causa do `WORKDIR`

<div class="nota">
Lembre da oficina: o código não fica "no Dockerfile".
O Dockerfile só tem instruções — e o COPY é a instrução
que traz o código para dentro.
</div>

---

## USER, EXPOSE e ENTRYPOINT

```dockerfile
USER bun
EXPOSE 3000/tcp
ENTRYPOINT ["bun", "run", "index.ts"]
```

**`USER bun`** — roda como usuário sem privilégios. A imagem tem um usuário `bun` pronto; usá-lo é segurança básica (Semana 12 aprofunda).

**`EXPOSE 3000/tcp`** — **documenta** a porta. Não abre nada; quem abre é o `-p` no `docker run`.

**`ENTRYPOINT`** — o processo principal. Enquanto ele viver, o container vive.

---

## Um detalhe que o Bun já resolve por nós

O erro nº 1 da oficina: servidor ouvindo em `localhost` **dentro do container** — ninguém de fora alcança.

```js
// Node clássico — o container sobe, a porta não responde
server.listen(3000, "localhost");
```

```ts
// Bun — o padrão do Bun.serve é 0.0.0.0
Bun.serve({ port: 3000, routes: { /* ... */ } });  // nasce certo
```

`0.0.0.0` = "aceite conexões de qualquer interface".

<div class="nota">
O Bun escolheu o padrão certo para servidores.
Quem usa Node é que precisa lembrar disso manualmente.
</div>

---

## .dockerignore

```
node_modules
*.test.ts
Dockerfile*
docker-compose*
.dockerignore
.git
.gitignore
README.md
.env
```

Mesma sintaxe do `.gitignore` (Semana 2): o que **não** entra no contexto de build.

- build leve e rápido — testes rodam no desenvolvimento, não em produção
- **`.env` nunca vai para dentro da imagem** — segredo não se empacota

<div class="nota">
Semana 12 volta nesse ponto. Por enquanto, fica o hábito:
arquivo de segredo fora do container.
</div>

---

<!-- _class: cover -->

## Atividade 5

# Build, run, logs — e uma surpresa

---

## Prática — build e run

```bash
docker build -t blog-api:1.0 .
```

```
[+] Building 2.1s (10/10) FINISHED
 => [base 1/2] FROM docker.io/library/oven/bun:1
 => [base 2/2] WORKDIR /usr/src/app
 => ...
 => naming to docker.io/library/blog-api:1.0
```

```bash
docker run -d --name blog-api -p 3000:3000 blog-api:1.0
curl http://localhost:3000/api/posts
```

Cada `[n]` do build é **uma camada** — a mesma ideia da Semana 3.

---

## Ver o que está rodando

```bash
docker ps
```

```
CONTAINER ID   IMAGE          PORTS                    NAMES
7f03e212a15e   blog-api:1.0   0.0.0.0:3000->3000/tcp   blog-api
```

```bash
docker logs -f blog-api
```

```
Ouvindo em http://localhost:3000/
```

`-f` acompanha em tempo real (`Ctrl+C` para sair).

---

## A surpresa — onde está o meu post?

1. Crie um post via `curl` e confirme que aparece
2. Rode os testes — **eles continuam verdes**. Por quê?
3. Derrube e suba o container de novo:

```bash
docker rm -f blog-api
docker run -d --name blog-api -p 3000:3000 blog-api:1.0
curl http://localhost:3000/api/posts
```

```
[]
```

**O post sumiu.** Mas o teste do `criarPost` passou. Por quê?

<div class="nota">
O teste garante o COMPORTAMENTO da função,
não os DADOS em memória. Dado é problema de banco — Semana 10.
</div>

---

## Por que sumiu — dois motivos, mesma raiz

1. o array **vive na memória do processo** — processo novo, array vazio
2. o container é **efêmero** — removeu o container, removeu a camada de escrita (Semana 3)

```
estado que precisa sobreviver  ->  não pode viver no processo
                              ->  precisa viver FORA: um banco de dados
```

<div class="nota">
Essa "falha" de hoje é a motivação inteira da Semana 10:
o array vira PostgreSQL + Prisma, dentro e fora do container.
</div>

---

## O ciclo de trabalho com Docker

Experimento: edite o `index.ts`, de `curl` de novo — **nada mudou**.

A imagem tem uma **cópia** do arquivo, feita no momento do `build`.

```bash
docker build -t blog-api:1.1 .
docker rm -f blog-api
docker run -d --name blog-api -p 3000:3000 blog-api:1.1
```

Agora a mudança aparece.

```
desenvolvimento:  bun --hot   (salvou, mudou)
container:        editar -> build -> run
```

---

<!-- _class: cover -->

## Recapitulando

---

## O que vimos hoje

**Bun**
- runtime all-in-one: runtime + pacotes + testes + bundler, um binário
- engine JavaScriptCore; TypeScript nativo, zero config
- `Bun.serve`: rotas por método HTTP, sem framework

**Testes, TDD e Refactoring**
- `bun test` nativo, arquivos `*.test.ts`
- TDD: RED -> GREEN -> REFACTOR — o teste nasce antes do código
- refactoring (Fowler): muda a estrutura, mantém o comportamento — provado pelos testes verdes

**CRUD e Docker**
- POST 201, GET 200/404, PUT 200/404, DELETE 204
- lógica testada (`posts.ts`) + rotas finas (`index.ts`)
- `FROM oven/bun:1`, ciclo editar -> build -> run

---

## Erros comuns

| Sintoma | Causa provável |
|---|---|
| `bun: command not found` | instalação/PATH — ou use a imagem Docker |
| Porta não responde no container | faltou `-p 3000:3000` |
| Porta ocupada no host | outro `bun --hot` ainda rodando |
| Teste passa sem o código existir | teste testando nada — confira o RED |
| POST criado "sumiu" | array em memória + container recriado — esperado hoje |
| Mudança no código não aparece | faltou o `docker build` |
| `docker run` diz nome em uso | `docker rm` no container antigo antes |

---

## Desafio da Semana 9

**Iniciante** — CRUD de **tarefas** (`/api/tarefas`) com `titulo` e `feita` (boolean), **um ciclo TDD por operação**: escreva o teste, veja falhar, faça passar. Teste as rotas com `curl`.

**Intermediário** — adicione validação **test-first**: escreva primeiro o teste "rejeita tarefa sem titulo" (`.toBeUndefined()` no retorno, ou o erro que decidirem), depois o código. Depois `GET /api/tarefas?busca=texto` filtrando por título — teste primeiro, claro.

**Avançado** — refactor: extraia a busca e a validação para funções puras próprias (`*.ts` separados), **mantendo todos os testes verdes sem editá-los**. Depois porta via variável de ambiente: `process.env.PORT` no `Bun.serve`, `docker run -e PORT=4000 -p 4000:4000`.

<div class="nota">
Repare no desafio avançado: refactoring é MUDAR a estrutura
e NÃO MUDAR os testes. Se você precisou editar o teste
para o código compilar, não foi refactor — foi reescrita.
</div>

---

## Próxima semana

O array vira banco de verdade:

- **modelar** o banco do blog (a Semana 7 ajuda aqui)
- **PostgreSQL** — banco relacional
- **Prisma** — o ORM que fala com ele por nós

E aqui o TDD paga: trocar o array pelo Prisma é **um refactoring gigante** —
as funções de `posts.ts` mudam por dentro, os testes e as rotas ficam.
Se os testes continuarem verdes, a troca é segura.

O design que vocês escreveram hoje **não muda**.
Só a fonte de dados muda.

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 9

# Sua API rodando. Seu container no ar.

Daqui para frente é construir.