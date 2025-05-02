# Implementações do Algoritmo da Mochila Binária

*Recursão com memorização × tabela 2‑D em C, explicadas fora do código.*

::: Resumo desta aula
Não leia se estiver vendo este handout pela primeira vez. Estes resumos servem apenas para consulta rápida depois de concluir todos os checkpoints.
- **[Resumo da Mochila 0‑1](resumo.html)**

:::
## 5. Implementação Recursiva Top‑Down (Memo)

### 5.1 Esqueleto em pseudocódigo

```text
solve(i, P):
    se i > n: retorna 0
    se vis[i][P]: retorna dp[i][P]
    vis[i][P] ← 1
    ans ← solve(i+1, P)
    se P ≥ peso[i]:
        ans ← max(ans, solve(i+1, P-peso[i]) + valor[i])
    dp[i][P] ← ans
    retorna ans
```

??? Checkpoint
Explique por que `vis[i][P]` impede que o algoritmo gere $2^n$ chamadas.

::: Gabarito
Cada par `(i,P)` é guardado após o primeiro cálculo; as chamadas seguintes apenas retornam o valor salvo, evitando recomputação de subárvores.
:::
???

### 5.2 Declarações em C

```c
#define MAXN 110
#define MAXP 100010

int  w[MAXN], v[MAXN];
long long dp[MAXN][MAXP];
char vis[MAXN][MAXP];
int n;
```

### 5.3 Função `solve`

```c
long long solve(int i, int P){
    if(i == n + 1) return 0;
    if(vis[i][P])  return dp[i][P];

    vis[i][P] = 1;
    long long ans = solve(i+1, P);
    if(P >= w[i]){
        long long cand = solve(i+1, P-w[i]) + v[i];
        if(cand > ans) ans = cand;
    }
    return dp[i][P] = ans;
}
```

*O procedimento retorna o melhor valor possível para cada estado `(i,P)`, salvando o resultado na tabela `dp` e marcando o estado como visitado em `vis` para reutilizações futuras.*

??? Checkpoint
Qual a complexidade de **tempo** e **espaço**?

::: Gabarito
Tempo $O(nC)$ porque cada `(i,P)` é avaliado uma vez.
Espaço $O(nC)$ para guardar `dp` e `vis`.
:::
???

---

## 6. Implementação Bottom‑Up (Tabela 2‑D)

### 6.1 Estrutura fundamental

```c
typedef struct {
    int capacity;
    int num_items;
    int *weights;
    int *values;
    int **dp_table;
} knapsack_problem;
```

*A `struct` reúne todos os dados do problema e a matriz `(n+1) × (C+1)` que será preenchida.*

??? Checkpoint
Quanto a matriz ocupa se `n = 200` e `C = 5000`?

::: Gabarito
` 201 × 5001 × 4` bytes ≈ **4,02 MB**.
:::
???

### 6.2 Inicialização da tabela

??? Exercício ✏️
Implemente ` init_dp_zeros(int **dp, int rows, int cols)` para zerar todos os elementos da matriz.

::: Gabarito
```c
static void init_dp_zeros(int **dp,int rows,int cols){
    for(int i=0;i<rows;++i) memset(dp[i],0,cols*sizeof(int));
}
```
:::
???

### 6.3 Preenchendo a tabela dinâmica

```c
void knapsack_solve(knapsack_problem *kp){
    for(int i = 1; i <= kp->num_items; ++i){
        for(int j = 0; j <= kp->capacity; ++j){
            int best = kp->dp_table[i-1][j];
            if(kp->weights[i-1] <= j){
                int cand = kp->dp_table[i-1][j - kp->weights[i-1]] +
                           kp->values[i-1];
                if(cand > best) best = cand;
            }
            kp->dp_table[i][j] = best;
        }
    }
}
```

*Para cada item `i`, a linha `i` deriva da linha `i-1`: se o item cabe, comparamos incluir com não incluir; caso contrário, copiamos o valor anterior.*

### 6.4 Reconstrução do subconjunto

```c
void knapsack_reconstruct(const knapsack_problem *kp, int *chosen){
    int j = kp->capacity;
    for(int i = kp->num_items; i >= 1; --i){
        if(kp->dp_table[i][j] != kp->dp_table[i-1][j]){
            chosen[i-1] = 1;
            j -= kp->weights[i-1];
        } else {
            chosen[i-1] = 0;
        }
    }
}
```

*A descida da última linha até a primeira identifica quais itens alteraram o valor da solução — esses são os selecionados. O vetor `chosen` recebe 1 para itens escolhidos e 0 caso contrário.*

### 6.5 `main` de demonstração

```c
int main(void){
    int w[]={2,3,4,5,3}, v[]={5,10,12,8,7};
    int n=5, C=10;
    knapsack_problem kp;
    knapsack_init(&kp,n,C,w,v);
    knapsack_solve(&kp);

    int *chosen = calloc(n,sizeof(int));
    knapsack_reconstruct(&kp,chosen);

    printf("Valor ótimo: %d\n", kp.dp_table[n][C]);
    // imprimir chosen, liberar memória...
    return 0;
}
```

*O programa monta o problema, resolve, reconstrói a lista de itens e mostra o valor ótimo.*

??? Checkpoint
Quais itens são escolhidos e qual o peso total?

::: Gabarito
`chosen = [0,1,1,0,1]` — itens com pesos 3 + 4 + 3 = 10 kg, valor 29.
:::
