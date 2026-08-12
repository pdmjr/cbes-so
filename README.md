# Sistemas Operacionais

Este repositório reúne os materiais práticos utilizados na disciplina de **Sistemas Operacionais** do curso de **Bacharelado em Engenharia de Software** do **IFPB**.

O objetivo é complementar os conceitos apresentados em sala de aula com atividades práticas realizadas em ambiente Linux, explorando ferramentas de linha de comando, programação em C, processos, threads, memória, sistemas de arquivos e outros mecanismos fundamentais de um sistema operacional.

## Organização dos conteúdos

Os conteúdos estão organizados em módulos independentes. Cada módulo possui seu próprio material, exemplos de código e, quando aplicável, exercícios práticos.

### 1. Preparação do Ambiente

[➡️ Acessar: Preparação do Ambiente](./01-preparacao-ambiente/README.md)

Configuração do ambiente que será utilizado durante as aulas práticas da disciplina.

Principais tópicos:

* utilização de uma **máquina virtual Ubuntu**;
* instalação e configuração do **servidor SSH**;
* criação e utilização de **chaves SSH pública/privada**;
* acesso remoto à máquina virtual;
* instalação e configuração do **Zsh**;
* instalação do **Oh My Zsh**;
* utilização de terminais em ambientes Windows, como **MINGW64/Git Bash** ou **Cygwin**;
* instalação do **Visual Studio Code**;
* instalação da extensão **Remote - SSH**;
* acesso e edição de arquivos da VM diretamente pelo VS Code.

---

### 2. Compilando um Código-Fonte em C

[➡️ Acessar: Compilando um Código-Fonte em C](./02-compilacao-c/README.md)

Introdução ao processo de criação e compilação de programas escritos em **C** utilizando a linha de comando do Linux.

Principais tópicos:

* estrutura básica de um programa em C;
* primeiro programa: `Hello, World!`;
* utilização do compilador **GCC**;
* diferença entre código-fonte e arquivo executável;
* compilação pela linha de comando;
* execução de programas no terminal;
* parâmetros da função `main`;
* utilização de `argc` e `argv`;
* passagem de argumentos pela linha de comando;
* introdução à automação do processo de compilação;
* criação de um **Makefile** simples;
* utilização do comando `make`.

---

## Próximos conteúdos

Ao longo da disciplina, novos módulos serão adicionados a este repositório.

Uma possível organização das próximas aulas é:

### 3. Processos

[➡️ Acessar: Processos](./03-processos/README.md)

* conceito de processo;
* PID e PPID;
* visualização de processos;
* comandos `ps`, `top` e `htop`;
* criação de processos com `fork()`;
* substituição de processos com a família `exec()`;
* espera por processos com `wait()` e `waitpid()`;
* processos pai e filho;
* processos órfãos e zumbis.

### 4. Threads

[➡️ Acessar: Threads](./04-threads/README.md)

* processos × threads;
* criação de threads com `pthread`;
* passagem de parâmetros;
* retorno de threads;
* `pthread_create()`;
* `pthread_join()`;
* compartilhamento de memória entre threads.

### 5. Sincronização e Concorrência

[➡️ Acessar: Sincronização e Concorrência](./05-sincronizacao/README.md)

* condições de corrida;
* regiões críticas;
* exclusão mútua;
* mutex;
* semáforos;
* problemas clássicos de sincronização.

### 6. Comunicação entre Processos

[➡️ Acessar: Comunicação entre Processos](./06-ipc/README.md)

* Interprocess Communication (IPC);
* pipes;
* named pipes;
* sinais;
* memória compartilhada;
* filas de mensagens.

### 7. Escalonamento de Processos

[➡️ Acessar: Escalonamento de Processos](./07-escalonamento/README.md)

* estados de um processo;
* filas de processos;
* escalonador;
* troca de contexto;
* algoritmos de escalonamento;
* prioridades;
* observação do escalonamento no Linux.

### 8. Deadlocks

[➡️ Acessar: Deadlocks](./08-deadlocks/README.md)

* definição de deadlock;
* condições necessárias;
* prevenção;
* evitação;
* detecção;
* recuperação;
* exemplos práticos.

### 9. Gerenciamento de Memória

[➡️ Acessar: Gerenciamento de Memória](./09-memoria/README.md)

* espaço de endereçamento de processos;
* stack e heap;
* alocação dinâmica;
* memória virtual;
* paginação;
* page faults;
* comandos para análise de memória no Linux.

### 10. Sistemas de Arquivos

[➡️ Acessar: Sistemas de Arquivos](./10-sistemas-arquivos/README.md)

* arquivos e diretórios;
* descritores de arquivos;
* chamadas de sistema;
* `open()`, `read()`, `write()` e `close()`;
* permissões;
* links;
* organização de sistemas de arquivos no Linux.

---

## Ambiente utilizado

Os exemplos das aulas consideram, preferencialmente, o seguinte ambiente:

```text
Sistema Operacional: Ubuntu Linux
Compilador: GCC
Editor/IDE: Visual Studio Code
Acesso remoto: SSH
Shell: Zsh + Oh My Zsh
Linguagem principal: C
```

Alguns exemplos poderão utilizar ferramentas adicionais, que serão apresentadas e instaladas ao longo da disciplina.

## Estrutura planejada do repositório

```text
sistemas-operacionais/
│
├── README.md
│
├── 01-preparacao-ambiente/
│   └── README.md
│
├── 02-compilacao-c/
│   ├── README.md
│   ├── hello.c
│   ├── argumentos.c
│   └── Makefile
│
├── 03-processos/
│   ├── README.md
│   └── exemplos/
│
├── 04-threads/
│   ├── README.md
│   └── exemplos/
│
├── 05-sincronizacao/
│   └── README.md
│
├── 06-ipc/
│   └── README.md
│
├── 07-escalonamento/
│   └── README.md
│
├── 08-deadlocks/
│   └── README.md
│
├── 09-memoria/
│   └── README.md
│
└── 10-sistemas-arquivos/
    └── README.md
```

## Como utilizar este material

Para acompanhar as atividades práticas, recomenda-se:

1. acessar a VM Ubuntu via SSH;
2. abrir o diretório da aula utilizando o VS Code;
3. acompanhar os exemplos apresentados durante a aula;
4. compilar e executar os códigos diretamente no terminal;
5. modificar os exemplos e observar o comportamento do sistema;
6. realizar os exercícios propostos em cada módulo.

> **Importante:** em Sistemas Operacionais, executar, modificar e observar os programas é parte fundamental do processo de aprendizagem. Sempre que possível, experimente alterar os exemplos e analisar o comportamento resultante.

---

**Disciplina:** Sistemas Operacionais
**Curso:** Bacharelado em Engenharia de Software
