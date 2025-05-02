O Problema da Mochila Binária
======

Subtítulo
---------

Imagine que você está preparando uma mochila para fazer uma trilha. Você tem vários itens disponíveis, cada um com um peso e um valor de utilidade, mas sua mochila, assim como qualquer outra, tem uma capacidade limitada. 

Como escolher os itens para levar de forma a **maximizar o valor total sem ultrapassar o peso máximo**? Esse é o clássico *Problema da Mochila Binária*, que trabalharemos ao longo desse handout.

??? Checkpoint

Para uma `md mochila de 6kg`, qual combinação de itens você escolheria? Por quê?
| Item     | Peso     | Valor    |
|----------|----------|----------|
|Lanterna  |2 kg       | 5        |
|Água      |3 kg       | 10       |
|Comida    |4 kg       |12        |
|Kit de Primeiros Socorros |1 kg | 8       |
|Câmera    |2 kg       |6        |

::: Gabarito
Entre todas, a combinação `md kit de primeiros socorros + comida` é a combinação que apresenta maior valor total ($12 + 8 = 20$) dentro da limitação da mochila ($1 + 4 \leq 6$ kg).
:::

???

Método da Força Bruta
---------

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
O número de combinações cresce exponencialmente. Por exemplo, para 30 itens, teríamos mais de 1 bilhão de combinações. Isso torna essa solução inviável para situações reais com muitos objetos (pense na complexidade dessa algoritmo). 
:::

???

Embora esse método não seja o mais adequado para resolver problemas maiores, ele nos ajuda a ter compreensão do funcionamento e da natureza do problema. A partir dessa visão geral, começamos a entender por que precisamos de algoritmos mais eficientes, que consigam chegar à melhor (ou uma boa) solução sem precisar testar todas as alternativas. A abordagem de força bruta é, portanto, um primeiro passo para avançarmos para estratégias mais inteligentes — e é isso que veremos a seguir!

Método Guloso
---------

O método **guloso** tenta resolver o problema fazendo a escolha que parece melhor naquele momento. Em vez de olhar todas as combinações possíveis (como a força bruta faz), o método guloso decide item por item, com base em um critério que parece vantajoso — a **importância** do item, medida pela razão entre valor e peso:

$$I (importância) = \frac{valor}{peso}$$

A ideia é: se eu puder levar um item leve e muito valioso, talvez ele traga mais benefício do que um item pesado com valor baixo. Assim, ordenamos os itens com base nesse índice, e vamos colocando os que têm maior valor por peso (ou importância) primeiro, até encher a mochila.

Vamos ver um exemplo:

Suponha que temos os seguintes itens, em uma mochila com capacidade de 15kg.

| Item | Peso (kg) | Valor (R\$) | Valor por Peso |
| ---- | --------- | ----------- | -------------- |
| A    | 5         | 10          | 2              |
| B    | 4         | 40          | 10             |
| C    | 6         | 30          | 5              |

Seguimos o **passo a passo**:

1. Ordenamos por valor/peso: B (10), C (5), A (2);

2. Adicionamos primeiro os itens de maior importância:
    - Colocamos o item B (4kg)
    - Depois o item C (6kg)
    - Depois o item A (5kg)

Ao final, teríamos:

* Peso total: 4 + 6 + 5 = 15kg — mochila cheia!
* Valor total: 40 + 30 + 10 = 80

Mas será que o método guloso sempre nos dá a melhor solução? Ou seja, será que sempre teremos o valor máximo que a mochila pode carregar?

??? Checkpoint

Você acha que o método guloso funcionaria para esse caso, com uma mochila de `md capacidade igual a 5kg`?

| Item | Peso (kg) | Valor (R\$) | Valor/Peso |
| ---- | --------- | ----------- | ---------- |
| A    | 2         | 40          | **20**     |
| B    | 3         | 51          | **17**     |
| C    | 5         | 95          | **19**     |



::: Dica 1
Siga o passo a passo, ordenando os itens pela importância. Depois adicione primeiro os itens mais importantes, até atingir a capacidade máxima.
:::

::: Dica 2
Tente testar todas as combinações possíveis e veja se ele realmente escolheu a combinação que maximiza o valor que a mochila carrega.
:::

::: Gabarito
1. Calculamos a importância (razão entre valor e peso) e ordenamos:
    - Item A: 40/2 = 20

    - Item C: 95/5 = 18

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
---------

Imagine que você está resolvendo o problema da mochila e, a cada nova combinação de itens, precisa recalcular tudo do zero. Repetir esse trabalho é cansativo — e ineficiente. É como tentar lembrar um número de telefone toda vez que vai ligar, em vez de simplesmente salvá-lo na agenda.

A programação dinâmica surge para resolver isso. Ela parte do princípio de que problemas grandes podem ser resolvidos a partir de subproblemas menores. Ou seja, se já sabemos a melhor solução para uma mochila de 3kg com 2 itens, podemos reaproveitar essa informação para resolver casos maiores, como 4kg com 3 itens.

No caso da mochila binária, a técnica consiste em preencher uma tabela que guarda, para cada combinação de item e capacidade, qual o melhor valor possível até aquele ponto. Assim, vamos construindo a solução ideal aos poucos, sem precisar recalcular.

**Como funciona a tabela**

Vamos imaginar o problema onde temos a mochila com **`md capacidade = w`** e devemos escolher entre  **`md n itens`**. Para construir a tabela `md dp`, devemos ter:

* n + 1 linhas (uma para cada item, de 0 a n);
* w + 1 colunas (uma para cada capacidade da mochila, de 0 a w).

A célula dp[i][w] armazena o melhor valor total possível usando os primeiros i itens e uma mochila de capacidade w.

* 


Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
