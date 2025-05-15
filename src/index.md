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

Até agora, tentamos resolver o problema da mochila binária testando diversas combinações e, a cada nova combinação de itens, precisamos recalcular tudo do zero. Mas será que a gente realmente precisa refazer os cálculos toda vez?

??? Checkpoint

Imagine que você já calculou qual o maior valor possível que pode ser carregado com uma mochila de 3kg usando os dois primeiros itens. Agora você precisa resolver o mesmo problema, mas com 4kg e três itens.

Você precisaria recalcular tudo ou seria possível reaproveitar algum cálculo anterior?

::: Gabarito
Se já sabemos a melhor solução para 3kg e 2 itens, podemos aproveitar esse resultado para resolver o novo problema. Afinal, estamos só adicionando mais uma opção à decisão.
:::

???

É exatamente essa a ideia por trás da programação dinâmica: guardar os resultados de subproblemas menores para reutilizá-los em problemas maiores. Ela parte do princípio de que problemas grandes podem ser resolvidos a partir de subproblemas menores. 

Se a ideia é reaproveitar subsoluções, precisamos de uma estrutura que nos permita guardar os melhores resultados parciais. Para isso, vamos montar uma **tabela dinâmica**. Essa técnica consiste em preencher na tabela, para cada combinação de item e capacidade, qual o melhor valor possível até aquele ponto. Assim, vamos construindo a solução ideal aos poucos, sem precisar recalcular.

**Como é o formato da tabela?**

Vamos imaginar o problema onde temos a mochila com **`md capacidade máxima = W`** e devemos escolher entre  **`md n itens`**. Para construir a tabela `md dp` precisamos saber quantas linhas e quantas colunas devemos ter.

??? Checkpoint
Se temos  **`md n itens`**, e a capacidade máxima da mochila é **`md W`**, quantas combinações diferentes de "itens usados" e "capacidade disponível" precisamos analisar? Ou seja, qual o número total de células da tabela?
::: Dica 1
Pense em quantas linhas e em quantas colunas essa tabela deve ter para representar todas as combinações. Se ainda não tiver ficado claro, veja a Dica 2.
:::
::: Dica 2
Tente pensar em "itens" como elementos das linhas da tabela, e as "capacidades" da mochila como elementos das colunas.
:::

::: Gabarito
Precisamos analisar todas as combinações possíveis entre:

* o número de itens considerados (de 0 até n, ou seja, n + 1 opções);

* e os valores possíveis de capacidade (de 0 até W, ou seja, W + 1 opções).

Portanto, o total de combinações é **`md (n+1)x(W+1)`**.

| Item | w = 0 | w = 1 | w =  2 | ...   | w = W |
| ---: | ----: | ----: | -----: | ----: | ----: |
|    0 |       |       |        |       |       |
|    1 |       |       |        |       |       |
|  ... |       |       |        |       |       |
|    n |       |       |        |       |       |

:::
???

??? Checkpoint

Por que começamos a contar a partir do 0? Por exemplo, linha 0 (nenhum item), coluna 0 (capacidade zero).

::: Gabarito
Porque precisamos considerar também os casos base:

* Nenhum item escolhido ainda
* Mochila com capacidade zero

Esses casos ajudam a construir a tabela desde o início.
:::
???

Com isso, em nossa tabela `md dp` temos:

* `md n + 1` linhas (uma linha para cada item, de 0 a n);
* `md W + 1` colunas (uma coluna para cada capacidade da mochila, de 0 a W).
* Cada célula `md dp[i][w]` armazena o máximo valor possível da mochila usando **os primeiros `md i` itens** e uma **capacidade `md w`**.

??? Checkpoint
Qual o **valor máximo** na primeira linha e primeira coluna da tabela?
::: Gabarito

Tudo deverá ser zero!
* Se não temos itens (linha 0), o valor da mochila é zero.
* Se a capacidade da mochila é zero (coluna 0), também não dá para levar nada.

| Item | w = 0 | w = 1 | w =  2 | ...   | w = W |w = W + 1|
| ---: | ----: | ----: | -----: | ----: | ----: | ----: |
|    0 |   0   |   0   |   0    |   0   |    0  |  0    |
|    1 |   0   |       |        |       |       |       |
|  ... |   0   |       |        |       |       |       |
|    n |   0   |       |        |       |       |       |
|n + 1 |   0   |       |        |       |       |       |
:::
???

**Como preencher a tabela?**

Vamos seguir com um exemplo prático. Suponha que temos os seguintes itens e uma mochila com capacidade máxima de 5kg:

| Item | Peso | Valor |
| ---- | ---- | ----- |
| A    | 2    | 3     |
| B    | 3    | 5     |
| C    | 4    | 9     |

Para cada item, temos duas opções:
1. Levar o item;
2. Não levar o item;

??? Checkpoint
Construa a tabela considerando os itens e a capacidade proposta. Depois preencha os valores da primeira linha e da primeira coluna.
::: Gabarito
Sua tabela deve ter ficado assim:

| Item | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - |     0 |     0 |      0 |     0 |     0 |     0 |
|    A |     0 |       |        |       |       |       |
|    B |     0 |       |        |       |       |       |
|    C |     0 |       |        |       |       |       |
:::
???

Já conhecemos o formato da tabela, agora devemos preencher a tabela com o valor máximo que podemos ter na mochila considerando os `md i` primeiros itens e a capacidade `md w`

??? Checkpoint

Analise a linha do item A. Se a mochila tiver capacidade 1kg, dá para levar o item A? Qual será o valor da mochila nesse caso?

::: Gabarito
Não é possível levar o item. O item A pesa 2kg e não cabe em mochilas de 0kg ou 1kg. O valor da mochila será zero.
:::
???

Se a mochila tiver capacidade de 2kg ou mais, temos duas opções:

* Levar o item; 
* Não levar o item.

??? Checkpoint
Se o objetivo do problema é maximizar o valor na mochila de capacidade w, em que devemos basear a decisão de levar ou não levar o item?
::: Gabarito
Se o **valor** da mochila *aumentar*, devemos *levar* o item. Do contrário, devemos manter a combinação anterior e não levar o item.
:::
???

Para o item A, o valor da mochila aumenta se levarmos o item. Portanto teríamos a seguinte construção:

| Item | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - |     0 |     0 |      0 |     0 |     0 |     0 |
|    A |     0 |     0 |      3 |     3 |     3 |     3 |
|    B |     0 |       |        |       |       |       |
|    C |     0 |       |        |       |       |       |

??? Checkpoint
Agora preencha a linha da tabela para o item B. Lembre-se que você está considerando o melhor valor usando os itens *até o item B*.
::: Gabarito
Sua tabela deve ter ficado assim:

| Item | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - |     0 |     0 |      0 |     0 |     0 |     0 |
|    A |     0 |     0 |      3 |     3 |     3 |     3 |
|    B |     0 |     0 |      3 |     5 |     5 |     8 |
|    C |     0 |       |        |       |       |       |
:::
???


??? Checkpoint
Por que copiamos o valor da célula acima quando o item B não coube?
::: Gabarito
Porque não podemos levar esse item, então mantemos a melhor solução com os itens anteriores.
:::
???

Para o item B, você deve ter feito o seguinte raciocínio:

- Para a capacidade `md w = 2` somente o item A cabe, então você copiou o valor da célula acima. 
- Para as capacidades `md w = 3` e `md w = 4` você deve ter comparado o valor de A e o valor de B, e escolhido levar o item B, que dava o maior valor.
- Para a capacidade `md w = 5` você percebeu que tinha espaço para levar o item B e que sobrava espaço para levar o item A. Ou seja, você somou o valor do item B com o melhor valor possível para o espaço restante.

??? Checkpoint
Para finalizar essa tabela, faça a implementação para a linha do item C.
::: Gabarito
Sua tabela deve ter ficado assim:

| Item | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - |     0 |     0 |      0 |     0 |     0 |     0 |
|    A |     0 |     0 |      3 |     3 |     3 |     3 |
|    B |     0 |     0 |      3 |     5 |     5 |     8 |
|    C |     0 |     0 |      3 |     5 |     9 |     9 |
:::
???

A partir da construção da tabela, identificamos que o valor máximo que podemos ter em uma mochila de `md w = 5` considerando os itens A, B e C é **`md 9`**.

??? Checkpoint
Analise a tabela e tente descobrir se é possível generalizar em qual célula teremos o máximo valor possível para `md n itens` e `md capacidade W`
::: Dica
Analise a construção que você fez para a linha anterior da tabela (linha do item B). Onde estava o valor máximo?
:::
::: Gabarito
O valor máximo estará na célula inferior direita. Ou seja, para `md n+1` itens (de 0 a n) e `md W+1` capacidades (de 0 a W), o **valor ótimo** estará na celula `md dp[n][W]`
:::
???

**Regra geral para preencher a tabela**

Até aqui, você preencheu linha por linha da tabela, tomando decisões baseadas em dois cenários:

1. Cenário I: Não levar o item atual → copiando o valor da célula acima.
2. Cenário II: Levar o item atual → somando o valor dele com a melhor solução para o espaço restante.

Para esses dois cenários, podemos representar capa um por uma fórmula que compara as células da tabela:

1. Cenário I: `md dp[i][w] = dp[i-1][w]`;
2. Cenário II: `md dp[i][w] = valor[i] + dp[i-1][w - peso[i]]`.

Por fim, para tomar a decisão de levar ou não levar o item, você comparou qual cenário daria o *maior* valor. 

Essa lógica pode ser expressa de forma geral pela seguinte fórmula:

$$
dp[i][w]=max(dp[i−1][w], valor[i] + dp[i−1][w−peso[i]])
$$

- Note que se o item não cabe na mochila, não comparamos `md dp[i−1][w − peso[i]]` (isso estaria fora do range da tabela). Nesse caso, consideramos `md dp[i][w] = dp[i−1][w]`.

- Além disso, é importante destacar que, para a linha i, consideramos **todos os itens até o item i**, ou seja, essa solução considera **subconjuntos de itens**.

**Reconstrução da solução ótima**

Ok, nós construímos toda a tabela `md dp` com programação dinâmica e descobrimos qual é o valor máximo que conseguimos carregar na mochila — e ele estará na célula **`md dp[n][w]`**.

Se nossos itens forem:

| Item | Peso | Valor |
| ---- | ---- | ----- |
| A    | 2    | 3     |
| B    | 3    | 5     |
| C    | 4    | 9     |

Teremos a seguinte tabela:

| Item | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - |     0 |     0 |      0 |     0 |     0 |     0 |
|    A |     0 |     0 |      3 |     3 |     3 |     3 |
|    B |     0 |     0 |      3 |     5 |     5 |     8 |
|    C |     0 |     0 |      3 |     5 |     9 |     9 |

Mas... quais itens exatamente compõem essa solução?

Para descobrir isso, vamos fazer o caminho inverso na tabela, analisando as decisões que levaram até esse valor.

??? Checkpoint
Comece partindo da última célula da tabela (`md dp[3][5] = 9`). Com que célula você deve comparar o valor para descobrir se o item C foi escolhido? Como saber se o item foi escolhido?
::: Gabarito
Devemos comparar o valor de `md dp[3][5] = 9` com a célula acima (`md dp[2][5] = 8`). Como o valor mudou, sabemos que C foi escolhido. 
???

??? Checkpoint
E agora como descobrir, a partir da tabela, se o item B foi ou não foi levado? Quais células devemos comparar?
::: Dica 1
Se sabemos que o item C foi levado, precisamos reduzir a capacidade da mochila para a capacidade restante. 
:::
::: Dica 2
Após reduzir a capacidade da mochila, devemos olhar para a coluna w = w - peso[item escolhido]. Nesse exemplo, w = 5 - 4 = 1.
:::
::: Gabarito
Devemos comparar as células `md dp[2][1] = 0` e `md dp[1][1] = 0`. Como o valor não mudou, o item B não foi escolhido.
:::
???

Por fim, como o item B não foi escolhido, para saber se o item A foi escolhido basta comparar o valor da célula `md dp[1][1]` com `md dp[0][1]`. Como o valor não mudou, o item A também não foi escolhido. 

Com isso, conseguimos chegar em um procedimento para a reconstrução da solução ótima. Partimos da última célula da tabela, `md dp[n][w]`, e vamos subindo, linha por linha. A cada passo, comparamos o valor atual com o valor logo acima:

1. Se `md dp[i][w]` = `md dp[i-1][w]`, o item i não foi incluído na mochila.
2. Se `md dp[i][w]` ≠ `md dp[i-1][w]`, o item i foi incluído. Nesse caso:

   - Adicionamos o item i à lista de itens escolhidos;
   - Reduzimos a capacidade restante da mochila em peso[i] (ou seja, w = w - peso[i]);
   - Subimos para a linha anterior: i = i - 1;
   - Repetimos o processo até i = 0 ou w = 0.

??? Checkpoint

Para os seguintes itens e tabela, tente fazer a reconstrução da solução ótima e descubra quais itens foram incluídos.

| Item | Peso | Valor |
| ---- | ---- | ----- |
| A    | 1    | 4     |
| B    | 2    | 3     |
| C    | 3    | 6     |

Teremos a seguinte tabela:

| Item | w = 0 | w = 1 | w =  2 | w = 3 | w = 4 | w = 5 |
| ---: | ----: | ----: | -----: | ----: | ----: | ----: |
|    - |     0 |     0 |      0 |     0 |     0 |     0 |
|    A |     0 |     4 |      4 |     4 |     4 |     4 |
|    B |     0 |     4 |      4 |     7 |     7 |     7 |
|    C |     0 |     4 |      4 |     7 |    10 |    10 |
::: Gabarito

Iniciamos em `md dp[3][5] = 10`

- Comparar `md dp[3][5] = 10` com `md dp[2][5] = 7`→ Como são diferentes, o item 3 ( C ) foi incluído → Atualiza: w = 5 - 3 = 2, i = 2

- Comparar `md dp[2][2] = 4 `com `md dp[1][2] = 4`→ Como são iguais, o item 2 (B) não foi incluído → Mantém: w = 2, i = 1

- Comparar `md dp[1][2] = 4 `com `md dp[0][2] = 0` → Como são diferentes, o item 1 (A) foi incluído → Atualiza: w = 2 - 1 = 1, i = 0 → fim da reconstrução

Portanto:

Itens incluídos: **A** e **C**
:::
???

A abordagem de programação dinâmica para o problema da mochila binária nos permite encontrar a melhor solução possível sem precisar testar todas as combinações. Em vez disso, vamos construindo respostas parciais, armazenando os melhores valores obtidos para cada quantidade de itens e para cada capacidade da mochila.

Ao final do preenchimento da tabela, conseguimos recuperar o valor máximo que pode ser carregado e, com a reconstrução da solução ótima, também conseguimos descobrir quais itens foram escolhidos para compor essa solução.

??? Checkpoint

Já vimos que esse método permite a resolução do problema a partir de subproblemas menores. Ou seja, não precisamos testar todas as combinações de itens. Explique por que isso é uma melhoria em relação à complexidade de tempo quando comparado com o método da força bruta. 

::: Gabarito

O método da força bruta testa todas as combinações possíveis de itens, o que resulta em uma complexidade de tempo exponencial:

$$
O (2^n)
$$


Isso ocorre porque, para cada item, existem duas escolhas: incluí-lo ou não.

Já a programação dinâmica evita repetir os mesmos cálculos, resolvendo cada subproblema uma única vez e armazenando seus resultados em uma tabela. No caso da mochila binária com n itens e capacidade W, a complexidade de tempo é:
$$
O(n⋅W)
$$

Essa abordagem é muito mais eficiente, especialmente quando n é grande, pois transforma um problema exponencial em um problema polinomial, aproveitando a sobreposição de subproblemas e o princípio da optimalidade.

:::

???

Apesar de ser um pouco mais trabalhoso de implementar, esse método tem uma eficiência muito superior à força bruta e sempre garante as respostas corretas. Suas principais vantagens são:

1. Reaproveita subsoluções. Ou seja, se você quiser adicionar mais um item, basta adicionar uma linha na tabela, sem precisar refazer todos os cálculos;
2. Funciona bem para capacidades e quantidades de itens moderadas;
3. Sempre encontra a melhor combinação possível, ou seja, o valor máximo.
4. Tem complexidade de tempo e memória *O(n⋅W)*, o que é suficiente para a maioria dos casos práticos. Para valores muito grandes, existem otimizações que reduzem o uso de memória — mas o raciocínio por trás continua sendo o mesmo.

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