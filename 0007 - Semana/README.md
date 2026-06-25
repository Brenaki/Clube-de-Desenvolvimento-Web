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

## Clube de Desenvolvimento Web — Semana 7

# Modelando Antes de Codar

*Diagrama de caso de uso e modelagem de banco de dados*

---

# Agenda

1. **Por que modelar antes de codar**
2. **Diagrama de caso de uso** — atores, casos e relacoes
3. **Modelagem de dados** — do conceito ao banco
4. **Modelo relacional vs documento** — modelar nos dois mundos
5. **Mao na massa** — o desafio comeca em sala

> "Codigo escrito sem modelo e construcao sem planta. Funciona ate a primeira mudanca."

---

# Por que parar para modelar?

Ate agora a gente construiu coisas. Hoje a gente desenha **o que** construir, **antes** de construir.

```
Sem modelo:
  Comeca a codar -> descobre regra de negocio no meio -> refatora tudo
  Tabela errada -> migracao dolorosa -> dado inconsistente

Com modelo:
  Entende o problema -> desenha atores e dados -> codigo vira consequencia
```

### O que modelagem resolve

- **Alinhamento** — todo mundo enxerga o mesmo sistema antes de uma linha de codigo
- **Antecipa erros** — e mais barato apagar uma seta no diagrama do que refatorar uma tabela em producao
- **Documentacao viva** — o diagrama explica o sistema melhor que qualquer texto

> Conecta direto com a Semana 4: la decidimos *como* guardar os dados. Hoje decidimos *o que* sao os dados e *quem* os usa.

---

<!-- _class: cover -->

## Parte 1

# Diagrama de Caso de Uso

---

# O que e um diagrama de caso de uso?

Uma representacao visual de **quem** usa o sistema e **o que** consegue fazer nele. Foca no comportamento, nao na implementacao.

```
        Sistema: Loja Online
   +------------------------------------+
   |                                    |
   |   (  Buscar produto  )             |
   |                                    |
 [Cliente] --- (  Fazer pedido  )       |
   |                                    |
   |   (  Acompanhar entrega )          |
   |                                    |
   |   (  Gerenciar estoque )--- [Admin]|
   +------------------------------------+
```

### Os 3 elementos basicos

- **Ator** — quem interage (pessoa, sistema externo). Desenhado como boneco palito
- **Caso de uso** — uma acao com valor para o ator. Desenhado como elipse
- **Sistema** — a fronteira (retangulo) que separa o que e interno do que e externo

---

# Atores e relacoes

```
Ator primario     ->  inicia o uso do sistema (ex: Cliente)
Ator secundario   ->  o sistema aciona (ex: Gateway de Pagamento)

Relacoes entre casos de uso:

  <<include>>   -> caso SEMPRE usa outro
                   "Fazer pedido" <<include>> "Calcular frete"

  <<extend>>    -> caso OPCIONALMENTE estende outro
                   "Fazer pedido" <<extend>> "Aplicar cupom"

  Generalizacao -> um ator/caso e um tipo especial de outro
                   "Admin" é um "Usuario" com mais permissoes
```

> Cuidado comum: caso de uso nao e tela nem botao. E um **objetivo** do ator. "Login" muitas vezes e um `<<include>>`, nao um caso de uso principal.

---

# Lendo um caso de uso na pratica

```
Sistema: Biblioteca

  [Leitor] ----------- ( Pesquisar acervo )
     |
     +---------------- ( Reservar livro ) ..include..> ( Autenticar )
     |
     +---------------- ( Devolver livro ) ..extend..> ( Pagar multa )

  [Bibliotecario] ---- ( Cadastrar livro )
     |
     +---------------- ( Registrar emprestimo )
```

### Como validar seu diagrama

- Todo caso de uso entrega **valor** a um ator? Se nao, talvez seja so um passo interno
- Os nomes sao **verbos no infinitivo**? (Pesquisar, Reservar, Devolver)
- Um ator novo consegue entender o sistema **so olhando** o diagrama?

---

<!-- _class: cover -->

## Parte 2

# Modelagem de Banco de Dados

---

# Os tres niveis de modelagem

Do mundo real ate o banco rodando — em tres camadas de abstracao:

```
[1] CONCEITUAL  -> O QUE existe no dominio (independe de tecnologia)
        |           Entidades, atributos, relacionamentos
        v           Ferramenta: Diagrama Entidade-Relacionamento (DER)
[2] LOGICO      -> COMO estruturar (ja escolheu o paradigma)
        |           Tabelas e chaves (relacional) ou colecoes (documento)
        v           Normalizacao, cardinalidade
[3] FISICO      -> Onde roda de verdade
                    Tipos, indices, constraints, o SQL/schema real
```

> Voce nao precisa decorar UML inteira. Precisa saber separar **o conceito** da **implementacao**.

---

# Modelo Entidade-Relacionamento (conceitual)

A pergunta central: quais sao as **coisas** do meu sistema e como elas **se conectam**?

```
   [ CLIENTE ]                    [ PEDIDO ]                  [ PRODUTO ]
   - id                           - id                        - id
   - nome            1      N     - data           N      N   - nome
   - email      ----------<>----- - status    ----<>----      - preco
                  "faz"            - total      "contem"       - estoque

Cardinalidade:
  1 Cliente  faz  N Pedidos          (um-para-muitos)
  N Pedidos contem  N Produtos       (muitos-para-muitos)
```

### As tres cardinalidades

| Tipo | Exemplo | No relacional vira |
|---|---|---|
| **1:1** | Usuario tem 1 Perfil | FK com restricao UNIQUE |
| **1:N** | Cliente tem N Pedidos | FK no lado "N" |
| **N:N** | Pedido tem N Produtos | Tabela associativa (juncao) |

---

# Mesmo dominio, dois mundos

A escolha da Semana 4 (relacional vs nao relacional) muda **como** o modelo vira realidade:

<div class="columns">

**Relacional — normalizado**
```sql
-- Dados divididos, ligados por FK
TABELA clientes (id, nome, email)
TABELA pedidos  (id, cliente_id, total)
TABELA itens    (pedido_id,
                 produto_id, qtd)
TABELA produtos (id, nome, preco)

-- Forca: integridade e
-- consultas com JOIN
```

**Documento — desnormalizado**
```js
// Dados agrupados onde sao lidos
{
  _id: "ped_42",
  cliente: {
    nome: "Joao",
    email: "joao@x.com"
  },
  itens: [
    { produto: "Teclado",
      preco: 250, qtd: 1 }
  ],
  total: 250
}
// Forca: leitura rapida,
// sem JOIN
```

</div>

> Nao existe modelo "certo" universal. Existe modelo certo **para o seu padrao de acesso**.

---

# Como escolher o paradigma para o SEU projeto

Retomando a Semana 4 — agora aplicado a decisao de modelagem:

```
Seus dados tem relacoes fortes e precisam de consistencia?
  (financeiro, estoque, pedidos)        -> Relacional

Seu esquema muda muito ou varia por registro?
  (perfis flexiveis, catalogo diverso)  -> Documento (NoSQL)

Voce le sempre o "objeto inteiro" junto?
  (1 pedido com seus itens de uma vez)  -> Documento favorece

Voce cruza dados de formas imprevisiveis?
  (relatorios, analytics)               -> Relacional favorece
```

### Perguntas que guiam a modelagem

- Quais sao as entidades centrais? (vire substantivos do dominio)
- Como elas se relacionam e com qual cardinalidade?
- O que voce vai **consultar com mais frequencia**? Modele para esse caminho.

> Lembre da Semana 5: seja relacional ou documento, o ORM/ODM vai mapear esse modelo para objetos no codigo.

---

<!-- _class: cover -->

## O desafio de hoje

# Agora e com voces

---

# Desafio da Semana 7

O foco hoje e **desenhar antes de codar**. Escolha um dominio simples: biblioteca, locadora, agenda de consultas, lista de tarefas, e-commerce pequeno.

### Nivel iniciante
Faca um **diagrama de caso de uso** do seu dominio: pelo menos 2 atores e 4 casos de uso. Identifique quem inicia cada acao. Pode desenhar na mao, no [draw.io](https://draw.io) ou no [Excalidraw](https://excalidraw.com).

### Nivel intermediario
Crie o **modelo conceitual (DER)** do mesmo dominio: ao menos 3 entidades, com atributos e cardinalidade entre elas. Marque chaves primarias e relacionamentos.

### Nivel avancado
Transforme o modelo em **schema real**, escolhendo o banco — relacional ou nao relacional. Justifique a escolha pelo padrao de acesso. Entregue o `CREATE TABLE` (relacional) ou a estrutura das colecoes (documento), e descreva como ficaria o modelo no **outro** paradigma.

> Traga o diagrama no proximo encontro. Vamos revisar os modelos juntos — erro de modelagem aqui custa um traço apagado, nao uma migração.

---

# Ferramentas sugeridas

| Ferramenta | Para que serve | Custo |
|---|---|---|
| **draw.io / diagrams.net** | Caso de uso e DER, generalista | Gratuito |
| **Excalidraw** | Rascunho rapido, estilo quadro branco | Gratuito |
| **dbdiagram.io** | DER focado em banco, gera SQL | Gratuito |
| **Mermaid** | Diagrama como codigo (Markdown) | Gratuito |
| **Papel e caneta** | O primeiro rascunho de todo modelo | Gratuito |

```
// Exemplo: ER em Mermaid (cola no GitHub ou no mermaid.live)
erDiagram
  CLIENTE ||--o{ PEDIDO : faz
  PEDIDO  }o--o{ PRODUTO : contem
  CLIENTE { int id  string nome  string email }
  PEDIDO  { int id  date data  float total }
```

> Comece no papel. Ferramenta e detalhe — o que importa e o raciocinio do modelo.

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 7

# Quem nao modela, refatora

*Nao importa o nivel. Importa entregar — desenhando o sistema antes de constru-lo.*
