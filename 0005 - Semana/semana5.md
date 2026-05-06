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

## Clube de Desenvolvimento Web — Semana 5

# APIs e ORMs

*REST, RESTful, GraphQL, SOAP e a camada que abstrai o banco*

---

# Agenda

1. **O que e uma API** — o contrato entre sistemas
2. **REST e RESTful** — principios, convencoes e diferencas
3. **GraphQL** — consultas flexiveis, uma unica rota
4. **SOAP** — o protocolo corporativo
5. **Comparativo** — quando usar cada abordagem
6. **ORMs** — vantagens, desvantagens e seguranca

> "Uma API bem projetada e a diferenca entre um sistema que escala e um que vira divida tecnica."

---

<!-- _class: cover -->

## Parte 1

# O que e uma API?

---

# API — Application Programming Interface

Uma API e um **contrato**: define o que um sistema oferece, como solicitar e o que esperar de volta.

```
[ Seu codigo ]  -->  requisicao bem definida  -->  [ Outro sistema ]
                <--  resposta previsivel      <--
```

Voce nao precisa saber como o outro sistema funciona por dentro — apenas o contrato.

### Exemplos do dia a dia

- Seu app de clima chama a API de uma empresa de meteorologia
- O checkout de um e-commerce chama a API do Stripe para processar pagamento
- Seu backend chama a API do SendGrid para enviar emails
- Um frontend React busca dados do proprio backend via API

> APIs sao o que permite que sistemas diferentes, escritos em linguagens diferentes, se comuniquem de forma confiavel.

---

# Por que APIs existem?

```
Sem API — integracao direta (impossivel em escala):
  Frontend acessa banco diretamente
  App mobile conhece a estrutura interna do servidor
  Servico externo tem credenciais do seu banco

Com API — separacao clara de responsabilidades:
  Frontend  -->  API  -->  Logica de negocio  -->  Banco
  App mobile  -->  mesma API
  Servico externo  -->  API com autorizacao especifica
```

### O que uma boa API resolve

- **Desacoplamento** — frontend e backend evoluem de forma independente
- **Seguranca** — o banco nunca fica exposto diretamente
- **Reutilizacao** — web, mobile e terceiros consomem a mesma API
- **Controle** — autenticacao, rate limiting e versionamento em um ponto central

---

<!-- _class: cover -->

## Parte 2

# REST e RESTful

---

# O que e REST?

**REST** — Representational State Transfer

Um conjunto de **principios arquiteturais** definidos por Roy Fielding em 2000, nao um protocolo ou padrao formal.

### Os 6 principios REST

1. **Cliente-Servidor** — separacao clara de responsabilidades
2. **Sem estado (Stateless)** — cada requisicao contem tudo que o servidor precisa
3. **Cacheavel** — respostas podem ser armazenadas em cache
4. **Interface uniforme** — URIs e metodos HTTP usados de forma consistente
5. **Sistema em camadas** — cliente nao sabe se fala com servidor final ou intermediario
6. **Codigo sob demanda** *(opcional)* — servidor pode enviar codigo executavel

> REST e uma filosofia. Uma API pode usar HTTP e JSON sem ser REST de verdade.

---

# REST vs RESTful

A diferenca que a maioria das pessoas ignora:

```
REST    ->  conjunto de principios arquiteturais

RESTful ->  API que REALMENTE segue esses principios
```

### O que torna uma API verdadeiramente RESTful

```
Nao RESTful (so parece REST):
  POST /buscarUsuario
  POST /deletarPedido
  GET  /getproduto?id=42

RESTful (usa HTTP como foi projetado):
  GET    /usuarios/1          <- buscar usuario
  DELETE /pedidos/7           <- deletar pedido
  GET    /produtos/42         <- buscar produto
  POST   /produtos            <- criar produto
  PATCH  /produtos/42         <- atualizar parcialmente
```

O recurso e o substantivo, o metodo HTTP e o verbo.

---

# Metodos HTTP e seus significados

| Metodo | Acao | Idempotente | Corpo |
|---|---|---|---|
| `GET` | Buscar recurso | Sim | Nao |
| `POST` | Criar recurso | Nao | Sim |
| `PUT` | Substituir recurso completo | Sim | Sim |
| `PATCH` | Atualizar campos especificos | Nao | Sim |
| `DELETE` | Remover recurso | Sim | Nao |

**Idempotente** = chamar N vezes produz o mesmo resultado que chamar uma vez.

```
GET /produtos/42          -> sempre retorna o produto 42
DELETE /produtos/42       -> primeira vez deleta, demais retornam 404
POST /produtos            -> cada chamada cria um produto novo
```

---

# Codigos de status HTTP — use corretamente

```
2xx — Sucesso
  200 OK              <- requisicao bem-sucedida
  201 Created         <- recurso criado (resposta de POST)
  204 No Content      <- sucesso sem corpo (resposta de DELETE)

4xx — Erro do cliente
  400 Bad Request     <- dados invalidos enviados
  401 Unauthorized    <- nao autenticado
  403 Forbidden       <- autenticado mas sem permissao
  404 Not Found       <- recurso nao existe
  422 Unprocessable   <- dados validos mas semanticamente errados

5xx — Erro do servidor
  500 Internal Error  <- algo quebrou no servidor
  503 Unavailable     <- servico temporariamente indisponivel
```

> Uma API que retorna `200 OK` com `{ "erro": "usuario nao encontrado" }` no corpo esta mentindo para o cliente.

---

# Estrutura de uma requisicao e resposta REST

```http
# Requisicao
GET /api/v1/pedidos/42 HTTP/1.1
Host: api.loja.com
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
Accept: application/json

# Resposta
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "status": "enviado",
  "total": 339.90,
  "itens": [
    { "produto": "Teclado", "quantidade": 1, "preco": 250.00 },
    { "produto": "Mouse",   "quantidade": 1, "preco": 89.90  }
  ],
  "criado_em": "2025-05-01T14:32:00Z"
}
```

---

<!-- _class: cover -->

## Parte 3

# GraphQL

---

# O problema que o GraphQL resolve

Com REST, o frontend frequentemente enfrenta dois problemas opostos:

```
Over-fetching — receber mais dados do que precisa:
  GET /usuarios/1
  Retorna: id, nome, email, telefone, endereco, foto, bio, configuracoes...
  Voce precisava: so o nome

Under-fetching — precisar de multiplas requisicoes:
  GET /pedidos/42           -> retorna pedido (sem dados do usuario)
  GET /usuarios/7           -> retorna usuario do pedido
  GET /produtos/15          -> retorna produto do item
  GET /produtos/23          -> mais um produto...
  = 4 requisicoes para montar uma tela
```

GraphQL resolve os dois: o cliente define **exatamente** o que quer, em **uma unica requisicao**.

---

# Como o GraphQL funciona

```
REST:   N recursos = N endpoints = N rotas no servidor
GraphQL: N recursos = 1 endpoint  = /graphql
```

O cliente envia uma **query** descrevendo o formato da resposta desejada:

```graphql
# O cliente pergunta exatamente isso:
query {
  pedido(id: 42) {
    status
    total
    usuario {
      nome
      email
    }
    itens {
      produto {
        nome
        preco
      }
      quantidade
    }
  }
}
```

---

# A resposta espelha a query

O servidor retorna exatamente o que foi solicitado — nem mais, nem menos:

```json
{
  "data": {
    "pedido": {
      "status": "enviado",
      "total": 339.90,
      "usuario": {
        "nome": "Joao",
        "email": "joao@email.com"
      },
      "itens": [
        { "produto": { "nome": "Teclado", "preco": 250.00 }, "quantidade": 1 },
        { "produto": { "nome": "Mouse",   "preco": 89.90  }, "quantidade": 1 }
      ]
    }
  }
}
```

Uma requisicao. Zero dados desnecessarios.

---

# Mutations e Subscriptions

GraphQL tem tres operacoes:

```graphql
# Query — leitura
query {
  produtos { id nome preco }
}

# Mutation — escrita (criar, atualizar, deletar)
mutation {
  criarPedido(input: { usuarioId: 1, itens: [{ produtoId: 42, quantidade: 2 }] }) {
    id
    status
    total
  }
}

# Subscription — tempo real via WebSocket
subscription {
  statusPedido(id: 42) {
    status
    atualizadoEm
  }
}
```

---

# Schema — o contrato do GraphQL

O servidor define tipos e o que pode ser consultado:

```graphql
type Produto {
  id:    ID!
  nome:  String!
  preco: Float!
}

type ItemPedido {
  produto:    Produto!
  quantidade: Int!
}

type Pedido {
  id:      ID!
  status:  String!
  total:   Float!
  usuario: Usuario!
  itens:   [ItemPedido!]!
}

type Query {
  pedido(id: ID!): Pedido
  produtos: [Produto!]!
}
```

O schema e autodocumentado — ferramentas como **GraphiQL** e **Apollo Studio** geram documentacao automaticamente.

---

<!-- _class: cover -->

## Parte 4

# SOAP

---

# O que e SOAP?

**SOAP** — Simple Object Access Protocol

Um protocolo de comunicacao baseado em **XML**, com especificacao formal rigorosa, criado pela Microsoft em 1998.

```xml
<!-- Requisicao SOAP para buscar saldo bancario -->
<soapenv:Envelope
  xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:ban="http://banco.com/servicos">

  <soapenv:Header>
    <ban:Autenticacao>
      <ban:Token>abc-123-xyz</ban:Token>
    </ban:Autenticacao>
  </soapenv:Header>

  <soapenv:Body>
    <ban:ConsultarSaldo>
      <ban:ContaId>987654</ban:ContaId>
    </ban:ConsultarSaldo>
  </soapenv:Body>

</soapenv:Envelope>
```

---

# Por que SOAP ainda existe?

SOAP parece verboso e ultrapassado — e e. Mas ha contextos em que ainda e exigido:

### Onde voce ainda encontra SOAP

- **Bancos e financeiras** — integracoes com sistemas legados dos anos 2000
- **Governo e setor publico** — Nota Fiscal Eletronica (NF-e) usa SOAP obrigatoriamente
- **Telecomunicacoes** — sistemas de faturamento e provisionamento antigos
- **ERP corporativo** — SAP, Oracle EBS expoe servicos via SOAP

### O que SOAP oferece que REST nao tem nativamente

- **WS-Security** — criptografia e assinatura digital na camada do protocolo
- **WSDL** — contrato formal, legivel por maquina, gerador de codigo
- **Transacoes distribuidas** — WS-AtomicTransaction
- **Entrega garantida** — WS-ReliableMessaging

> Voce provavelmente nao vai projetar SOAP — mas pode precisar consumir.

---

# Comparativo: REST vs GraphQL vs SOAP

| Criterio | REST | GraphQL | SOAP |
|---|---|---|---|
| **Formato** | JSON (tipicamente) | JSON | XML obrigatorio |
| **Endpoints** | Um por recurso | Um unico `/graphql` | Um por operacao (WSDL) |
| **Contrato formal** | OpenAPI (opcional) | Schema obrigatorio | WSDL obrigatorio |
| **Flexibilidade de query** | Fixa por endpoint | Total — cliente decide | Fixa por operacao |
| **Over/Under-fetching** | Comum | Eliminado | Comum |
| **Curva de aprendizado** | Baixa | Media | Alta |
| **Performance** | Alta | Media (resolvers) | Baixa (XML pesado) |
| **Casos ideais** | APIs publicas, CRUD, microservicos | Apps com muitos clientes diferentes | Integracao corporativa e legada |

---

# Quando escolher cada um

```
Construindo uma API publica ou de uso geral?
  -> REST — padrao do mercado, facil de consumir

Frontend com multiplas telas e necessidades diferentes?
  -> GraphQL — flexibilidade maxima para o cliente

Integrando com sistema bancario, NF-e ou ERP legado?
  -> SOAP — nao ha escolha, o sistema exige

Microservicos internos com comunicacao de alta performance?
  -> REST ou gRPC (protocolo binario, nao coberto hoje)

App com requisitos de tempo real (chat, notificacoes)?
  -> GraphQL Subscriptions ou WebSocket direto
```

> Na pratica, sistemas maduros usam mais de um: REST para a API principal, GraphQL para o frontend, SOAP para integracoes legadas.

---

<!-- _class: cover -->

## Parte 5

# ORMs

---

# O que e um ORM?

**ORM** — Object-Relational Mapper

Uma camada que mapeia **tabelas do banco** para **classes do codigo**, permitindo operar em objetos ao inves de escrever SQL diretamente.

```
Sem ORM — SQL escrito manualmente:
  const result = await db.query(
    "SELECT * FROM usuarios WHERE id = $1", [id]
  );
  const usuario = result.rows[0];

Com ORM — operacao em objeto:
  const usuario = await Usuario.findById(id);
```

O ORM traduz a operacao em objeto para a query SQL correspondente — e executa no banco.

---

# Como o ORM mapeia o banco

```javascript
// Definicao do modelo (Prisma)
model Usuario {
  id        Int      @id @default(autoincrement())
  nome      String
  email     String   @unique
  pedidos   Pedido[]
  criadoEm DateTime @default(now())
}

// O ORM gera e gerencia a tabela correspondente no banco
// Operacoes em codigo viram SQL automaticamente:

await prisma.usuario.create({
  data: { nome: "Joao", email: "joao@email.com" }
});
// INSERT INTO "Usuario" (nome, email) VALUES ('Joao', 'joao@email.com')

await prisma.usuario.findMany({
  where: { criadoEm: { gte: new Date("2025-01-01") } }
});
// SELECT * FROM "Usuario" WHERE "criadoEm" >= '2025-01-01'
```

---

# ORMs mais usados

| ORM | Linguagem | Banco suportado | Destaque |
|---|---|---|---|
| **Prisma** | Node.js / TypeScript | PostgreSQL, MySQL, SQLite, MongoDB | Schema declarativo, type-safe, migracao integrada |
| **TypeORM** | Node.js / TypeScript | Maioria dos relacionais | Decorators, estilo ActiveRecord ou DataMapper |
| **Sequelize** | Node.js | PostgreSQL, MySQL, SQLite | Mais antigo, muito usado em projetos legados |
| **SQLAlchemy** | Python | Maioria dos relacionais | Mais poderoso do ecossistema Python |
| **Django ORM** | Python | Maioria dos relacionais | Integrado ao Django, batteries included |
| **Hibernate** | Java | Maioria dos relacionais | Padrao da industria Java/Spring |

---

# Vantagens do ORM

### Produtividade

```javascript
// Buscar usuario com todos os pedidos e produtos de cada pedido
const usuario = await prisma.usuario.findUnique({
  where: { id: 1 },
  include: {
    pedidos: {
      include: { itens: { include: { produto: true } } }
    }
  }
});

// Sem ORM, voce escreveria JOINs aninhados manualmente —
// e ainda teria que mapear o resultado para objetos.
```

- **Menos codigo** — operacoes comuns em uma linha
- **Refatoracao mais segura** — rename de campo atualiza o modelo, nao queries espalhadas
- **Migracao de banco** — ORM rastreia e aplica mudancas de schema automaticamente
- **Portabilidade** — trocar de PostgreSQL para MySQL com mudanca minima de codigo

---

# Seguranca — o argumento mais forte para usar ORM

A vantagem de seguranca e estrutural, nao opcional:

```javascript
// Sem ORM — risco de SQL Injection na concatenacao:
const email = req.body.email;  // entrada do usuario

// Desenvolvedor descuidado faz:
const query = `SELECT * FROM usuarios WHERE email = '${email}'`;
// Se email = "' OR '1'='1", a query retorna todos os usuarios

// Com ORM — parametrizacao e obrigatoria por design:
const usuario = await prisma.usuario.findUnique({
  where: { email: req.body.email }
  // O ORM NUNCA concatena strings — sempre usa parametros preparados
});
// Equivale a: SELECT * FROM usuarios WHERE email = $1
// com req.body.email como parametro separado — SQL Injection impossivel
```

> Com um ORM, SQL Injection por concatenacao acidental se torna estruturalmente impossivel. O caminho padrao ja e o caminho seguro.

---

# Desvantagens do ORM — conhecer para usar bem

### Performance em queries complexas

```javascript
// ORM pode gerar queries ineficientes para casos complexos
const relatorio = await prisma.pedido.findMany({
  where: { status: "enviado" },
  include: { itens: { include: { produto: true } }, usuario: true }
});
// Gera multiplas queries ou JOIN complexo — pode ser lento em escala

// SQL manual otimizado para o mesmo caso:
SELECT p.id, p.total, u.nome,
       json_agg(json_build_object('produto', pr.nome, 'qtd', i.quantidade)) AS itens
FROM pedidos p
JOIN usuarios u ON u.id = p.usuario_id
JOIN itens_pedido i ON i.pedido_id = p.id
JOIN produtos pr ON pr.id = i.produto_id
WHERE p.status = 'enviado'
GROUP BY p.id, u.nome;
```

---

# Desvantagens — continuacao

### Abstracao que esconde o que acontece

```javascript
// O que parece simples pode gerar N+1 queries:
const pedidos = await prisma.pedido.findMany();      // 1 query

for (const pedido of pedidos) {
  const usuario = await prisma.usuario.findUnique({  // 1 query POR pedido
    where: { id: pedido.usuarioId }
  });
}
// 1 pedido = 1 query. 1000 pedidos = 1001 queries. Performance catastrofica.

// Solucao: usar include/eager loading corretamente
const pedidos = await prisma.pedido.findMany({
  include: { usuario: true }   // 1 query com JOIN — correto
});
```

### Resumo das desvantagens

- Curva de aprendizado inicial — cada ORM tem suas convencoes
- Queries muito complexas ou de alta performance ainda exigem SQL raw
- Abstracao pode ocultar problemas de performance ate o sistema estar em producao
- Migracao entre ORMs e trabalhosa

---

# ORM e SQL raw — nao e uma escolha exclusiva

ORMs maduros permitem escapar para SQL quando necessario:

```javascript
// Prisma — SQL raw para casos especificos
const resultado = await prisma.$queryRaw`
  SELECT
    date_trunc('month', criado_em) AS mes,
    SUM(total)                     AS receita
  FROM pedidos
  WHERE status = 'pago'
  GROUP BY mes
  ORDER BY mes DESC
`;

// Note: mesmo no SQL raw do Prisma, template literals
// sao parametrizados automaticamente — SQL Injection continua impossivel
```

> A estrategia ideal: ORM para operacoes do dia a dia, SQL raw para queries analiticas e de alta performance. Os dois coexistem.

---

# Resumo: quando usar ORM

<div class="columns">

**Use ORM quando:**
- CRUD padrao — criar, listar, atualizar, deletar
- Time com niveis variados — protege contra erros de seguranca
- Prototipagem rapida — produtividade importa mais
- Migracao de schema frequente
- Portabilidade entre bancos e necessaria

**Prefira SQL direto quando:**
- Relatorios e analytics complexos
- Queries que o ORM nao consegue expressar bem
- Performance critica com dados em grande volume
- Voce ja domina SQL e o custo de abstracao nao compensa

</div>

> Na Semana 2, vimos SQL Injection como vetor de ataque. Na Semana 4, vimos prepared statements como defesa. ORM e a camada que torna essa defesa automatica e estrutural — sem depender da disciplina de cada desenvolvedor.

---

<!-- _class: cover -->

## Recapitulando

# O que vimos hoje

---

# Resumo

<div class="columns">

**APIs**
- API e um contrato entre sistemas
- REST e um conjunto de principios, RESTful e segui-los de verdade
- Metodos HTTP tem significado semantico — use corretamente
- Codigos de status comunicam o que aconteceu

**Estilos de API**
- REST — padrao do mercado, simples, escalavel
- GraphQL — cliente define a query, elimina over/under-fetching
- SOAP — XML verboso, ainda obrigatorio em sistemas legados e governo
- Cada um resolve um contexto diferente — nao ha vencedor universal

</div>

---

# Resumo (continuacao)

<div class="columns">

**ORMs**
- Mapeiam tabelas para objetos — menos SQL escrito manualmente
- Parametrizacao obrigatoria por design — SQL Injection estruturalmente impossivel
- Produtividade, migracao e refatoracao mais seguros
- Risco de N+1 queries e performance oculta — exige atencao

**Conexao com semanas anteriores**
- Semana 2: SQL Injection como ataque
- Semana 4: prepared statements como defesa
- Semana 5: ORM como defesa automatica e estrutural
- A seguranca se acumula em camadas — cada semana adiciona uma

</div>

---

# Desafio da Semana 5

### Nivel iniciante
Documente a API publica do JSONPlaceholder (`jsonplaceholder.typicode.com`): liste os endpoints disponiveis, os metodos aceitos e um exemplo de requisicao e resposta para cada um. Use o formato de uma tabela REST.

### Nivel intermediario
Construa uma API RESTful em Node.js com pelo menos 3 recursos (ex: usuarios, produtos, pedidos). Use **Prisma** como ORM com PostgreSQL via Docker. Todas as rotas devem retornar o codigo de status HTTP correto.

### Nivel avancado
Implemente a mesma API em GraphQL usando **Apollo Server**. Defina o schema com tipos, queries e mutations. Compare uma tela complexa: quantas requisicoes REST vs quantas queries GraphQL sao necessarias para montar os dados.

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 5

# Interface bem definida, sistema bem protegido

*Nao importa o nivel. Importa entregar — sabendo como os sistemas se comunicam.*
