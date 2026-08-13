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

# Estruturas de Dados e Complexidade

*Big O, arrays, listas encadeadas, busca binaria e ordenacao*

---

# Agenda

1. **Por que isso importa** — algoritmo e a base invisivel da web
2. **Notacao Big O** — como medir o custo de um codigo
3. **Array vs Lista Encadeada** — memoria, ponteiros e trade-offs
4. **Busca Binaria** — encontrar rapido em dados ordenados
5. **Ordenacao por Selecao** — o algoritmo mais didatico que existe
6. **Atividades ao vivo** — voces resolvem, a gente discute

> "Framework voce troca a cada 3 anos. Algoritmo continua o mesmo ha 50."

---

# Onde algoritmo aparece no seu dia de dev web

Ate agora vimos as **camadas** do sistema. Hoje olhamos o que roda **dentro** de cada uma.

```
Semana 4  ->  Banco de dados       ->  INDEX usa arvore de busca
Semana 5  ->  API e ORM            ->  N+1 queries e um problema de complexidade
Semana 3  ->  Cache / LocalStorage ->  cache LRU usa lista encadeada
Frontend  ->  Renderizar listas    ->  filtrar 10.000 itens no navegador
```

### A pergunta que todo dev acaba enfrentando

> "Funcionava com 100 registros. Por que travou com 100.000?"

A resposta quase nunca esta na linguagem. Esta no **algoritmo escolhido**.

---

<!-- _class: cover -->

## Parte 1

# Notacao Big O

---

# O problema de medir em segundos

```
Mesmo codigo, maquinas diferentes:

Notebook do Joao   ->  0,8 segundos
PC gamer da Maria  ->  0,2 segundos
Servidor da AWS    ->  0,4 segundos

Qual e o "tempo real" do algoritmo?  Nenhum desses.
```

Hardware muda. Linguagem muda. Compilador muda.

O que **nao muda** e quantas operacoes o algoritmo faz conforme a entrada cresce.

> Big O nao mede tempo. Mede **como o custo cresce** quando a entrada cresce.

---

# Contando operacoes

```c
// Quantas vezes a linha de dentro executa?

for (int i = 0; i < n; i++) {
    printf("%d\n", vetor[i]);      // executa n vezes  ->  O(n)
}

for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        printf("%d\n", i * j);     // executa n * n vezes  ->  O(n^2)
    }
}

printf("%d\n", vetor[0]);          // executa 1 vez  ->  O(1)
```

### Regras praticas do Big O

- **Ignore constantes**: `O(2n)` vira `O(n)`
- **Mantenha o termo dominante**: `O(n^2 + n)` vira `O(n^2)`
- Big O descreve o **pior caso** — o cenario que voce precisa suportar

---

# As complexidades que voce vai encontrar

| Notacao | Nome | Exemplo tipico |
|---|---|---|
| `O(1)` | Constante | Acessar `vetor[5]`, ler chave no Redis |
| `O(log n)` | Logaritmica | Busca binaria, indice do banco |
| `O(n)` | Linear | Percorrer um vetor, `map()` em um array |
| `O(n log n)` | Linearitmica | Merge sort, quick sort, `Array.sort()` |
| `O(n^2)` | Quadratica | Selection sort, loop dentro de loop |
| `O(2^n)` | Exponencial | Forca bruta, subconjuntos |

> Ate `O(n log n)` voce dorme tranquilo. A partir de `O(n^2)` voce precisa saber o tamanho de `n`.

---

# O impacto real do crescimento

Supondo 1 milhao de operacoes por segundo:

| n | `O(log n)` | `O(n)` | `O(n log n)` | `O(n^2)` |
|---|---|---|---|---|
| **10** | 3 | 10 | 33 | 100 |
| **1.000** | 10 | 1.000 | 10.000 | 1 milhao |
| **100.000** | 17 | 100 mil | 1,7 milhao | 10 bilhoes |
| **1.000.000** | 20 | 1 milhao | 20 milhoes | 1 trilhao |

```
n = 1.000.000

O(log n)   ->  20 operacoes        ->  instantaneo
O(n)       ->  1 segundo
O(n^2)     ->  ~11 dias de processamento
```

O mesmo problema. A mesma maquina. Apenas um algoritmo diferente.

---

# Paralelo direto com desenvolvimento web

```javascript
// O(n^2) escondido em codigo que parece inocente
const pedidos = await buscarPedidos();      // 1.000 pedidos
const usuarios = await buscarUsuarios();    // 1.000 usuarios

const resultado = pedidos.map(pedido => ({
  ...pedido,
  usuario: usuarios.find(u => u.id === pedido.usuarioId)  // find e O(n)
}));
// 1.000 pedidos x 1.000 usuarios = 1.000.000 comparacoes

// O(n) — usando um mapa de acesso constante
const mapa = new Map(usuarios.map(u => [u.id, u]));       // O(n) uma vez
const resultado = pedidos.map(pedido => ({
  ...pedido,
  usuario: mapa.get(pedido.usuarioId)                     // O(1) por pedido
}));
// 2.000 operacoes no total
```

> O problema N+1 que vimos na Semana 5 e exatamente isso: complexidade escondida atras de uma abstracao confortavel.

---

<!-- _class: cover -->

## Atividade 1

# Qual e a complexidade?

---

# Classifique cada trecho

Responda em voz alta: `O(1)`, `O(n)`, `O(log n)` ou `O(n^2)`?

```c
// A
int primeiro = vetor[0];

// B
for (int i = 0; i < n; i++)
    soma += vetor[i];

// C
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        if (vetor[i] == vetor[j]) contador++;

// D
while (n > 1) { n = n / 2; passos++; }

// E
for (int i = 0; i < n; i++) soma += vetor[i];
for (int j = 0; j < n; j++) produto *= vetor[j];
```

> Dica para o item E: dois lacos **em sequencia** nao sao a mesma coisa que dois lacos **aninhados**.

---

<!-- _class: cover -->

## Parte 2

# Array e Lista Encadeada

---

# Array — memoria contigua

Um array reserva um bloco **continuo** de memoria. E por isso que o acesso e instantaneo.

```c
int vetor[5] = {10, 20, 30, 40, 50};

Endereco:  0x1000  0x1004  0x1008  0x100C  0x1010
           +-------+-------+-------+-------+-------+
Valor:     |  10   |  20   |  30   |  40   |  50   |
           +-------+-------+-------+-------+-------+
Indice:        0       1       2       3       4
```

O computador nao "procura" o indice 3 — ele **calcula** o endereco:

```c
endereco(vetor[i]) = endereco_base + (i * sizeof(int))
                   = 0x1000 + (3 * 4)
                   = 0x100C

// Por isso essas duas linhas sao equivalentes em C:
vetor[3]
*(vetor + 3)
```

> Acesso por indice e `O(1)` — nao importa se o array tem 10 ou 10 milhoes de posicoes.

---

# O custo escondido do array

```c
// Inserir no inicio: todo mundo precisa andar uma casa
int vetor[6] = {10, 20, 30, 40, 50};

// Inserir o valor 5 na posicao 0:
[10][20][30][40][50]
  |   |   |   |   |
  v   v   v   v   v
[ 5][10][20][30][40][50]     <- n deslocamentos  ->  O(n)
```

### Limitacoes do array em C

- **Tamanho fixo** — `int vetor[100]` nao cresce sozinho
- Crescer exige `realloc` e possivel copia de tudo para outro lugar
- Inserir ou remover no meio custa `O(n)`
- Precisa de um bloco contiguo livre na memoria

> No JavaScript, `array.splice(0, 0, item)` parece uma linha simples. Por baixo, e o mesmo deslocamento `O(n)`.

---

# Lista Encadeada — memoria espalhada

Cada elemento e um **no** que guarda o valor e um **ponteiro** para o proximo.

```c
typedef struct No {
    int valor;
    struct No *proximo;   // ponteiro para o proximo no
} No;
```

```
Memoria (posicoes quaisquer, nao contiguas):

  0x2A00            0x7F10            0x3C88
+---------+       +---------+       +---------+
| 10 | *--|-----> | 20 | *--|-----> | 30 |NULL|
+---------+       +---------+       +---------+
   cabeca                              fim da lista
```

Nao existe indice. Para chegar no terceiro elemento, voce **caminha** ate ele.

---

# Lista encadeada na pratica — ponteiros em acao

```c
#include <stdlib.h>

No *criar_no(int valor) {
    No *novo = malloc(sizeof(No));   // aloca memoria dinamicamente
    novo->valor = valor;
    novo->proximo = NULL;
    return novo;
}

// Ponteiro para ponteiro: precisamos alterar a propria cabeca
void inserir_inicio(No **cabeca, int valor) {
    No *novo = criar_no(valor);
    novo->proximo = *cabeca;   // novo aponta para a antiga cabeca
    *cabeca = novo;            // cabeca passa a ser o novo no
}

void imprimir(No *atual) {
    while (atual != NULL) {
        printf("%d -> ", atual->valor);
        atual = atual->proximo;   // caminha para o proximo
    }
    printf("NULL\n");
}
```

> `No **cabeca` e o motivo classico de existir ponteiro de ponteiro: para mudar o valor de um ponteiro dentro de uma funcao.

---

# Por que o ponteiro duplo e necessario

```c
// ERRADO — a funcao recebe uma COPIA do ponteiro
void inserir_errado(No *cabeca, int valor) {
    No *novo = criar_no(valor);
    novo->proximo = cabeca;
    cabeca = novo;      // muda so a copia local — some ao sair da funcao
}

// CORRETO — recebe o ENDERECO do ponteiro
void inserir_inicio(No **cabeca, int valor) { ... }

// Chamada:
No *lista = NULL;
inserir_inicio(&lista, 30);   // passa o endereco de lista
inserir_inicio(&lista, 20);
inserir_inicio(&lista, 10);
// lista: 10 -> 20 -> 30 -> NULL
```

### Nao esqueca de liberar

```c
void liberar(No *atual) {
    while (atual != NULL) {
        No *proximo = atual->proximo;   // guarda antes de destruir
        free(atual);
        atual = proximo;
    }
}
```

---

# Array vs Lista Encadeada

| Operacao | Array | Lista Encadeada |
|---|---|---|
| **Acesso por indice** | `O(1)` | `O(n)` — precisa caminhar |
| **Busca por valor** | `O(n)` | `O(n)` |
| **Inserir no inicio** | `O(n)` | `O(1)` |
| **Inserir no fim** | `O(1)`* | `O(n)` ou `O(1)` com ponteiro para o fim |
| **Remover do meio** | `O(n)` | `O(1)` se ja estiver no no |
| **Memoria extra** | Nenhuma | Um ponteiro por elemento |
| **Cache do processador** | Excelente | Ruim — dados espalhados |
| **Tamanho** | Fixo (ou realloc) | Cresce sob demanda |

\* considerando que ha espaco alocado disponivel

> Array e melhor para **ler muito**. Lista encadeada e melhor para **inserir e remover muito**.

---

# Onde isso aparece no mundo web

### Lista encadeada disfarcada

```
DOM do navegador     ->  node.nextSibling, node.previousSibling
                         e literalmente uma lista duplamente encadeada

Cache LRU            ->  lista encadeada + hash map
                         mover item para o topo em O(1)

Historico do browser ->  voltar e avancar = lista duplamente encadeada

Git                  ->  cada commit aponta para o commit anterior

Undo / Redo          ->  pilha construida sobre lista encadeada
```

### Array disfarcado

```
Array em JavaScript  ->  na verdade e um objeto otimizado pelo motor V8
                         quando os indices sao densos, vira array real

Buffer de rede       ->  bytes contiguos, acesso por offset

Tabela do banco      ->  paginas de dados sequenciais em disco
```

---

<!-- _class: cover -->

## Atividade 2

# Qual estrutura voce usaria?

---

# Escolha e justifique

Para cada cenario: **array** ou **lista encadeada**? Por que?

<div class="columns">

**Cenario A**
Uma playlist onde o usuario arrasta musicas para reordenar e remove itens do meio constantemente.

**Cenario B**
Os 500 produtos de uma pagina de catalogo, que sao carregados uma vez e apenas exibidos e paginados.

**Cenario C**
Um chat em tempo real onde mensagens novas chegam e sao inseridas no topo da lista.

**Cenario D**
Uma matriz de pixels de uma imagem, acessada por coordenada `(x, y)` milhares de vezes por segundo.

</div>

> Nao existe resposta universal — existe resposta para o **padrao de acesso** do seu problema.

---

<!-- _class: cover -->

## Parte 3

# Busca Binaria

---

# Busca linear — a forma ingenua

```c
int busca_linear(int vetor[], int tamanho, int alvo) {
    for (int i = 0; i < tamanho; i++) {
        if (vetor[i] == alvo) {
            return i;      // encontrou — retorna a posicao
        }
    }
    return -1;             // nao encontrou
}
```

```
Vetor com 1.000.000 de elementos, alvo na ultima posicao:
-> 1.000.000 comparacoes  ->  O(n)
```

Funciona sempre — ordenado ou nao. Mas paga caro por isso.

> Existe uma forma muito melhor. O preco: os dados precisam estar **ordenados**.

---

# A ideia da busca binaria

Pense em procurar uma palavra no dicionario. Voce nao comeca na pagina 1.

```
Procurar o 23 em: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                    0  1  2   3   4   5   6   7   8   9

Passo 1: inicio=0, fim=9  ->  meio=4  ->  vetor[4]=16
         16 < 23  ->  o alvo esta na METADE DA DIREITA
         descarta indices 0..4

Passo 2: inicio=5, fim=9  ->  meio=7  ->  vetor[7]=56
         56 > 23  ->  o alvo esta na METADE DA ESQUERDA
         descarta indices 7..9

Passo 3: inicio=5, fim=6  ->  meio=5  ->  vetor[5]=23
         ENCONTROU no indice 5
```

3 comparacoes em vez de 6. Cada passo **elimina metade** do que sobrou.

---

# Implementacao em C

```c
int busca_binaria(int vetor[], int tamanho, int alvo) {
    int inicio = 0;
    int fim = tamanho - 1;

    while (inicio <= fim) {
        int meio = inicio + (fim - inicio) / 2;   // evita overflow

        if (vetor[meio] == alvo) {
            return meio;            // encontrou
        }
        if (vetor[meio] < alvo) {
            inicio = meio + 1;      // descarta a metade da esquerda
        } else {
            fim = meio - 1;         // descarta a metade da direita
        }
    }
    return -1;                      // nao existe no vetor
}
```

> Detalhe importante: `vetor[]` como parametro e, na verdade, um **ponteiro**. A funcao nao sabe o tamanho — por isso ele vai como argumento separado.

---

# Por que `O(log n)` e tao poderoso

```
Cada passo corta o problema pela metade:

n = 1.000.000
  -> 500.000 -> 250.000 -> 125.000 -> ... -> 1

Quantas divisoes por 2 ate chegar em 1?  log2(1.000.000) = ~20
```

| Tamanho do vetor | Busca linear (pior caso) | Busca binaria (pior caso) |
|---|---|---|
| 100 | 100 comparacoes | 7 |
| 10.000 | 10.000 | 14 |
| 1.000.000 | 1.000.000 | 20 |
| 1.000.000.000 | 1 bilhao | 30 |

> Multiplicar os dados por mil adiciona apenas **10 comparacoes**. Essa e a natureza do logaritmo.

---

# Busca binaria no desenvolvimento web

### O indice do banco de dados

```sql
-- Sem indice: full table scan  ->  O(n)
SELECT * FROM usuarios WHERE email = 'joao@email.com';

-- Com indice (B-tree, primo da busca binaria)  ->  O(log n)
CREATE INDEX idx_usuarios_email ON usuarios (email);
```

```
1 milhao de usuarios:
  Sem indice  ->  ate 1.000.000 leituras de linha
  Com indice  ->  ~20 saltos na arvore
```

### Outros lugares onde ela aparece

- `git bisect` — encontra o commit que quebrou o build em `log n` testes
- Autocomplete e busca em listas ordenadas no frontend
- Versionamento de dependencias e resolucao de ranges no `npm`
- Debug por bisseccao: comentar metade do codigo para isolar o bug

> Na Semana 4 falamos que indice deixa a query rapida. Hoje voces sabem **por que**.

---

<!-- _class: cover -->

## Atividade 3

# Rastreie a busca

---

# Execute a busca binaria na mao

```
Vetor:  [3, 7, 11, 15, 19, 24, 31, 42, 55, 68, 79, 88]
Indice:  0  1   2   3   4   5   6   7   8   9  10  11
```

### Parte 1 — preencha a tabela para o alvo `31`

| Passo | inicio | fim | meio | vetor[meio] | Decisao |
|---|---|---|---|---|---|
| 1 | 0 | 11 | ? | ? | ? |
| 2 | ? | ? | ? | ? | ? |
| 3 | ? | ? | ? | ? | ? |

### Parte 2 — perguntas para discutir

- Quantas comparacoes foram necessarias?
- E se o alvo fosse `50` (que nao existe no vetor)? Onde o laco termina?
- Qual seria o **pior caso** para esse vetor de 12 elementos?

---

<!-- _class: cover -->

## Parte 4

# Ordenacao por Selecao

---

# Busca binaria exige ordem

```
Busca binaria em vetor desordenado:
[42, 7, 91, 15, 3]  ->  a logica de "descartar metade" nao faz sentido
```

Se descartar metade depende de saber que os menores estao a esquerda, entao os dados **precisam estar ordenados**.

### Selection Sort — a ideia

```
1. Percorra o vetor e encontre o MENOR elemento
2. Troque ele com o elemento da primeira posicao
3. Repita para a posicao seguinte, ignorando o que ja foi ordenado
```

> "Selecao" porque a cada rodada voce **seleciona** o menor do que sobrou.

---

# Rastreando o Selection Sort

```
Inicial:  [64, 25, 12, 22, 11]

Rodada 1: procura o menor entre indices 0..4  ->  11 (indice 4)
          troca posicao 0 com posicao 4
          [11 | 25, 12, 22, 64]

Rodada 2: procura o menor entre indices 1..4  ->  12 (indice 2)
          troca posicao 1 com posicao 2
          [11, 12 | 25, 22, 64]

Rodada 3: procura o menor entre indices 2..4  ->  22 (indice 3)
          troca posicao 2 com posicao 3
          [11, 12, 22 | 25, 64]

Rodada 4: procura o menor entre indices 3..4  ->  25 (ja esta)
          nenhuma troca
          [11, 12, 22, 25 | 64]

Ordenado: [11, 12, 22, 25, 64]
```

A barra `|` separa a parte **ja ordenada** da parte que ainda falta.

---

# Implementacao em C

```c
// Troca dois valores usando ponteiros
void troca(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void selection_sort(int vetor[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int indice_menor = i;

        // procura o menor no trecho ainda nao ordenado
        for (int j = i + 1; j < n; j++) {
            if (vetor[j] < vetor[indice_menor]) {
                indice_menor = j;
            }
        }

        if (indice_menor != i) {
            troca(&vetor[i], &vetor[indice_menor]);
        }
    }
}
```

> `troca(&vetor[i], ...)` e o exemplo mais direto de por que ponteiros existem: sem eles, a funcao trocaria apenas copias e o vetor original ficaria intacto.

---

# A complexidade do Selection Sort

```
Rodada 1:  n - 1 comparacoes
Rodada 2:  n - 2 comparacoes
Rodada 3:  n - 3 comparacoes
...
Rodada n:  1 comparacao

Total = (n-1) + (n-2) + ... + 1 = n(n-1)/2 = (n^2 - n)/2

Descartando constantes e termos menores  ->  O(n^2)
```

| Caracteristica | Selection Sort |
|---|---|
| **Melhor caso** | `O(n^2)` — sempre compara tudo |
| **Caso medio** | `O(n^2)` |
| **Pior caso** | `O(n^2)` |
| **Memoria extra** | `O(1)` — ordena no proprio vetor |
| **Numero de trocas** | No maximo `n-1` — o menor entre os simples |
| **Estavel** | Nao — pode inverter elementos iguais |

> Ponto curioso: mesmo com o vetor ja ordenado, ele faz todas as comparacoes. Nao existe "atalho".

---

# Como ele se compara aos outros

| Algoritmo | Melhor | Medio | Pior | Memoria |
|---|---|---|---|---|
| **Selection Sort** | `O(n^2)` | `O(n^2)` | `O(n^2)` | `O(1)` |
| **Bubble Sort** | `O(n)` | `O(n^2)` | `O(n^2)` | `O(1)` |
| **Insertion Sort** | `O(n)` | `O(n^2)` | `O(n^2)` | `O(1)` |
| **Merge Sort** | `O(n log n)` | `O(n log n)` | `O(n log n)` | `O(n)` |
| **Quick Sort** | `O(n log n)` | `O(n log n)` | `O(n^2)` | `O(log n)` |

### Entao por que estudar Selection Sort?

- E o mais simples de entender e implementar — porta de entrada para o resto
- Faz o **minimo de trocas** possivel: util quando escrever custa caro (memoria flash)
- Ensina o padrao de laco aninhado que voce vai reconhecer como `O(n^2)` para sempre

> Na producao voce usa `Array.sort()`, `ORDER BY` ou `qsort()`. Todos usam variantes de quick sort ou merge sort.

---

# Ordenacao no mundo web

```javascript
// Frontend — o motor JS ja usa um algoritmo O(n log n)
produtos.sort((a, b) => a.preco - b.preco);
```

```sql
-- Banco de dados — ordena antes de devolver
SELECT * FROM produtos ORDER BY preco ASC LIMIT 20;

-- Com indice na coluna, o banco nem precisa ordenar:
CREATE INDEX idx_produtos_preco ON produtos (preco);
-- O indice JA esta ordenado  ->  leitura direta
```

### A decisao de arquitetura

| Situacao | Onde ordenar |
|---|---|
| 10.000 registros no banco, mostra 20 | **No banco** — `ORDER BY` + `LIMIT` |
| 50 itens ja carregados na tela | **No frontend** — sem nova requisicao |
| Relatorio com milhoes de linhas | **No banco**, com indice adequado |
| Ordenacao que muda a cada clique do usuario | **No frontend**, se o volume permitir |

> Trazer 100.000 registros para o navegador ordenar e trocar `O(n log n)` no servidor por trafego de rede e travamento da interface.

---

<!-- _class: cover -->

## Atividade 4

# Complete o codigo

---

# Preencha as lacunas

```c
void selection_sort(int vetor[], int n) {
    for (int i = 0; i < _______; i++) {

        int indice_menor = _______;

        for (int j = _______; j < n; j++) {
            if (vetor[j] _____ vetor[indice_menor]) {
                indice_menor = _______;
            }
        }

        troca(&vetor[i], &vetor[_______]);
    }
}
```

### Depois de completar, discuta

- O que muda se trocarmos `<` por `>` na comparacao?
- Por que o laco externo vai ate `n - 1` e nao ate `n`?
- Por que passamos `&vetor[i]` e nao `vetor[i]` para a funcao `troca`?

---

<!-- _class: cover -->

## Atividade 5

# Adivinhe o numero

---

# Busca binaria com a turma

### Regras

```
1. Escolho um numero entre 1 e 1000
2. Voces chutam
3. Eu respondo apenas: MAIOR ou MENOR
4. Contamos quantos chutes foram necessarios
```

### Antes de comecar, respondam

- Qual e o **melhor primeiro chute**? Por que?
- Qual e o numero **maximo** de chutes necessarios para garantir o acerto?
- Se o intervalo fosse de 1 a 1.000.000, quantos chutes a mais?

```
Dica:  log2(1000)      = ~10
       log2(1.000.000) = ~20
```

> Se voces sempre chutarem o meio, estao executando busca binaria com o cerebro.

---

<!-- _class: cover -->

## Recapitulando

# O que vimos hoje

---

# Resumo

<div class="columns">

**Notacao Big O**
- Mede crescimento do custo, nao tempo em segundos
- Ignore constantes, mantenha o termo dominante
- `O(1)` < `O(log n)` < `O(n)` < `O(n log n)` < `O(n^2)`
- Laco aninhado e o sinal visual de `O(n^2)`

**Array vs Lista Encadeada**
- Array: memoria contigua, acesso `O(1)`, insercao `O(n)`
- Lista: memoria espalhada, acesso `O(n)`, insercao `O(1)`
- Escolha pelo padrao de acesso do problema
- Lista encadeada exige ponteiros e `free()`

</div>

---

# Resumo (continuacao)

<div class="columns">

**Busca Binaria**
- Elimina metade do espaco a cada comparacao — `O(log n)`
- Exige dados **ordenados**
- Indice de banco de dados e a mesma ideia em arvore
- 1 bilhao de itens = 30 comparacoes

**Ordenacao por Selecao**
- Encontra o menor e troca — `O(n^2)` sempre
- Minimo de trocas, memoria constante
- Base didatica para entender algoritmos melhores
- Producao usa merge sort e quick sort — `O(n log n)`

</div>

---

# Conexao com as semanas anteriores

```
Semana 2  ->  Seguranca            ->  hashing lento e proposital: custo como defesa
Semana 3  ->  Cache no navegador   ->  cache LRU e lista encadeada + hash map
Semana 4  ->  Banco de dados       ->  INDEX = busca binaria em arvore B-tree
Semana 5  ->  API e ORM            ->  N+1 queries e complexidade escondida
Semana 6  ->  Modelagem            ->  chave e relacao definem o custo do JOIN
Semana 7  ->  Algoritmos           ->  o que roda por baixo de tudo acima
```

> Framework abstrai. Banco otimiza. ORM protege. Mas quando o sistema fica lento e ninguem sabe por que, a resposta esta aqui.

---

# Desafio da Semana 7

### Nivel iniciante
Implemente em C uma funcao `busca_linear` e uma `busca_binaria` que recebam um vetor ordenado, o tamanho e o alvo. Adicione um contador de comparacoes em cada uma e imprima o resultado para um vetor de 20 elementos. Compare os numeros.

### Nivel intermediario
Implemente o `selection_sort` com uma funcao `troca` usando ponteiros. Gere 1.000 numeros aleatorios, ordene e cronometre com `clock()`. Depois faca o mesmo com 10.000 numeros e explique por que o tempo cresceu muito mais que 10 vezes.

### Nivel avancado
Implemente uma lista encadeada completa em C com `inserir_inicio`, `inserir_fim`, `remover`, `buscar` e `liberar`. Depois construa uma pagina web que consome uma API, e implemente busca binaria em JavaScript sobre a lista ordenada de resultados — comparando o numero de comparacoes com `Array.find()`.

---

<!-- _class: cover -->

## Clube de Desenvolvimento Web — Semana 7

# O codigo certo importa mais que a maquina rapida

*Nao importa o nivel. Importa entregar — sabendo o custo do que voce escreveu.*
