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

## Clube de Desenvolvimento Web — Semana 4

# Banco de Dados

*Relacional, nao relacional, LLMs e seguranca*

---

# Agenda

1. **O que e um banco de dados** — por que nao usar so arquivos
2. **Bancos relacionais** — estrutura, SQL e quando usar
3. **Bancos nao relacionais** — tipos, casos de uso e quando usar
4. **Comparativo** — como escolher
5. **Bancos de dados e LLMs** — RAG, embeddings e vetores
6. **Seguranca no banco de dados** — ameacas e defesas

> "Dados sao o ativo mais valioso de qualquer aplicacao. O banco e o cofre."

---

<!-- _class: cover -->

## Parte 1

# Por que um Banco de Dados?

---

# O problema de guardar dados em arquivos

```
# Solucao ingenua: salvar usuarios em um arquivo .txt
echo "joao,joao@email.com,senha123" >> usuarios.txt
echo "maria,maria@email.com,abc456" >> usuarios.txt

# Agora tente:
# - Buscar usuario por email com 1 milhao de registros
# - Atualizar o email de um usuario especifico
# - Dois processos gravando ao mesmo tempo
# - Recuperar apos falha de energia no meio da escrita
```

### O que falta nos arquivos

- **Busca eficiente** — indices e otimizacao de queries
- **Consistencia** — transacoes atomicas (tudo ou nada)
- **Concorrencia** — multiplas leituras e escritas simultaneas seguras
- **Integridade** — restricoes que garantem dados validos
- **Recuperacao** — rollback em caso de falha

> Um banco de dados resolve exatamente esses problemas — de forma confiavel e escalavel.

---

<!-- _class: cover -->

## Parte 2

# Bancos Relacionais

---

# O modelo relacional

Dados organizados em **tabelas** com linhas e colunas. Relacoes entre tabelas sao expressas por chaves.

```
Tabela: usuarios
+----+--------+---------------------+
| id | nome   | email               |
+----+--------+---------------------+
|  1 | Joao   | joao@email.com      |
|  2 | Maria  | maria@email.com     |
+----+--------+---------------------+

Tabela: pedidos
+----+-------------+------------+--------+
| id | usuario_id  | produto    | valor  |
+----+-------------+------------+--------+
|  1 |           1 | Teclado    | 250.00 |
|  2 |           1 | Mouse      | 89.90  |
|  3 |           2 | Monitor    | 980.00 |
+----+-------------+------------+--------+

usuario_id e uma chave estrangeira (FK) — garante integridade referencial
```

---

# SQL — a linguagem dos bancos relacionais

**Structured Query Language** — padrao para consultar e manipular dados

```sql
-- Selecionar todos os pedidos do usuario Joao com valor acima de 100
SELECT p.produto, p.valor
FROM pedidos p
JOIN usuarios u ON u.id = p.usuario_id
WHERE u.nome = 'Joao'
  AND p.valor > 100.00
ORDER BY p.valor DESC;

-- Inserir novo usuario
INSERT INTO usuarios (nome, email) VALUES ('Carlos', 'carlos@email.com');

-- Atualizar email
UPDATE usuarios SET email = 'novo@email.com' WHERE id = 1;

-- Remover pedido
DELETE FROM pedidos WHERE id = 2;
```

---

# Transacoes — a garantia ACID

Operacoes que precisam ser atomicas: ou tudo acontece, ou nada acontece.

```sql
-- Transferencia bancaria: debitar de A e creditar em B
BEGIN;

UPDATE contas SET saldo = saldo - 500 WHERE id = 1;  -- debita A
UPDATE contas SET saldo = saldo + 500 WHERE id = 2;  -- credita B

-- Se qualquer linha falhar, nenhuma mudanca persiste
COMMIT;  -- confirma tudo
-- ou
ROLLBACK;  -- desfaz tudo
```

### Propriedades ACID

| Propriedade | Significado |
|---|---|
| **Atomicidade** | Tudo ou nada |
| **Consistencia** | O banco nunca fica em estado invalido |
| **Isolamento** | Transacoes nao interferem entre si |
| **Durabilidade** | Dados confirmados nao sao perdidos |

---

# Bancos relacionais mais usados

| Banco | Licenca | Destaque |
|---|---|---|
| **PostgreSQL** | Open source | Mais completo — JSON, arrays, full-text search, extensoes |
| **MySQL / MariaDB** | Open source | Muito popular em aplicacoes web, facil de iniciar |
| **SQLite** | Dominio publico | Arquivo unico, sem servidor — ideal para apps locais e testes |
| **SQL Server** | Microsoft | Integrado ao ecossistema Windows e Azure |
| **Oracle DB** | Comercial | Usado em sistemas financeiros e corporativos de grande porte |

> Para projetos novos, **PostgreSQL** e a escolha mais solida: open source, robusto e com suporte a tipos modernos como JSON e vetores.

---

<!-- _class: cover -->

## Parte 3

# Bancos Nao Relacionais

---

# Por que surgiu o NoSQL?

Aplicacoes modernas criaram demandas que os bancos relacionais atendiam com dificuldade:

```
Problema 1: Esquema rigido
  -> Rede social onde cada usuario tem campos diferentes
  -> Adicionar coluna em tabela com 500 milhoes de linhas e lento e arriscado

Problema 2: Escala horizontal
  -> Banco relacional cresce verticalmente (servidor maior)
  -> NoSQL foi projetado para distribuir dados em multiplos nos

Problema 3: Tipos de dado diferentes
  -> Documentos JSON, grafos de relacionamentos, series temporais
  -> Modelo de tabela nao e natural para todos esses casos
```

NoSQL nao significa "sem SQL" — significa **Not Only SQL**.

---

# Tipos de bancos NoSQL

### Banco de documentos

Armazena documentos JSON — flexivel, sem esquema fixo

```json
// MongoDB — cada documento pode ter estrutura diferente
{
  "_id": "abc123",
  "nome": "Joao",
  "email": "joao@email.com",
  "enderecos": [
    { "tipo": "casa", "cidade": "Sao Paulo" },
    { "tipo": "trabalho", "cidade": "Campinas" }
  ],
  "preferencias": { "tema": "escuro", "notificacoes": true }
}
```

**Quando usar:** CMS, catálogos de produtos, perfis de usuario com campos variaveis

---

# Tipos de bancos NoSQL

### Banco chave-valor

Estrutura mais simples possivel — chave unica mapeia para um valor

```
Redis:
SET session:abc123 '{"user_id":1,"role":"admin"}' EX 3600
GET session:abc123   ->  '{"user_id":1,"role":"admin"}'

SET cache:produto:42 '{"nome":"Teclado","preco":250}' EX 300
GET cache:produto:42
```

**Quando usar:** cache de sessoes, filas de mensagens, contadores em tempo real, rate limiting

> Redis e o banco de dados mais rapido em uso hoje — opera inteiramente em memoria.

---

# Tipos de bancos NoSQL

### Banco de grafos

Modela entidades como nos e relacoes como arestas

```
(Joao) -[:SEGUE]-> (Maria)
(Joao) -[:CURTIU]-> (Post:Introducao ao Docker)
(Maria) -[:COMENTOU]-> (Post:Introducao ao Docker)
(Carlos) -[:SEGUE]-> (Joao)

-- Cypher (Neo4j): quem sao os amigos dos amigos de Joao?
MATCH (joao:Usuario {nome: "Joao"})-[:SEGUE*2]->(pessoa)
WHERE pessoa <> joao
RETURN DISTINCT pessoa.nome
```

**Quando usar:** redes sociais, sistemas de recomendacao, deteccao de fraude, mapeamento de dependencias

---

# Tipos de bancos NoSQL

### Banco colunar e de series temporais

**Colunar (Cassandra, HBase):** otimizado para escrita massiva distribuida

```sql
-- Cassandra: ideal para dados de IoT, logs em alta escala
INSERT INTO leituras_sensor (sensor_id, timestamp, temperatura)
VALUES ('sensor-01', toTimestamp(now()), 23.4);
```

**Series temporais (InfluxDB, TimescaleDB):** otimizado para dados com dimensao temporal

```sql
-- TimescaleDB: metricas de infraestrutura, dados financeiros
SELECT time_bucket('1 hour', time) AS hora,
       avg(valor) AS media_temperatura
FROM leituras
WHERE sensor_id = 'sensor-01'
  AND time > NOW() - INTERVAL '24 hours'
GROUP BY hora ORDER BY hora;
```

---

# Comparativo: Relacional vs NoSQL

| Criterio | Relacional | NoSQL |
|---|---|---|
| **Esquema** | Rigido, definido antes | Flexivel, pode variar por registro |
| **Relacoes** | JOINs nativos e eficientes | Geralmente sem JOINs — dados desnormalizados |
| **Consistencia** | ACID por padrao | Varia — eventual consistency em muitos casos |
| **Escala** | Vertical (servidor maior) | Horizontal (mais nos) |
| **Queries complexas** | SQL expressivo e padronizado | API propria por banco, menos expressiva |
| **Casos ideais** | Financeiro, ERP, dados estruturados | Alta escala, esquema variavel, casos especializados |

> A escolha nao e exclusiva — sistemas modernos frequentemente combinam os dois tipos.

---

<!-- _class: cover -->

## Parte 4

# Bancos de Dados e LLMs

---

# O problema da memoria dos LLMs

Modelos de linguagem (como ChatGPT, Claude, Llama) sao treinados com dados ate uma data de corte — nao sabem o que aconteceu depois.

```
Usuario: "Qual e o preco atual do produto X no nosso catalogo?"

LLM sem acesso ao banco:
  -> Nao sabe. Dados internos da empresa nunca entraram no treino.
  -> Pode inventar uma resposta (alucinacao).

LLM com acesso ao banco via RAG:
  -> Busca o dado relevante em tempo real
  -> Usa o dado como contexto para responder com precisao
```

**RAG — Retrieval-Augmented Generation**: tecnica que conecta LLMs a fontes de dados externas no momento da inferencia.

---

# Como o RAG funciona na pratica

```
Pergunta do usuario
        |
        v
[1] Transformar em embedding (vetor numerico)
        |
        v
[2] Buscar no banco vetorial os trechos mais similares
        |
        v
[3] Montar prompt: contexto recuperado + pergunta original
        |
        v
[4] LLM gera resposta baseada no contexto real
        |
        v
Resposta fundamentada em dados atuais e precisos
```

> RAG e a ponte entre a inteligencia do modelo e o conhecimento especifico da sua aplicacao.

---

# O que e um Embedding?

Uma representacao numerica do significado semantico de um texto.

```python
# Exemplo conceitual
texto = "Como configurar autenticacao com JWT?"

embedding = modelo.encode(texto)
# [0.23, -0.87, 0.54, 0.11, -0.32, ...]  <- vetor de 1536 dimensoes

# Textos semanticamente similares ficam proximos no espaco vetorial:
texto_similar   = "Implementar login seguro com tokens"
texto_diferente = "Receita de bolo de cenoura"

similaridade(texto, texto_similar)   ->  0.91  (muito proximo)
similaridade(texto, texto_diferente) ->  0.04  (muito distante)
```

O banco vetorial armazena esses embeddings e permite busca por **similaridade semantica** — nao por palavras-chave exatas.

---

# Bancos vetoriais — o banco do ecossistema de IA

| Banco | Tipo | Destaque |
|---|---|---|
| **pgvector** | Extensao do PostgreSQL | Vetores dentro do banco relacional ja existente |
| **Pinecone** | SaaS gerenciado | Simples de usar, escalavel, sem infraestrutura |
| **Weaviate** | Open source | Busca hibrida — vetorial + keyword |
| **Chroma** | Open source | Leve, ideal para prototipagem local |
| **Qdrant** | Open source | Alta performance, escrito em Rust |

### Caso de uso tipico: base de conhecimento

```
Documentacao da empresa (PDFs, wikis, tickets)
        |
        v
Gerar embeddings e armazenar no banco vetorial
        |
        v
Usuario faz pergunta -> LLM responde com base nos seus documentos
```

---

# pgvector — vetores dentro do PostgreSQL

Se voce ja usa PostgreSQL, pode adicionar capacidade vetorial sem trocar de banco:

```sql
-- Habilitar extensao
CREATE EXTENSION IF NOT EXISTS vector;

-- Criar tabela com coluna de embedding
CREATE TABLE documentos (
  id        SERIAL PRIMARY KEY,
  conteudo  TEXT,
  embedding vector(1536)   -- dimensao depende do modelo usado
);

-- Buscar os 5 documentos mais similares a uma pergunta
SELECT conteudo,
       1 - (embedding <=> '[0.23, -0.87, 0.54, ...]') AS similaridade
FROM documentos
ORDER BY embedding <=> '[0.23, -0.87, 0.54, ...]'
LIMIT 5;
```

> Para muitas aplicacoes, `pgvector` elimina a necessidade de um banco vetorial separado.

---

<!-- _class: cover -->

## Parte 5

# Seguranca no Banco de Dados

---

# As principais ameacas

```
Aplicacao Web
      |
      |-- SQL Injection          <- entrada do usuario vira comando SQL
      |-- Credenciais expostas   <- senhas no codigo-fonte ou .env publico
      |-- Permissoes excessivas  <- usuario da app com acesso de admin
      |-- Dados sensiveis        <- senhas e CPFs sem criptografia
      |-- Backup sem protecao    <- dump copiado sem criptografia
      |-- Acesso direto exposto  <- porta 5432 aberta para a internet
```

> A maioria dos vazamentos de banco nao e exploracao sofisticada — e configuracao errada.

---

# SQL Injection — revisao com foco no banco

Ja vimos XSS e SQL Injection na Semana 2. Aqui o foco e na camada do banco:

```python
# ERRADO — concatenacao direta de input
query = f"SELECT * FROM usuarios WHERE email = '{email}'"
cursor.execute(query)

# Atacante envia: ' OR '1'='1
# Query resultante:
# SELECT * FROM usuarios WHERE email = '' OR '1'='1'
# Retorna TODOS os usuarios

# CORRETO — parametros separados dos dados
cursor.execute(
    "SELECT * FROM usuarios WHERE email = %s",
    (email,)  # driver trata como dado, nunca como SQL
)
```

### Defesas no nivel do banco
- Criar usuario de banco com permissoes minimas — sem `DROP`, sem `CREATE`
- Usar stored procedures para operacoes criticas
- Ativar query logging para auditoria

---

# Principio do menor privilegio

O usuario que a aplicacao usa para conectar ao banco **nao precisa de todos os poderes**.

```sql
-- Criar usuario especifico para a aplicacao
CREATE USER app_usuario WITH PASSWORD 'senha-forte-aqui';

-- Conceder apenas o necessario
GRANT SELECT, INSERT, UPDATE ON TABLE pedidos TO app_usuario;
GRANT SELECT ON TABLE produtos TO app_usuario;

-- Nao conceder
-- DROP TABLE, CREATE TABLE, TRUNCATE, pg_dump, acesso a outras tabelas
```

### Por que importa

```
Atacante explora SQL Injection
        |
        v
Acessa o banco com o usuario da aplicacao
        |
Com privilegio total:   DROP TABLE users;  <- catastrofico
Com privilegio minimo:  SELECT na tabela pedidos apenas  <- dano contido
```

---

# Criptografia de dados sensiveis

Nem todo dado deve ser armazenado como texto puro — mesmo dentro do banco.

```sql
-- ERRADO — CPF em texto puro
INSERT INTO clientes (nome, cpf) VALUES ('Joao', '123.456.789-00');

-- CORRETO — criptografar antes de inserir (pgcrypto no PostgreSQL)
INSERT INTO clientes (nome, cpf)
VALUES ('Joao', pgp_sym_encrypt('123.456.789-00', 'chave-secreta'));

-- Descriptografar ao consultar
SELECT nome, pgp_sym_decrypt(cpf::bytea, 'chave-secreta') AS cpf
FROM clientes WHERE id = 1;
```

### O que deve ser criptografado ou hasheado

| Dado | Tecnica |
|---|---|
| Senha do usuario | Hash com Argon2id ou bcrypt — nunca reversivel |
| CPF, RG, passaporte | Criptografia simetrica (AES-256) |
| Cartao de credito | PCI-DSS — tokenizacao, nunca armazenar o numero completo |
| Dados medicos | Criptografia + controle de acesso por papel |

---

# Credenciais fora do codigo-fonte

```python
# ERRADO — credenciais hardcoded
conn = psycopg2.connect(
    host="db.empresa.com",
    database="producao",
    user="admin",
    password="minhasenha123"  # <- aparece no Git para sempre
)

# CORRETO — variaveis de ambiente
import os
conn = psycopg2.connect(
    host=os.environ["DB_HOST"],
    database=os.environ["DB_NAME"],
    user=os.environ["DB_USER"],
    password=os.environ["DB_PASSWORD"]
)
```

### Boas praticas
- `.env` no `.gitignore` — nunca commitar
- Usar secrets managers em producao: AWS Secrets Manager, HashiCorp Vault, Doppler
- Rotacionar credenciais periodicamente
- Auditar repositorios publicos com `git-secrets` ou `truffleHog`

---

# Protecao da infraestrutura do banco

```
Internet publica
      |
   Firewall / Security Group
      |
   Servidor de aplicacao (VPC privada)
      |
   Banco de dados (sub-rede privada — sem acesso direto da internet)
```

### Checklist de infraestrutura

- Porta do banco (`5432` PostgreSQL, `3306` MySQL) **nunca exposta** para a internet
- Acesso apenas por IP interno ou VPN
- SSL/TLS obrigatorio na conexao entre app e banco
- Backups automaticos criptografados e testados regularmente
- Monitorar tentativas de login falhas e queries anomalas
- Versao do banco sempre atualizada — patches de seguranca importam

> Um banco exposto na internet sem protecao e encontrado por scanners automatizados em minutos.

---

# LGPD e dados pessoais no banco

A Lei Geral de Protecao de Dados (Lei 13.709/2018) impoe obrigacoes sobre qualquer sistema que armazene dados de brasileiros.

### O que isso significa na pratica

| Obrigacao | Implementacao tecnica |
|---|---|
| **Finalidade** | Armazenar so o que e necessario — sem coleta excessiva |
| **Acesso controlado** | Logs de quem acessou qual dado e quando |
| **Direito ao esquecimento** | Mecanismo para deletar ou anonimizar dados do usuario |
| **Notificacao de vazamento** | Processo definido para identificar e reportar incidentes |
| **Seguranca** | Criptografia, menor privilegio, auditorias |

> Ignorar LGPD nao e so risco juridico — e risco de reputacao. Multas chegam a 2% do faturamento, limitadas a R$50 milhoes por infracao.

---

<!-- _class: cover -->

## Recapitulando

# O que vimos hoje

---

# Resumo

<div class="columns">

**Bancos Relacionais**
- Tabelas, esquema rigido, SQL padronizado
- Transacoes ACID — garantia de consistencia
- JOINs expressivos para dados interrelacionados
- PostgreSQL como escolha solida para projetos novos

**Bancos NoSQL**
- Documentos, chave-valor, grafos, series temporais
- Esquema flexivel, escala horizontal
- Cada tipo resolve um problema especifico
- Frequentemente usados junto com banco relacional

</div>

---

# Resumo (continuacao)

<div class="columns">

**Bancos e LLMs**
- RAG conecta LLMs a dados atuais em tempo real
- Embeddings representam significado como vetores
- Bancos vetoriais: Pinecone, Weaviate, Chroma, Qdrant
- pgvector adiciona vetores direto no PostgreSQL

**Seguranca**
- SQL Injection — sempre usar parametros separados
- Menor privilegio — usuario da app sem poderes de admin
- Criptografar dados sensiveis — CPF, cartao, dados medicos
- Credenciais em variaveis de ambiente, nunca no codigo
- Banco nunca exposto diretamente na internet

</div>

---

# Desafio da Semana 4

### Nivel iniciante
Instale o PostgreSQL via Docker (`docker run -d -e POSTGRES_PASSWORD=senha postgres:16`) e crie uma tabela de usuarios com `id`, `nome` e `email`. Insira 3 registros e consulte com `SELECT`.

### Nivel intermediario
Construa uma API em Node.js ou Python que conecta ao PostgreSQL e expoe rotas para criar e listar registros. Use **parametros preparados** em todas as queries — nenhuma concatenacao de string.

### Nivel avancado
Configure `pgvector` no seu PostgreSQL, gere embeddings de textos curtos usando a API da OpenAI ou um modelo local, armazene no banco e implemente uma rota de busca semantica que retorna os documentos mais similares a uma pergunta.

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 4

# Dados bem guardados, sistema bem construido

*Nao importa o nivel. Importa entregar — sabendo onde e como os dados vivem.*
