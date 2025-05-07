O Problema da Mochila Binária
==============================
Entendendo o problema
----------

Imagine que você está preparando uma mochila para fazer uma trilha. Você tem vários itens disponíveis, cada um com um peso e um valor, mas sua mochila, assim como qualquer outra, tem uma capacidade limitada.

Como escolher os itens para levar de forma a **maximizar o valor total sem ultrapassar o peso máximo**? O *Problema da Mochila Binária* é um problema clássico de otimização combinatória que consiste em escolher um subconjunto de itens para colocar em uma mochila, respeitando um limite de peso, de modo a maximizar o valor total carregado. Ele recebe esse nome porque, para cada item, você deve tomar uma decisão binária: **ou coloca o item na mochila (1), ou não coloca (0)** — não é permitido fracionar os itens.


## Definição Objetiva do Problema

Dado um conjunto de `md n` itens e uma mochila com capacidade máxima `md W`, o objetivo é **escolher um subconjunto de itens** que:

1. **Maximize a soma dos valores dos itens escolhidos**;
2. **Tenha soma dos pesos que não ultrapasse a capacidade da mochila**.

Essa é uma típica situação de **trade-off**: quanto mais itens de valor você quiser levar, mais peso sua mochila vai ter — e seu espaço é limitado.

## Definição Detalhada

**Entradas**:  
- Capacidade máxima da mochila: `md W` (por exemplo, 6 kg)  
- `md n` itens, cada um com:
  * Peso `md w[i]`
  * Valor `md v[i]`

**Saída desejada**:  
- Conjunto de itens a serem levados

**Restrição**:  
- A soma dos pesos dos itens escolhidos ≤ `md W`

**Objetivo**:  
- Maximizar a soma dos valores dos itens escolhidos


??? Checkpoint

Para uma `md mochila de 6kg`, qual combinação de itens você escolheria? Por quê?

| Item                      | Peso | Valor |
| ------------------------- | ---- | ----- |
| Lanterna                  | 2 kg | 5     |
| Água                     | 3 kg | 10    |
| Comida                    | 4 kg | 12    |
| Kit de Primeiros Socorros | 1 kg | 8     |
| Câmera                   | 2 kg | 6     |

::: Gabarito
Entre todas, a combinação `md kit de primeiros socorros + comida` é a combinação que apresenta maior valor total ($12 + 8 = 20$) dentro da limitação da mochila ($1 + 4 \leq 6$ kg).
:::

???

Método da Força Bruta
-----------------------

Quando não sabemos por onde começar, é comum tentarmos analisar todas as combinações possíveis. Vamos voltar ao exemplo da mochila: imagine que você tem uma mochila com capacidade limitada e precisa escolher entre alguns itens, cada um com um peso e um valor. O que acontece se você testar todas as combinações possíveis de itens? Ao fazer isso, você pode comparar os resultados de cada combinação e escolher aquela que dá o maior valor possível sem ultrapassar o limite de peso. É exatamente assim que funciona a abordagem da **força bruta**!

Cada item tem duas possbilidades:

1. Levar o item;
2. Não levar o item.

??? Checkpoint

Considerando que cada item tem o estado `md pegar` e `md não pegar`: Se você tem `md 5 itens`, quantas combinações você teria ao fim do processo? E se tiver `md n itens`?

::: Gabarito
Para **5 itens**, você teria **32** combinações. Para **n itens**, você teria **$2^n$** combinações.
:::

???

Para cada uma dessas combinações, ainda precisamos:

1. Somar os pesos dos itens escolhidos;
2. Se o total de peso for menor ou igual à capacidade da mochila, somamos os valores;
3. Guardamos o maior valor encontrado.

Essa é uma abordagem simples de entender, mas você já deve ter começado a perceber alguns desafios.

??? Checkpoint

Quais desvantagens você consegue enxergar nessa abordagem de força bruta?

::: Gabarito
O número de combinações cresce exponencialmente. Por exemplo, para 30 itens, teríamos mais de 1 bilhão de combinações. Isso torna essa solução inviável para situações reais com muitos objetos (pense na complexidade desse algoritmo).
:::

???

Embora esse método não seja o mais adequado para resolver problemas maiores, ele nos ajuda a ter compreensão do funcionamento e da natureza do problema. A partir dessa visão geral, começamos a entender por que precisamos de algoritmos mais eficientes, que consigam chegar à melhor (ou uma boa) solução sem precisar testar todas as alternativas. A abordagem de força bruta é, portanto, um primeiro passo para avançarmos para estratégias mais inteligentes — e é isso que veremos a seguir!

Método Guloso
--------------

O método **guloso** tenta resolver o problema fazendo a escolha que parece melhor naquele momento. Em vez de olhar todas as combinações possíveis (como a força bruta faz), o método guloso decide item por item, com base em um critério que parece vantajoso — a **importância** do item, medida pela razão entre valor e peso:

$$
I (importância) = \frac{valor}{peso}
$$

A ideia é: se eu puder levar um item leve e muito valioso, talvez ele traga mais benefício do que um item pesado com valor baixo. Assim, ordenamos os itens com base nesse índice, e vamos colocando os que têm maior valor por peso (a *importância*) primeiro, até encher a mochila.

Vamos ver um exemplo:

Suponha que temos os seguintes itens, em uma mochila com capacidade de 15kg.

| Item | Peso (Kg) | Valor | Importância (I) |
| ---- | --------- | ----- | ---------------- |
| A    | 5         | 10    | 2                |
| B    | 4         | 40    | 10               |
| C    | 6         | 30    | 5                |

Seguimos o **passo a passo**:

1. Ordenamos por importância: B (10), C (5), A (2);
2. Adicionamos primeiro os itens de maior importância:

   - Colocamos o item B (4kg)
   - Depois o item C (6kg)
   - Depois o item A (5kg)

Ao final, teríamos:

* Peso total: 4 + 6 + 5 = 15kg — mochila cheia!
* Valor total: 40 + 30 + 10 = 80 -  valor máximo!

Mas será que o método guloso sempre nos dá a melhor solução? Ou seja, será que sempre teremos o valor máximo que a mochila pode carregar?

??? Checkpoint

Você acha que o método guloso funcionaria para esse caso, com uma mochila de `md capacidade igual a 5kg`?

| Item | Peso (kg) | Valor (R\$) | Valor/Peso |
| ---- | --------- | ----------- | ---------- |
| A    | 2         | 40          | 20         |
| B    | 3         | 51          | 17         |
| C    | 5         | 95          | 19         |

::: Dica 1
Siga o passo a passo, ordenando os itens pela importância. Depois adicione primeiro os itens mais importantes, até atingir a capacidade máxima.
:::

::: Dica 2
Tente testar todas as combinações possíveis e veja se ele realmente escolheu a combinação que maximiza o valor que a mochila carrega.
:::

::: Gabarito

1. Calculamos a importância (razão entre valor e peso) e ordenamos:

   - Item A: 40/2 = 20
   - Item C: 95/5 = 19
   - Item B: 51/3 = 17
2. Adicionamos o **Item A** na mochila. Ele tem a maior importância e pesa 2Kg. Ainda sobra 3Kg. O Item C tem a segunda maior importância, mas não cabe, então adicionamos o **Item B**.

Ao final, teríamos:

* Peso: 2 + 3 = 5 - mochila cheia!
* Valor Total: 40 + 51 =  91

3. Testando as possibilidades, vemos que o **Item C** sozinho pesa *5Kg* e tem *valor 95*.

Ou seja, o método *guloso* pegou o *Item A* por parecer mais "importante", mas perdeu a chance de pegar o *Item C*, que maximiza o valor carregado na mochila.
:::

???

Apesar de ser prático, o método guloso nem sempre encontra a melhor solução para o problema da mochila binária. Ele se baseia apenas na decisão que parece melhor no momento, sem considerar o impacto que essa escolha pode ter nas próximas. É como encher uma mala escolhendo primeiro o que parece mais valioso por quilo, mas esquecer de verificar se a melhor combinação caberia de outro jeito.

Na próxima abordagem, vamos ver como a programação dinâmica consegue garantir uma melhor solução para o problema da mochila binária.

Programação Dinâmica (Tabela Dinâmica)
------------------------------------------

Imagine que você está resolvendo o problema da mochila e, a cada nova combinação de itens, precisa recalcular tudo do zero. Repetir esse trabalho é cansativo — e ineficiente. É como tentar lembrar um número de telefone toda vez que vai ligar, em vez de simplesmente salvá-lo na agenda.

A programação dinâmica surge para resolver isso. Ela parte do princípio de que problemas grandes podem ser resolvidos a partir de subproblemas menores. Ou seja, se já sabemos a melhor solução para uma mochila de 3kg com 2 itens, podemos reaproveitar essa informação para resolver casos maiores, como 4kg com 3 itens.

No caso da mochila binária, a técnica consiste em preencher uma tabela que guarda, para cada combinação de item e capacidade, qual o melhor valor possível até aquele ponto. Assim, vamos construindo a solução ideal aos poucos, sem precisar recalcular.

**Como funciona a tabela**

Vamos imaginar o problema onde temos a mochila com **`md capacidade = w`** e devemos escolher entre  **`md n itens`**. Para construir a tabela `md dp`, devemos ter:

* n + 1 linhas (uma linha para cada item, de 0 a n);
* w + 1 colunas (uma coluna para cada capacidade da mochila, de 0 a w).
* O preenchimento da tabela começa com 0 e vai sendo atualizado com os melhores valores possíveis em cada célula.

Pode parecer óbvio, mas não custa lembrar: se nenhum item é adicionado, o valor total da mochila é zero. Se nenhum item cabe na mochila, seu valor também é zero.

A célula `md dp[i][w]` armazena o melhor valor total possível usando os primeiros i itens e uma mochila de capacidade w.

A ideia é decidir, para cada item:

1. Não levar o item → mantemos a solução anterior: `md dp[i][w] = dp[i-1][w]`;
2. Levar o item, se couber → somamos o valor dele com a melhor solução para o espaço restante: `md dp[i][w] = valor[i] + dp[i-1][w - peso[i]]`.

Ou seja, para cada célula da tabela, usamos a fórmula:

$$
dp[i][w]=max(dp[i−1][w], valor[i] + dp[i−1][w−peso[i]])
$$

!!! Aviso
Note que `md dp[i−1][w−peso[i]]` pode estar fora do range da tabela. Nesse caso, considere `md dp[i][w] = dp[i−1][w]`.

Além disso, é importante destacar que, para a linha i, consideramos **todos os itens até o item i**, ou seja, essa solução considera **subconjuntos de itens**.
!!!

??? Checkpoint

Ok, sabemos que pode estar difícil de visualizar esse método, então vamos construir essa tabela juntos, com um exemplo prático.

Vamos supor uma mochila com capacidade `md w = 5`. Considere ainda que as entradas são:

| Item | Peso | Valor |
| ---- | ---- | ----- |
| A    | 2    | 4     |
| B    | 3    | 5     |
| C    | 4    | 6     |

Vamos montar a tabela dp, com 4 linhas (0 a 3 itens → linha 0 representa “nenhum item”) e 6 colunas (capacidades de 0 a 5). Para fins didáticos, vamos apenas adicionar uma coluna à esquerda identificando os "nomes" dos itens para facilitar a visualização e referência.

|                                                                         Item | i | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---------------------------------------------------------------------------: | -: | ----: | ----: | -----: | ----: | ----: | ----: |
|                                                                            - | 0 |       |       |        |       |       |       |
|                                                                            A | 1 |       |       |        |       |       |       |
|                                                                            B | 2 |       |       |        |       |       |       |
|                                                                            C | 3 |       |       |        |       |       |       |
| Começamos preenchendo a primeira linha e a primeira coluna com zeros, pois: |   |       |       |        |       |       |       |

- Nenhum item → valor total é zero
- Mochila de capacidade zero → não dá pra levar nada

| Item | i | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | -: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - | 0 |     0 |     0 |      0 |     0 |     0 |     0 |
|    A | 1 |     0 |       |        |       |       |       |
|    B | 2 |     0 |       |        |       |       |       |
|    C | 3 |     0 |       |        |       |       |       |

Agora, vamos preencher linha por linha, considerando se vale a pena ou não incluir o item através de:

$$
dp[i][w]=max(dp[i−1][w], valor[i] + dp[i−1][w−peso[i]])
$$

* **Até o Item A:**

Para cada coluna (capacidade w):

- Se w < 2 → não cabe o item → copia o de cima → dp[1][w] = dp[0][w]
- Se w ≥ 2 → compara as duas opções e escolhe o valor máximo entre elas:

  1. Não levar A: dp[0][w]
  2. Levar A: 4 + dp[0][w - 2]

Resultado da tabela até o item A:

| Item | i | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | -: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - | 0 |     0 |     0 |      0 |     0 |     0 |     0 |
|    A | 1 |     0 |     0 |      4 |     4 |     4 |     4 |
|    B | 2 |     0 |       |        |       |       |       |
|    C | 3 |     0 |       |        |       |       |       |

* **Até o Item B:**

Agora, para cada w:

- Se w < 3 → não cabe B → mantém valor de cima
- Se w ≥ 3 → compara:

  1. Não levar B: dp[1][w]
  2. Levar B: 5 + dp[1][w - 3]

Resultado da tabela até o item B:

| Item | i | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | -: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - | 0 |     0 |     0 |      0 |     0 |     0 |     0 |
|    A | 1 |     0 |     0 |      4 |     4 |     4 |     4 |
|    B | 2 |     0 |     0 |      4 |     5 |     5 |     9 |
|    C | 3 |     0 |       |        |       |       |       |

Vamos ver se você entendeu? Tente fazer a construção até o Item C.

::: Gabarito

Repetimos o mesmo processo para cada w:

- Se w < 4 → não cabe C → repete valor de cima
- Se w ≥ 4 → compara:

  1. Não levar C: dp[2][w]
  2. Levar C: 6 + dp[2][w - 4]

Resultado da tabela até o Item C:

| Item | i | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | -: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - | 0 |     0 |     0 |      0 |     0 |     0 |     0 |
|    A | 1 |     0 |     0 |      4 |     4 |     4 |     4 |
|    B | 2 |     0 |     0 |      4 |     5 |     5 |     9 |
|    C | 3 |     0 |     0 |      4 |     5 |     6 |     9 |

A célula dp[3][5] contém o valor 9, que é o maior valor possível para uma mochila de 5kg com esses 3 itens.

:::

???

Ok, nós construímos toda a tabela `md dp` com programação dinâmica e descobrimos qual é o valor máximo que conseguimos carregar na mochila — ele estará na célula `md dp[n][w]`.

Mas... quais itens exatamente compõem essa solução?

Para descobrir isso, usamos uma técnica chamada **backtracking**, que nada mais é do que voltar pela tabela e analisar as decisões que foram tomadas ao longo do caminho.

**Backtracking**

Partimos da última célula da tabela, `md dp[n][w]`, e vamos subindo, linha por linha. A cada passo, comparamos o valor atual com o valor logo acima:

1. Se `md dp[i][w]` = `md dp[i-1][w]`, o item i não foi incluído na mochila.
2. Se `md dp[i][w]` ≠ `md dp[i-1][w]`, o item i foi incluído. Nesse caso:

   - Adicionamos o item i à lista de itens escolhidos;
   - Reduzimos a capacidade restante da mochila em peso[i] (ou seja, w = w - peso[i]);
   - Subimos para a linha anterior: i = i - 1;
   - Repetimos o processo até i = 0 ou w = 0.

??? Checkpoint

Tente fazer o backtracking para o exemplo que construímos. Aqui está a tabela que chegamos:

| Item | i | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | -: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - | 0 |     0 |     0 |      0 |     0 |     0 |     0 |
|    A | 1 |     0 |     0 |      4 |     4 |     4 |     4 |
|    B | 2 |     0 |     0 |      4 |     5 |     5 |     9 |
|    C | 3 |     0 |     0 |      4 |     5 |     6 |     9 |

::: Gabarito
Considere a célula dp[3][5] = 9

* Comparando com dp[2][5] = 9
  → os valores são iguais → o item C (linha 3) não foi incluído
* Subimos para dp[2][5] = 9, comparamos com dp[1][5] = 4

  * os valores são diferentes → o item B (linha 2) foi incluído

  - Adicionamos B à lista
  - Subtraímos seu peso: w = 5 - peso[2] = 5 - 3 = 2
  - Subimos para i = 1
* Em dp[1][2] = 4, comparamos com dp[0][2] = 0

  * valor diferente → o item A foi incluído

  - Adicionamos A à lista
  - Subtraímos w = 2 - peso[1] = 2 - 2 = 0
  - Fim do processo
* **Itens incluídos: A e B**
  :::

???

A abordagem de programação dinâmica para o problema da mochila binária nos permite encontrar a melhor solução possível sem precisar testar todas as combinações. Em vez disso, vamos construindo respostas parciais, armazenando os melhores valores obtidos para cada quantidade de itens e para cada capacidade da mochila.

Ao final do preenchimento da tabela, conseguimos recuperar o valor máximo que pode ser carregado e, com o auxílio do backtracking, também conseguimos descobrir quais itens foram escolhidos para compor essa solução.

??? Checkpoint

Prometo que esse checkpoint é curto! Vamos só pensar nas vantagens desse método?

::: Gabarito

1. Reaproveita subsoluções. Ou seja, se você quisesse adicionar mais um item, bastava adicionar uma linha na tabela, sem precisar refazer todos os cálculos;
2. Funciona bem para capacidades e quantidades de itens moderadas;
3. Sempre encontra a melhor combinação possível, ou seja, o valor máximo.
4. Tem complexidade de tempo e memória O(n × W), o que é suficiente para a maioria dos casos práticos.

:::

???

Apesar de ser um pouco mais trabalhoso de implementar, esse método tem uma eficiência muito superior à força bruta, com complexidade de tempo O(n × w), onde n é o número de itens e w é a capacidade da mochila.

A tabela que usamos também ocupa memória proporcional a n × w, o que funciona bem para entradas de tamanho moderado. Para valores muito grandes, existem otimizações que reduzem o uso de memória — mas o raciocínio por trás continua sendo o mesmo.

Implementações do Algoritmo da Mochila Binária
--------------------------------

A partir daqui, vamos ver como transformar a lógica da mochila binária em **código em linguagem C**, usando duas estratégias:

- Uma abordagem **recursiva com memorização** (top-down), que reaproveita os resultados já calculados para evitar recomputações;
- Uma abordagem **iterativa com tabela (bottom-up)**, que constrói a solução a partir dos menores subproblemas.

Ambas chegam à mesma resposta final, mas têm estruturas e estilos diferentes de implementação. Vamos começar pela versão recursiva!

Para realizar a implementação, primeiro precisamos deixar claro quais são as entradas do problema:

**Entradas**
- n: número total de itens disponíveis;

- peso[n]: vetor com o peso de cada item;

- valor[n]: vetor com o valor de cada item;

- w: capacidade máxima da mochila (peso total que ela suporta).

Cada item pode ser levado por completo ou não levado — não há divisões parciais (por isso o problema é chamado de mochila binária ou 0/1 knapsack). É importante notar que o peso[i] corresponde ao item i que possui valor[i]. 

**Saídas esperadas**
- O maior valor total que pode ser carregado sem ultrapassar a capacidade da mochila;
- Quais itens compõem essa solução ótima (essa saída é opcional, é o nosso *backtracking*)

## Implementação Recursiva Top‑Down (Memo)

*Recursão com cache (memoization) implementada com tabela 2D em C*

A abordagem top-down com memorização segue o raciocínio natural da recursão: para resolver um problema grande, vamos resolvendo versões menores dele e guardando os resultados já calculados para evitar recomputações.

No contexto da mochila binária, queremos responder: **Qual o maior valor que posso obter considerando os itens de índice i em diante, com p quilos ainda disponíveis na mochila?**

Chamaremos essa função de `md solve(i, p)`. A cada chamada, ela tenta duas possibilidades:

- Ignorar o item i e seguir com os próximos;

- Incluir o item i, se ele couber, e somar seu valor ao resultado da submochila restante.

Como muitos desses subproblemas se repetem, usamos uma tabela dp[i][p] para guardar as respostas já calculadas. Um vetor vis[i][p] nos ajuda a saber se já resolvemos esse estado antes.

**Entradas esperadas da função solve(i, p)**
- `md i`: índice do item atual (de 1 até n, inclusive);

- `md p`: peso restante da mochila (capacidade ainda disponível);

**Saída esperada**
- Retorna o maior valor total possível que pode ser obtido considerando os itens de i até n, respeitando a capacidade restante p.

??? Checkpoint
O que é esperado no retorno de `md solve(1, 10)`? 
:::
Responde qual o maior valor possível ao considerar todos os itens a partir do item 1, com 10kg de capacidade restante.
:::
???


Veja o esqueleto em pseudocódigo:

```text
função solve(i, p):
    // Caso base: já passamos do último item
    se i > n:
        retorna 0

    // Se já calculamos esse estado, retornamos o valor salvo
    se vis[i][p] == verdadeiro:
        retorna dp[i][p]

    // Marcamos esse estado como visitado
    vis[i][p] ← verdadeiro

    // Inicialmente, testamos a opção de NÃO levar o item i
    melhor_valor ← solve(i + 1, p)

    // Se o item i couber na mochila, testamos também levar ele
    se p ≥ peso[i]:
        valor_com_item ← solve(i + 1, p - peso[i]) + valor[i]
        melhor_valor ← máximo(melhor_valor, valor_com_item)

    // Guardamos a melhor resposta possível para esse estado
    dp[i][p] ← melhor_valor

    retorna melhor_valor
```

??? Checkpoint
Reflita por que `md vis[i][p]` impede que o algoritmo gere $2^n$ chamadas.

::: Gabarito
Cada par `md (i,p)` é guardado após o primeiro cálculo; as chamadas seguintes apenas retornam o valor salvo, evitando recomputação de subárvores.
:::
???

**Declarações em C**

Agora vamos à implementação do código em C:

```c
#define MAXN 110
#define MAXP 100010

int  w[MAXN], v[MAXN];
long long dp[MAXN][MAXP];
char vis[MAXN][MAXP];
int n;
```

**Função `md solve`**

```c
long long solve(int i, int p){
    if(i == n + 1) return 0;
    if(vis[i][p])  return dp[i][p];

    vis[i][p] = 1;
    long long melhor_valor = solve(i+1, p);
    if(p >= w[i]){
        long long valor_com_item = solve(i+1, p - w[i]) + v[i];
        if(valor_com_item > melhor_valor)
            melhor_valor = valor_com_item;
    }
    return dp[i][p] = melhor_valor;
}
```

Essa função retorna o melhor valor possível para cada estado (i, p), salvando o resultado em dp e marcando vis como visitado, o que permite reutilizar as respostas em futuras chamadas recursivas.

??? Checkpoint
Qual a complexidade de **tempo** e **espaço**?

::: Gabarito
Tempo $O(nw)$ porque cada `md (i,p)` é avaliado uma vez.
Espaço $O(nw)$ para guardar `md dp` e `md vis`.
:::
???
 
!!! Aviso  
Usamos `md long long` para garantir que os valores somados em `md dp` não excedam o limite de um `md int` ou  `md long`. Isso é especialmente importante quando os valores dos itens são grandes ou quando há muitos itens na entrada.  
!!!

## Implementação Bottom‑Up (Tabela 2‑D)

Na abordagem **bottom-up**, resolvemos o problema de forma iterativa, preenchendo uma tabela `ms dp[i][w]` onde cada entrada representa o **maior valor possível com os primeiros `md i` itens e capacidade `md w`**. A ideia é ir **acumulando soluções de subproblemas menores**, sem repetir nenhum cálculo.

### Estrutura fundamental

```c
typedef struct {
    int w;           // capacidade da mochila
    int n;           // número total de itens
    int *peso;       // vetor de pesos
    int *valor;      // vetor de valores
    int **dp;        // tabela dp de (n+1) x (w+1)
} knapsack_problem;
```

A `md struct` reúne todos os dados do problema e a matriz `md (n+1) × (w+1)` que será preenchida.*

??? Checkpoint
Quanto de memória a matriz ocupa se `md n = 200` e `md w = 5000`?

::: Gabarito
`md 201 × 5001 × 4` bytes ≈ **4,02 MB**.
:::
???

**Inicialização da tabela**

Antes de preencher a tabela, é importante inicializar todos os valores com zero, já que:

- Uma mochila vazia (capacidade 0) não pode levar nada;

- Nenhum item (linha 0) leva a valor 0 para qualquer capacidade.

??? Exercício
Implemente `md init_dp_zeros(int **dp, int rows, int cols)` para zerar todos os elementos da matriz.

::: Gabarito

```c
static void init_dp_zeros(int **dp, int rows, int cols){
    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            dp[i][j] = 0; // inicializa célula por célula
        };
    };
}
```
Você tambem pode usar `md memset` para inicializar uma sequência de dados com zeros.
:::
???

**Preenchendo a tabela dinâmica**

Agora que a matriz `md dp` foi alocada e inicializada com zeros, vamos preenchê-la **de baixo para cima**, seguindo o raciocínio da programação dinâmica.

A ideia é iterar por cada item `md i` e cada capacidade `md j` da mochila, e decidir:

- Se o item `md i` **não for incluído**: copiamos o valor da linha anterior `md dp[i-1][j]`;
- Se o item `md i` **couber na mochila** e for incluído: somamos seu valor `md valor[i-1]` com o valor da mochila restante `md dp[i-1][j - peso[i-1]]`;
- Salvamos o **melhor dos dois valores** em `md dp[i][j]`.

```c
void knapsack_solve(knapsack_problem *kp){
    for (int i = 1; i <= kp->n; ++i){
        for (int j = 0; j <= kp->w; ++j){
            int melhor_valor = kp->dp[i - 1][j]; // não leva o item i

            if (kp->peso[i - 1] <= j){
                int valor_com_item = kp->dp[i - 1][j - kp->peso[i - 1]] + kp->valor[i - 1];
                if (valor_com_item > melhor_valor)
                    melhor_valor = valor_com_item; // leva o item i
            }

            kp->dp[i][j] = melhor_valor;
        }
    }
}
```

Para cada item `md i`, a linha `md i` deriva da linha `md i-1`: se o item cabe, comparamos incluir com não incluir; caso contrário, copiamos o valor anterior.

**Reconstrução do subconjunto**

Depois que a tabela dinâmica dp estiver preenchida, o próximo passo é descobrir quais itens compõem a solução ótima.

Para isso, percorremos a tabela de baixo para cima. Em cada linha i, comparamos`md dp[i][j]` com `md dp[i-1][j]`:

- Se os valores forem iguais → o item i-1 não foi incluído;

- Se os valores forem diferentes → o item i-1 foi incluído, então subtraímos seu peso da capacidade restante.

Essa técnica é o *backtracking na tabela*.

```c
void knapsack_reconstruct(const knapsack_problem *kp, int *chosen){
    int j = kp->w;
    for (int i = kp->n; i >= 1; --i){
        if (kp->dp[i][j] != kp->dp[i - 1][j]){
            chosen[i - 1] = 1;             // item foi escolhido
            j -= kp->peso[i - 1];          // atualiza capacidade restante
        } else {
            chosen[i - 1] = 0;             // item não foi escolhido
        }
    }
}
```

A descida da última linha até a primeira identifica quais itens alteraram o valor da solução — esses são os selecionados. 
O vetor `md chosen[i]` indica com 1 se o item i foi incluído na mochila, ou 0 caso contrário.

**Inicialização do problema com malloc**

A seguir está a função knapsack_init, responsável por:

- Copiar os vetores de entrada (peso e valor);

- Alocar a matriz `md dp[n+1][w+1]` com malloc;

- Zerar a matriz, chamando init_dp_zeros().

```C
void knapsack_init(knapsack_problem *kp, int n, int w, int *peso, int *valor){
    kp->n = n;
    kp->w = w;

    // Copiando os vetores de peso e valor
    kp->peso = malloc(n * sizeof(int));
    kp->valor = malloc(n * sizeof(int));
    for (int i = 0; i < n; ++i){
        kp->peso[i] = peso[i];
        kp->valor[i] = valor[i];
    }

    // Alocando a matriz dp (n+1 linhas por w+1 colunas)
    kp->dp = malloc((n + 1) * sizeof(int *));
    for (int i = 0; i <= n; ++i){
        kp->dp[i] = malloc((w + 1) * sizeof(int));
    }

    // Inicializa a matriz com zeros
    init_dp_zeros(kp->dp, n + 1, w + 1);
}
```
**Liberação de memória**

Para evitar vazamentos de memória, lembre-se de liberar tudo que foi alocado dinamicamente:

```C
void knapsack_free(knapsack_problem *kp){
    for (int i = 0; i <= kp->n; ++i)
        free(kp->dp[i]);
    free(kp->dp);
    free(kp->peso);
    free(kp->valor);
}
```

**Função `md main` de demonstração**

```c
int main(void){
    int peso[] = {2, 3, 4, 5, 3};     // pesos dos itens
    int valor[] = {5, 10, 12, 8, 7};   // valores dos itens
    int n = 5, w = 10;

    // Inicializa o problema com vetores e capacidade
    knapsack_problem kp;
    knapsack_init(&kp, n, w, peso, valor);

    // Preenche a tabela dinâmica
    knapsack_solve(&kp);

    // Vetor para guardar os itens escolhidos
    int *chosen = malloc(n * sizeof(int));
    for (int i = 0; i < n; ++i) {
        chosen[i] = 0; //inicializa todos os valores com zero
    };

    knapsack_reconstruct(&kp, chosen);

    // Mostra o valor ótimo encontrado
    printf("Valor ótimo: %d\n", kp.dp[n][w]);

    // Imprime os itens escolhidos
    printf("Itens escolhidos: ");
    for (int i = 0; i < n; ++i){
        if (chosen[i]) printf("%d ", i);  // imprime o índice do item
    }
    printf("\n");

    // Libera a memória alocada
    knapsack_free(&kp);
    free(chosen);

    return 0;
}

```

O programa monta o problema, resolve, reconstrói a lista de itens e mostra o valor ótimo.

??? Checkpoint
No exemplo acima, quais itens foram escolhidos e qual o peso total da mochila?

Considere os seguintes dados de entrada, mostrados no exemplo da função `md main`:

- Pesos: `md peso = {2, 3, 4, 5, 3}`
- Valores: `md valor = {5, 10, 12, 8, 7}`
- Capacidade da mochila: `md w = 10`

:::
Gabarito:

Os itens escolhidos são `md chosen = [0, 1, 1, 0, 1]`, ou seja, os itens de índice 1, 2 e 4.

- Pesos escolhidos: 3 + 4 + 3 = **10**
- Valores escolhidos: 10 + 12 + 7 = **29**

Portanto, o valor ótimo é `md 29`, e a mochila ficou cheia.
:::
???

Conclusão
----------

O problema da mochila binária ilustra o cerne da otimização combinatória, equilibrando valor máximo e restrições de capacidade. Através das abordagens apresentadas, vimos como algoritmos evoluem para lidar com complexidade.

Programação Dinâmica não é só uma técnica — é ainda uma mentalidade para decompor problemas complexos em subproblemas gerenciáveis, armazenando soluções parciais para eficiência.