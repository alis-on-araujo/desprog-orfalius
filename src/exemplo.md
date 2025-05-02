O Problema da Mochila Binária
======

Subtítulo
---------

Imagine que você está preparando uma mochila para fazer uma trilha. Você tem vários itens disponíveis, cada um com um peso e um valor de utilidade, mas sua mochila, assim como qualquer outra, tem uma capacidade limitada. 

Como escolher os itens para levar de forma a **maximizar o valor total sem ultrapassar o peso máximo**? O *Problema da Mochila Binária* é um problema clássico de otimização combinatória que consiste em escolher um subconjunto de itens para colocar em uma mochila, respeitando um limite de peso, de modo a maximizar o valor total carregado. Ele recebe esse nome porque, para cada item, você deve tomar uma decisão binária: **ou coloca o item na mochila (1), ou não coloca (0)** — não é permitido fracionar os itens.


## Definição Objetiva do Problema

Dado um conjunto de `n` itens e uma mochila com capacidade máxima `W`, o objetivo é **escolher um subconjunto de itens** que:

1. **Maximize a soma dos valores dos itens escolhidos**;
2. **Tenha soma dos pesos que não ultrapasse a capacidade da mochila**.

Essa é uma típica situação de **trade-off**: quanto mais itens de valor você quiser levar, mais peso sua mochila vai ter — e seu espaço é limitado.

## Definição Detalhada

**Entradas**:  
- Capacidade máxima da mochila: `W` (por exemplo, 6 kg)  
- `n` itens, cada um com:
  * Peso `w[i]`
  * Valor `v[i]`

**Saída desejada**:  
- Conjunto de itens a serem levados

**Restrição**:  
- A soma dos pesos dos itens escolhidos ≤ `W`

**Objetivo**:  
- Maximizar a soma dos valores dos itens escolhidos


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
