# 02b — Introdução à Linguagem C para Programadores Python

Este material apresenta conceitos fundamentais da linguagem **C** para estudantes que já possuem experiência com **Python**, mas ainda não tiveram contato aprofundado com elementos importantes de C, como:

* tipos de dados;
* declaração de variáveis;
* funções;
* passagem de parâmetros;
* arrays;
* strings;
* ponteiros;
* `struct`;
* alocação dinâmica de memória;
* manipulação de arquivos;
* organização de memória;
* diferenças entre passagem por valor e manipulação por referência.

O objetivo não é apresentar toda a linguagem C, mas fornecer a base necessária para compreender os exemplos utilizados posteriormente na disciplina de **Sistemas Operacionais**.

---

# 1. Python × C

Python e C permitem resolver muitos dos mesmos problemas, mas possuem filosofias bastante diferentes.

Considere este código Python:

```python
x = 10
nome = "Linux"

print(x)
print(nome)
```

Em C:

```c
#include <stdio.h>

int main(void) {
    int x = 10;
    char nome[] = "Linux";

    printf("%d\n", x);
    printf("%s\n", nome);

    return 0;
}
```

Ou compilando diretamente do **bash**:

```bash
cat <<'EOF' | gcc -x c - -o programa
#include <stdio.h>

int main(void) {
    int x = 10;
    char nome[] = "Linux";

    printf("%d\n", x);
    printf("%s\n", nome);

    return 0;
}
EOF
```

Algumas diferenças aparecem imediatamente:

| Python                                | C                                                  |
| ------------------------------------- | -------------------------------------------------- |
| Tipagem dinâmica                      | Tipagem estática                                   |
| Não exige declaração prévia do tipo   | Tipo deve ser declarado                            |
| Blocos definidos por indentação       | Blocos definidos por `{ }`                         |
| Gerenciamento automático de memória   | Memória pode ser gerenciada manualmente            |
| Strings são objetos                   | Strings são arrays de `char`                       |
| Listas podem crescer dinamicamente    | Arrays possuem tamanho definido                    |
| Objetos escondem endereços de memória | Ponteiros permitem manipular endereços diretamente |

Essa maior proximidade com a memória e com o hardware torna C particularmente importante no estudo de Sistemas Operacionais.

---

# 2. Tipos Básicos

Em Python:

```python
idade = 20
altura = 1.75
letra = "A"
```

Em C, precisamos informar o tipo:

```c
int idade = 20;
float altura = 1.75;
char letra = 'A';
```

Alguns tipos comuns:

```text
char
int
float
double
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    int idade = 20;
    float altura = 1.75f;
    double pi = 3.1415926535;
    char letra = 'A';

    printf("Idade: %d\n", idade);
    printf("Altura: %.2f\n", altura);
    printf("Pi: %.5f\n", pi);
    printf("Letra: %c\n", letra);

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra tipos.c -o tipos
```

Execute:

```bash
./tipos
```

---

# 3. O tamanho dos tipos

C permite descobrir quanto espaço um tipo ocupa na memória com:

```c
sizeof
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    printf("char:   %zu byte(s)\n", sizeof(char));
    printf("int:    %zu byte(s)\n", sizeof(int));
    printf("float:  %zu byte(s)\n", sizeof(float));
    printf("double: %zu byte(s)\n", sizeof(double));

    return 0;
}
```

Execute:

```bash
gcc sizeof.c -o sizeof
./sizeof
```

Em uma máquina típica, poderíamos encontrar:

```text
char:   1 byte(s)
int:    4 byte(s)
float:  4 byte(s)
double: 8 byte(s)
```

Os tamanhos podem depender da plataforma e da arquitetura.

---

# 4. Variáveis precisam ser declaradas

Em Python podemos escrever:

```python
x = 10
x = "Linux"
```

A mesma variável pode passar a referenciar um objeto de outro tipo.

Em C:

```c
int x = 10;
```

Depois disso não podemos escrever:

```c
x = "Linux";
```

porque `x` foi declarada como `int`.

A informação de tipo faz parte da forma como o compilador organiza e interpreta a memória.

---

# 5. Operadores

Os operadores básicos são bastante semelhantes aos de Python.

```c
a + b
a - b
a * b
a / b
a % b
```

Entretanto, observe a divisão entre inteiros:

```c
#include <stdio.h>

int main(void) {
    int a = 5;
    int b = 2;

    printf("%d\n", a / b);

    return 0;
}
```

Resultado:

```text
2
```

Não:

```text
2.5
```

Para obter uma divisão com ponto flutuante:

```c
printf("%.2f\n", (double) a / b);
```

Aqui ocorre uma **conversão explícita de tipo**, chamada de *cast*:

```c
(double) a
```

---

# 6. Estruturas condicionais

Python:

```python
idade = 20

if idade >= 18:
    print("Maior de idade")
else:
    print("Menor de idade")
```

C:

```c
#include <stdio.h>

int main(void) {
    int idade = 20;

    if (idade >= 18) {
        printf("Maior de idade\n");
    } else {
        printf("Menor de idade\n");
    }

    return 0;
}
```

Observe:

* a condição fica entre `()`;
* o bloco fica entre `{ }`;
* as instruções normalmente terminam com `;`.

---

# 7. Comparação: `=` × `==`

Esse é um erro clássico de iniciantes em C.

```c
x = 10;
```

significa:

> atribuir `10` a `x`.

Já:

```c
x == 10
```

significa:

> verificar se `x` é igual a `10`.

Portanto:

```c
if (x == 10) {
    printf("x vale 10\n");
}
```

---

# 8. Laços

## `for`

Python:

```python
for i in range(5):
    print(i)
```

C:

```c
#include <stdio.h>

int main(void) {
    for (int i = 0; i < 5; i++) {
        printf("%d\n", i);
    }

    return 0;
}
```

A estrutura:

```c
for (int i = 0; i < 5; i++)
```

possui três partes:

```text
inicialização
condição
incremento
```

---

# 9. `while`

Python:

```python
i = 0

while i < 5:
    print(i)
    i += 1
```

C:

```c
int i = 0;

while (i < 5) {
    printf("%d\n", i);
    i++;
}
```

Em C:

```c
i++;
```

é equivalente a:

```c
i = i + 1;
```

---

# 10. Funções

Em Python:

```python
def soma(a, b):
    return a + b
```

Em C precisamos especificar:

1. tipo retornado;
2. nome da função;
3. tipo de cada parâmetro.

```c
int soma(int a, int b) {
    return a + b;
}
```

Programa completo:

```c
#include <stdio.h>

int soma(int a, int b) {
    return a + b;
}

int main(void) {
    int resultado = soma(10, 20);

    printf("Resultado: %d\n", resultado);

    return 0;
}
```

---

# 11. Anatomia de uma função

Considere:

```c
int soma(int a, int b)
```

Temos:

```text
int          → tipo retornado
soma         → nome da função
int a        → primeiro parâmetro
int b        → segundo parâmetro
```

Se uma função não retorna valor:

```c
void mensagem(void) {
    printf("Sistemas Operacionais\n");
}
```

Nesse caso:

```text
void
```

indica ausência de valor de retorno.

---

# 12. Protótipos de funções

Considere:

```c
#include <stdio.h>

int soma(int a, int b);

int main(void) {
    printf("%d\n", soma(10, 20));
    return 0;
}

int soma(int a, int b) {
    return a + b;
}
```

Esta linha:

```c
int soma(int a, int b);
```

é o **protótipo da função**.

Ela informa ao compilador que existe uma função:

```text
soma
```

que recebe:

```text
int
int
```

e retorna:

```text
int
```

Essa separação será importante ao trabalhar com arquivos `.h`.

---

# 13. Escopo de variáveis

Considere:

```c
void funcao(void) {
    int x = 10;
}
```

A variável `x` existe apenas dentro da função.

Isso é chamado de **variável local**.

Também podemos declarar:

```c
int contador = 0;

int main(void) {
    ...
}
```

fora das funções.

Nesse caso temos uma **variável global**.

Sempre que possível, prefira limitar o escopo das variáveis.

---

# 14. Passagem de parâmetros por valor

Considere:

```c
#include <stdio.h>

void alterar(int x) {
    x = 100;
}

int main(void) {
    int numero = 10;

    alterar(numero);

    printf("%d\n", numero);

    return 0;
}
```

Qual será a saída?

```text
10
```

Não:

```text
100
```

A função recebeu uma **cópia** do valor.

Visualmente:

```text
main:

numero
┌──────┐
│  10  │
└──────┘


alterar:

x
┌──────┐
│  10  │
└──────┘
```

Alterar `x` não altera `numero`.

Para alterar diretamente a variável original, precisaremos utilizar **ponteiros**.

---

# 15. Arrays

Python:

```python
numeros = [10, 20, 30, 40]
```

Em C:

```c
int numeros[4] = {10, 20, 30, 40};
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    int numeros[4] = {10, 20, 30, 40};

    printf("%d\n", numeros[0]);
    printf("%d\n", numeros[1]);

    return 0;
}
```

Assim como em Python, os índices começam em zero.

---

# 16. Percorrendo um array

```c
#include <stdio.h>

int main(void) {
    int numeros[] = {10, 20, 30, 40, 50};

    for (int i = 0; i < 5; i++) {
        printf("numeros[%d] = %d\n", i, numeros[i]);
    }

    return 0;
}
```

Um problema importante é que o array não armazena automaticamente, de forma acessível pela variável, quantos elementos possui.

Podemos calcular:

```c
int quantidade = sizeof(numeros) / sizeof(numeros[0]);
```

Exemplo:

```c
int quantidade = sizeof(numeros) / sizeof(numeros[0]);

for (int i = 0; i < quantidade; i++) {
    printf("%d\n", numeros[i]);
}
```

---

# 17. C não verifica automaticamente os limites do array

Considere:

```c
int numeros[3] = {10, 20, 30};

printf("%d\n", numeros[100]);
```

O índice:

```text
100
```

não existe nesse array.

Python normalmente produziria uma exceção:

```text
IndexError
```

C não é obrigado a verificar isso.

O programa pode:

* imprimir lixo;
* acessar outra região da memória;
* terminar com erro;
* aparentemente funcionar.

Esse comportamento é uma das razões pelas quais programação em C exige maior atenção ao gerenciamento da memória.

---

# 18. Caracteres

Em C, um único caractere é representado pelo tipo:

```c
char
```

Exemplo:

```c
char letra = 'A';
```

Observe o uso de aspas simples.

```c
'A'
```

representa um caractere.

Já:

```c
"A"
```

representa uma string.

---

# 19. Strings em C

Em Python:

```python
nome = "Linux"
```

Em C:

```c
char nome[] = "Linux";
```

Na memória, essa string é aproximadamente:

```text
┌───┬───┬───┬───┬───┬────┐
│ L │ i │ n │ u │ x │ \0 │
└───┴───┴───┴───┴───┴────┘
```

O caractere:

```text
\0
```

é chamado de **terminador nulo**.

Ele marca o fim da string.

---

# 20. Percorrendo uma string

```c
#include <stdio.h>

int main(void) {
    char nome[] = "Linux";

    int i = 0;

    while (nome[i] != '\0') {
        printf("%c\n", nome[i]);
        i++;
    }

    return 0;
}
```

Resultado:

```text
L
i
n
u
x
```

---

# 21. Biblioteca `string.h`

C possui funções para manipulação de strings.

Inclua:

```c
#include <string.h>
```

Exemplo:

```c
#include <stdio.h>
#include <string.h>

int main(void) {
    char texto[] = "Sistemas Operacionais";

    printf("Tamanho: %zu\n", strlen(texto));

    return 0;
}
```

Algumas funções conhecidas:

```text
strlen()
strcmp()
strcpy()
strcat()
```

É importante entender os limites dos buffers ao usar funções de manipulação de strings.

---

# 22. Comparando strings

Em Python:

```python
if nome == "Linux":
    ...
```

Em C, **não devemos comparar strings utilizando `==`**.

Errado:

```c
if (nome == "Linux")
```

Utilize:

```c
strcmp()
```

Exemplo:

```c
#include <stdio.h>
#include <string.h>

int main(void) {
    char nome[] = "Linux";

    if (strcmp(nome, "Linux") == 0) {
        printf("Strings iguais\n");
    }

    return 0;
}
```

---

# 23. Endereços de memória

Uma variável ocupa uma posição na memória.

```c
int numero = 10;
```

Podemos imaginar:

```text
Endereço        Conteúdo

0x7ffd1234      10
```

Para obter o endereço de uma variável utilizamos:

```c
&
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    int numero = 10;

    printf("Valor: %d\n", numero);
    printf("Endereco: %p\n", (void *)&numero);

    return 0;
}
```

O endereço será diferente a cada execução ou ambiente.

---

# 24. Ponteiros

Um **ponteiro** é uma variável que armazena um endereço de memória.

```c
int numero = 10;

int *ponteiro = &numero;
```

Visualmente:

```text
numero
┌─────────┐
│   10    │
└─────────┘
  ▲
  │
  │ endereço
  │
ponteiro
┌──────────────┐
│ 0x7ffd1234   │
└──────────────┘
```

---

# 25. Declarando um ponteiro

```c
int *p;
```

significa:

> `p` é um ponteiro para um `int`.

Podemos atribuir:

```c
int numero = 10;

p = &numero;
```

Ou diretamente:

```c
int *p = &numero;
```

---

# 26. Operadores `&` e `*`

Existem dois operadores fundamentais.

## `&`

Obtém o endereço de uma variável:

```c
&numero
```

## `*`

Acessa o valor armazenado no endereço apontado:

```c
*p
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    int numero = 10;
    int *p = &numero;

    printf("numero = %d\n", numero);
    printf("*p = %d\n", *p);

    return 0;
}
```

Resultado:

```text
numero = 10
*p = 10
```

---

# 27. Alterando uma variável por meio de um ponteiro

```c
#include <stdio.h>

int main(void) {
    int numero = 10;
    int *p = &numero;

    *p = 100;

    printf("%d\n", numero);

    return 0;
}
```

Resultado:

```text
100
```

Isso acontece porque:

```c
*p = 100;
```

significa:

> coloque `100` na posição de memória apontada por `p`.

---

# 28. Função que altera uma variável

Agora podemos corrigir o exemplo de passagem por valor.

```c
#include <stdio.h>

void alterar(int *x) {
    *x = 100;
}

int main(void) {
    int numero = 10;

    alterar(&numero);

    printf("%d\n", numero);

    return 0;
}
```

Resultado:

```text
100
```

Observe:

```c
alterar(&numero);
```

Estamos enviando o **endereço** de `numero`.

A função recebe:

```c
int *x
```

e modifica:

```c
*x
```

---

# 29. Exemplo clássico: `swap`

Em Python podemos escrever:

```python
a, b = b, a
```

Em C, uma função para trocar dois valores pode utilizar ponteiros:

```c
#include <stdio.h>

void swap(int *a, int *b) {
    int temporario = *a;

    *a = *b;
    *b = temporario;
}

int main(void) {
    int x = 10;
    int y = 20;

    swap(&x, &y);

    printf("x = %d\n", x);
    printf("y = %d\n", y);

    return 0;
}
```

Resultado:

```text
x = 20
y = 10
```

---

# 30. Ponteiros e arrays

Arrays e ponteiros possuem uma relação muito próxima em C.

Considere:

```c
int numeros[] = {10, 20, 30};
```

O nome:

```text
numeros
```

em muitas expressões representa o endereço do primeiro elemento.

Assim:

```c
numeros
```

e:

```c
&numeros[0]
```

representam o mesmo endereço inicial.

---

# 31. Aritmética de ponteiros

Considere:

```c
int numeros[] = {10, 20, 30};

int *p = numeros;
```

Podemos acessar:

```c
*p
```

Resultado:

```text
10
```

E:

```c
*(p + 1)
```

Resultado:

```text
20
```

Também:

```c
*(p + 2)
```

Resultado:

```text
30
```

Assim:

```text
numeros[i]
```

é conceitualmente relacionado a:

```text
*(numeros + i)
```

---

# 32. `NULL`

Um ponteiro pode não apontar para nenhum objeto válido.

Para representar isso usamos:

```c
NULL
```

Exemplo:

```c
int *p = NULL;
```

Antes de acessar:

```c
*p
```

devemos garantir que o ponteiro seja válido.

Por exemplo:

```c
if (p != NULL) {
    printf("%d\n", *p);
}
```

Tentar acessar um ponteiro inválido pode provocar um erro como:

```text
Segmentation fault
```

---

# 33. O que é um `Segmentation fault`?

Considere:

```c
int *p = NULL;

*p = 10;
```

O programa está tentando escrever em uma região inválida da memória.

O sistema operacional pode interromper o processo:

```text
Segmentation fault (core dumped)
```

Esse tipo de erro aparecerá novamente durante o estudo de memória e processos.

---

# 34. `struct`

Python permite representar objetos com classes, dicionários, `dataclass`, entre outras estruturas.

Em C podemos agrupar diferentes campos utilizando:

```c
struct
```

Exemplo:

```c
struct Aluno {
    char nome[50];
    int matricula;
    float nota;
};
```

Criando uma variável:

```c
struct Aluno aluno;
```

---

# 35. Utilizando uma `struct`

```c
#include <stdio.h>
#include <string.h>

struct Aluno {
    char nome[50];
    int matricula;
    float nota;
};

int main(void) {
    struct Aluno aluno;

    strcpy(aluno.nome, "Maria");
    aluno.matricula = 20260001;
    aluno.nota = 9.5;

    printf("Nome: %s\n", aluno.nome);
    printf("Matricula: %d\n", aluno.matricula);
    printf("Nota: %.1f\n", aluno.nota);

    return 0;
}
```

Os campos são acessados utilizando:

```text
.
```

Por exemplo:

```c
aluno.nota
```

---

# 36. Inicializando uma `struct`

Podemos inicializar diretamente:

```c
struct Aluno aluno = {
    "Maria",
    20260001,
    9.5
};
```

Programa:

```c
#include <stdio.h>

struct Aluno {
    char nome[50];
    int matricula;
    float nota;
};

int main(void) {
    struct Aluno aluno = {
        "Maria",
        20260001,
        9.5
    };

    printf("%s\n", aluno.nome);

    return 0;
}
```

---

# 37. `typedef`

É comum utilizar `typedef` para simplificar nomes de tipos.

Em vez de:

```c
struct Aluno aluno;
```

podemos escrever:

```c
typedef struct {
    char nome[50];
    int matricula;
    float nota;
} Aluno;
```

Depois:

```c
Aluno aluno;
```

Exemplo completo:

```c
#include <stdio.h>

typedef struct {
    char nome[50];
    int matricula;
    float nota;
} Aluno;

int main(void) {
    Aluno aluno = {"Maria", 20260001, 9.5};

    printf("%s\n", aluno.nome);

    return 0;
}
```

---

# 38. Ponteiro para `struct`

Considere:

```c
Aluno aluno;
Aluno *p = &aluno;
```

Podemos acessar um campo utilizando:

```c
(*p).nota
```

Porém C fornece uma sintaxe mais conveniente:

```c
p->nota
```

Exemplo:

```c
#include <stdio.h>

typedef struct {
    int matricula;
    float nota;
} Aluno;

int main(void) {
    Aluno aluno = {20260001, 8.5};

    Aluno *p = &aluno;

    printf("%d\n", p->matricula);
    printf("%.1f\n", p->nota);

    return 0;
}
```

O operador:

```text
->
```

será muito comum em estruturas de dados e APIs do sistema operacional.

---

# 39. Organização básica da memória de um processo

Uma visão simplificada da memória de um programa pode ser:

```text
Endereços maiores
      ▲
      │
┌───────────────────┐
│      Stack        │
│ variáveis locais  │
│ chamadas funções  │
├───────────────────┤
│                   │
│                   │
├───────────────────┤
│       Heap        │
│ memória dinâmica  │
├───────────────────┤
│ Dados globais     │
├───────────────────┤
│ Código            │
└───────────────────┘
      │
      ▼
Endereços menores
```

Essa organização será aprofundada posteriormente.

Por enquanto, é importante diferenciar:

```text
Stack
```

e:

```text
Heap
```

---

# 40. Stack

Variáveis locais normalmente possuem armazenamento associado à pilha de execução.

Exemplo:

```c
void funcao(void) {
    int x = 10;
    int y = 20;
}
```

Quando a função termina, essas variáveis deixam de existir.

O gerenciamento normalmente ocorre automaticamente.

---

# 41. Heap

Quando não sabemos antecipadamente quanto espaço será necessário, podemos solicitar memória dinamicamente.

Para isso utilizamos funções de:

```c
stdlib.h
```

principalmente:

```text
malloc()
calloc()
realloc()
free()
```

---

# 42. `malloc`

`malloc()` solicita uma quantidade de bytes ao sistema de gerenciamento de memória do processo.

Exemplo:

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    int *p;

    p = malloc(sizeof(int));

    if (p == NULL) {
        printf("Erro de alocacao\n");
        return 1;
    }

    *p = 42;

    printf("%d\n", *p);

    free(p);

    return 0;
}
```

---

# 43. Entendendo o `malloc`

Nesta linha:

```c
p = malloc(sizeof(int));
```

solicitamos memória suficiente para armazenar um `int`.

Visualmente:

```text
Stack                      Heap

p
┌─────────────┐           ┌─────────┐
│  endereço ──┼──────────►│   ?     │
└─────────────┘           └─────────┘
```

Depois:

```c
*p = 42;
```

temos:

```text
Stack                      Heap

p
┌─────────────┐           ┌─────────┐
│  endereço ──┼──────────►│   42    │
└─────────────┘           └─────────┘
```

---

# 44. Sempre verifique `malloc`

A alocação pode falhar.

Portanto:

```c
int *p = malloc(sizeof(int));

if (p == NULL) {
    return 1;
}
```

é uma prática importante.

---

# 45. `free`

Memória obtida dinamicamente deve ser liberada quando não for mais necessária:

```c
free(p);
```

Caso contrário, podemos criar um:

```text
memory leak
```

ou **vazamento de memória**.

---

# 46. Vazamento de memória

Considere:

```c
int *p = malloc(1000);

p = NULL;
```

O endereço da memória alocada foi perdido antes de chamar:

```c
free()
```

O programa não possui mais uma referência para aquela região.

Em aplicações que executam por longos períodos, vazamentos podem consumir quantidades crescentes de memória.

---

# 47. Array alocado dinamicamente

Imagine que o usuário informa quantos números deseja armazenar.

Em Python:

```python
n = int(input())
numeros = [0] * n
```

Em C:

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    int n;

    printf("Quantidade: ");
    scanf("%d", &n);

    int *numeros = malloc(n * sizeof(int));

    if (numeros == NULL) {
        return 1;
    }

    for (int i = 0; i < n; i++) {
        numeros[i] = i * 10;
    }

    for (int i = 0; i < n; i++) {
        printf("%d\n", numeros[i]);
    }

    free(numeros);

    return 0;
}
```

Observe:

```c
n * sizeof(int)
```

A quantidade de memória depende do número de elementos solicitado.

---

# 48. Por que `scanf` usa `&`?

Considere:

```c
int idade;

scanf("%d", &idade);
```

A função precisa **alterar a variável `idade`**.

Por isso não recebe simplesmente seu valor.

Ela recebe:

```c
&idade
```

ou seja, o endereço onde o valor deverá ser armazenado.

Essa é uma aplicação prática direta de ponteiros.

---

# 49. `calloc`

Outra função é:

```c
calloc()
```

Exemplo:

```c
int *numeros = calloc(10, sizeof(int));
```

Ela reserva espaço para:

```text
10 elementos
```

cada um com:

```text
sizeof(int)
```

bytes.

Uma diferença importante é que a memória retornada por `calloc()` é inicializada com bits zero.

---

# 50. `realloc`

Podemos alterar o tamanho de uma região previamente alocada:

```c
int *novo = realloc(numeros, novo_tamanho * sizeof(int));
```

Uma forma mais segura é:

```c
int *novo = realloc(numeros, novo_tamanho * sizeof(int));

if (novo != NULL) {
    numeros = novo;
}
```

Isso evita perder o ponteiro original caso a realocação falhe.

---

# 51. Erros comuns com ponteiros

Alguns problemas clássicos:

### Ponteiro não inicializado

```c
int *p;

*p = 10;
```

### Ponteiro nulo

```c
int *p = NULL;

*p = 10;
```

### Uso depois de `free`

```c
free(p);

printf("%d\n", *p);
```

### `free` duplicado

```c
free(p);
free(p);
```

### Acesso fora dos limites

```c
int *p = malloc(3 * sizeof(int));

p[100] = 10;
```

Esses erros podem causar comportamento indefinido.

---

# 52. Boa prática após `free`

Após:

```c
free(p);
```

é comum escrever:

```c
p = NULL;
```

Assim:

```c
free(p);
p = NULL;
```

não elimina todos os possíveis problemas, mas pode ajudar a evitar o uso acidental de um ponteiro que já foi liberado.

---

# 53. Arquivos em C

Python:

```python
with open("dados.txt", "w") as arquivo:
    arquivo.write("Linux\n")
```

Em C, a biblioteca padrão utiliza:

```c
FILE *
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    FILE *arquivo;

    arquivo = fopen("dados.txt", "w");

    if (arquivo == NULL) {
        printf("Erro ao abrir arquivo\n");
        return 1;
    }

    fprintf(arquivo, "Linux\n");

    fclose(arquivo);

    return 0;
}
```

---

# 54. `FILE *`

Nesta declaração:

```c
FILE *arquivo;
```

`arquivo` é um ponteiro para uma estrutura interna utilizada pela biblioteca C para representar um fluxo de arquivo.

A função:

```c
fopen()
```

retorna esse ponteiro.

Se ocorrer erro:

```c
arquivo == NULL
```

---

# 55. Modos de abertura

Alguns modos importantes:

```text
"r"   leitura
"w"   escrita
"a"   adicionar ao final
```

Também existem modos binários:

```text
"rb"
"wb"
"ab"
```

Exemplo:

```c
fopen("dados.txt", "r");
```

abre para leitura.

---

# 56. Escrevendo em um arquivo

```c
#include <stdio.h>

int main(void) {
    FILE *arquivo = fopen("dados.txt", "w");

    if (arquivo == NULL) {
        return 1;
    }

    fprintf(arquivo, "Sistemas Operacionais\n");
    fprintf(arquivo, "Linux\n");
    fprintf(arquivo, "C\n");

    fclose(arquivo);

    return 0;
}
```

Compile e execute:

```bash
gcc escrita.c -o escrita
./escrita
```

Depois:

```bash
cat dados.txt
```

---

# 57. Lendo um arquivo linha por linha

Podemos utilizar:

```c
fgets()
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    FILE *arquivo = fopen("dados.txt", "r");

    if (arquivo == NULL) {
        return 1;
    }

    char linha[100];

    while (fgets(linha, sizeof(linha), arquivo) != NULL) {
        printf("%s", linha);
    }

    fclose(arquivo);

    return 0;
}
```

---

# 58. Gravando estruturas em formato texto

Considere:

```c
typedef struct {
    char nome[50];
    int matricula;
    float nota;
} Aluno;
```

Podemos gravar:

```c
fprintf(
    arquivo,
    "%s;%d;%.1f\n",
    aluno.nome,
    aluno.matricula,
    aluno.nota
);
```

Resultado no arquivo:

```text
Maria;20260001;9.5
```

---

# 59. Arquivos binários

Também podemos escrever diretamente blocos de memória.

Exemplo:

```c
#include <stdio.h>

typedef struct {
    int matricula;
    float nota;
} Aluno;

int main(void) {
    Aluno aluno = {20260001, 9.5};

    FILE *arquivo = fopen("aluno.bin", "wb");

    if (arquivo == NULL) {
        return 1;
    }

    fwrite(&aluno, sizeof(Aluno), 1, arquivo);

    fclose(arquivo);

    return 0;
}
```

Observe:

```c
&aluno
```

Mais uma vez estamos fornecendo um endereço de memória.

---

# 60. Lendo arquivo binário

```c
#include <stdio.h>

typedef struct {
    int matricula;
    float nota;
} Aluno;

int main(void) {
    Aluno aluno;

    FILE *arquivo = fopen("aluno.bin", "rb");

    if (arquivo == NULL) {
        return 1;
    }

    fread(&aluno, sizeof(Aluno), 1, arquivo);

    fclose(arquivo);

    printf("Matricula: %d\n", aluno.matricula);
    printf("Nota: %.1f\n", aluno.nota);

    return 0;
}
```

---

# 61. Entrada padrão, saída padrão e erro padrão

Um processo normalmente possui três fluxos importantes:

```text
stdin
stdout
stderr
```

Correspondendo a:

```text
entrada padrão
saída padrão
saída de erro
```

Quando fazemos:

```c
printf("Hello\n");
```

estamos escrevendo em:

```text
stdout
```

Também poderíamos escrever explicitamente:

```c
fprintf(stdout, "Hello\n");
```

Para erros:

```c
fprintf(stderr, "Erro ao abrir arquivo\n");
```

---

# 62. Relação com o terminal

Execute:

```bash
./programa
```

A saída padrão normalmente aparece no terminal.

Mas podemos redirecioná-la:

```bash
./programa > saida.txt
```

Agora:

```bash
cat saida.txt
```

Essa relação entre:

* processos;
* arquivos;
* descritores;
* entrada e saída;

será central no estudo de Sistemas Operacionais.

---

# 63. `const`

Quando um valor não deve ser modificado, podemos utilizar:

```c
const
```

Exemplo:

```c
const int MAXIMO = 100;
```

Também é bastante comum com ponteiros e strings:

```c
void imprimir(const char *texto) {
    printf("%s\n", texto);
}
```

Aqui estamos indicando que a função não deve modificar os caracteres apontados por `texto`.

---

# 64. `enum`

Quando um conjunto limitado de valores possui significado, podemos usar:

```c
enum
```

Exemplo:

```c
enum Estado {
    PRONTO,
    EXECUTANDO,
    BLOQUEADO
};
```

Depois:

```c
enum Estado estado = PRONTO;
```

Isso será útil para representar conceitos como estados de processos.

---

# 65. Exemplo com `enum`

```c
#include <stdio.h>

typedef enum {
    PRONTO,
    EXECUTANDO,
    BLOQUEADO
} Estado;

int main(void) {
    Estado estado = EXECUTANDO;

    if (estado == EXECUTANDO) {
        printf("Processo em execucao\n");
    }

    return 0;
}
```

---

# 66. Operadores lógicos

Em Python:

```python
if x > 0 and x < 10:
```

Em C:

```c
if (x > 0 && x < 10)
```

Correspondências:

```text
Python     C

and        &&
or         ||
not        !
```

Exemplo:

```c
if (idade >= 18 && possui_documento) {
    ...
}
```

---

# 67. Valores booleanos

Tradicionalmente C interpreta:

```text
0      → falso
não 0  → verdadeiro
```

Também podemos utilizar:

```c
#include <stdbool.h>
```

Exemplo:

```c
#include <stdio.h>
#include <stdbool.h>

int main(void) {
    bool ativo = true;

    if (ativo) {
        printf("Ativo\n");
    }

    return 0;
}
```

---

# 68. Incremento e decremento

C oferece:

```c
i++;
```

e:

```c
i--;
```

Correspondendo aproximadamente a:

```c
i = i + 1;
```

e:

```c
i = i - 1;
```

Também existem:

```c
++i;
--i;
```

A diferença entre pré e pós-incremento importa quando utilizados dentro de expressões.

Para código introdutório, prefira expressões simples e claras.

---

# 69. Operadores bit a bit

Como C é muito utilizada em programação de sistemas, operadores sobre bits são importantes.

```text
&   AND
|   OR
^   XOR
~   NOT
<<  deslocamento à esquerda
>>  deslocamento à direita
```

Exemplo:

```c
#include <stdio.h>

int main(void) {
    unsigned int x = 5;

    printf("%u\n", x << 1);

    return 0;
}
```

Como:

```text
5 = 00000101
```

um deslocamento:

```text
00000101 << 1
```

produz:

```text
00001010
```

ou:

```text
10
```

Esses operadores aparecerão em permissões, flags e interfaces de baixo nível.

---

# 70. Cabeçalhos importantes

Alguns cabeçalhos da biblioteca padrão aparecerão frequentemente:

```c
#include <stdio.h>
```

Entrada e saída.

```c
#include <stdlib.h>
```

Alocação de memória, conversões e outras funções utilitárias.

```c
#include <string.h>
```

Manipulação de strings e memória.

```c
#include <stdbool.h>
```

Tipo booleano.

```c
#include <stdint.h>
```

Tipos inteiros com tamanho explícito.

---

# 71. Tipos inteiros de tamanho explícito

Em programação de baixo nível pode ser útil especificar exatamente o tamanho de um inteiro.

Utilize:

```c
#include <stdint.h>
```

Exemplos:

```c
int8_t
uint8_t

int16_t
uint16_t

int32_t
uint32_t

int64_t
uint64_t
```

O prefixo:

```text
u
```

indica um tipo **unsigned**, ou seja, sem sinal.

---

# 72. Um exemplo integrando os conceitos

Crie:

```text
alunos.c
```

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int matricula;
    float nota;
} Aluno;

int main(void) {
    int quantidade;

    printf("Quantidade de alunos: ");

    if (scanf("%d", &quantidade) != 1 || quantidade <= 0) {
        fprintf(stderr, "Quantidade invalida\n");
        return 1;
    }

    Aluno *alunos = malloc(quantidade * sizeof(Aluno));

    if (alunos == NULL) {
        fprintf(stderr, "Erro de memoria\n");
        return 1;
    }

    for (int i = 0; i < quantidade; i++) {
        printf("Matricula: ");
        scanf("%d", &alunos[i].matricula);

        printf("Nota: ");
        scanf("%f", &alunos[i].nota);
    }

    for (int i = 0; i < quantidade; i++) {
        printf(
            "%d: %.1f\n",
            alunos[i].matricula,
            alunos[i].nota
        );
    }

    free(alunos);

    return 0;
}
```

Esse exemplo reúne:

* funções;
* entrada e saída;
* `struct`;
* arrays;
* ponteiros;
* alocação dinâmica;
* acesso à memória;
* validação simples de erros.

---

# 73. Compile com avisos habilitados

Durante os exercícios, utilize preferencialmente:

```bash
gcc -Wall -Wextra programa.c -o programa
```

Também podemos adicionar:

```bash
-Wpedantic
```

Assim:

```bash
gcc -Wall -Wextra -Wpedantic programa.c -o programa
```

O compilador passa a ajudar na identificação de vários problemas.

---

# 74. Ferramenta útil: AddressSanitizer

Erros de memória nem sempre são simples de encontrar.

O GCC pode instrumentar o programa utilizando:

```bash
-fsanitize=address
```

Exemplo:

```bash
gcc -Wall -Wextra -fsanitize=address programa.c -o programa
```

Depois:

```bash
./programa
```

O AddressSanitizer pode ajudar a identificar:

* acesso fora de arrays;
* uso de memória depois de `free`;
* alguns vazamentos;
* acessos inválidos.

Isso é especialmente útil durante o aprendizado de ponteiros.

---

# 75. Exemplo propositalmente incorreto

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    int *numeros = malloc(3 * sizeof(int));

    numeros[10] = 100;

    printf("%d\n", numeros[10]);

    free(numeros);

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra -fsanitize=address erro.c -o erro
```

Execute:

```bash
./erro
```

Observe as informações fornecidas pela ferramenta.

---

# 76. Python esconde detalhes que C expõe

Em Python:

```python
lista = [10, 20, 30]

lista.append(40)
```

O interpretador cuida de:

* reservar memória;
* verificar se há espaço;
* ampliar a região quando necessário;
* armazenar os elementos;
* liberar memória futuramente.

Em C, essas decisões podem precisar ser tomadas explicitamente.

Por exemplo:

```c
int *lista = malloc(3 * sizeof(int));
```

Depois, para aumentar:

```c
int *nova = realloc(lista, 4 * sizeof(int));
```

Essa diferença é particularmente importante para Sistemas Operacionais porque torna visíveis conceitos que Python normalmente abstrai.

---

# 77. Python e C: equivalências úteis

| Conceito                  | Python                        | C                              |
| ------------------------- | ----------------------------- | ------------------------------ |
| inteiro                   | `x = 10`                      | `int x = 10;`                  |
| ponto flutuante           | `x = 3.14`                    | `double x = 3.14;`             |
| caractere                 | string de tamanho 1           | `char c = 'A';`                |
| string                    | `str`                         | array de `char`                |
| lista                     | `list`                        | array / memória dinâmica       |
| dicionário/objeto simples | `dict` / classe               | `struct`                       |
| função                    | `def`                         | tipo + nome + parâmetros       |
| valor ausente             | `None`                        | frequentemente `NULL`          |
| arquivo                   | objeto retornado por `open()` | `FILE *`                       |
| memória                   | automática                    | automática + manual            |
| exceções                  | `try/except`                  | normalmente códigos de retorno |
| referência a objetos      | transparente                  | ponteiros explícitos           |

---

# 78. Exercício 1 — Funções

Implemente as funções:

```c
int soma(int a, int b);
int maior(int a, int b);
int quadrado(int x);
```

Teste todas no `main`.

Exemplo:

```text
soma(10, 20) = 30
maior(5, 8) = 8
quadrado(4) = 16
```

---

# 79. Exercício 2 — Array

Crie um programa contendo:

```c
int numeros[] = {5, 8, 3, 10, 7};
```

Calcule:

* soma;
* média;
* maior valor;
* menor valor.

Não utilize funções prontas para esses cálculos.

---

# 80. Exercício 3 — Strings

Receba um texto pela linha de comando:

```bash
./texto "sistemas operacionais"
```

O programa deverá informar:

```text
Texto: sistemas operacionais
Quantidade de caracteres: 20
```

Depois modifique o programa para contar quantas letras:

```text
a
```

existem no texto.

---

# 81. Exercício 4 — Ponteiros

Crie:

```c
void dobrar(int *valor);
```

A função deverá alterar diretamente a variável recebida.

Exemplo:

```c
int x = 10;

dobrar(&x);

printf("%d\n", x);
```

Resultado:

```text
20
```

---

# 82. Exercício 5 — `swap`

Implemente:

```c
void swap(int *a, int *b);
```

Teste:

```text
Antes:
a = 10
b = 20

Depois:
a = 20
b = 10
```

---

# 83. Exercício 6 — `struct`

Defina:

```c
typedef struct {
    char nome[50];
    int pid;
    int prioridade;
} Processo;
```

Crie três processos e apresente suas informações.

Exemplo:

```text
Processo: navegador
PID: 101
Prioridade: 10
```

Esse exercício introduz uma estrutura que será útil nas próximas aulas.

---

# 84. Exercício 7 — Array de estruturas

Crie:

```c
Processo processos[3];
```

Preencha os dados dos três processos e percorra o array usando um `for`.

Depois determine qual processo possui a maior prioridade.

---

# 85. Exercício 8 — Memória dinâmica

Receba pela linha de comando a quantidade de números:

```bash
./numeros 5
```

Aloque dinamicamente espaço para cinco inteiros.

Preencha:

```text
10
20
30
40
50
```

Mostre todos os valores e libere corretamente a memória.

---

# 86. Exercício 9 — Arquivos

Crie um programa:

```text
log.c
```

que gere:

```text
log.txt
```

contendo:

```text
Programa iniciado
Processando dados
Programa finalizado
```

Depois crie outro programa que leia e exiba esse arquivo.

---

# 87. Exercício 10 — Processando arquivo

Considere um arquivo:

```text
numeros.txt
```

com:

```text
10
20
30
40
50
```

Crie um programa que leia os valores e apresente:

```text
Quantidade: 5
Soma: 150
Media: 30.00
```

---

# 88. Desafio — Mini monitor de processos

Crie uma estrutura:

```c
typedef struct {
    int pid;
    char nome[50];
    float cpu;
    int memoria;
} Processo;
```

O programa deverá:

1. receber a quantidade de processos;
2. alocar dinamicamente um array de `Processo`;
3. preencher os dados;
4. apresentar todos os processos;
5. encontrar o processo com maior uso de CPU;
6. salvar os dados em `processos.txt`;
7. liberar corretamente a memória.

Exemplo de saída:

```text
PID     NOME        CPU     MEMORIA
101     navegador   12.5    350
102     editor       5.8    120
103     servidor    30.2    480

Maior uso de CPU:
PID 103 - servidor - 30.2%
```

Esse exercício combina grande parte dos conceitos apresentados nesta aula.

---

# 89. Checklist conceitual

Antes de avançar para as próximas aulas, certifique-se de compreender a diferença entre:

```text
valor
endereço
ponteiro
```

e entre:

```text
array estático
memória alocada dinamicamente
```

Também deve ser possível explicar:

* para que serve `&`;
* para que serve `*`;
* o que significa `NULL`;
* por que `malloc()` pode retornar `NULL`;
* por que devemos utilizar `free()`;
* o que é uma `struct`;
* a diferença entre `.` e `->`;
* o papel de `FILE *`;
* a diferença entre stack e heap;
* por que acesso inválido à memória pode causar `Segmentation fault`.

---

# 90. Relação com Sistemas Operacionais

Grande parte dos exemplos das próximas aulas utilizará diretamente esses conceitos.

Ao estudar **processos**, poderemos encontrar funções como:

```c
fork()
exec()
wait()
```

Ao estudar **arquivos**, teremos chamadas como:

```c
open()
read()
write()
close()
```

Ao estudar **memória**, serão importantes:

```text
endereços
ponteiros
stack
heap
memória virtual
```

Ao estudar **threads**, teremos APIs que recebem ponteiros e estruturas como parâmetros.

Por isso, a linguagem C funciona nesta disciplina não apenas como uma linguagem de programação, mas também como uma forma de observar mais diretamente a interface entre:

```text
Programa
   │
   ▼
Bibliotecas
   │
   ▼
Chamadas de Sistema
   │
   ▼
Sistema Operacional
   │
   ▼
Hardware
```

---

# 91. Resultado esperado

Ao final deste material complementar, o estudante deverá ser capaz de:

* declarar e utilizar tipos básicos em C;
* escrever funções com parâmetros e valores de retorno;
* compreender passagem de parâmetros por valor;
* manipular arrays;
* compreender como strings são representadas;
* compreender endereços de memória;
* declarar e utilizar ponteiros;
* alterar valores por meio de ponteiros;
* compreender a relação entre arrays e ponteiros;
* criar e utilizar `struct`;
* utilizar ponteiros para estruturas;
* diferenciar stack e heap;
* utilizar `malloc()`, `calloc()`, `realloc()` e `free()`;
* reconhecer erros comuns de memória;
* abrir, ler, escrever e fechar arquivos;
* compreender `stdin`, `stdout` e `stderr`;
* utilizar recursos de C necessários às próximas práticas de Sistemas Operacionais.

---

# Próxima Aula

➡️ [03 — Processos](../03-processos/README.md)

No próximo conteúdo, o foco deixa de ser apenas o **programa armazenado em disco** e passa a ser o **programa em execução**.

Esses conceitos serão utilizados para explorar diretamente recursos oferecidos pelo sistema operacional, incluindo:

* programa × processo;
* PID e PPID;
* estados de processos;
* `ps`;
* `top`;
* `/proc`;
* criação de processos;
* `fork()`;
* `exec()`;
* `wait()`;
* processos pai e filho.
* memória dos processos;
* comunicação com o kernel;
* interfaces disponibilizadas pelo Linux.
