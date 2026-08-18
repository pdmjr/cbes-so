# 02a — Compilando um Código-Fonte em C

Nesta aula será apresentado o processo básico de desenvolvimento de programas em **C** utilizando a linha de comando do Linux.

O objetivo é compreender o caminho entre a criação de um arquivo de código-fonte e a geração de um programa executável, utilizando o compilador **GCC**. Também serão apresentados argumentos de linha de comando e uma introdução à automação da compilação com **Makefile**.

Ao final desta aula, o fluxo básico de desenvolvimento deverá ser familiar:

```text
Código-fonte (.c)
      │
      │ gcc
      ▼
Arquivo executável
      │
      │ execução
      ▼
Programa em funcionamento
```

---

# 1. Preparando o Diretório da Aula

Crie um diretório para os exemplos:

```bash
mkdir -p ~/sistemas-operacionais/02a-compilacao-c
```

Entre no diretório:

```bash
cd ~/sistemas-operacionais/02a-compilacao-c
```

Verifique:

```bash
pwd
```

Uma saída possível é:

```text
/home/aluno/sistemas-operacionais/02a-compilacao-c
```

Durante esta aula, os arquivos serão criados dentro desse diretório.

---

# 2. Estrutura Básica de um Programa em C

Um programa simples em C pode ser escrito da seguinte forma:

```c
#include <stdio.h>

int main(void) {
    printf("Hello, World!\n");

    return 0;
}
```

Salve esse código no arquivo:

```text
hello.c
```

Podemos visualizar o conteúdo com:

```bash
cat hello.c
```

---

# 3. Entendendo o Código

Vamos analisar cada parte do programa.

## 3.1 Inclusão de bibliotecas

```c
#include <stdio.h>
```

A diretiva `#include` solicita ao pré-processador que inclua as definições presentes no arquivo de cabeçalho `stdio.h`.

A biblioteca padrão de entrada e saída fornece, entre outras funções:

```c
printf()
```

que utilizaremos para escrever informações na saída padrão.

---

## 3.2 Função `main`

Todo programa C executável possui uma função principal:

```c
int main(void)
```

A execução do programa começa por ela.

O tipo:

```c
int
```

indica que a função retorna um número inteiro ao sistema operacional.

---

## 3.3 Exibindo uma mensagem

A instrução:

```c
printf("Hello, World!\n");
```

envia a mensagem para a saída padrão.

O caractere especial:

```text
\n
```

representa uma quebra de linha.

---

## 3.4 Retorno do programa

A instrução:

```c
return 0;
```

indica ao sistema operacional que o programa terminou normalmente.

Por convenção:

```text
0
```

representa sucesso.

Valores diferentes de zero normalmente são utilizados para representar algum tipo de erro.

---

# 4. Código-Fonte e Arquivo Executável

O arquivo:

```text
hello.c
```

é um **arquivo de código-fonte**.

Ele contém instruções escritas em uma linguagem compreensível por programadores.

Podemos confirmar seu tipo com:

```bash
file hello.c
```

Uma saída típica é:

```text
hello.c: C source, ASCII text
```

Esse arquivo, porém, não pode ser executado diretamente pelo processador.

Antes disso, precisa ser transformado em código executável.

---

# 5. O Compilador GCC

Nesta disciplina utilizaremos o **GCC — GNU Compiler Collection**.

Verifique se ele está instalado:

```bash
gcc --version
```

Uma saída típica será semelhante a:

```text
gcc (Ubuntu ...) ...
```

Caso o GCC não esteja instalado:

```bash
sudo apt update
sudo apt install build-essential
```

O pacote `build-essential` inclui ferramentas importantes como:

```text
gcc
g++
make
```

---

# 6. Compilando o Primeiro Programa

Para compilar:

```bash
gcc hello.c
```

Verifique o conteúdo do diretório:

```bash
ls
```

Agora deverá existir um arquivo chamado:

```text
a.out
```

Por padrão, quando não especificamos o nome do executável, o GCC utiliza esse nome.

Podemos verificar:

```bash
file a.out
```

A saída será semelhante a:

```text
a.out: ELF 64-bit LSB pie executable, x86-64, ...
```

Observe a diferença:

```text
hello.c  → código-fonte
a.out    → arquivo executável
```

---

# 7. Executando o Programa

Para executar o programa:

```bash
./a.out
```

Resultado:

```text
Hello, World!
```

O prefixo:

```text
./
```

indica que o programa está localizado no diretório atual.

---

# 8. Por que não Digitar Apenas `a.out`?

Experimente:

```bash
a.out
```

Dependendo da configuração do sistema, a saída deverá ser semelhante a:

```text
command not found
```

O shell procura comandos nos diretórios definidos na variável:

```bash
echo $PATH
```

O diretório atual normalmente não faz parte do `PATH`.

Por isso utilizamos:

```bash
./a.out
```

onde:

```text
.
```

representa o diretório atual.

---

# 9. Definindo o Nome do Executável

Normalmente é mais conveniente escolher um nome para o programa.

Utilize a opção:

```text
-o
```

Por exemplo:

```bash
gcc hello.c -o hello
```

Agora:

```bash
ls
```

deverá mostrar:

```text
hello
hello.c
```

Execute:

```bash
./hello
```

Resultado:

```text
Hello, World!
```

---

# 10. Alterando o Código e Recompilando

Modifique `hello.c`:

```c
#include <stdio.h>

int main(void) {
    printf("Sistemas Operacionais\n");

    return 0;
}
```

Se executarmos imediatamente:

```bash
./hello
```

o executável antigo ainda será utilizado.

É necessário recompilar:

```bash
gcc hello.c -o hello
```

Agora:

```bash
./hello
```

Resultado:

```text
Sistemas Operacionais
```

Esse comportamento é importante:

```text
alterar o .c ≠ alterar automaticamente o executável
```

Após cada alteração no código-fonte, é necessário gerar novamente o programa.

---

# 11. Etapas da Compilação

Embora normalmente utilizemos apenas:

```bash
gcc hello.c -o hello
```

o processo de compilação possui várias etapas.

De forma simplificada:

```text
hello.c
  │
  ▼
Pré-processamento
  │
  ▼
Compilação
  │
  ▼
Assembly
  │
  ▼
Código objeto
  │
  ▼
Linkedição
  │
  ▼
hello
```

O GCC pode permitir a observação dessas etapas separadamente.

---

# 12. Pré-processamento

Execute:

```bash
gcc -E hello.c -o hello.i
```

Agora:

```bash
ls
```

teremos algo semelhante a:

```text
hello
hello.c
hello.i
```

O arquivo:

```text
hello.i
```

contém o resultado do pré-processamento.

Observe:

```bash
less hello.i
```

Nesse estágio, diretivas como:

```c
#include <stdio.h>
```

já foram processadas.

Saia do `less` pressionando:

```text
q
```

---

# 13. Gerando Assembly

Podemos solicitar ao GCC a geração de código Assembly:

```bash
gcc -S hello.c -o hello.s
```

Visualize:

```bash
less hello.s
```

O arquivo contém instruções de baixo nível geradas a partir do código C.

Isso demonstra uma transformação importante:

```text
C → Assembly
```

---

# 14. Gerando Código Objeto

Execute:

```bash
gcc -c hello.c -o hello.o
```

O arquivo:

```text
hello.o
```

é um **arquivo objeto**.

Verifique:

```bash
file hello.o
```

Exemplo:

```text
hello.o: ELF 64-bit LSB relocatable, x86-64, ...
```

Ele contém código de máquina, porém ainda não representa necessariamente o executável final.

---

# 15. Linkedição

Podemos gerar o executável a partir do arquivo objeto:

```bash
gcc hello.o -o hello
```

Agora:

```bash
./hello
```

funcionará normalmente.

Assim, uma visão simplificada é:

```text
hello.c
   │
   │ gcc -c
   ▼
hello.o
   │
   │ gcc
   ▼
hello
```

---

# 16. Compilação com Avisos

O compilador pode detectar diversas situações potencialmente problemáticas.

É recomendável habilitar avisos durante a compilação:

```bash
gcc -Wall hello.c -o hello
```

Uma opção ainda mais útil é:

```bash
gcc -Wall -Wextra hello.c -o hello
```

Durante a disciplina, podemos adotar como padrão:

```bash
gcc -Wall -Wextra programa.c -o programa
```

Os avisos não são necessariamente erros, mas frequentemente revelam problemas no código.

---

# 17. Exemplo de Warning

Considere:

```c
#include <stdio.h>

int main(void) {
    int valor;

    printf("Hello!\n");

    return 0;
}
```

Salve como:

```text
warning.c
```

Compile:

```bash
gcc -Wall -Wextra warning.c -o warning
```

O compilador poderá avisar que:

```text
valor
```

foi declarado, mas não utilizado.

Warnings devem ser analisados, e não simplesmente ignorados.

---

# 18. Erros de Compilação

Considere o arquivo:

```c
#include <stdio.h>

int main(void) {
    printf("Hello, World!\n")

    return 0;
}
```

Observe que está faltando:

```text
;
```

após o `printf`.

Compile:

```bash
gcc hello.c -o hello
```

O GCC apresentará um erro e não produzirá corretamente o executável.

Uma parte importante da programação em C é aprender a interpretar mensagens do compilador.

---

# 19. Verificando o Código de Retorno

Quando um programa termina, ele devolve um valor ao sistema operacional.

Execute:

```bash
./hello
```

Depois:

```bash
echo $?
```

Se o programa retornou:

```c
return 0;
```

a saída será:

```text
0
```

Agora considere:

```c
#include <stdio.h>

int main(void) {
    printf("Encerrando com erro\n");

    return 1;
}
```

Compile e execute:

```bash
gcc retorno.c -o retorno
./retorno
echo $?
```

A saída final deverá ser:

```text
1
```

Esse mecanismo será importante posteriormente ao trabalhar com processos e shell scripts.

---

# 20. Parâmetros da Função `main`

Até agora utilizamos:

```c
int main(void)
```

Outra forma comum é:

```c
int main(int argc, char *argv[])
```

Os dois parâmetros permitem que um programa receba argumentos pela linha de comando.

Temos:

```text
argc
```

e:

```text
argv
```

onde:

* `argc` indica a quantidade de argumentos;
* `argv` contém os argumentos recebidos pelo programa.

---

# 21. Primeiro Exemplo com `argc`

Crie:

```text
argc.c
```

com:

```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    printf("Quantidade de argumentos: %d\n", argc);

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra argc.c -o argc
```

Execute:

```bash
./argc
```

Resultado:

```text
Quantidade de argumentos: 1
```

Isso pode parecer estranho inicialmente.

Mesmo sem passarmos argumentos adicionais, o próprio nome utilizado para executar o programa é considerado um argumento.

---

# 22. Passando Argumentos

Execute:

```bash
./argc teste
```

Resultado:

```text
Quantidade de argumentos: 2
```

Agora:

```bash
./argc um dois tres
```

Resultado:

```text
Quantidade de argumentos: 4
```

Portanto:

```text
./argc um dois tres
```

corresponde a:

```text
argv[0] = "./argc"
argv[1] = "um"
argv[2] = "dois"
argv[3] = "tres"
```

e:

```text
argc = 4
```

---

# 23. Utilizando `argv`

Crie:

```text
argumentos.c
```

com:

```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    printf("argc = %d\n", argc);

    for (int i = 0; i < argc; i++) {
        printf("argv[%d] = %s\n", i, argv[i]);
    }

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra argumentos.c -o argumentos
```

Execute:

```bash
./argumentos sistemas operacionais linux
```

Resultado esperado:

```text
argc = 4
argv[0] = ./argumentos
argv[1] = sistemas
argv[2] = operacionais
argv[3] = linux
```

---

# 24. Argumentos com Espaços

Considere:

```bash
./argumentos sistemas operacionais
```

O shell interpreta:

```text
sistemas
```

e:

```text
operacionais
```

como argumentos separados.

Para enviar um texto contendo espaços como um único argumento:

```bash
./argumentos "sistemas operacionais"
```

Resultado:

```text
argc = 2
argv[0] = ./argumentos
argv[1] = sistemas operacionais
```

---

# 25. Programa que Recebe um Nome

Crie:

```text
nome.c
```

```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Uso: %s <nome>\n", argv[0]);
        return 1;
    }

    printf("Olá, %s!\n", argv[1]);

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra nome.c -o nome
```

Execute:

```bash
./nome Paulo
```

Resultado:

```text
Olá, Paulo!
```

Experimente executar sem argumento:

```bash
./nome
```

Resultado:

```text
Uso: ./nome <nome>
```

Verifique:

```bash
echo $?
```

Resultado:

```text
1
```

Esse padrão é bastante utilizado em programas de linha de comando.

---

# 26. Recebendo Números pela Linha de Comando

Os elementos de `argv` são strings.

Considere:

```bash
./soma 10 20
```

Os valores:

```text
10
20
```

chegam ao programa como texto.

Para convertê-los para números, podemos utilizar funções da biblioteca padrão.

Um exemplo simples:

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
    if (argc != 3) {
        printf("Uso: %s <numero1> <numero2>\n", argv[0]);
        return 1;
    }

    int a = atoi(argv[1]);
    int b = atoi(argv[2]);

    printf("%d + %d = %d\n", a, b, a + b);

    return 0;
}
```

Salve como:

```text
soma.c
```

Compile:

```bash
gcc -Wall -Wextra soma.c -o soma
```

Execute:

```bash
./soma 10 20
```

Resultado:

```text
10 + 20 = 30
```

> `atoi()` é adequada para um primeiro exemplo didático, embora programas reais normalmente utilizem funções que permitem melhor validação dos valores recebidos, como `strtol()`.

---

# 27. Compilando Vários Arquivos Manualmente

Um programa não precisa estar inteiramente em um único arquivo.

Considere:

```text
main.c
mensagem.c
mensagem.h
```

Arquivo `mensagem.h`:

```c
#ifndef MENSAGEM_H
#define MENSAGEM_H

void exibir_mensagem(void);

#endif
```

Arquivo `mensagem.c`:

```c
#include <stdio.h>
#include "mensagem.h"

void exibir_mensagem(void) {
    printf("Sistemas Operacionais\n");
}
```

Arquivo `main.c`:

```c
#include "mensagem.h"

int main(void) {
    exibir_mensagem();

    return 0;
}
```

Podemos compilar tudo de uma vez:

```bash
gcc -Wall -Wextra main.c mensagem.c -o programa
```

Execute:

```bash
./programa
```

Resultado:

```text
Sistemas Operacionais
```

---

# 28. Compilando os Arquivos Separadamente

Também podemos produzir arquivos objeto independentes.

Compile:

```bash
gcc -Wall -Wextra -c main.c -o main.o
```

Depois:

```bash
gcc -Wall -Wextra -c mensagem.c -o mensagem.o
```

Verifique:

```bash
ls
```

Agora teremos:

```text
main.c
main.o
mensagem.c
mensagem.h
mensagem.o
```

Finalmente, faça a linkedição:

```bash
gcc main.o mensagem.o -o programa
```

Execute:

```bash
./programa
```

Esse processo começa a mostrar por que automatizar compilações é importante.

---

# 29. O Problema da Compilação Manual

Imagine um projeto com:

```text
main.c
processo.c
memoria.c
arquivo.c
util.c
```

Seria possível compilar utilizando:

```bash
gcc main.c processo.c memoria.c arquivo.c util.c -o programa
```

Porém, conforme o projeto cresce:

* os comandos ficam maiores;
* precisamos lembrar quais arquivos compõem o programa;
* repetimos opções de compilação;
* frequentemente recompilamos arquivos que não foram modificados.

Uma ferramenta tradicional para resolver esse problema é o:

```text
make
```

---

# 30. Introdução ao `make`

O comando:

```bash
make
```

é utilizado para automatizar tarefas de construção de software.

Ele normalmente lê instruções de um arquivo chamado:

```text
Makefile
```

Um Makefile descreve:

* quais arquivos devem ser gerados;
* de quais arquivos eles dependem;
* quais comandos devem ser executados.

---

# 31. Primeiro Makefile

Para nosso `hello.c`, crie um arquivo chamado exatamente:

```text
Makefile
```

com:

```makefile
hello: hello.c
	gcc -Wall -Wextra hello.c -o hello
```

> A linha com `gcc` deve começar com um caractere de **tabulação**, e não apenas espaços.

Execute:

```bash
make
```

A saída deverá mostrar:

```text
gcc -Wall -Wextra hello.c -o hello
```

Depois:

```bash
./hello
```

---

# 32. Entendendo a Regra do Makefile

A estrutura básica é:

```makefile
alvo: dependencias
	comando
```

No nosso exemplo:

```makefile
hello: hello.c
	gcc -Wall -Wextra hello.c -o hello
```

Temos:

```text
alvo:
hello
```

```text
dependência:
hello.c
```

```text
comando:
gcc -Wall -Wextra hello.c -o hello
```

Em outras palavras:

> Para gerar `hello`, utilize `hello.c` e execute o comando indicado.

---

# 33. Executando `make` Novamente

Depois de compilar, execute novamente:

```bash
make
```

Uma saída típica será:

```text
make: 'hello' is up to date.
```

O `make` compara as datas de modificação dos arquivos.

Se:

```text
hello.c
```

não foi alterado desde a última compilação, não existe necessidade de gerar `hello` novamente.

Agora modifique `hello.c` e execute:

```bash
make
```

A compilação será realizada novamente.

---

# 34. Variáveis em um Makefile

Podemos evitar repetição utilizando variáveis:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra

hello: hello.c
	$(CC) $(CFLAGS) hello.c -o hello
```

Agora:

```bash
make
```

equivale a executar:

```bash
gcc -Wall -Wextra hello.c -o hello
```

As variáveis facilitam a manutenção do projeto.

---

# 35. Makefile com Regra `clean`

É comum criar uma regra para remover arquivos gerados durante a compilação.

```makefile
CC = gcc
CFLAGS = -Wall -Wextra

hello: hello.c
	$(CC) $(CFLAGS) hello.c -o hello

clean:
	rm -f hello
```

Agora podemos executar:

```bash
make
```

e:

```bash
make clean
```

Depois:

```bash
ls
```

O executável terá sido removido.

---

# 36. Alvos `.PHONY`

Como `clean` representa uma ação e não um arquivo que desejamos gerar, podemos marcá-lo como:

```makefile
.PHONY: clean
```

O Makefile completo fica:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra

hello: hello.c
	$(CC) $(CFLAGS) hello.c -o hello

clean:
	rm -f hello

.PHONY: clean
```

---

# 37. Regra `all`

Também é comum existir uma regra principal chamada:

```text
all
```

Exemplo:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra

all: hello

hello: hello.c
	$(CC) $(CFLAGS) hello.c -o hello

clean:
	rm -f hello

.PHONY: all clean
```

Agora:

```bash
make
```

executará o primeiro alvo, `all`.

Também podemos escrever explicitamente:

```bash
make all
```

---

# 38. Makefile para Vários Arquivos

Retomando o exemplo:

```text
main.c
mensagem.c
mensagem.h
```

podemos criar:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra

programa: main.o mensagem.o
	$(CC) main.o mensagem.o -o programa

main.o: main.c mensagem.h
	$(CC) $(CFLAGS) -c main.c -o main.o

mensagem.o: mensagem.c mensagem.h
	$(CC) $(CFLAGS) -c mensagem.c -o mensagem.o

clean:
	rm -f *.o programa

.PHONY: clean
```

Execute:

```bash
make
```

O `make` determinará quais arquivos precisam ser gerados e em qual ordem.

---

# 39. Modificando Apenas um Arquivo

Compile:

```bash
make
```

Depois modifique somente:

```text
mensagem.c
```

Execute novamente:

```bash
make
```

O `make` não precisa recompilar `main.c`.

Ele recompilará apenas:

```text
mensagem.c → mensagem.o
```

e depois realizará a linkedição novamente.

Essa é uma das principais vantagens do sistema de dependências.

---

# 40. Limpando e Recompilando

Para remover os arquivos gerados:

```bash
make clean
```

Verifique:

```bash
ls
```

Depois compile novamente:

```bash
make
```

Uma sequência bastante comum é:

```bash
make clean
make
```

---

# 41. Inspecionando o Executável

Depois de compilar:

```bash
gcc hello.c -o hello
```

podemos investigar o arquivo.

Utilize:

```bash
file hello
```

Também podemos verificar o tamanho:

```bash
ls -lh hello
```

Ou:

```bash
du -h hello
```

Outro comando interessante é:

```bash
ldd hello
```

Ele mostra bibliotecas compartilhadas utilizadas pelo programa.

Uma saída poderá incluir:

```text
libc.so.6
```

Isso será explorado com mais profundidade posteriormente.

---

# 42. Permissões do Executável

Execute:

```bash
ls -l hello
```

Uma saída típica:

```text
-rwxr-xr-x 1 aluno aluno ... hello
```

Observe os caracteres:

```text
x
```

Eles representam permissão de execução.

Podemos remover essa permissão:

```bash
chmod -x hello
```

Agora:

```bash
./hello
```

deverá gerar erro de permissão.

Restabeleça:

```bash
chmod +x hello
```

Execute novamente:

```bash
./hello
```

---

# 43. Código-Fonte Não é Executável

Observe:

```bash
ls -l hello.c
```

Normalmente teremos algo semelhante a:

```text
-rw-r--r-- ...
```

Não há permissão `x`.

Mesmo adicionando:

```bash
chmod +x hello.c
```

isso não transforma o código-fonte C em um programa compilado.

A compilação e a permissão de execução são conceitos diferentes.

---

# 44. Exercício 1 — Informações do Aluno

Crie um programa chamado:

```text
aluno.c
```

que produza:

```text
Disciplina: Sistemas Operacionais
Curso: Engenharia de Software
Aluno: Nome do aluno
```

Compile utilizando:

```bash
gcc -Wall -Wextra aluno.c -o aluno
```

Execute:

```bash
./aluno
```

---

# 45. Exercício 2 — Argumentos

Crie um programa chamado:

```text
info.c
```

que receba dois argumentos:

```text
nome
matricula
```

Exemplo:

```bash
./info Paulo 20260001
```

Saída:

```text
Nome: Paulo
Matrícula: 20260001
```

Caso a quantidade de argumentos seja incorreta, o programa deverá apresentar:

```text
Uso: ./info <nome> <matricula>
```

e retornar código diferente de zero.

---

# 46. Exercício 3 — Operação Aritmética

Crie:

```text
multiplica.c
```

O programa deverá receber dois números:

```bash
./multiplica 5 8
```

e apresentar:

```text
5 x 8 = 40
```

Compile utilizando:

```bash
gcc -Wall -Wextra multiplica.c -o multiplica
```

---

# 47. Exercício 4 — Listando os Argumentos

Crie um programa que receba uma quantidade arbitrária de argumentos.

Exemplo:

```bash
./listar Linux processos threads memoria
```

Saída:

```text
Argumento 0: ./listar
Argumento 1: Linux
Argumento 2: processos
Argumento 3: threads
Argumento 4: memoria

Total de argumentos: 5
```

---

# 48. Exercício 5 — Criando um Makefile

Escolha um dos programas anteriores e crie um `Makefile` contendo:

* variável `CC`;
* variável `CFLAGS`;
* alvo `all`;
* regra de compilação;
* alvo `clean`;
* declaração `.PHONY`.

O fluxo esperado deverá ser:

```bash
make
```

```bash
./programa
```

```bash
make clean
```

---

# 49. Desafio — Mini Calculadora

Crie um programa chamado:

```text
calc.c
```

que receba três argumentos:

```text
numero1 operação numero2
```

Exemplo:

```bash
./calc 10 + 20
```

Resultado:

```text
30
```

Outros exemplos:

```bash
./calc 10 - 3
```

```text
7
```

```bash
./calc 5 x 4
```

```text
20
```

O programa também deverá verificar se a quantidade de argumentos está correta.

---

# 50. Comandos Principais da Aula

Os comandos mais importantes utilizados nesta aula foram:

```bash
gcc arquivo.c
```

```bash
gcc arquivo.c -o programa
```

```bash
gcc -Wall -Wextra arquivo.c -o programa
```

```bash
./programa
```

```bash
echo $?
```

```bash
gcc -E arquivo.c -o arquivo.i
```

```bash
gcc -S arquivo.c -o arquivo.s
```

```bash
gcc -c arquivo.c -o arquivo.o
```

```bash
file programa
```

```bash
make
```

```bash
make clean
```

---

# 51. Resultado Esperado

Ao final desta aula, o estudante deverá compreender a sequência:

```text
┌──────────────────┐
│ Código C         │
│ programa.c       │
└────────┬─────────┘
         │
         │ GCC
         ▼
┌──────────────────┐
│ Código objeto    │
│ programa.o       │
└────────┬─────────┘
         │
         │ Linkedição
         ▼
┌──────────────────┐
│ Executável       │
│ programa         │
└────────┬─────────┘
         │
         │ ./
         ▼
┌──────────────────┐
│ Processo em      │
│ execução         │
└──────────────────┘
```

Além disso, deverá ser capaz de:

* escrever um programa básico em C;
* compilar utilizando GCC;
* interpretar erros e warnings básicos;
* executar programas pelo terminal;
* compreender códigos de retorno;
* utilizar `argc` e `argv`;
* passar argumentos pela linha de comando;
* compreender a diferença entre `.c`, `.o` e executável;
* criar um `Makefile` simples;
* utilizar `make` e `make clean`.

---

# 52. Relação com Sistemas Operacionais

Embora esta aula tenha foco inicial em programação C e ferramentas de compilação, os conceitos apresentados serão utilizados diretamente nas próximas atividades.

Quando executamos:

```bash
./hello
```

o arquivo executável não é simplesmente "aberto".

O sistema operacional precisa, entre outras atividades:

* localizar o arquivo executável;
* verificar suas permissões;
* interpretar seu formato;
* criar um novo processo;
* criar um espaço de endereçamento;
* carregar código e dados em memória;
* preparar a pilha;
* disponibilizar os argumentos de linha de comando;
* iniciar a execução;
* gerenciar a utilização da CPU;
* receber o código de retorno ao término do processo.

Portanto, esse programa aparentemente simples:

```c
#include <stdio.h>

int main(void) {
    printf("Hello, World!\n");
    return 0;
}
```

será o ponto de partida para estudar vários mecanismos internos de um sistema operacional.

---

# Próximo Conteúdo

➡️ [02b — Introdução à Linguagem C](../02b-introducao-c/README.md)

# Resumo do Conteúdo

O próximo material apresenta uma **introdução à linguagem C para estudantes que já possuem experiência com Python**, com foco nos conceitos necessários para acompanhar as próximas aulas de **Sistemas Operacionais**. 

Os principais conteúdos abordados incluem:

* diferenças entre Python e C;
* tipos de dados e variáveis;
* operadores e estruturas de controle;
* funções e passagem de parâmetros;
* arrays e strings;
* endereços de memória;
* ponteiros;
* passagem por referência;
* `struct`, `typedef` e `enum`;
* stack e heap;
* alocação dinâmica de memória;
* `malloc()`, `calloc()`, `realloc()` e `free()`;
* erros de acesso à memória;
* manipulação de arquivos;
* `stdin`, `stdout` e `stderr`;
* compilação e depuração;
* relação entre C e o sistema operacional.

Esses conceitos servem de base para compreender posteriormente **processos, memória, arquivos, threads, chamadas de sistema e a comunicação entre programas, kernel e hardware**.