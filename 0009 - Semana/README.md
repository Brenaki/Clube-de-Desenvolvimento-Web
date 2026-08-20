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

## Clube de Desenvolvimento Web — Semana 8

# Rust — Seguranca de Memoria sem Coletor de Lixo

*Variaveis, funcoes, if e tipos primitivos — rustlings 00 a 04*

---

# Agenda

1. **Por que Rust** — o problema que ele resolve, direto da Semana 7
2. **Ferramentas** — `rustup`, `cargo` e o `rustlings`
3. **Variaveis** — imutabilidade por padrao, `mut` e shadowing
4. **Funcoes** — sintaxe, tipos e a diferenca entre statement e expressao
5. **If como expressao** — a maior mudanca de mentalidade vindo de C
6. **Tipos primitivos** — inteiros, float, bool, char, tuplas e arrays
7. **Atividades ao vivo** — rustlings junto, ao vivo

> "C confia em voce. Rust verifica voce. As duas coisas tem seu preco."

---

# De onde viemos

Voces passaram a Semana 7 escrevendo isso:

```c
No *criar_no(int valor) {
    No *novo = malloc(sizeof(No));
    novo->valor = valor;
    novo->proximo = NULL;
    return novo;
}

void liberar(No *atual) {
    while (atual != NULL) {
        No *proximo = atual->proximo;
        free(atual);
        atual = proximo;
    }
}
```

`malloc`, `free`, ponteiro duplo, `No **cabeca`. Voces ja sabem por que ponteiro existe e por que ele e perigoso.

> Hoje voces vao ver a mesma ideia — memoria, endereco, referencia — em uma linguagem que **nao deixa voce errar em silencio**.

---

<!-- _class: cover -->

## Parte 1

# Por que Rust?

---

# Os bugs que o C permite escrever

Todos compilam. Nenhum avisa. O comportamento so aparece em producao, tarde da noite.

```c
// 1. Use-after-free — usar memoria ja liberada
No *no = criar_no(10);
free(no);
printf("%d\n", no->valor);      // le memoria que ja nao e sua

// 2. Double free — liberar duas vezes
free(no);
free(no);                       // corrompe o alocador

// 3. Dangling pointer — ponteiro para algo que nao existe mais
No *pega_ponteiro() {
    No local = {10, NULL};
    return &local;               // endereco de variavel que morreu
}

// 4. Buffer overflow — escrever fora dos limites do array
int vetor[5];
vetor[10] = 99;                  // sem checagem, sem erro, sem aviso
```

> Esses 4 bugs sao responsaveis por boa parte das falhas de seguranca criticas ja encontradas em software escrito em C e C++.

---

# O que o Rust faz diferente

Rust tem um componente no compilador chamado **borrow checker**. Ele analisa quem "possui" cada pedaco de memoria — **antes do codigo rodar**.

```
C:      compila  ->  roda  ->  talvez quebre em producao
Rust:   nao compila se houver risco  ->  o bug nunca chega a rodar
```

```rust
fn pega_ponteiro() -> &i32 {
    let local = 10;
    &local   // ERRO DE COMPILACAO:
             // `local` sai de escopo no fim da funcao,
             // a referencia nao pode sobreviver a ela
}
```

O mesmo bug de "dangling pointer" que em C compila e quebra depois, em Rust **nem chega a virar binario**.

> Isso e o que Rust quer dizer com "seguranca de memoria sem coletor de lixo": nao existe um processo em runtime limpando memoria (como no JavaScript) — as regras sao verificadas em tempo de compilacao, com custo zero na execucao.

---

# Ownership — a ideia por tras de tudo

Nas proximas semanas voces vao ver isso em profundidade. Por hoje, guarde a regra central:

```
Toda memoria em Rust tem exatamente UM dono.

Quando o dono sai de escopo, a memoria e liberada
automaticamente — sem voce chamar free().

Quando voce passa o dono para outro lugar,
o original perde o acesso.
```

Compare com o que voces fizeram na Semana 7:

| Em C, voce fazia manualmente | Em Rust, o compilador garante |
|---|---|
| Chamar `free()` no lugar certo | Liberacao automatica ao sair de escopo |
| Nao usar ponteiro apos `free()` | Impossivel — o compilador bloqueia |
| Nao liberar duas vezes | Impossivel — so existe um dono |
| Verificar limites do array na mao | Checagem de limites em tempo de execucao |

> Ownership nao e sobre escrever menos codigo. E sobre o compilador ser seu revisor de codigo mais rigoroso, antes de qualquer usuario ver o bug.

---

<!-- _class: cover -->

## Parte 2

# Ferramentas

---

# rustup e cargo

**rustup** — instala e gerencia versoes do compilador Rust
**cargo** — gerenciador de pacotes e build, equivalente ao `npm` do Node ou ao `pip` do Python

```bash
# Instalar o Rust (Linux/macOS)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Verificar instalacao
rustc --version
cargo --version

# Criar um novo projeto
cargo new meu_projeto
cd meu_projeto

# Rodar
cargo run

# Compilar sem rodar
cargo build

# Rodar os testes
cargo test
```

### Paralelo com Docker (Semana 3)

`cargo` fixa as versoes das dependencias no `Cargo.lock`, igual `package-lock.json` no Node — o mesmo problema de "funciona na minha maquina" que o Docker resolve em outra camada.

---

# rustlings — aprendendo com o compilador

`rustlings` e uma colecao de pequenos exercicios que **nao compilam de proposito**. Sua tarefa e consertar o codigo ate ele compilar e passar no teste.

```bash
# Instalar
cargo install rustlings

# Iniciar o curso na pasta atual
rustlings init
cd rustlings

# Roda em modo "watch" — reexecuta a cada save
rustlings watch
```

### O fluxo de cada exercicio

```
1. Abra o arquivo indicado em exercises/
2. Leia o erro do compilador com atencao — Rust explica MUITO bem
3. Corrija o codigo
4. Salve — rustlings roda de novo automaticamente
5. Peça uma dica com: rustlings hint <nome_do_exercicio>
```

> Hoje vamos ate os modulos `03_if` e `04_primitive_types`. Chegar ate ali e o objetivo do encontro — o resto e desafio de casa.

---

<!-- _class: cover -->

## Atividade 1

# Primeiro contato

---

# Rode o ambiente

Antes de seguir, confirme que o ambiente esta pronto:

```bash
rustc --version      # deve mostrar uma versao instalada
cargo --version       # idem
rustlings             # deve listar o progresso, tudo com X vermelho
```

### Enquanto isso, discuta com quem esta do lado

- Voces ja tinham ouvido falar de Rust? Onde?
- Alguem sabe qual empresa ou projeto famoso usa Rust hoje? (dica: navegadores, sistemas operacionais, ferramentas de linha de comando)

> Se o ambiente nao rodar, levante a mao — resolvemos antes de continuar.

---

<!-- _class: cover -->

## Parte 3

# Variaveis

---

# Imutavel por padrao — a primeira surpresa

Em C, toda variavel e mutavel a menos que voce escreva `const`. Em Rust, e o contrario.

```rust
fn main() {
    let x = 5;
    println!("O valor de x e: {}", x);

    x = 6;   // ERRO DE COMPILACAO:
             // cannot assign twice to immutable variable `x`
}
```

```rust
// Para permitir mudanca, seja explicito:
fn main() {
    let mut x = 5;
    println!("O valor de x e: {}", x);

    x = 6;   // agora compila
    println!("Agora x e: {}", x);
}
```

> Em C, `int x = 5;` sempre pode mudar. Em Rust, `let x = 5;` e um compromisso: "isso nunca vai mudar" — o compilador cobra esse compromisso.

---

# Por que isso importa

Nao e frescura de sintaxe. Imutabilidade por padrao previne uma classe inteira de bugs de concorrencia e de logica.

```rust
// O compilador te avisa se voce declarar mut sem precisar:
let mut total = 0;
println!("{}", total);
// warning: variable does not need to be mutable
```

```
Se a variavel nunca muda -> nao ha risco de outra parte
do codigo (ou outra thread) alterar o valor sem voce esperar.

Voce le "let mut" no codigo e imediatamente sabe:
"esse valor vai mudar em algum lugar aqui embaixo — preste atencao."
```

> Semana 4 falamos de banco de dados imutavel vs mutavel em contexto de auditoria. A mesma logica se aplica aqui: menos coisa podendo mudar = menos lugares para o bug se esconder.

---

# Shadowing — reusar o nome, nao o valor

Diferente de `mut`, shadowing cria uma **variavel nova** com o mesmo nome.

```rust
fn main() {
    let x = 5;
    let x = x + 1;        // nova variavel x, valor 6
    let x = x * 2;         // nova variavel x, valor 12

    println!("O valor de x e: {}", x);   // 12
}
```

### Diferenca pratica entre `mut` e shadowing

```rust
let mut espacos = "   ";
espacos = espacos.len();   // ERRO: nao pode mudar de &str para usize

let espacos = "   ";
let espacos = espacos.len(); // OK: e uma variavel nova, pode mudar de tipo
```

> Shadowing permite transformar um valor passo a passo (ex: texto -> numero) sem precisar inventar `espacos_str` e `espacos_num`.

---

# Constantes

```rust
const LIMITE_TENTATIVAS: u32 = 3;
const PI: f64 = 3.14159;

fn main() {
    println!("Voce tem {} tentativas", LIMITE_TENTATIVAS);
}
```

| Criterio | `let` | `let mut` | `const` |
|---|---|---|---|
| Pode mudar de valor | Nao | Sim | Nunca |
| Precisa de tipo anotado | Nao (inferido) | Nao (inferido) | Sim, sempre |
| Escopo | Bloco onde foi criada | Bloco onde foi criada | Pode ser global |
| Calculado em tempo de execucao | Sim | Sim | Nao — so em tempo de compilacao |

> Convencao: constantes em `SCREAMING_SNAKE_CASE`, variaveis em `snake_case` — o compilador nao obriga, mas o `clippy` (linter do Rust) reclama se voce nao seguir.

---

<!-- _class: cover -->

## Atividade 2

# Corrija o rustlings — variables

---

# `exercises/01_variables`

Va ate o rustlings e resolva, na ordem, `variables1.rs` ate `variables6.rs`.

```
variables1.rs  ->  falta um `let`
variables2.rs  ->  tipo incompativel na comparacao
variables3.rs  ->  falta `mut`
variables4.rs  ->  mudou de tipo — precisa de shadowing, nao de mut
variables5.rs  ->  shadowing dentro de um bloco { }
variables6.rs  ->  constante sem tipo anotado
```

### Antes de rodar de novo, leia o erro

```
error[E0384]: cannot assign twice to immutable variable `x`
  --> exercises/01_variables/variables1.rs:6:5
   |
5  |     let x: i32 = 1;
   |         - first assignment to `x`
6  |     x = 2;
   |     ^^^^^ cannot assign twice to immutable variable
   |
help: consider making this binding mutable: `mut x`
```

> Rust nao so fala que esta errado — ele sugere a correcao. Leia antes de adivinhar.

---

<!-- _class: cover -->

## Parte 4

# Funcoes

---

# Sintaxe basica

```rust
fn soma(a: i32, b: i32) -> i32 {
    a + b     // sem `return` e sem `;` -> isso e o valor devolvido
}

fn saudacao(nome: &str) {
    println!("Ola, {}!", nome);   // sem retorno -> devolve () implicito
}

fn main() {
    let resultado = soma(3, 4);
    println!("{}", resultado);   // 7

    saudacao("Vitor");
}
```

### Comparando com C

```c
int soma(int a, int b) {
    return a + b;     // return e ; sao obrigatorios
}
```

```
C:     tipo de retorno vem ANTES do nome:      int soma(...)
Rust:  tipo de retorno vem DEPOIS, com seta:    fn soma(...) -> i32
```

> Em Rust, todo parametro **exige** o tipo declarado — sem isso o compilador nao aceita. Nao existe parametro "generico por padrao" como em JavaScript.

---

# Statement vs Expression — a distincao que muda tudo

Essa e a base para entender o `if` da proxima parte.

```
Statement (comando)  ->  executa uma acao, nao produz valor
Expression (expressao) ->  avalia e RESULTA em um valor
```

```rust
fn main() {
    let y = {          // este bloco inteiro e uma expressao
        let x = 3;
        x + 1           // sem ; -> e o valor que o bloco "retorna"
    };

    println!("{}", y);  // 4
}
```

```rust
// Se voce colocar ; no fim, vira statement e passa a valer ()
let y = {
    let x = 3;
    x + 1;    // <- esse ; transforma a expressao em statement
};
// y agora e (), nao 4. Erro de tipo em tempo de compilacao.
```

> `;` no fim de uma linha em Rust nao e so estetica — ele muda se a linha **produz** um valor ou nao. Esse detalhe explode em erro de compilacao com muita frequencia no comeco.

---

<!-- _class: cover -->

## Atividade 3

# Corrija o rustlings — functions

---

# `exercises/02_functions`

Resolva `functions1.rs` ate `functions5.rs`.

```
functions1.rs  ->  chamando uma funcao que nao existe ainda
functions2.rs  ->  tipo do parametro errado na chamada
functions3.rs  ->  falta declarar o tipo do parametro
functions4.rs  ->  falta declarar o tipo de retorno com ->
functions5.rs  ->  ; sobrando no fim, que transforma expression em statement
```

### Pergunta para discutir em grupo antes de seguir

Nesse trecho, qual e o valor de `resultado` e por que?

```rust
fn calcula(x: i32) -> i32 {
    let y = x * 2;
    y + 1;
}
```

> Dica: procure o `;` que nao deveria estar ali.

---

<!-- _class: cover -->

## Parte 5

# If como Expressao

---

# If sem parenteses, com chaves obrigatorias

```rust
fn main() {
    let numero = 6;

    if numero % 4 == 0 {
        println!("divisivel por 4");
    } else if numero % 3 == 0 {
        println!("divisivel por 3");
    } else {
        println!("nenhum dos dois");
    }
}
```

```c
// C — parenteses obrigatorios, chaves opcionais (perigoso)
if (numero % 4 == 0)
    printf("divisivel por 4\n");
```

```
C:     if (condicao) { ... }     parenteses SIM, chaves opcional
Rust:  if condicao { ... }        parenteses NAO, chaves SEMPRE
```

> Chaves obrigatorias eliminam uma classe classica de bug em C: o famoso "dangling else", onde um `if` sem chaves so controla a proxima linha e voce jura que controlava o bloco inteiro.

---

# A condicao TEM que ser bool

Essa e a mudanca mais brusca vindo de C.

```c
// C — qualquer inteiro diferente de zero e "verdadeiro"
int numero = 3;
if (numero) {
    printf("verdadeiro\n");   // compila e roda — numero=3 e truthy
}
```

```rust
// Rust — NAO EXISTE conversao automatica de numero para bool
let numero = 3;
if numero {
    println!("verdadeiro");
}
// ERRO DE COMPILACAO:
// expected `bool`, found integer
```

```rust
// A forma correta em Rust: seja explicito
let numero = 3;
if numero != 0 {
    println!("verdadeiro");
}
```

> Em C, `if (x = 5)` (atribuicao em vez de comparacao) compila, roda e vira um bug classico de producao. Em Rust isso e impossivel: uma atribuicao nao e um `bool`, o compilador rejeita antes de voce rodar uma unica vez.

---

# If como expressao — atribuindo o resultado

Como todo bloco `{ }` pode ser uma expressao, `if` pode ser usado direto em um `let`.

```rust
fn main() {
    let condicao = true;
    let numero = if condicao { 5 } else { 6 };

    println!("O valor de numero e: {}", numero);
}
```

### A regra que o compilador cobra: os dois ramos precisam devolver o MESMO tipo

```rust
let numero = if condicao { 5 } else { "seis" };
// ERRO DE COMPILACAO:
// `if` and `else` have incompatible types
// expected integer, found `&str`
```

```c
// Em C, o equivalente seria o operador ternario:
int numero = condicao ? 5 : 6;
// C tambem exige tipos compativeis aqui — mas so nesse operador especifico.
// Rust generaliza essa ideia para QUALQUER bloco { }.
```

> Isso conecta direto com Big O da Semana 7: o compilador precisa saber, em tempo de compilacao, exatamente quanto espaco `numero` ocupa. Se os ramos tivessem tipos diferentes, o tamanho em memoria seria imprevisivel.

---

<!-- _class: cover -->

## Atividade 4

# Preveja o erro

---

# Esses trechos compilam?

Para cada um, decida: **compila** ou **erro de compilacao**? Se der erro, qual e a mensagem provavel?

```rust
// A
let x = 5;
if x { println!("ok"); }

// B
let y = if true { 10 } else { 20 };

// C
let z = if true { 10 } else { "vinte" };

// D
let mut contador = 0;
if contador == 0 {
    contador = 1;
}

// E
let resultado = if contador > 0 { "positivo" } else { "zero ou negativo" };
```

> Depois de decidir em grupo, rode no rustlings ou em https://play.rust-lang.org e confira.

---

<!-- _class: cover -->

## Atividade 5

# Corrija o rustlings — if

---

# `exercises/03_if`

Resolva `if1.rs` e `if2.rs`.

```
if1.rs  ->  os dois ramos do if precisam devolver o mesmo tipo
if2.rs  ->  falta implementar a logica de comparacao dentro da funcao
```

### Discuta em grupo antes de corrigir

O `if1.rs` do rustlings tem uma funcao parecida com isso:

```rust
pub fn bigger(a: i32, b: i32) -> i32 {
    if a > b {
        a
    } else {
        b
    }
}
```

- Por que essa funcao **nao precisa** de `return`?
- O que aconteceria se voce colocasse `;` depois de `a` no primeiro ramo?

---

<!-- _class: cover -->

## Parte 6

# Tipos Primitivos

---

# Inteiros — Rust obriga voce a escolher o tamanho

Em C, `int` costuma ser 4 bytes, mas isso **depende do compilador e da plataforma**. Em Rust, o tamanho e parte do nome do tipo.

| Tipo Rust | Tamanho | Faixa (com sinal) | Equivalente aproximado em C |
|---|---|---|---|
| `i8` / `u8` | 1 byte | -128 a 127 / 0 a 255 | `char` / `unsigned char` |
| `i16` / `u16` | 2 bytes | -32.768 a 32.767 | `short` |
| `i32` / `u32` | 4 bytes | ~-2,1bi a 2,1bi | `int` |
| `i64` / `u64` | 8 bytes | ~-9,2 quintilhoes | `long long` |
| `usize` / `isize` | depende da arquitetura | tamanho de um ponteiro | `size_t` |

```rust
let idade: u8 = 25;         // nunca negativo, cabe em 1 byte
let saldo: i64 = -500_000;  // pode ser negativo, precisa de faixa grande
let indice: usize = 0;      // usado especificamente para indexar arrays
```

> `usize` e o tipo usado para indices de array e tamanhos — o compilador **exige** esse tipo especifico ali, nao aceita `i32` direto. Isso evita index negativo por engano, algo que em C so vira bug em tempo de execucao.

---

# Overflow — o que acontece quando estoura

```c
// C — overflow silencioso, comportamento indefinido
unsigned char x = 255;
x = x + 1;
printf("%d\n", x);   // imprime 0 — "deu a volta" sem avisar nada
```

```rust
// Rust em modo debug — PANIC, o programa para imediatamente
let x: u8 = 255;
let y = x + 1;
// thread 'main' panicked at 'attempt to add with overflow'
```

```rust
// Rust em modo release (otimizado) — "da a volta" como o C, silenciosamente
// Para controlar isso explicitamente, use metodos como:
let x: u8 = 255;
let y = x.wrapping_add(1);        // 0, intencional e explicito
let y = x.checked_add(1);         // None, porque estourou
let y = x.saturating_add(1);      // 255, trava no maximo
```

> De novo o mesmo padrao da Semana 7 com `shell=True`: C confia cegamente na entrada, Rust obriga voce a decidir explicitamente o que fazer quando algo sai do esperado.

---

# Float, bool e char

```rust
let preco: f64 = 19.90;      // ponto flutuante de 64 bits, padrao
let desconto: f32 = 0.1;     // 32 bits, precisa ser explicito

let ativo: bool = true;      // so true ou false — nunca 0 ou 1

let letra: char = 'A';       // SEMPRE 4 bytes, um caractere Unicode completo
let emoji: char = '🦀';       // char em Rust aceita emoji, acento, kanji...
```

### A diferenca de `char` para C

```c
char letra = 'A';   // em C, char e 1 BYTE — so ASCII, nao aceita 'é' direto
```

```
C    char   ->  1 byte   ->  so 256 valores possiveis (ASCII estendido)
Rust char   ->  4 bytes  ->  qualquer caractere Unicode (mais de 1 milhao)
```

> Semana 4: falamos de LGPD e dados de brasileiros. Nomes com acento (`Joao` vs `João`) sao exatamente o tipo de dado que quebra em sistemas que assumem `char` de 1 byte. Rust resolve isso no nivel do tipo primitivo.

---

# Tuplas — agrupando tipos diferentes

```rust
fn main() {
    let pessoa: (&str, u8, bool) = ("Joao", 25, true);

    // Acesso por posicao, com ponto
    let nome = pessoa.0;
    let idade = pessoa.1;

    // Ou destruturando de uma vez
    let (nome, idade, ativo) = pessoa;

    println!("{} tem {} anos", nome, idade);
}
```

```rust
// Uso comum: funcao devolvendo mais de um valor
fn dividir(a: i32, b: i32) -> (i32, i32) {
    (a / b, a % b)   // (quociente, resto)
}

fn main() {
    let (q, r) = dividir(17, 5);
    println!("quociente: {}, resto: {}", q, r);
}
```

> Em C, para devolver dois valores voce precisava de `struct` ou de ponteiros de saida (`int *resto`). A tupla resolve o caso simples sem exigir declarar um tipo novo.

---

# Arrays — tamanho fixo, checado em tempo de compilacao

```rust
fn main() {
    let numeros: [i32; 5] = [1, 2, 3, 4, 5];
    // tipo: array de 5 elementos i32 — o tamanho E PARTE DO TIPO

    println!("{}", numeros[0]);   // 1
    println!("{}", numeros[10]);  // ERRO DE COMPILACAO ou PANIC em runtime
}
```

Compare com a Semana 7:

```c
int vetor[5] = {1, 2, 3, 4, 5};
printf("%d\n", vetor[10]);   // COMPILA, RODA, le memoria de outra variavel
                              // (o buffer overflow que vimos na Parte 1 de hoje)
```

```rust
// Rust checa limites em TEMPO DE EXECUCAO quando o compilador nao pode
// provar em tempo de compilacao (ex: indice vindo de input do usuario)
let numeros = [1, 2, 3, 4, 5];
let indice = 10;
println!("{}", numeros[indice]);
// thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10'
```

> `vetor[10]` em C e o mesmo bug de buffer overflow do inicio da aula — so que agora voces veem o antes (C, silencioso) e o depois (Rust, barulhento e imediato) lado a lado.

---

# Array com tamanho e valor repetido

```rust
let zeros = [0; 5];          // [0, 0, 0, 0, 0]
let tabuleiro = [0; 64];     // um tabuleiro de xadrez linear, 64 zeros

// Fatiando um array (slice) — referencia para uma parte dele
let numeros = [1, 2, 3, 4, 5];
let meio = &numeros[1..4];   // [2, 3, 4] — sem copiar os dados
```

### Array (Rust) vs Lista Encadeada (Semana 7) — o mesmo debate, nova linguagem

| Criterio | Array Rust `[T; N]` | Lista encadeada (C, Semana 7) |
|---|---|---|
| Memoria | Contigua, na pilha | Espalhada, no heap |
| Acesso por indice | `O(1)`, checado | `O(n)`, sem checagem nativa |
| Tamanho | Fixo, parte do tipo | Cresce livremente |
| Seguranca de acesso | Panic controlado se passar do limite | Undefined behavior |

> A escolha entre array e lista continua sendo sobre padrao de acesso, como na Semana 7. O que muda e que em Rust, errar o limite do array **derruba o programa de forma controlada** em vez de corromper memoria silenciosamente.

---

<!-- _class: cover -->

## Atividade 6

# Corrija o rustlings — primitive_types

---

# `exercises/04_primitive_types`

Resolva `primitive_types1.rs` ate `primitive_types6.rs`.

```
primitive_types1.rs  ->  falta declarar o tipo de uma variavel
primitive_types2.rs  ->  char precisa de aspas simples, nao duplas
primitive_types3.rs  ->  slice de array com indices errados
primitive_types4.rs  ->  acesso a tupla com indice errado
primitive_types5.rs  ->  destruturacao de tupla incompleta
primitive_types6.rs  ->  indexacao de array fora dos limites
```

### Antes de corrigir o `primitive_types6.rs`

Pense: por que Rust prefere travar o programa (`panic`) a devolver um valor de memoria aleatorio como o C faz? Quem essa escolha protege — o desenvolvedor, o usuario final, ou os dois?

---

<!-- _class: cover -->

## Recapitulando

# O que vimos hoje

---

# Resumo

<div class="columns">

**Por que Rust**
- Ownership resolve em tempo de compilacao o que C deixa para o programador em runtime
- Use-after-free, double free e dangling pointer viram erro de compilacao
- Sem coletor de lixo — o custo e verificado antes, nao pago durante a execucao

**Variaveis e Funcoes**
- Imutavel por padrao — `mut` e uma decisao explicita
- Shadowing troca o tipo sem sujar o nome da variavel
- `;` no fim de uma linha muda expression para statement

</div>

---

# Resumo (continuacao)

<div class="columns">

**If como Expressao**
- Condicao precisa ser `bool` — nada de inteiro "truthy"
- Chaves obrigatorias eliminam o bug do dangling else
- Os dois ramos do `if` precisam devolver o mesmo tipo

**Tipos Primitivos**
- Inteiros com tamanho explicito no nome (`u8`, `i32`, `usize`)
- `char` sempre Unicode de 4 bytes — nao e o `char` de 1 byte do C
- Array `[T; N]` com checagem de limites — o buffer overflow da Semana 7, corrigido

</div>

---

# Conexao com a Semana 7

```
Semana 7  ->  malloc / free manual        ->  Semana 8  ->  ownership automatico
Semana 7  ->  ponteiro duplo No **cabeca  ->  Semana 8  ->  referencias & (em breve)
Semana 7  ->  vetor[10] sem checagem      ->  Semana 8  ->  panic controlado
Semana 7  ->  Big O mede custo em tempo   ->  Semana 8  ->  tipo mede custo em memoria
Semana 7  ->  troca(&a, &b) com ponteiro  ->  Semana 8  ->  mesma ideia, sem risco de free()
```

> Voces nao estao aprendendo uma linguagem nova do zero. Estao aprendendo como o compilador pode fazer, automaticamente, a disciplina que vocês tiveram que ter na mao com ponteiro em C.

---

# Desafio da Semana 8

### Nivel iniciante
Complete todos os exercicios de `00_intro` ate `03_if` no rustlings (`intro1-2`, `variables1-6`, `functions1-5`, `if1-2`). Para cada um, escreva em um comentario o que o erro do compilador disse ANTES de voce corrigir.

### Nivel intermediario
Complete tambem `04_primitive_types` (`primitive_types1-6`). Depois, escreva um programa Rust do zero (`cargo new`) que recebe um array fixo de 10 numeros, usa um `if` como expressao para classificar cada numero como par ou impar, e imprime o resultado.

### Nivel avancado
Reimplemente em Rust o `selection_sort` da Semana 7 usando um array `[i32; N]` de tamanho fixo — sem usar `Vec` ainda. Compare o codigo com a versao em C: onde voce precisou de menos codigo defensivo (checagem de limites, `NULL`) porque o compilador ja garantia isso?

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 8

# O compilador como seu primeiro revisor de codigo

*Nao importa o nivel. Importa entregar — deixando o compilador pegar o que a vista humana deixa passar.*
