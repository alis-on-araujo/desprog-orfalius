Mochila Binária (0‑1 Knapsack)
==============================

Problema
--------

**Entrada**

* `w`, vetor de pesos dos itens;  
* `v`, vetor de valores dos itens (mesmo tamanho de `w`);  
* `n`, número de itens;  
* `C`, capacidade máxima da mochila.

**Saída**

* escolhe subconjunto cujo peso ≤ `C` e valor total é **máximo**;  
* devolve esse valor ótimo (e, opcionalmente, quais itens foram usados).

Ideia
-----

Para cada item há duas escolhas: **pegar** ou **não pegar**.  
A melhor decisão para os *n* itens com capacidade `C` depende apenas:

1. do melhor valor para os *(n‑1)* primeiros itens **sem** pegar o último;  
2. do melhor valor para os *(n‑1)* primeiros itens **pegando** o último  
   e reduzindo a capacidade em `w[n‑1]`.

Guardando essas respostas em uma tabela `(i, P)` evitamos recomputar
sub‑problemas.

Pseudocódigo
------------

*Versão em alto nível*

```

enquanto ainda existirem sub‑problemas não resolvidos
resolva menor sub‑problema e armazene o resultado

```

*Versão em baixo nível (top‑down com memo)*

```
fun solve(i, P):
se i > n      ->  retorna 0
se vis\[i]\[P]  ->  retorna dp\[i]\[P]

```
```
vis[i][P] ← 1
ans ← solve(i+1, P)                    # não pegar
se P ≥ w[i]:
    ans ← máx(ans, solve(i+1, P-w[i]) + v[i])  # pegar
dp[i][P] ← ans
retorna ans
```


*Versão refinada (bottom‑up 2‑D)*

```

para i = 1..n:
para P = 0..C:
dp\[i]\[P] ← dp\[i-1]\[P]                          # não pegar
se w\[i] ≤ P:
cand ← dp\[i-1]\[P-w\[i]] + v\[i]              # pegar
dp\[i]\[P] ← máx(dp\[i]\[P], cand)

````

Implementação em C
------------------

*A lógica do algoritmo está toda na função; o texto abaixo do código explica
cada parte.*

```c
int knapsack(int w[], int v[], int n, int C){
    /* cria tabela (n+1) × (C+1) inicializada em 0 */
    int **dp = malloc((n+1) * sizeof(int*));
    for(int i=0;i<=n;++i){
        dp[i] = calloc(C+1, sizeof(int));
    }

    for(int i=1;i<=n;++i){
        for(int P=0;P<=C;++P){
            int best = dp[i-1][P];                 /* não pegar */
            if(w[i-1] <= P){
                int cand = dp[i-1][P-w[i-1]] + v[i-1];
                if(cand > best) best = cand;       /* pegar */
            }
            dp[i][P] = best;
        }
    }
    int ans = dp[n][C];
    /* libera tabela */
    for(int i=0;i<=n;++i) free(dp[i]);
    free(dp);
    return ans;
}
````

**Explicação**

* Aloca‑se uma matriz onde `dp[i][P]` guarda o melhor valor usando os `i`
  primeiros itens e capacidade `P`.
* Para cada item `i`:

  * copia‑se a melhor solução sem pegá‑lo (`dp[i-1][P]`);
  * se o item cabe, calcula‑se o valor de pegar (`cand`) e mantém‑se o maior.
* A resposta final é `dp[n][C]`, o valor ótimo com todos os itens e capacidade
  cheia.

## Complexidades

* **Tempo** `O(n C)` — dois laços aninhados.
* **Espaço** `O(n C)` — a matriz completa (pode cair para `O(C)` com vetor 1‑D).

## Uso típico

```c
int main(){
    int pesos[]   = {2,3,4,5,3};
    int valores[] = {5,10,12,8,7};
    int n = 5, capacidade = 10;
    printf("Valor ótimo: %d\n",
           knapsack(pesos, valores, n, capacidade));
}
```

Saída esperada: `Valor ótimo: 29` — escolhendo itens de peso 3, 4 e 3.

