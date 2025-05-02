O Problema da Mochila Binária
==============================

Subtítulo
----------

Imagine que você está preparando uma mochila para fazer uma trilha. Você tem vários itens disponíveis, cada um com um peso e um valor de utilidade, mas sua mochila, assim como qualquer outra, tem uma capacidade limitada.

Como escolher os itens para levar de forma a **maximizar o valor total sem ultrapassar o peso máximo**? Esse é o clássico *Problema da Mochila Binária*, que trabalharemos ao longo desse handout.

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

Se você tem `md 5 itens`, quantas combinações você teria? E se tiver `md n itens`?

::: Dica
Pense para 1 item. Depois para 2. E depois para 3. Você consegue identificar um padrão?
:::

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
| ---- | --------- | ----------- | -------------- |
| A    | 5         | 10          | 2              |
| B    | 4         | 40          | 10             |
| C    | 6         | 30          | 5              |

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

| Item | Peso (kg) | Valor (R\$) | Valor/Peso   |
| ---- | --------- | ----------- | ------------ |
| A    | 2         | 40          | 20 |
| B    | 3         | 51          | 17 |
| C    | 5         | 95          | 19 |

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
| A    |  2   |    4  |
| B    |  3   |    5  |
| C    |  4   |    6  |

Vamos montar a tabela dp, com 4 linhas (0 a 3 itens → linha 0 representa “nenhum item”) e 6 colunas (capacidades de 0 a 5). Para fins didáticos, vamos apenas adicionar uma coluna à esquerda identificando os "nomes" dos itens para facilitar a visualização e referência.
| Item  |  i | w = 0 |  w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ----: | -: | -:    | -:     | -:     | -:    | -:    | -:    |
|     - | 0  |      |        |        |        |       |       |
|     A | 1  |      |        |        |       |       |       |
|     B | 2  |      |        |        |       |       |       |
|     C | 3  |      |        |        |       |       |       |
Começamos preenchendo a primeira linha e a primeira coluna com zeros, pois:

- Nenhum item → valor total é zero

- Mochila de capacidade zero → não dá pra levar nada

| Item  |  i | w = 0 |  w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ----: | -: | -:    | -:     | -:     | -:    | -:    | -:    |
|     - | 0  |  0    |  0     |  0     |  0    |     0 |     0 |
|     A | 1  |  0    |        |        |       |       |       |
|     B | 2  |  0    |        |        |       |       |       |
|     C | 3  |  0    |        |        |       |       |       |

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

| Item  |  i | w = 0 |  w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ----: | -: | -:    | -:     | -:     | -:    | -:    | -:    |
|     - | 0  |  0    |  0     |  0     |  0    |     0 |     0 |
|     A | 1  |  0    |  0     |  4     |  4    |  4    | 4     |
|     B | 2  |  0    |        |        |       |       |       |
|     C | 3  |  0    |        |        |       |       |       |

* **Até o Item B:**

Agora, para cada w:

- Se w < 3 → não cabe B → mantém valor de cima

- Se w ≥ 3 → compara:

    1. Não levar B: dp[1][w]

    2. Levar B: 5 + dp[1][w - 3]

Resultado da tabela até o item B:

| Item  |  i | w = 0 |  w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ----: | -: | -:    | -:     | -:     | -:    | -:    | -:    |
|     - | 0  |  0    |  0     |  0     |  0    |     0 |     0 |
|     A | 1  |  0    |  0     |  4     |  4    |  4    | 4     |
|     B | 2  |  0    |  0     |  4     |  5    |  5    | 9     |
|     C | 3  |  0    |        |        |       |       |       |

Vamos ver se você entendeu? Tente fazer a construção até o Item C.


::: Gabarito

Repetimos o mesmo processo para cada w:

- Se w < 4 → não cabe C → repete valor de cima

- Se w ≥ 4 → compara:
    
    1. Não levar C: dp[2][w]

    2. Levar C: 6 + dp[2][w - 4]

Resultado da tabela até o Item C:

| Item  |  i | w = 0 |  w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ----: | -: | -:    | -:     | -:     | -:    | -:    | -:    |
|     - | 0  |  0    |  0     |  0     |  0    |     0 |     0 |
|     A | 1  |  0    |  0     |  4     |  4    |  4    | 4     |
|     B | 2  |  0    |  0     |  4     |  5    |  5    | 9     |
|     C | 3  |  0    |  0     |  4     |  5    |  6    | 9     |

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

| Item  |  i | w = 0 |  w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ----: | -: | -:    | -:     | -:     | -:    | -:    | -:    |
|     - | 0  |  0    |  0     |  0     |  0    |     0 |     0 |
|     A | 1  |  0    |  0     |  4     |  4    |  4    | 4     |
|     B | 2  |  0    |  0     |  4     |  5    |  5    | 9     |
|     C | 3  |  0    |  0     |  4     |  5    |  6    | 9     |

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

3. Sempre encontra a melhor combinação possível, ou veja, o valor máximo.

4. Tem complexidade de tempo e memória O(n × W), o que é suficiente para a maioria dos casos práticos.

:::

???

Apesar de ser um pouco mais trabalhoso de implementar, esse método tem uma eficiência muito superior à força bruta, com complexidade de tempo O(n × w), onde n é o número de itens e w é a capacidade da mochila.

A tabela que usamos também ocupa memória proporcional a n × w, o que funciona bem para entradas de tamanho moderado. Para valores muito grandes, existem otimizações que reduzem o uso de memória — mas o raciocínio por trás continua sendo o mesmo.
