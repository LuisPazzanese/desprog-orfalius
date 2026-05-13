Gravity Sort
============

Considere o problema clássico de **ordenação**: dada uma sequência de $n$
inteiros positivos, queremos reorganizá-la em ordem crescente. Existem vários
algoritmos para isso, mas o que vamos estudar aqui resolve o problema de um
jeito bem diferente do usual: em vez de comparar pares de elementos, ele usa
a **gravidade**.


## A ideia

Imagine a seguinte situação. Temos algumas hastes horizontais empilhadas
verticalmente, e cada haste carrega algumas bolinhas. A quantidade de
bolinhas em cada haste representa um número da sequência de entrada.

Por exemplo, para a sequência $[2, 5, 1]$:

![Hastes horizontais com bolinhas representando a sequência 2, 5, 1](img/antes.png)

Agora, imagine que as bolinhas estão sujeitas à gravidade e podem escorregar
para baixo, caindo de uma haste para a de baixo, até encontrar outra bolinha
ou o fundo. Cada bolinha cai **na vertical**, sem se mover para os lados.

??? Checkpoint

Imagine o que acontece quando as bolinhas caem por gravidade. Tente
desenhar (no papel ou mentalmente) como ficam as hastes depois que todas
as bolinhas terminam de cair.

::: Gabarito

Depois da queda, as bolinhas se acumulam embaixo em cada posição vertical:

![Hastes após a gravidade, com bolinhas acumuladas embaixo](img/depois.png)

:::

???

??? Checkpoint

Conte as bolinhas em cada haste no estado final, de cima para baixo.
Que sequência você obtém? O que isso tem a ver com a sequência original?

::: Gabarito

De cima para baixo, as hastes ficam com $1, 2, 5$ bolinhas. Essa é
exatamente a sequência $[2, 5, 1]$ **ordenada em ordem crescente**!

A gravidade fez com que hastes com mais bolinhas naturalmente afundassem
e hastes com menos bolinhas subissem. Ou seja, a gravidade ordenou a
sequência.

:::

???

Esse é o **Gravity Sort** (também chamado de *Bead Sort*). Ele ordena
inteiros positivos simulando a queda de bolinhas por gravidade.


## Representação como matriz

Para implementar essa ideia, precisamos de uma forma de representar as hastes
e as bolinhas no computador. Vamos usar uma **matriz** de $0$s e $1$s.

Cada **linha** da matriz representa uma haste, e cada **coluna** representa
uma posição onde uma bolinha pode estar. O valor $1$ significa "tem bolinha
aqui" e $0$ significa "posição vazia". Em cada linha, as bolinhas ficam
encostadas à esquerda.

É importante distinguir duas quantidades:

- $n$ = a quantidade de números na sequência, que determina o **número de
  linhas** da matriz.
- $k$ = o maior valor da sequência, que determina o **número de colunas** da
  matriz.

Para a sequência $[2, 5, 1]$, temos $n = 3$ e $k = 5$, resultando em uma
matriz $3 \times 5$. Note como $n$ e $k$ são valores bem diferentes: temos
apenas $3$ números, mas o maior deles é $5$.

|        | col 0 | col 1 | col 2 | col 3 | col 4 |
|--------|:-----:|:-----:|:-----:|:-----:|:-----:|
| haste 0 |   1   |   1   |   0   |   0   |   0   |
| haste 1 |   1   |   1   |   1   |   1   |   1   |
| haste 2 |   1   |   0   |   0   |   0   |   0   |

A haste 0 tem dois $1$s porque representa o valor $2$. A haste 1 tem cinco
$1$s porque representa o valor $5$, e assim por diante.

??? Checkpoint

Considere a sequência $[3, 1, 2, 5]$. Quais são os valores de $n$ e $k$?
Monte a matriz correspondente.

::: Gabarito

Temos $n = 4$ números e o maior valor é $k = 5$, então a matriz é
$4 \times 5$ (quatro linhas, cinco colunas):

|        | col 0 | col 1 | col 2 | col 3 | col 4 |
|--------|:-----:|:-----:|:-----:|:-----:|:-----:|
| haste 0 |   1   |   1   |   1   |   0   |   0   |
| haste 1 |   1   |   0   |   0   |   0   |   0   |
| haste 2 |   1   |   1   |   0   |   0   |   0   |
| haste 3 |   1   |   1   |   1   |   1   |   1   |

:::

???


## Gravidade na matriz

Aplicar a gravidade significa, em cada **coluna**, empurrar todos os $1$s
para baixo. As bolinhas caem até o fundo da coluna, formando um bloco
contínuo de $1$s nas últimas posições.

??? Checkpoint

Pegue a matriz da sequência $[2, 5, 1]$ e aplique a gravidade coluna
por coluna. Depois, conte os $1$s em cada linha do resultado, de cima para
baixo. O que você obtém?

**Dica:** comece pela coluna 0. Ela tem três $1$s e três posições,
então nada muda. Agora olhe a coluna 1: ela tem $1$s nas linhas 0 e 1.
Depois da queda, esses dois $1$s devem ocupar as linhas 1 e 2.

::: Gabarito

Antes e depois da gravidade:

| **ANTES** | c0 | c1 | c2 | c3 | c4 |   | **DEPOIS** | c0 | c1 | c2 | c3 | c4 |
|-----------|:--:|:--:|:--:|:--:|:--:|---|------------|:--:|:--:|:--:|:--:|:--:|
| haste 0   |  1 |  1 |  0 |  0 |  0 |   | haste 0    |  1 |  0 |  0 |  0 |  0 |
| haste 1   |  1 |  1 |  1 |  1 |  1 |   | haste 1    |  1 |  1 |  0 |  0 |  0 |
| haste 2   |  1 |  0 |  0 |  0 |  0 |   | haste 2    |  1 |  1 |  1 |  1 |  1 |

Contando os $1$s em cada linha do resultado: $[1, 2, 5]$.

É a sequência original ordenada!

:::

???

??? Checkpoint

Faça o mesmo com a sequência $[3, 1, 2, 5]$: monte a matriz, aplique a
gravidade e leia o resultado.

::: Gabarito

Antes e depois:

| **ANTES** | c0 | c1 | c2 | c3 | c4 |   | **DEPOIS** | c0 | c1 | c2 | c3 | c4 |
|-----------|:--:|:--:|:--:|:--:|:--:|---|------------|:--:|:--:|:--:|:--:|:--:|
| haste 0   |  1 |  1 |  1 |  0 |  0 |   | haste 0    |  1 |  0 |  0 |  0 |  0 |
| haste 1   |  1 |  0 |  0 |  0 |  0 |   | haste 1    |  1 |  1 |  0 |  0 |  0 |
| haste 2   |  1 |  1 |  0 |  0 |  0 |   | haste 2    |  1 |  1 |  1 |  0 |  0 |
| haste 3   |  1 |  1 |  1 |  1 |  1 |   | haste 3    |  1 |  1 |  1 |  1 |  1 |

Resultado: $[1, 2, 3, 5]$. A sequência ordenada.

:::

???


## Por que funciona?

Já vimos que a gravidade ordena nos exemplos, mas por quê isso sempre
funciona? Vamos entender passo a passo.

??? Checkpoint

Olhe novamente o antes e o depois da gravidade na sequência $[2, 5, 1]$.
Compare as colunas individualmente. O que permanece igual entre o antes e o
depois?

**Dica:** conte os $1$s em cada coluna, antes e depois.

::: Gabarito

A quantidade de $1$s em cada coluna é a mesma antes e depois:

- Coluna 0: 3 $\rightarrow$ 3
- Coluna 1: 2 $\rightarrow$ 2
- Coluna 2: 1 $\rightarrow$ 1
- Coluna 3: 1 $\rightarrow$ 1
- Coluna 4: 1 $\rightarrow$ 1

Isso faz sentido: a gravidade apenas rearranja as bolinhas dentro de cada
coluna. Ela não cria nem destrói bolinhas. Então a quantidade por coluna se
mantém.

:::

???

??? Checkpoint

Considere a coluna $j$ da matriz (antes da gravidade). Quando é que a linha
$i$ tem um $1$ na coluna $j$?

**Dica:** lembre que a linha $i$ tem $1$s nas primeiras $v[i]$ colunas.

::: Gabarito

A linha $i$ tem um $1$ na coluna $j$ quando $v[i] > j$, ou seja, quando o
valor representado pela haste $i$ é estritamente maior que $j$.

Portanto, a quantidade total de $1$s na coluna $j$ é igual à quantidade de
elementos da entrada que são **estritamente maiores que $j$**. Chamemos essa
quantidade de $c_j$.

:::

???

??? Checkpoint

Depois da gravidade, os $c_j$ uns da coluna $j$ ocupam as últimas $c_j$
linhas (as de índice mais alto). Ou seja, a linha $i$ recebe um $1$ da
coluna $j$ quando $i \geq n - c_j$.

Use esse fato para explicar por que a linha $i$, depois da gravidade,
contém exatamente o $(i+1)$-ésimo menor valor da entrada.

**Dica:** pense em quantas colunas $j$ contribuem um $1$ para a linha $i$.

::: Gabarito

A linha $i$ recebe um $1$ da coluna $j$ quando $c_j \geq n - i$, ou seja,
quando existem pelo menos $n - i$ elementos da entrada maiores que $j$.

Contar quantas colunas $j$ satisfazem essa condição é o mesmo que perguntar:
"qual é o $(i+1)$-ésimo menor valor da entrada?"

Em resumo: cada linha vira a posição de ranking do elemento na entrada
ordenada. Por isso as linhas saem em ordem crescente de cima para baixo.

:::

???


## Complexidade

??? Checkpoint

O Gravity Sort precisa construir uma matriz $n \times k$ e processar cada
coluna. Qual é a complexidade de tempo do algoritmo em função de $n$
(quantidade de números) e $k$ (maior valor)?

::: Gabarito

- Preencher a matriz: percorre $n$ linhas de $k$ colunas cada $\rightarrow$
  $O(n \cdot k)$.
- Aplicar a gravidade: para cada uma das $k$ colunas, percorre $n$ linhas
  para contar e reescrever $\rightarrow$ $O(n \cdot k)$.
- Ler o resultado: percorre $n$ linhas de $k$ colunas $\rightarrow$
  $O(n \cdot k)$.

Total: $O(n \cdot k)$.

:::

???

??? Checkpoint

Compare a complexidade $O(n \cdot k)$ do Gravity Sort com a complexidade
$O(n \log n)$ de algoritmos como merge sort. Em que situações o Gravity
Sort é melhor? E quando ele é pior?

::: Gabarito

- Se $k$ é pequeno (por exemplo, $k < \log n$), o Gravity Sort é mais
  rápido que merge sort.
- Se $k$ é muito grande (por exemplo, $k = 1\,000\,000$ para poucos
  elementos), o Gravity Sort é muito mais lento.

Além disso, o Gravity Sort consome $O(n \cdot k)$ de memória para a
matriz, enquanto merge sort usa $O(n)$.

:::

???


## Limitações

??? Checkpoint

Liste pelo menos duas limitações do Gravity Sort em comparação com
algoritmos clássicos como merge sort ou quicksort.

::: Gabarito

1. **Só funciona com inteiros positivos.** Não há como representar "meia
   bolinha" ou "bolinha negativa".

2. **Consome muita memória.** A matriz tem $n \times k$ posições. Para
   valores grandes, isso é proibitivo. Por exemplo, ordenar $[1, 1\,000\,000]$
   aloca uma matriz com dois milhões de posições para apenas dois elementos.

3. **Pode ser muito mais lento** que merge sort ou quicksort quando os
   valores são grandes, mesmo com poucos elementos.

4. **Não é estável.** Elementos com o mesmo valor perdem a ordem relativa
   original.

:::

???


## De matriz para vetor

A versão com matriz funciona, mas será que realmente precisamos dela toda?
Vamos investigar.

??? Checkpoint

Pense no que acontece quando aplicamos a gravidade em uma coluna. Depois
da gravidade, a coluna $j$ fica com $c_j$ uns embaixo e o resto zeros em
cima. Para reconstruir o resultado final (contar os $1$s em cada linha),
você realmente precisa saber **onde** cada $1$ estava antes, ou basta saber
**quantos** $1$s havia em cada coluna?

::: Gabarito

Basta saber a **quantidade** $c_j$ de $1$s em cada coluna! Depois da
gravidade, a posição dos $1$s é completamente determinada por $c_j$: eles
ocupam as últimas $c_j$ linhas. Os $1$s e $0$s individuais da matriz são
informação redundante.

Isso significa que podemos trocar a matriz inteira por um **vetor de
contagens** de tamanho $k$.

:::

???

??? Checkpoint

Lembre que $c_j$ é a quantidade de elementos da entrada estritamente
maiores que $j$. Pense em uma maneira de calcular todos os $c_j$ a partir
da sequência original, **sem montar a matriz**.

**Dica:** se um elemento da entrada tem valor $v$, para quais colunas ele
contribui um $1$?

::: Gabarito

Um elemento de valor $v$ contribui um $1$ para as colunas $0, 1, 2, \ldots,
v-1$. Ou seja, ele incrementa $c_j$ para todo $j < v$.

Percorrendo a entrada uma vez e, para cada elemento, incrementando as
posições correspondentes no vetor de contagens, obtemos todos os $c_j$ sem
precisar da matriz.

:::

???

??? Checkpoint

Existe uma forma ainda mais simples de pensar nesse vetor. Em vez de
guardar "quantos elementos são maiores que $j$", podemos guardar **quantas
vezes cada valor aparece na entrada**. Isso é um **histograma**.

Se $h[j]$ é o número de vezes que o valor $j+1$ aparece na entrada, como
você reconstruiria a sequência ordenada a partir de $h$?

**Dica:** se $h[0] = 2$, o que isso significa para o resultado?

::: Gabarito

Se $h[0] = 2$, significa que o valor $1$ aparece duas vezes. Então
escrevemos duas cópias de $1$ no resultado. Se $h[1] = 0$, o valor $2$ não
aparece. Se $h[2] = 1$, escrevemos uma cópia de $3$, e assim por diante.

A sequência ordenada é simplesmente o histograma "expandido":

$$h[0] \text{ cópias de } 1, \quad h[1] \text{ cópias de } 2, \quad \ldots, \quad h[k-1] \text{ cópias de } k$$

:::

???

??? Checkpoint

Aplique essa ideia à sequência $[3, 1, 4, 1, 2]$.

1. Calcule o histograma $h$.
2. Expanda o histograma para obter a sequência ordenada.

::: Gabarito

Sequência: $[3, 1, 4, 1, 2]$. O maior valor é $k = 4$.

Histograma:

- $h[0] = 2$ (o valor $1$ aparece duas vezes)
- $h[1] = 1$ (o valor $2$ aparece uma vez)
- $h[2] = 1$ (o valor $3$ aparece uma vez)
- $h[3] = 1$ (o valor $4$ aparece uma vez)

Expandindo: $1, 1, 2, 3, 4$. A sequência ordenada!

:::

???

??? Checkpoint

Compare essa versão com vetor com a versão com matriz. Quais são as
vantagens?

::: Gabarito

- **Memória:** usa um vetor de tamanho $k$ em vez de uma matriz
  $n \times k$. Para $n = 1\,000$ e $k = 100$, são $100$ posições em vez
  de $100\,000$.
- **Simplicidade:** o código fica mais curto e direto.
- **Mesma complexidade de tempo:** ainda é $O(n + k)$, na verdade até
  melhor que a versão com matriz, que era $O(n \cdot k)$.

:::

???

Se essa versão com vetor parece familiar, é porque ela é! Esse algoritmo é
exatamente o clássico **Counting Sort**. O Gravity Sort com matriz é, no
fundo, uma versão visualmente intuitiva do Counting Sort. A matriz que tanto
construímos era apenas uma representação redundante de um histograma bem mais
simples.


## Implementação (desafio)

Agora que a ideia está clara, o desafio é traduzir o algoritmo para código.
Abaixo está um esqueleto da versão com histograma (Counting Sort).

??? Desafio

Complete a implementação abaixo:

```c
void gravity_sort(int *v, int n) {
    // 1) encontre o maior valor k
    int k = v[0];
    for (int i = 1; i < n; i++) {
        // ?
    }

    // 2) crie o histograma
    int *h = malloc(k * sizeof(int));
    for (int j = 0; j < k; j++) {
        h[j] = 0;
    }
    for (int i = 0; i < n; i++) {
        // ?
    }

    // 3) expanda o histograma de volta para v
    int idx = 0;
    for (int j = 0; j < k; j++) {
        for (int t = 0; t < h[j]; t++) {
            // ?
        }
    }

    free(h);
}
```

::: Gabarito

```c
void gravity_sort(int *v, int n) {
    // 1) encontre o maior valor k
    int k = v[0];
    for (int i = 1; i < n; i++) {
        if (v[i] > k) {
            k = v[i];
        }
    }

    // 2) crie o histograma
    int *h = malloc(k * sizeof(int));
    for (int j = 0; j < k; j++) {
        h[j] = 0;
    }
    for (int i = 0; i < n; i++) {
        h[v[i] - 1]++;
    }

    // 3) expanda o histograma de volta para v
    int idx = 0;
    for (int j = 0; j < k; j++) {
        for (int t = 0; t < h[j]; t++) {
            v[idx] = j + 1;
            idx++;
        }
    }

    free(h);
}
```

:::

???
